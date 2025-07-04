# 🩺 AzuraForge: Sorun Giderme Rehberi

Bu belge, AzuraForge platformunda geliştirme yaparken karşılaşılan yaygın sorunları, nedenlerini ve kanıtlanmış çözümlerini içerir. Bir sorunla karşılaştığınızda ilk başvuracağınız yer burası olmalıdır.

## 1. Konteyner Başlatma Hataları (`Exited (1)`)

Bir veya daha fazla servis (`api`, `worker` vb.) `docker-compose up` sonrası `Exited (1)` durumuna geçiyorsa, bu genellikle servisin başlangıç anında bir hata ile karşılaştığını gösterir.

**Kontrol Adımları:**
1.  **Logları İncele:** Hatanın kök nedenini bulmak için her zaman ilk olarak çöken servisin loglarını kontrol edin:
    ```bash
    docker-compose logs <servis_adi>
    ```
2.  Aşağıdaki yaygın senaryoları kontrol edin.

---

### Vaka Analizi 1: `ImportError` veya `NameError`
*   **Hata Logu:** `ImportError: cannot import name ...` veya `NameError: name '...' is not defined`.
*   **Neden:** Genellikle bir Python dosyasının en üstüne gerekli `import` satırının eklenmesi unutulmuştur. Projemizdeki son `NameError: name 'abstractmethod' is not defined` hatası buna bir örnektir.
*   **Çözüm:** Hata mesajında belirtilen dosyayı açın ve eksik olan importu (örn: `from abc import abstractmethod`) ekleyin.

### Vaka Analizi 2: Döngüsel İçe Aktarma (`Circular Import`)
*   **Hata Logu:** `ImportError: cannot import name '...' from partially initialized module '...' (most likely due to a circular import)`
*   **Neden:** İki Python modülü (örneğin `A.py` ve `B.py`) birbirini import etmeye çalıştığında oluşur. Bu, kodun "Tek Sorumluluk Prensibi"ne aykırı şekilde yapılandırıldığını gösterir.
*   **Çözüm:** Bağımlılıkları yeniden yapılandırın. Ortak fonksiyonları, her iki modülün de import edebileceği üçüncü bir yardımcı modüle (örn: `utils.py`) taşıyın. Böylece bağımlılık akışı tek yönlü hale gelir (`A -> utils`, `B -> utils`).

### Vaka Analizi 3: `passlib` ve `bcrypt` Uyumsuzluğu
*   **Hata Logu:** `(trapped) error reading bcrypt version` veya `AttributeError: module 'bcrypt' has no attribute '__about__'`
*   **Neden:** `pip` tarafından kurulan `passlib` ve `bcrypt` kütüphanelerinin versiyonları birbiriyle uyumlu değildir.
*   **Çözüm:** İlgili servisin (`api` gibi) `pyproject.toml` dosyasında, bu kütüphanelerin uyumlu olduğu bilinen versiyonlarını sabitleyin ("pinleyin"):
    ```toml
    dependencies = [
        "passlib[bcrypt]",
        "bcrypt==4.1.3",
        ...
    ]
    ```

### Vaka Analizi 4: Celery Worker Süreçlerinde Veritabanı Bağlantı Hatası
*   **Hata Logu:** `RuntimeError: Database engine not initialized for this worker process.`
*   **Neden:** Celery'nin `prefork` modelinde, ana worker süreci başladığında oluşturulan veritabanı bağlantısı (`engine`), görevleri işleyen çocuk süreçlere miras kalmaz.
*   **Çözüm:** Her bir çocuk sürecin kendi veritabanı bağlantısını oluşturmasını sağlamak için Celery'nin `worker_process_init` sinyalini kullanın. `worker/src/azuraforge_worker/celery_app.py` dosyasındaki gibi bir yapı kurun:
    ```python
    from celery.signals import worker_process_init
    
    engine = None

    @worker_process_init.connect
    def init_worker_db_connection(**kwargs):
        global engine
        # ... engine'i burada oluştur ...
    ```

---

## 2. Docker Compose ve Ortam Değişkenleri

### Sorun: `The "VAR_NAME" variable is not set. Defaulting to a blank string.` Uyarısı
*   **Neden:** Bu bir **uyarıdır, hata değil**. `docker-compose.yml` dosyanızda `${VAR_NAME}` gibi bir değişken varsa, Docker Compose bu değeri önce `.env` dosyanızda arar. Bulamazsa bu uyarıyı verir.
*   **Ne Zaman Güvenli?** Eğer bu değişkeni, bizim yaptığımız gibi, Docker'ın `secrets` mekanizması ile konteynerin içine **doğrudan** atıyorsanız, bu uyarı zararsızdır ve görmezden gelinebilir. `postgres` servisindeki `healthcheck` komutu buna bir örnektir.
*   **Ne Zaman Tehlikeli?** Eğer bu değişkeni, uygulamanızın **doğrudan okuması** gerekiyorsa (örneğin `environment` bloğunda `DATABASE_URL=${DB_USER}:${DB_PASS}...` gibi bir kullanım), bu uyarı, uygulamanıza boş değerlerin gittiği ve muhtemelen çökeceği anlamına gelir.
*   **En Sağlam Çözüm:** Hassas bilgileri (`secrets` ile) ve hassas olmayan konfigürasyonları (`environment` ile) ayrı tutun. Python/Node.js kodunuzun bu iki kaynaktan da okuyacak şekilde akıllı olmasını sağlayın. `dbmodels/database.py` dosyasındaki `get_database_url` fonksiyonu buna iyi bir örnektir.
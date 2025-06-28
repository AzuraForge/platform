# AzuraForge Platform 🚀

**AzuraForge Platform**, yapay zeka modellerini sıfırdan oluşturmak, eğitmek ve yönetmek için tasarlanmış modüler, dağıtık ve eklenti tabanlı bir MLOps platformudur. Modern mikroservis mimarisi prensipleriyle inşa edilmiştir.

Bu depo, AzuraForge ekosistemindeki tüm ana servisleri (API, Worker, Dashboard) ve kütüphaneleri (Core, Learner, Applications) bir araya getiren **orkestrasyon katmanıdır**.

## 🎯 Temel Amaçlar ve Felsefe

*   **Sıfırdan İnşa:** Derin öğrenme motoru ve temel bileşenler sıfırdan geliştirilmiştir.
*   **Modülerlik ve Bağımsızlık:** Her bileşen (kütüphane, API, worker, UI, uygulama) kendi bağımsız repo'sunda yaşar ve kendi sorumluluğuna sahiptir.
*   **Olay Güdümlü Mimari:** Servisler arası iletişim olay tabanlı (Celery, Redis, WebSockets) gerçekleşir.
*   **Eklenti Tabanlı:** Yeni yapay zeka modelleri ve uygulamaları, platformun çekirdek koduna dokunmadan birer eklenti (plugin) olarak eklenebilir.
*   **Ölçeklenebilirlik:** Dağıtık servisler sayesinde yatayda ölçeklenebilir.
*   **Profesyonel Geliştirici Deneyimi:** Otomatik kurulum, test ve dokümantasyon ile geliştirme sürecini kolaylaştırmak.

## 🏛️ Mimari Genel Bakış

AzuraForge platformu, aşağıdaki bağımsız GitHub depolarından oluşan bir mikroservis mimarisini benimser:

-   **`core`** (`azuraforge-core`): Otomatik türev yeteneklerine sahip temel matematik motoru (NumPy/CuPy).
-   **`learner`** (`azuraforge-learner`): `core` üzerinde geliştirilmiş yüksek seviyeli derin öğrenme kütüphanesi (Katmanlar, Optimizatörler, Kayıp Fonksiyonları, `Learner` sınıfı).
-   **`applications`** (`azuraforge-applications`): Platform için resmi uygulama eklentilerinin katalogu (JSON dosyası).
-   **`app-stock-predictor`** (`azuraforge-app-stock-predictor`): Gerçek bir uygulama eklentisi örneği (Hisse Senedi Tahmini).
-   **`api`** (`azuraforge-api`): RESTful API ve WebSocket endpoint'leri sunan iletişim katmanı.
-   **`worker`** (`azuraforge-worker`): Arka plan görevlerini işleyen ve uygulama eklentilerini çalıştıran işçi servisi.
-   **`dashboard`** (`azuraforge-dashboard`): Platform için web tabanlı kullanıcı arayüzü.

Bu repo, tüm bu servisleri tek bir `docker-compose` komutuyla ayağa kaldıran ana orkestrasyon katmanıdır.

## 🚀 Hızlı Başlangıç (Docker Compose ile)

Tüm platformu yerel makinenizde tek bir komutla başlatmak için:

1.  **Docker Desktop'ın yüklü ve çalıştığından emin olun.**
2.  **Bu repoyu klonlayın:**
    ```bash
    git clone https://github.com/AzuraForge/platform.git
    cd platform
    ```
3.  **.env dosyasını oluşturun:**
    Proje kök dizininde `.env` adında bir dosya oluşturun ve içine raporların kaydedileceği dizini belirtin.
    ```
    # .env
    REDIS_URL=redis://redis:6379/0
    # Rapor dizini: Worker'ın sonuçları yazacağı ve API'nin okuyacağı host makinedeki dizin
    # Windows için C:/azuraforge_platform_reports veya ./reports
    # Linux/macOS için: ./reports
    REPORTS_DIR=./reports 
    ```
    (Windows kullanıyorsanız `REPORTS_DIR`'i `C:/azuraforge_platform_reports` gibi bir mutlak yola ayarlamanız daha güvenli olabilir).
4.  **Platformu başlatın:**
    ```bash
    docker-compose up --build -d
    ```
    (`-d` parametresi arka planda çalıştırmayı sağlar.)

5.  **Platforma erişin:**
    -   **Dashboard:** `http://localhost:5173`
    -   **API Dokümantasyonu:** `http://localhost:8000/api/v1/docs`

## 🛠️ Geliştirme Rehberi ve İç Detaylar

Bu rehber, platformun nasıl çalıştığını, nasıl katkıda bulunacağınızı ve geliştirme ortamınızı nasıl yöneteceğinizi detaylandırır.

**[Tam Geliştirme Rehberine Git](./docs/DEVELOPMENT_GUIDE.md)**

## 🤝 Katkıda Bulunma

Projenin gelişimine katkıda bulunmak için [CONTRIBUTING.md](./docs/CONTRIBUTING.md) dosyasını inceleyin.

## 🗺️ Yol Haritası ve Gelecek Vizyonu

Projenin tamamlanan aşamaları, mevcut durumu ve gelecek hedefleri hakkında bilgi almak için [PROJECT_JOURNEY.md](./docs/PROJECT_JOURNEY.md) dosyasını okuyun.
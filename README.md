# AzuraForge Platform 🚀

**AzuraForge**, yapay zeka modellerini sıfırdan oluşturmak, eğitmek, canlı olarak takip etmek ve sonuçlarını interaktif raporlarla analiz etmek için tasarlanmış, **olay güdümlü, eklenti tabanlı ve dağıtık bir MLOps platformudur.**

Bu depo, AzuraForge ekosistemindeki tüm ana servisleri (API, Worker, Dashboard) ve kütüphaneleri (Core, Learner, Applications) bir araya getiren **orkestrasyon katmanıdır**.

## ✨ Platform Felsefesi

Platformumuz iki temel prensip üzerine kuruludur:

1.  **Sıfırdan İnşa ve Derin Anlayış:** Temel algoritmaları sıfırdan yazarak sistem üzerinde tam kontrol ve derinlemesine bir anlayış sağlıyoruz.
2.  **Modüler ve Ölçeklenebilir Ekosistem:** Bağımsız servisler, olaylar (Redis Pub/Sub) ve görevler (Celery) üzerinden haberleşerek sağlam, esnek ve büyümeye açık bir yapı oluşturur.

Projemizin felsefesini, evrimini ve detaylı yol haritasını öğrenmek için **[Proje Yolculuğu](./docs/PROJECT_JOURNEY.md)** belgemizi inceleyin.

## 🌟 Platform Yetenekleri

*   **Sıfırdan İnşa Edilmiş Çekirdek:** Otomatik türev, `LSTM` gibi gelişmiş katmanlar ve `Adam` optimizer içeren, saf Python/NumPy tabanlı bir derin öğrenme motoru.
*   **Modüler Mikroservis Mimarisi:** Her bileşen (`api`, `worker`, `learner` vb.) kendi bağımsız reposunda yaşar, bağımsız olarak geliştirilebilir ve kurulabilir.
*   **Olay Güdümlü ve Bloklamayan Akış:** `Celery` ve `Redis Pub/Sub` üzerine kurulu mimari sayesinde, yoğun model eğitimleri bile sistemi bloklamaz.
*   **Canlı Deney Takibi:** `WebSocket` aracılığıyla, devam eden bir eğitimin ilerleme çubuğunu, anlık kayıp değerini ve hatta **tahmin grafiklerinin canlı evrimini** anlık olarak izleme imkanı.
*   **Dinamik ve İnteraktif Raporlama:** Tamamlanan her deney için, `Dashboard` üzerinden erişilebilen, `Chart.js` ile çizilmiş interaktif grafikler ve detaylı metrikler içeren rapor sayfaları.
*   **Genişletilebilir Eklenti Sistemi:** Yeni AI uygulamaları, platformun çekirdek koduna dokunmadan, standartlaştırılmış bir `BasePipeline`'den türetilerek kolayca eklenebilir.

## 🏛️ Mimari Genel Bakış

AzuraForge, aşağıdaki bağımsız GitHub depolarından oluşur:

-   **`core`**: Temel matematik motoru (`Tensor`).
-   **`learner`**: Yüksek seviyeli öğrenme kütüphanesi (`Learner`, `Callback` sistemi, `BasePipeline`).
-   **`applications`**: Resmi uygulama katalogu.
-   **`app-stock-predictor`**: Gerçek bir uygulama eklentisi örneği.
-   **`api`**: RESTful API ve WebSocket (Pub/Sub tabanlı) sunan iletişim katmanı.
-   **`worker`**: Arka plan görevlerini işleyen ve raporları oluşturan işçi servisi.
-   **`dashboard`**: React tabanlı, canlı takip ve dinamik raporlama yeteneklerine sahip web arayüzü.

## 🚀 Hızlı Başlangıç (Docker Compose ile)

1.  **Docker Desktop'ın yüklü ve çalıştığından emin olun.**
2.  **Bu repoyu klonlayın:** `git clone https://github.com/AzuraForge/platform.git`
3.  **.env dosyasını oluşturun:** Proje kök dizininde `.env` oluşturup içine aşağıdaki satırları ekleyin:
    ```
    REDIS_URL=redis://redis:6379/0
    REPORTS_DIR=./reports
    CACHE_DIR=./.cache
    ```
4.  **Gerekli Dizinleri Oluşturun:** `mkdir -p ./reports ./.cache`
5.  **Platformu başlatın:** `docker-compose up --build -d`
6.  **Platforma erişin:**
    -   **Dashboard:** `http://localhost:5173`
    -   **API Dokümantasyonu:** `http://localhost:8000/api/v1/docs`
7.  **Deneyin ve Keşfedin:** Dashboard'dan bir deney başlatın, canlı takip panelini izleyin ve deney bittiğinde "Raporu Görüntüle" butonuyla interaktif sonuçları inceleyin.

## 🛠️ Geliştirme Rehberi ve Katkıda Bulunma

Platformun nasıl çalıştığını, nasıl katkıda bulunacağınızı ve geliştirme ortamınızı nasıl yöneteceğinizi öğrenmek için aşağıdaki belgelere göz atın:

-   **[Geliştirme Rehberi](./docs/DEVELOPMENT_GUIDE.md)**
-   **[Katkıda Bulunma Rehberi](./docs/CONTRIBUTING.md)**
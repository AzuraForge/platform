# AzuraForge Platform 🚀

**AzuraForge Platform**, yapay zeka modellerini sıfırdan oluşturmak, eğitmek ve yönetmek için tasarlanmış modüler, dağıtık ve eklenti tabanlı bir MLOps platformudur. Modern mikroservis mimarisi prensipleriyle inşa edilmiştir.

Bu depo, AzuraForge ekosistemindeki tüm ana servisleri (API, Worker, Dashboard) ve kütüphaneleri (Core, Learner, Applications) bir araya getiren **orkestrasyon katmanıdır**.

## 🎯 Temel Amaçlar ve Felsefe

*   **Sıfırdan İnşa:** Derin öğrenme motoru (`azuraforge-core`) ve temel bileşenler sıfırdan geliştirilmiştir.
*   **Modülerlik ve Bağımsızlık:** Her bileşen (kütüphane, API, worker, UI, uygulama) kendi bağımsız repo'sunda yaşar ve kendi sorumluluğuna sahiptir.
*   **Olay Güdümlü Mimari:** Servisler arası iletişim olay tabanlı (Celery, Redis Pub/Sub, WebSockets) gerçekleşir. Bu sayede, yoğun hesaplama görevleri bile kullanıcı arayüzünü bloklamaz.
*   **Eklenti Tabanlı:** Yeni yapay zeka modelleri ve uygulamaları, platformun çekirdek koduna dokunmadan birer eklenti (plugin) olarak eklenebilir.
*   **Zengin ve Otomatik Çıktılar:** Platform, sadece sayısal sonuçlar üretmekle kalmaz, her deneyin sonunda performans metriklerini ve görselleştirmeleri içeren **otomatik Markdown raporları** oluşturur.

## 🏛️ Mimari Genel Bakış

AzuraForge platformu, aşağıdaki bağımsız GitHub depolarından oluşan bir mikroservis mimarisini benimser:

-   **`core`**: Temel otomatik türev motoru.
-   **`learner`**: `core` üzerinde geliştirilmiş yüksek seviyeli öğrenme kütüphanesi. `LSTM` gibi gelişmiş katmanları, `Adam` gibi optimizer'ları, `Callback` sistemini ve **otomatik raporlama araçlarını** içerir.
-   **`applications`**: Platform için resmi uygulama eklentilerinin katalogu.
-   **`app-stock-predictor`**: Gerçek bir uygulama eklentisi örneği.
-   **`api`**: RESTful API ve WebSocket (Pub/Sub tabanlı) sunan iletişim katmanı.
-   **`worker`**: Arka plan görevlerini işleyen ve raporları oluşturan işçi servisi.
-   **`dashboard`**: Platform için web tabanlı, canlı takip yeteneklerine sahip kullanıcı arayüzü.

Bu repo, tüm bu servisleri tek bir `docker-compose` komutuyla ayağa kaldıran ana orkestrasyon katmanıdır.

## 🚀 Hızlı Başlangıç (Docker Compose ile)

1.  **Docker Desktop'ın yüklü ve çalıştığından emin olun.**
2.  **Bu repoyu klonlayın:** `git clone https://github.com/AzuraForge/platform.git`
3.  **.env dosyasını oluşturun:** Proje kök dizininde `.env` oluşturup içine `REPORTS_DIR=./reports` ve `REDIS_URL=redis://redis:6379/0` yazın.
4.  **Rapor Dizinini Oluşturun:** `mkdir -p ./reports`
5.  **Platformu başlatın:** `docker-compose up --build -d`
6.  **Platforma erişin:**
    -   **Dashboard:** `http://localhost:5173`
    -   **API Dokümantasyonu:** `http://localhost:8000/api/v1/docs`
7.  **Deneyi Çalıştırın ve Raporu Görüntüleyin:** Dashboard'dan bir deney başlattıktan sonra, host makinenizdeki `./reports` klasörünü kontrol ederek oluşturulan `report.md` ve `images/` klasörünü inceleyin.

## 🛠️ Geliştirme Rehberi ve İç Detaylar

Platformun nasıl çalıştığını, nasıl katkıda bulunacağınızı ve geliştirme ortamınızı nasıl yöneteceğinizi öğrenmek için **[Geliştirme Rehberi](./docs/DEVELOPMENT_GUIDE.md)**'ne göz atın.

## 🗺️ Yol Haritası ve Gelecek Vizyonu

Projenin tamamlanan aşamaları, mevcut durumu ve gelecek hedefleri hakkında bilgi almak için **[Proje Yolculuğu](./docs/PROJECT_JOURNEY.md)** belgesini okuyun.
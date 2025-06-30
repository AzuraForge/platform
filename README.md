# AzuraForge Platform 🚀

**AzuraForge**, yapay zeka modellerini sıfırdan oluşturmak, eğitmek, canlı olarak takip etmek ve sonuçlarını interaktif raporlarla analiz etmek için tasarlanmış, **olay güdümlü, eklenti tabanlı ve dağıtık bir MLOps platformudur.**

Bu depo, AzuraForge ekosistemindeki tüm ana servisleri ve kütüphaneleri bir araya getiren **orkestrasyon katmanıdır**.

## 🏛️ Platform Mimarisi ve Felsefesi

AzuraForge, basit bir araç setinden daha fazlasıdır; modern yapay zeka sistemlerinin nasıl inşa edilmesi gerektiğine dair bir felsefeyi temsil eder. Bu felsefeyi, yol haritamızı ve projenin gelişim hikayesini derinlemesine anlamak için **[Proje Vizyonu ve Yol Haritası](./docs/VISION_AND_ROADMAP.md)** belgemizi inceleyin.

Platformumuz dört temel prensip üzerine kuruludur: **"The AzuraForge Way"**

1.  **Sıfırdan İnşa ve Derin Anlayış:**
    Temel algoritmaları (`Tensor`, `LSTM` vb.) sıfırdan yazarak sistem üzerinde tam kontrol, şeffaflık ve derinlemesine bir "know-how" sağlıyoruz. Dış dünyaya minimal bağımlılıkla, "kara kutu"lardan arınmış bir yapı hedefliyoruz.

2.  **Modüler ve Ölçeklenebilir Ekosistem:**
    Her bileşen (`api`, `worker`, `learner` vb.) kendi bağımsız reposunda yaşar, bağımsız olarak geliştirilebilir ve kurulabilir. Bu mikroservis yaklaşımı, projenin bakımını ve ölçeklenmesini kolaylaştırır.

3.  **Olay Güdümlü ve Asenkron Akış:**
    `Celery` ve `Redis Pub/Sub` üzerine kurulu mimari sayesinde, yoğun model eğitimleri bile sistemi bloklamaz. Bu, platformun en kritik gücüdür ve kullanıcıya akıcı, gerçek zamanlı bir deneyim sunar. Bu mimarinin detayları için **[Mimari Belgesi](./docs/ARCHITECTURE.md)**'ne göz atın.

4.  **Genişletilebilir Eklenti Sistemi:**
    Yeni AI uygulamaları, platformun çekirdek koduna dokunmadan, Python'un `entry_points` mekanizması kullanılarak sisteme "eklenti" olarak dahil edilebilir. Bu, platformun yeteneklerinin organik olarak büyümesini sağlar.

---

## ✨ Ana Yetenekler

*   **Sıfırdan İnşa Edilmiş Çekirdek:** Otomatik türev, `LSTM` gibi gelişmiş katmanlar ve `Adam` optimizer içeren, saf Python/NumPy tabanlı bir derin öğrenme motoru.
*   **Canlı Deney Takibi:** `WebSocket` aracılığıyla, devam eden bir eğitimin ilerleme çubuğunu, anlık kayıp değerini ve tahmin grafiklerinin canlı evrimini anlık olarak izleme imkanı.
*   **Dinamik ve İnteraktif Raporlama:** Tamamlanan her deney için, `Dashboard` üzerinden erişilebilen, `Chart.js` ile çizilmiş interaktif grafikler ve detaylı metrikler içeren rapor sayfaları.
*   **Deney Karşılaştırma:** Birden fazla deney sonucunu tek bir arayüzde görsel olarak karşılaştırarak en iyi modeli kolayca belirleme.

---

## 🗺️ Ekosisteme Genel Bakış

AzuraForge platformu, aşağıdaki bağımsız GitHub depolarından oluşur:

| Repo                         | Sorumluluk                                                                       | Teknoloji      |
| ---------------------------- | -------------------------------------------------------------------------------- | -------------- |
| **Çekirdek Kütüphaneler**    |                                                                                  |                |
| `core`                       | Temel tensör matematiği ve otomatik türev (geri yayılım) motoru.                   | `Python`, `NumPy` |
| `learner`                    | Yüksek seviyeli öğrenme kütüphanesi (Katmanlar, Optimizatörler, Pipeline'lar).     | `Python`       |
| **Uygulama Eklentileri**     |                                                                                  |                |
| `applications`               | Resmi ve test edilmiş uygulama eklentilerinin katalogunu tutar.                    | `JSON`         |
| `app-stock-predictor`        | Gerçek bir zaman serisi tahmin eklentisi örneği.                                 | `Python`       |
| **Platform Servisleri**      |                                                                                  |                |
| `api`                        | RESTful API ve WebSocket (Pub/Sub) sunan merkezi iletişim katmanı.                 | `FastAPI`      |
| `worker`                     | Arka plan görevlerini (model eğitimi) işleyen ve raporları oluşturan işçi servisi. | `Celery`, `Redis` |
| `dashboard`                  | React tabanlı, canlı takip ve raporlama yeteneklerine sahip web arayüzü.           | `React`, `Vite` |
| **Orkestrasyon (Bu Repo)**   |                                                                                  |                |
| `platform`                   | Tüm servisleri `docker-compose` ile bir araya getirir ve ana dokümantasyonu barındırır. | `Docker`, `YAML` |

---

## 🚀 Hızlı Başlangıç (Docker Compose ile)

1.  **Docker Desktop'ın yüklü ve çalıştığından emin olun.**
2.  **Bu repoyu klonlayın:** `git clone https://github.com/AzuraForge/platform.git && cd platform`
3.  **.env dosyasını oluşturun:** Proje kök dizininde `.env.example` dosyasını kopyalayarak `.env` adıyla yeni bir dosya oluşturun. (Varsayılan değerler genellikle yeterlidir).
    ```bash
    cp .env.example .env
    ```
4.  **Gerekli Dizinleri Oluşturun:**
    ```bash
    mkdir -p ./reports ./.cache
    ```
5.  **Platformu başlatın:** (İlk başlatma, imajlar build edileceği için biraz zaman alabilir.)
    ```bash
    docker-compose up --build -d
    ```
6.  **Platforma erişin:**
    *   **Dashboard:** `http://localhost:5173`
    *   **API Dokümantasyonu:** `http://localhost:8000/api/v1/docs`
7.  **Keşfedin:** Dashboard'dan bir deney başlatın, canlı takip panelini izleyin ve deney bittiğinde "Raporu Görüntüle" butonuyla interaktif sonuçları inceleyin.

---

## 🛠️ Geliştirme ve Katkıda Bulunma

Platformda geliştirme yapmak, yeni bir eklenti oluşturmak veya projeye katkıda bulunmak için aşağıdaki rehberlerimize göz atın. Tüm geliştirme süreçlerimiz, "The AzuraForge Way" prensiplerine dayanmaktadır.

*   **[Geliştirme Rehberi](./docs/DEVELOPMENT_GUIDE.md):** Yerel geliştirme ortamınızı nasıl kuracağınızı ve servisleri nasıl çalıştıracağınızı öğrenin.
*   **[Katkıda Bulunma Rehberi](./docs/CONTRIBUTING.md):** Kodlama standartlarımız, commit mesaj formatımız ve Pull Request sürecimiz hakkında bilgi edinin.


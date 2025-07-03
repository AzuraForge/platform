# 🛠️ AzuraForge Platform Geliştirme Rehberi

Bu belge, AzuraForge platformunda geliştirme yapmak isteyenler için adım adım kurulum, çalışma prensipleri ve katkıda bulunma yönergelerini içerir.

## 🎯 Temel Felsefemiz

AzuraForge'da geliştirme yaparken, iki temel prensibi aklımızda tutarız:

1.  **Bağımsız Paketler:** Her repo (`core`, `learner`, `api` vb.), kendi başına yaşayan, kurulabilir ve test edilebilir bağımsız bir Python/JavaScript paketidir.
2.  **Düzenlenebilir Kurulum:** Repolar arası bağımlılıklar, yerel geliştirmeyi hızlandırmak için `pip install -e` (editable) komutuyla kurulur. Bu sayede bir kütüphanede yaptığınız değişiklik, diğerlerine anında yansır.

## 📦 Proje Repolarına Genel Bakış

AzuraForge platformu, aşağıdaki bağımsız GitHub depolarından oluşur. Geliştirme yaparken bu repoların bir kısmını veya tamamını yerel makinenizde klonlamanız gerekecektir.

*   `core`: Temel otomatik türev motoru.
*   `learner`: `core` üzerinde yüksek seviyeli öğrenme kütüphanesi.
*   `api`: RESTful API ve WebSocket sunucusu.
*   `worker`: Arka plan görevlerini işleyen Celery worker.
*   `dashboard`: React tabanlı web kullanıcı arayüzü.
*   `platform`: Tüm servisleri bir araya getiren ana orkestrasyon deposu (bu repo).
*   Uygulama Eklentileri: `app-stock-predictor`, `app-weather-forecaster` vb.

## ⚙️ Geliştirme Ortamı Kurulumu

1.  **Gerekli Araçlar:** Git, Python 3.10+, Node.js & npm, Docker Desktop.

2.  **Repoları Klonlama:**
    İlgili tüm repoları aynı seviyede bir klasöre klonlayın (`azuraforge-dev` gibi).

3.  **.env Dosyasını Oluşturma:**
    Ana `platform` dizininde, `.env.example` dosyasını kopyalayarak bir `.env` dosyası oluşturun.

4.  **Sanal Ortam ve Bağımlılıklar (Python):**
    En pratik yol, **`platform` projesinin** kök dizininde tek bir sanal ortam oluşturup tüm Python bağımlılıklarını oraya "düzenlenebilir" modda kurmaktır.
    ```bash
    cd platform # Ana `platform` reposunun içine gir
    python -m venv .venv
    # Windows: .\.venv\Scripts\activate | Linux/macOS: source ./.venv/bin/activate
    
    # Tüm Python repolarını "düzenlenebilir" modda kur
    pip install -e ../core
    pip install -e ../learner
    pip install -e ../api
    pip install -e ../worker
    pip install -e ../app-stock-predictor 
    # ... diğer eklentiler
    ```

5.  **JavaScript Bağımlılıkları (Dashboard):**
    ```bash
    cd ../dashboard # `dashboard` reposunun içine gir
    npm install
    ```

6.  **Altyapı Servislerini Başlatma (Docker ile):**
    `platform` dizinindeyken, `docker-compose.yml` dosyasındaki sadece `postgres` ve `redis` servislerini başlatın.
    ```bash
    docker-compose up -d postgres redis
    ```

## ▶️ Servisleri Çalıştırma (Yerel Geliştirme)

Sanal ortamınız aktifken ve altyapı servisleri çalışırken, her uygulama servisini ayrı bir terminalde, kendi repo dizininden başlatın:

1.  **API Sunucusu (`api` reposundan):** `start-api`
2.  **Worker Servisi (`worker` reposundan):** `start-worker`
3.  **Dashboard (`dashboard` reposundan):** `npm run dev`

Bu kurulumla, herhangi bir Python veya JavaScript dosyasında yaptığınız değişiklik, ilgili servis tarafından otomatik olarak algılanacak ve yeniden yüklenecektir.

##  🔄 İteratif Geliştirme Akışı

*   **Kod Değişikliği:** Bir dosyayı değiştirip kaydedin. İlgili servis (`api`, `worker` veya `dashboard`) genellikle değişikliği otomatik olarak algılar.
*   **Yeni Bağımlılık Ekleme:** Bir reponun `pyproject.toml` dosyasına yeni bir paket eklediyseniz, ana `platform` dizinine dönüp o reponun `-e` kurulum komutunu tekrar çalıştırın (`pip install -e ../ilgili-repo`). Pip sadece eksik olan yeni bağımlılığı kuracaktır.

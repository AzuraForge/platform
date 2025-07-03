# 🛠️ AzuraForge Platform Geliştirme Rehberi

Bu belge, AzuraForge platformunda geliştirme yapmak isteyenler için adım adım kurulum, çalışma prensipleri ve en iyi pratikleri içerir.

## 🎯 Temel Felsefemiz: Bağımsızlık ve İşbirliği

AzuraForge'da geliştirme yaparken, iki temel prensibi aklımızda tutarız:

1.  **Bağımsız Paketler:** Her repo (`core`, `learner`, `api` vb.), kendi başına yaşayan, kurulabilir ve test edilebilir bağımsız bir projedir.
2.  **Kardeş Klasör Yapısı:** Tüm repolar, tek bir ana "çalışma alanı" (workspace) klasörü içinde, birbirinin kardeşi olarak durur. Bu, mantıksal ayrımı korurken, hem Docker ile hem de yerel ortamda geliştirmeyi kolaylaştırır.
3.  **Düzenlenebilir Kurulum:** Repolar arası bağımlılıklar, yerel geliştirmeyi hızlandırmak için `pip install -e` (editable) komutuyla kurulur. Bu sayede bir kütüphanede yaptığınız değişiklik, diğerlerine anında yansır.

---

## ⚙️ 1. Geliştirme Ortamı Kurulumu

Bu adımlar, platformun tüm parçalarını yerel geliştirme için hazır hale getirir.

### Adım 1.1: Gerekli Araçlar

*   Git
*   Python 3.10+
*   Node.js & npm
*   Docker & Docker Compose

### Adım 1.2: Repoları Klonlama (Kardeş Klasör Yapısı)

Tüm ilgili repoları, aynı seviyede bir ana klasör altına klonlayın. Bu yapı, projenin temelini oluşturur.

```bash
# Ana çalışma alanınızı oluşturun
mkdir azuraforge
cd azuraforge

# Tüm repoları kardeş olarak klonlayın
git clone https://github.com/AzuraForge/platform.git
git clone https://github.com/AzuraForge/core.git
git clone https://github.com/AzuraForge/learner.git
git clone https://github.com/AzuraForge/dbmodels.git
git clone https://github.com/AzuraForge/api.git
git clone https://github.com/AzuraForge/worker.git
git clone https://github.com/AzuraForge/dashboard.git
git clone https://github.com/AzuraForge/applications.git
git clone https://github.com/AzuraForge/app-stock-predictor.git
git clone https://github.com/AzuraForge/app-weather-forecaster.git
git clone https://github.com/AzuraForge/app-image-classifier.git
git clone https://github.com/AzuraForge/app-voice-generator.git
```

### Adım 1.3: Ortam Değişkenlerini Ayarlama

`platform` klasöründeki `.env.example` dosyasını kopyalayarak `.env` adıyla yeni bir dosya oluşturun. Bu dosya, tüm servisler tarafından kullanılacak ortak konfigürasyonları içerir.

```bash
cd platform
cp .env.example .env
cd ..
```

### Adım 1.4: Python Bağımlılıklarını Kurma (Tek Sanal Ortam)

En pratik yol, **`platform` projesinin** kök dizininde tek bir sanal ortam oluşturup tüm Python bağımlılıklarını oraya "düzenlenebilir" modda kurmaktır.

```bash
# Platform dizinine girin
cd platform

# Sanal ortamı oluşturun ve aktive edin
python -m venv .venv
# Windows: .\.venv\Scripts\activate
# Linux/macOS: source ./.venv/bin/activate

# Şimdi TÜM Python projelerini bu tek sanal ortama kurun
# '..' kullanarak bir üst dizindeki kardeş klasörlere ulaşıyoruz
pip install -e ../core
pip install -e ../learner
pip install -e ../dbmodels
pip install -e ../api
pip install -e ../worker
pip install -e ../applications
pip install -e ../app-stock-predictor
pip install -e ../app-weather-forecaster
pip install -e ../app-image-classifier
pip install -e ../app-voice-generato
```

### Adım 1.5: JavaScript Bağımlılıklarını Kurma (Dashboard)

```bash
# Dashboard dizinine girin
cd ../dashboard

# Bağımlılıkları kurun
npm install
```

---

## ▶️ 2. Platformu Çalıştırma

İki ana çalışma senaryonuz var:

### Senaryo A: Docker ile Tüm Platformu Çalıştırma (Önerilen Başlangıç)

Bu, tüm servislerin birbiriyle entegre bir şekilde nasıl çalıştığını görmek için en iyi yoldur.

1.  **Gerekli Dizinleri Oluşturun:**
    `platform` dizinindeyken, `docker-compose.yml`'de kullanılan paylaşımlı dizinleri oluşturun.
    ```bash
    # platform dizininde olduğunuzdan emin olun
    mkdir -p ./reports ./.cache
    ```
2.  **Platformu Başlatın:**
    `docker-compose` komutunu `platform` dizininden çalıştırın. Docker, kardeş klasörlerdeki diğer projeleri otomatik olarak bulacaktır.
    ```bash
    # platform dizininde olduğunuzdan emin olun
    docker-compose up --build -d
    ```
3.  **Platforma Erişin:**
    *   **Dashboard:** `http://localhost:5173`
    *   **API Dokümantasyonu:** `http://localhost:8000/api/v1/docs`

### Senaryo B: Hızlı Yerel Geliştirme (Docker olmadan)

Bir serviste (örneğin `api`) hızlıca değişiklik yapıp test etmek istediğinizde bu yöntemi kullanın. Bu çok daha hızlı bir geliştirme döngüsü sağlar.

1.  **Altyapıyı Başlatın:**
    Önce sadece veritabanı ve Redis'i Docker ile ayağa kaldırın.
    ```bash
    # platform dizininde olduğunuzdan emin olun
    docker-compose up -d postgres redis
    ```
2.  **Servisleri Ayrı Terminallerde Çalıştırın:**
    Her servis için ayrı bir terminal açın, **önceden oluşturduğunuz sanal ortamı aktive edin** ve ilgili komutu çalıştırın.

    *   **Terminal 1 (API):**
        ```bash
        cd ../api
        start-api
        ```
    *   **Terminal 2 (Worker):**
        ```bash
        cd ../worker
        start-worker
        ```
    *   **Terminal 3 (Dashboard):**
        ```bash
        cd ../dashboard
        npm run dev
        ```

Bu kurulumla, herhangi bir Python veya JavaScript dosyasında yaptığınız değişiklik, ilgili servis tarafından otomatik olarak algılanacak ve yeniden yüklenecektir.

---

## 🔄 İteratif Geliştirme Akışı

*   **Kod Değişikliği:** Bir dosyayı değiştirip kaydedin. **Senaryo B**'de çalışıyorsanız, ilgili servis (`api`, `worker` veya `dashboard`) genellikle değişikliği otomatik olarak algılar. **Senaryo A**'da çalışıyorsanız, `docker-compose up --build` komutunu tekrar çalıştırmanız gerekir.
*   **Yeni Bağımlılık Ekleme:** Bir reponun `pyproject.toml` dosyasına yeni bir paket eklediyseniz, ana `platform` dizinine dönüp o reponun `-e` kurulum komutunu tekrar çalıştırın (`pip install -e ../ilgili-repo`). Pip, sadece eksik olan yeni bağımlılığı kuracaktır.


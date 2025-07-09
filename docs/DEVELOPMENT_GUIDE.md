# 🛠️ AzuraForge Platform Geliştirme Rehberi

Bu belge, AzuraForge platformunda geliştirme yapmak isteyenler için adım adım kurulum, çalışma prensipleri ve en iyi pratikleri içerir.

## 🎯 Temel Felsefemiz: Bağımsızlık ve İşbirliği

AzuraForge'da geliştirme yaparken, iki temel prensibi aklımızda tutarız:

1.  **Bağımsız Paketler:** Her repo (`core`, `learner`, `api` vb.), kendi başına yaşayan, kurulabilir ve test edilebilir bağımsız bir projedir.
2.  **Kardeş Klasör Yapısı:** Tüm repolar, tek bir ana "çalışma alanı" (workspace) klasörü içinde, birbirinin kardeşi olarak durur. Bu, mantıksal ayrımı korurken, hem Docker ile hem de yerel ortamda geliştirmeyi kolaylaştırır.
3.  **Düzenlenebilir Kurulum:** Repolar arası bağımlılıklar, yerel geliştirmeyi hızlandırmak için `pip install -e` (editable) komutuyla kurulur. Bu sayede bir kütüphanede yaptığınız değişiklik, diğerlerine anında yansır.

---

## ⚙️ 1. Geliştirme Ortamı Kurulumu

### Adım 1.1: Gerekli Araçlar

*   Git
*   Python 3.10+
*   Node.js & npm
*   Docker & Docker Compose
*   **(Opsiyonel, GPU için)** NVIDIA GPU, güncel sürücüler ve [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit).

### Adım 1.2: Repoları Klonlama (Kardeş Klasör Yapısı)

Tüm ilgili repoları, aynı seviyede bir ana klasör altına klonlayın:
```bash
mkdir azuraforge && cd azuraforge
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

### Adım 1.3: Ortam Değişkenlerini Ayarlama (Yeni Modüler Yöntem)

Her servisin kendi konfigürasyonunu yönetmesi için ilgili `.env.example` dosyalarını kopyalayın.

*   **Platform için (Docker Compose):**
    ```bash
    cd platform
    cp .env.example .env
    cd ..
    ```
*   **Dashboard için:**
    ```bash
    cd dashboard
    cp .env.example .env
    cd ..
    ```
*   **API ve Worker için (İsteğe bağlı, Docker'sız çalıştırma için):**
    ```bash
    cd api
    cp .env.example .env
    cd ../worker
    cp .env.example .env
    cd .. 
    ```

### Adım 1.4: Python Bağımlılıklarını Kurma (Tek Sanal Ortam)

En pratik yol, **`platform` projesinin** kök dizininde tek bir sanal ortam oluşturup tüm Python bağımlılıklarını oraya "düzenlenebilir" modda kurmaktır.
```bash
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
pip install -e ../app-voice-generator
```

### Adım 1.5: (Opsiyonel) GPU Desteği için CuPy Kurulumu

Eğer platformu yerel makinenizde GPU ile çalıştırmak ve test etmek istiyorsanız, `CuPy` kütüphanesini sanal ortamınıza kurmanız gerekmektedir.

```bash
# Sanal ortamınız aktifken:
pip install cupy
```
**Not:** `cupy` kurulumu, sisteminizdeki CUDA versiyonu ile uyumlu olmalıdır. `pip` genellikle doğru paketi bulmaya çalışır. Kurulumda sorun yaşarsanız, [CuPy'nin resmi kurulum rehberini](https://docs.cupy.dev/en/stable/install.html) inceleyerek CUDA versiyonunuza özel (`pip install cupy-cuda11x` gibi) kurulum yapabilirsiniz.

### Adım 1.6: JavaScript Bağımlılıklarını Kurma (Dashboard)
```bash
cd ../dashboard
npm install
```

---

## ▶️ 2. Platformu Çalıştırma

### Senaryo A: Docker ile Tüm Platformu Çalıştırma (Önerilen)

1.  **Gerekli Dizinleri Oluşturun:** `platform` dizinindeyken `mkdir -p ./reports ./.cache` komutunu çalıştırın.
2.  **Platformu Başlatın:** `platform` dizinindeyken `docker-compose up --build -d` komutunu çalıştırın.
3.  **Platforma Erişin:**
    *   **Dashboard:** `http://localhost:5173`
    *   **API Dokümantasyonu:** `http://localhost:8000/api/v1/docs`
    *   Giriş: `admin` / `DefaultPassword123!`

### Senaryo B: Hızlı Yerel Geliştirme (Docker olmadan)

Bu yöntem, her servisin kendi `.env` dosyasından konfigürasyonunu okuması sayesinde artık çok daha kolay.

1.  **Altyapıyı Başlatın:** `platform` dizinindeyken `docker-compose up -d postgres redis` komutunu çalıştırın.
2.  **Servisleri Ayrı Terminallerde Çalıştırın:**
    *   Her terminalde sanal ortamı (`source platform/.venv/bin/activate`) aktive edin.
    *   **API:** `api/` dizinine gidin ve `uvicorn azuraforge_api.main:app --reload` komutunu çalıştırın.
    *   **Worker:** `worker/` dizinine gidin ve `celery -A azuraforge_worker.celery_app worker --loglevel=info` komutunu çalıştırın.
    *   **Dashboard:** `dashboard/` dizinine gidin ve `npm run dev` komutunu çalıştırın.

### **Komutları Çalıştırma**

1.  Önce `pip install cupy` komutunu çalıştırarak `CuPy` kütüphanesini kur.
2.  Kurulum bittikten sonra, GPU ile çalıştırma komutunu tekrar dene:
    ```bash
    $env:AZURAFORGE_DEVICE="gpu"; python tools/run_isolated.py
    ```

Bu adımlardan sonra, `azuraforge-core` motoru `CuPy`'ı başarıyla import edecek ve terminalde şu mesajı göreceksin:
`✅ AzuraForge Core: CuPy (GPU) backend successfully loaded.`

Ardından eğitim süreci başlayacak ve her bir epoch'un saniyeler içinde tamamlandığını göreceksin. Bu, platformun donanım soyutlama katmanının başarıyla çalıştığını ve artık hem CPU hem de GPU üzerinde geliştirme yapabildiğimizi gösterir.

---

## 🔄 İteratif Geliştirme Akışı

*   **Kod Değişikliği:** Bir dosyayı değiştirip kaydedin. **Senaryo B**'de çalışıyorsanız, ilgili servis (`api`, `worker` veya `dashboard`) genellikle değişikliği otomatik olarak algılar. **Senaryo A**'da çalışıyorsanız, `docker-compose up --build -d ilgili-servis-adi` komutuyla sadece değişen servisi yeniden build edebilirsiniz.
*   **Yeni Bağımlılık Ekleme:** Bir reponun `pyproject.toml` veya `package.json` dosyasına yeni bir paket eklediyseniz, ilgili `pip install -e` veya `npm install` komutunu tekrar çalıştırın.

---

## 🐞 Sorun Giderme (Troubleshooting)

Platformu ayağa kaldırırken veya geliştirme yaparken bir sorunla mı karşılaştınız? Yalnız değilsiniz! Bu tür karmaşık sistemlerde hatalar geliştirme sürecinin doğal bir parçasıdır.

Karşılaşılan yaygın sorunlar ve kanıtlanmış çözümleri için lütfen öncelikle merkezi **[Sorun Giderme Rehberimize](./TROUBLESHOOTING_GUIDE.md)** göz atın.
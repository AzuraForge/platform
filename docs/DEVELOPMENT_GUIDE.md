# 🛠️ AzuraForge Platform Geliştirme Rehberi

Bu belge, AzuraForge platformunda geliştirme yapmak isteyenler için adım adım kurulum, çalışma prensipleri ve katkıda bulunma yönergelerini içerir.

## 🎯 Temel Felsefemiz

Her repomuz, kendi başına yaşayan, kurulabilir ve test edilebilir bağımsız bir Python/JavaScript paketidir. Repolar arası bağımlılıklar, Git adresleri (`@git+https://...`) üzerinden kurulur.

## 📦 Proje Repolarına Genel Bakış

AzuraForge platformu, aşağıdaki bağımsız GitHub depolarından oluşur. Geliştirme yaparken bu repoların bir kısmını veya tamamını yerel makinenizde klonlamanız gerekecektir.

*   **`core`** ([link](https://github.com/AzuraForge/core)): Temel otomatik türev motoru.
*   **`learner`** ([link](https://github.com/AzuraForge/learner)): `core` üzerinde yüksek seviyeli öğrenme kütüphanesi.
*   **`app-stock-predictor`** ([link](https://github.com/AzuraForge/app-stock-predictor)): Bir uygulama eklentisi örneği.
*   **`applications`** ([link](https://github.com/AzuraForge/applications)): Resmi uygulama katalogu (sadece JSON veri içerir).
*   **`api`** ([link](https://github.com/AzuraForge/api)): RESTful API ve WebSocket sunucusu.
*   **`worker`** ([link](https://github.com/AzuraForge/worker)): Arka plan görevlerini (eğitimleri) işleyen Celery worker.
*   **`dashboard`** ([link](https://github.com/AzuraForge/dashboard)): React tabanlı web kullanıcı arayüzü.

## ⚙️ Geliştirme Ortamı Kurulumu

Bu adımlar, platformun tüm parçalarını yerel geliştirme için hazır hale getirir.

1.  **Gerekli Araçlar:**
    *   **Git:** Repoları klonlamak için.
    *   **Python 3.8+:** Tüm Python bileşenleri için.
    *   **Node.js & npm:** Frontend bileşeni için.
    *   **Docker Desktop:** Redis ve Dockerize edilmiş ortamda test için (alternatifler belirtilecektir).

2.  **Repoları Klonlama:**
    Platformda geliştirmek için tüm ilgili repoları aynı seviyede bir klasöre klonlamanız önerilir:
    ```bash
    mkdir azuraforge-dev
    cd azuraforge-dev

    git clone https://github.com/AzuraForge/core.git
    git clone https://github.com/AzuraForge/learner.git
    git clone https://github.com/AzuraForge/app-stock-predictor.git
    git clone https://github.com/AzuraForge/applications.git
    git clone https://github.com/AzuraForge/api.git
    git clone https://github.com/AzuraForge/worker.git
    git clone https://github.com/AzuraForge/dashboard.git
    git clone https://github.com/AzuraForge/platform.git # Orkestrasyon için
    ```

3.  **Sanal Ortam Kurulumu (Python):**
    Her Python projesinin kendi sanal ortamı olabilir veya merkezi bir tane kullanabiliriz. Yerel geliştirme için, **`api` projesinin** kök dizininde tek bir sanal ortam oluşturmak ve tüm Python bağımlılıklarını oraya kurmak en pratik yoldur.

    ```bash
    cd api # `api` reposunun içine gir
    python -m venv .venv
    .\.venv\Scripts\activate # Windows için
    # source ./.venv/bin/activate # Linux/macOS için
    ```

4.  **Python Bağımlılıklarını Kurma (Tüm Python Repoları için):**
    Sanal ortam aktifken, tüm Python repolarını "düzenlenebilir" (editable) modda kurmalıyız. Bu, kodda yaptığınız değişikliklerin anında yansımasını sağlar. **`api` projesinin** kök dizininde olduğunuzdan emin olun.

    ```bash
    # Önce en alt seviyeden başlayarak kütüphaneleri kurun
    pip install -e ../core 
    pip install -e ../learner
    pip install -e ../app-stock-predictor # İlk uygulama eklentisi
    pip install -e ../applications       # Uygulama katalogu
    
    # Sonra API ve Worker'ı kurun
    pip install -e .                     # `api` projesini kurar
    pip install -e ../worker             # `worker` projesini kurar
    ```
    Bu komutlar, her bir reponun `pyproject.toml` dosyasını okuyacak ve tüm bağımlılık zincirini doğru bir şekilde çözecektir.

5.  **JavaScript Bağımlılıklarını Kurma (Dashboard için):**
    ```bash
    cd ../dashboard # `dashboard` reposunun içine gir
    npm install
    ```

6.  **Redis Kurulumu:**
    Platform, bir Redis sunucusuna ihtiyaç duyar. En kolay yol Docker kullanmaktır:
    ```bash
    docker run -d -p 6379:6379 --name azuraforge_redis redis
    ```

## ▶️ Servisleri Çalıştırma (Yerel Geliştirme)

Sanal ortamınız aktifken ve Redis çalışırken, her servisi ayrı bir terminalde başlatın.

1.  **API Sunucusu (`api` reposundan):**
    ```bash
    cd api
    .\.venv\Scripts\activate # Sanal ortam aktif değilse
    start-api
    ```
    (Tarayıcıda `http://localhost:8000/api/v1/docs` adresini kontrol edin.)

2.  **Worker Servisi (`worker` reposundan):**
    ```bash
    cd worker
    .\.venv\Scripts\activate
    start-worker
    ```
    (Worker terminalinde "Discovered pipeline..." loglarını kontrol edin.)

3.  **Dashboard (`dashboard` reposundan):**
    ```bash
    cd dashboard
    npm run dev
    ```
    (Tarayıcıda `http://localhost:5173` adresini açın.)

## 🧪 Test Etme ve Hata Ayıklama

*   **Uçtan Uca Akış:** Dashboard'dan yeni bir deney başlatarak tüm sistemin (`Dashboard -> API -> Worker -> Uygulama Eklentisi -> Kütüphane`) sorunsuz çalıştığını doğrulayın. Canlı takip ekranını ve kayıp grafiğini izleyin.
*   **API Testleri:** `http://localhost:8000/api/v1/docs` adresinden API endpoint'lerini test edin.
*   **Birim Testleri:** Her bir repoda (örn: `core`, `learner`, `app-stock-predictor`) kendi `pytest` testlerini çalıştırın.
    ```bash
    cd core # veya learner, app-stock-predictor
    .\.venv\Scripts\activate
    pytest
    ```
    (Bu testleri koşabilmek için ilgili repoda `pip install -e ".[dev]"` yapmış olmanız gerekir.)

## 🔄 İteratif Geliştirme Akışı

Çoğu zaman, kodda küçük değişiklikler yapıp bunları hızla test etmek istersiniz.

1.  **Kütüphanede Değişiklik (örn: `core/src/azuraforge_core/tensor.py`):**
    *   Değişikliği yapın ve kaydedin.
    *   Bu değişikliğin `learner` veya diğer kütüphanelerde anında etkili olması için **ekstra bir `pip install` komutuna GEREK YOKTUR**, çünkü `pip install -e` ile kuruldukları için doğrudan kaynak dosyayı kullanırlar.
    *   `core` projesine geri dönüp `pytest` ile kendi testlerini koşun.
    *   Değişikliği `commit`'leyin ve `push`'layın.

2.  **Uygulama/Servis Değişikliği (örn: `app-stock-predictor/src/azuraforge_stockapp/pipeline.py`):**
    *   Değişikliği yapın ve kaydedin.
    *   `api` veya `worker` servisleri otomatik olarak `reload` (yeniden yükleme) yapacaktır (eğer `uvicorn --reload` ile çalışıyorlarsa).
    *   `api` ve `worker`'ı yeniden başlatmak genellikle yeterlidir.
    *   Değişikliği `commit`'leyin ve `push`'layın.

3.  **Yeni Bir Bağımlılık Eklendiğinde (`pyproject.toml` değiştiğinde):**
    *   İlgili reponun kök dizinine gidin (örn: `api`).
    *   Sanal ortamınızı aktive edin.
    *   `pip install -e .` komutunu tekrar çalıştırın. `pip`, sadece eksik olan yeni bağımlılıkları ekleyecektir.

## 🤝 Katkıda Bulunma

Bu proje bir açık kaynak projesi olarak geliştirilmektedir. Katkıda bulunmak için lütfen `platform/docs/CONTRIBUTING.md` dosyasını inceleyin.

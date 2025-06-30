# 🛠️ AzuraForge Platform Geliştirme Rehberi

Bu belge, AzuraForge platformunda geliştirme yapmak isteyenler için adım adım kurulum, çalışma prensipleri ve katkıda bulunma yönergelerini içerir.

## 🎯 Temel Felsefemiz

Her repomuz, kendi başına yaşayan, kurulabilir ve test edilebilir bağımsız bir Python/JavaScript paketidir. Repolar arası bağımlılıklar, Git adresleri (`@git+https://...`) üzerinden kurulur.

## 📦 Proje Repolarına Genel Bakış

AzuraForge platformu, aşağıdaki bağımsız GitHub depolarından oluşur. Geliştirme yaparken bu repoların bir kısmını veya tamamını yerel makinenizde klonlamanız gerekecektir.

*   **`core`**: Temel otomatik türev motoru.
*   **`learner`**: `core` üzerinde yüksek seviyeli öğrenme kütüphanesi.
*   **`app-stock-predictor`**: Bir uygulama eklentisi örneği.
*   **`applications`**: Resmi uygulama katalogu.
*   **`api`**: RESTful API ve WebSocket sunucusu (Redis Pub/Sub dinleyicisi).
*   **`worker`**: Arka plan görevlerini işleyen Celery worker (Redis Pub/Sub yayıncısı).
*   **`dashboard`**: React tabanlı web kullanıcı arayüzü.
*   **`platform`**: Tüm servisleri bir araya getiren ana orkestrasyon deposu (bu repo).

## ⚙️ Geliştirme Ortamı Kurulumu

Bu adımlar, platformun tüm parçalarını yerel geliştirme için hazır hale getirir.

1.  **Gerekli Araçlar:** Git, Python 3.8+, Node.js & npm, Docker Desktop.

2.  **Repoları Klonlama:**
    Tüm ilgili repoları aynı seviyede bir klasöre klonlayın:
    ```bash
    mkdir azuraforge-dev
    cd azuraforge-dev

    git clone https://github.com/AzuraForge/platform.git
    git clone https://github.com/AzuraForge/core.git
    git clone https://github.com/AzuraForge/learner.git
    git clone https://github.com/AzuraForge/applications.git
    git clone https://github.com/AzuraForge/app-stock-predictor.git
    git clone https://github.com/AzuraForge/api.git
    git clone https://github.com/AzuraForge/worker.git
    git clone https://github.com/AzuraForge/dashboard.git
    ```

3.  **Sanal Ortam ve Bağımlılıklar (Python):**
    Yerel geliştirme için, **`platform` projesinin** kök dizininde tek bir sanal ortam oluşturup tüm Python bağımlılıklarını oraya kurmak en pratik yoldur.

    ```bash
    cd platform # Ana `platform` reposunun içine gir
    python -m venv .venv
    # Windows: .\.venv\Scripts\activate | Linux/macOS: source ./.venv/bin/activate
    ```
    # Tüm Python repolarını "düzenlenebilir" modda kur
    ```bash
    pip install -e ../core 
    pip install -e ../learner
    pip install -e ../applications
    pip install -e ../app-stock-predictor
    pip install -e ../api
    pip install -e ../worker
    ```

4.  **JavaScript Bağımlılıkları (Dashboard):**
    ```bash
    cd ../dashboard # `dashboard` reposunun içine gir
    npm install
    ```

5.  **Redis Kurulumu (Docker ile):**
    ```bash
    docker run -d -p 6379:6379 --name azuraforge_redis redis
    ```

## ▶️ Servisleri Çalıştırma (Yerel Geliştirme)

Sanal ortamınız aktifken ve Redis çalışırken, her servisi ayrı bir terminalde başlatın.

1.  **API Sunucusu (`api` reposundan):**
    ```bash
    cd ../api # veya bulunduğunuz yere göre ayarlayın
    # Gerekirse sanal ortamı aktive et
    start-api
    ```

2.  **Worker Servisi (`worker` reposundan):**
    ```bash
    cd ../worker
    # Gerekirse sanal ortamı aktive et
    start-worker
    ```

3.  **Dashboard (`dashboard` reposundan):**
    ```bash
    cd ../dashboard
    npm run dev
    ```

##  🔄 İteratif Geliştirme Akışı

Çoğu zaman, kodda küçük değişiklikler yapıp bunları hızla test etmek istersiniz.

1.  **Kütüphanede Değişiklik (örn: `core/src/azuraforge_core/tensor.py`):**
    *   Değişikliği yapın ve kaydedin.
    *   Bu değişikliğin diğer kütüphanelerde anında etkili olması için **ekstra bir `pip install` komutuna GEREK YOKTUR**, çünkü `-e` ile kuruldukları için doğrudan kaynak dosyayı kullanırlar.
    *   `core` projesine geri dönüp birim testlerini (`pytest`) koşarak değişikliği doğrulayın.
    *   Değişikliği `commit`'leyin ve `push`'layın.

2.  **Uygulama/Servis Değişikliği (örn: `app-stock-predictor/src/azuraforge_stockapp/pipeline.py`):**
    *   Değişikliği yapın ve kaydedin.
    *   `api` veya `worker` servisleri otomatik olarak `reload` (yeniden yükleme) yapacaktır (eğer `uvicorn --reload` ile çalışıyorlarsa). Değişikliğin etkisini görmek için genellikle ilgili servisi (API veya Worker) yeniden başlatmak yeterlidir.
    *   Değişikliği `commit`'leyin ve `push`'layın.

3.  **Yeni Bir Bağımlılık Eklendiğinde (`pyproject.toml` değiştiğinde):**
    *   Bir reponun (örn: `learner`) `pyproject.toml` dosyasına yeni bir bağımlılık (örn: `pandas`) eklediyseniz, bu değişikliğin diğer repolar tarafından tanınması için **bağımlılık zincirini yeniden kurmanız gerekir.**
    *   `platform` klasöründeki ana sanal ortamınızı aktive edin.
    *   `pip install -e ../learner` komutunu tekrar çalıştırın. `pip`, sadece eksik olan yeni bağımlılıkları (`pandas`) ekleyecektir.

##  Canlı Takip Mimarisi Nasıl Çalışır?

1.  `Dashboard`, `API`'ye bir `/experiments` POST isteği atar.
2.  `API`, görevi `Celery` kuyruğuna bırakır ve `Dashboard`'a bir `task_id` döner.
3.  `Dashboard`, bu `task_id` ile `API`'nin `/ws/task_status/{task_id}` WebSocket endpoint'ine bağlanır.
4.  `API`, bu bağlantı için bir Redis istemcisi oluşturur ve `task-progress:{task_id}` kanalına **abone (subscribe)** olur.
5.  `Worker`, görevi kuyruktan alır ve `Learner`'ı, içine `RedisProgressCallback` enjekte edilmiş şekilde çalıştırır.
6.  `Learner`, her epoch sonunda `on_epoch_end` olayını yayınlar.
7.  `RedisProgressCallback`, bu olayı yakalar ve ilerleme verisini (epoch, loss) Redis'teki `task-progress:{task_id}` kanalına **yayınlar (publish)**.
8.  `API`, abone olduğu kanalda yeni bir mesaj duyar, onu alır ve WebSocket üzerinden anında `Dashboard`'a iletir.
9.  `Dashboard`'daki `LiveTrackerPane` bileşeni, gelen bu veriyle kendini günceller.

Bu yapı, `Worker`'ın CPU kullanımı ne kadar yoğun olursa olsun, raporlama ve arayüz güncellemesinin bloklanmadan, anlık olarak gerçekleşmesini sağlar.

## 🤝 Katkıda Bulunma

Bu proje bir açık kaynak projesi olarak geliştirilmektedir. Katkıda bulunmak için lütfen `platform/docs/CONTRIBUTING.md` dosyasını inceleyin.
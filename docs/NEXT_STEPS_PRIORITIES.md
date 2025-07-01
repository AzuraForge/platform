# 🚀 AzuraForge: Sonraki Adımlar ve İterasyon Odak Noktaları

**Bu belge, AzuraForge projesinin `PERPETUAL_REVIEW_GUIDE.md` rehberinde tanımlanan tüm uzmanlık perspektiflerinin sentezlenmiş halidir. Amacı, projenin mevcut durumunu değerlendirerek önümüzdeki dönem (örn: bir sonraki 1-2 iterasyon) için en kritik ve etkili odak noktalarını belirlemek ve "The AzuraForge Way" felsefesini operasyonel adımlara dönüştürmektir.**

---

### **Genel Değerlendirme ve Mevcut Durum:**

AzuraForge platformu, "sıfırdan inşa edilmiş" AI motoru (`azuraforge-core`, `learner`), olay güdümlü mimarisi (Redis Pub/Sub) ve Docker tabanlı dağıtık yapısıyla sağlam bir MVP (Minimum Viable Product) aşamasındadır. Canlı takip ve dinamik raporlama gibi ana yetenekler başarıyla gösterilmiştir. Dokümantasyonun olgunluğu, projenin iyi yönetildiğini göstermektedir.

Ancak, derinlemesine incelemelerimiz sonucunda, platformun daha da olgunlaşması, ölçeklenmesi, güvenliği ve geliştirici deneyimi için atılması gereken kritik adımlar bulunmaktadır. Mevcut yol haritasındaki **Faz 0: Büyük Birleşme (Konsolidasyon)** ve **Faz 1: Temeli Sağlamlaştırma (Foundation Hardening)** hedeflerimiz doğrultusunda önceliklendirme yapacağız.

---

### **Öncelikli Odak Alanları (Acil / Önümüzdeki 1-2 İterasyon)**

Bu alanlar, projenin mevcut mimari ve operasyonel risklerini azaltmaya, temel kararlılığı artırmaya ve kritik eksiklikleri gidermeye yöneliktir.

#### **1. Mimari Ayrıştırma ve Bağımlılık Yönetimi (Backend & Proje Yönetimi)**

*   **Sorun:** API servisinin, Worker'ın iç detaylarına (`azuraforge_worker.tasks.training_tasks.AVAILABLE_PIPELINES_AND_CONFIGS`) doğrudan bağımlılığı. Ayrıca, repolar arası bağımlılık güncellemelerinin (pyproject.toml) manuel olması.
*   **Çözüm:**
    *   **Pipeline Keşfi Dekuplajı:** Worker, başlatıldığında mevcut pipeline'ları ve varsayılan konfigürasyonlarını Redis'e (veya hafif bir servis kayıt sistemine) yayınlamalıdır. API, bu bilgileri Redis'ten çekerek Worker'ın iç yapısını bilme ihtiyacını ortadan kaldırmalıdır. Bu, her iki servisin bağımsız olarak dağıtılmasına ve güncellenmesine olanak tanır.
    *   **Otomatik Bağımlılık Senkronizasyonu:** Bir GitOps botu (örn. Dependabot) veya özel bir GitHub Action kullanarak, bir kütüphane (örn. `learner`) yeni bir versiyonla yayınlandığında, bu kütüphaneyi kullanan diğer depolara (`api`, `app-stock-predictor`) otomatik olarak bağımlılık güncelleme Pull Request'leri açılmasını sağlamak.
*   **Hedef:** Servisler arası bağımsızlığı artırmak ve manuel bağımlılık güncelleme süreçlerinden kaynaklanan hata riskini ortadan kaldırmak.

#### **2. Gelişmiş Gözlemlenebilirlik (Run Yönetimi & Backend)**

*   **Sorun:** Logların merkezi olmaması ve üretim ortamında izleme eksikliği. Hassas bilgilerin (`.env`) güvenli olmaması.
*   **Çözüm:**
    *   **Merkezi Log Yönetimi:** Tüm servis loglarını (API, Worker, Redis, Postgres) toplayacak ve görselleştirecek hafif bir merkezi loglama çözümü entegre etmek (örn: Loki + Promtail + Grafana veya ELK Stack'in daha hafif bir varyantı).
    *   **Temel Metrik İzleme:** Celery kuyruk uzunluğu, Worker yoğunluğu, API istek latansı, bellek/CPU kullanımı gibi temel operasyonel metrikleri toplamak ve basit bir Grafana paneliyle izlemek.
    *   **Sır Yönetimi:** `POSTGRES_USER`, `POSTGRES_PASSWORD` gibi hassas bilgileri `.env` dosyasından çıkararak Docker Secrets ile yönetmeye başlamak.
*   **Hedef:** Sistem sağlığını ve performansını üretimde proaktif olarak izleyebilmek, sorunları hızlıca tespit etmek ve güvenliği artırmak.

#### **3. Kullanıcı Geri Bildirimi ve Hata Deneyimi (Ürün Tasarımcısı & Frontend)**

*   **Sorun:** Hata mesajlarının bazen teknik ve kullanıcı için anlaşılmaz olması.
*   **Çözüm:**
    *   **Kullanıcı Odaklı Hata Mesajları:** Backend'den gelen teknik hata mesajlarını, frontend'de son kullanıcıya daha anlaşılır, aksiyon odaklı ve bilgilendirici mesajlara dönüştüren bir mekanizma geliştirmek.
    *   **Gelişmiş Form Doğrulama Geri Bildirimi:** `NewExperiment.jsx` formunda, özellikle dinamik ve liste tabanlı girişlerde, hatalı alanları daha belirgin hale getiren ve kullanıcının hatayı düzeltmesine yardımcı olan daha spesifik görsel geri bildirimler sunmak.
*   **Hedef:** Kullanıcı deneyimini iyileştirmek ve hata durumlarında kullanıcıya daha iyi rehberlik etmek.

#### **4. Core AI Katmanı Sağlamlaştırma (AI Mimarı & Backend)**

*   **Sorun:** `core/tensor.py` içindeki GPU fallback loglamasının daha belirgin olması ve `LSTM` implementasyonunun daha kapsamlı senaryoları ele alması gerekliliği.
*   **Çözüm:**
    *   **GPU Hata Yönetimi:** CuPy yüklenemediğinde veya GPU bulunamadığında (`AZURAFORGE_DEVICE=gpu` olmasına rağmen), `azuraforge-core`'un daha net ve yönlendirici hata/uyarı logları üretmesini sağlamak. Docker Compose `deploy` konfigürasyonunun doğru çalıştığını doğrulamak.
    *   **LSTM İyileştirmeleri:** LSTM katmanının farklı boyut ve sekans senaryolarında (örn: çok değişkenli zaman serisi) daha robust çalışmasını sağlamak için birim testlerini artırmak ve potansiyel kenar durumları ele almak.
*   **Hedef:** AI motorunun güvenilirliğini ve performansını artırmak, donanım bağımlılıklarını daha iyi yönetmek.

---

### **Orta Vadeli Hedefler (Sonraki 3-6 Ay / Faz 2 Hedefleri)**

Bu hedefler, platformun ana MLOps yeteneklerini genişletmeye ve kullanıcıya daha fazla değer sunmaya yöneliktir.

*   **Model Kayıt Defteri ve Model Sunumu:** Eğitilen en iyi modellerin kalıcı olarak saklanabileceği bir "Model Kayıt Defteri" (`Model Registry`) sistemi geliştirmek. Bu modeller üzerinden tahmin yapmak için `API`'ye yeni endpoint'ler (`/models/{model_id}/predict`) eklemek. Dashboard'a da bir "Model Kütüphanesi" sayfası eklemek. (Ürün, Backend, AI Mimarı)
*   **Hiperparametre Optimizasyonu (AutoML-light):** Dashboard üzerinden parametre aralıkları tanımlayarak hiperparametre optimizasyon görevleri başlatma arayüzü geliştirmek. Worker'ın bu görevleri paralel çalıştırmasını sağlamak ve sonuçları Dashboard'da interaktif grafiklerle görselleştirmek. (Ürün, Backend, AI Mimarı, Frontend)
*   **Frontend Modülerleşmesi ve State Yönetimi:** `App.css` gibi tek büyük CSS dosyasından uzaklaşıp, daha modüler bir CSS stratejisine (CSS Modules, Tailwind CSS) geçiş yapmak. Daha kompleks hale gelen frontend state'i yönetmek için Redux veya Zustand gibi bir kütüphane entegrasyonunu araştırmak ve başlamak. (Frontend, Ürün Tasarımcısı)
*   **Görüntü İşleme Temelleri:** `azuraforge-core`'a `Conv2D`, `MaxPool2D`, `Flatten` gibi temel görüntü işleme katmanlarını sıfırdan eklemek. `azuraforge-learner`'a da `BaseImagePipeline` soyut sınıfını ve temel sınıflandırma raporlamasını entegre etmek. (AI Mimarı, Backend)

---

### **Uzun Vadeli Vizyon (6+ Ay / Faz 3 Hedefleri ve İnovasyon)**

Bu hedefler, AzuraForge'u sektörde lider, ileri görüşlü ve kapsamlı bir AI/ML platformu haline getirmeyi amaçlamaktadır.

*   **Görsel Model Genetik Laboratuvarı:** Kullanıcıların `azuraforge-core`'daki AI katmanlarını sürükle-bırak arayüzü ile birleştirerek kendi nöral ağ mimarilerini görsel olarak tasarlayabildiği ve otomatik kod üretebildiği bir modül geliştirmek. Bu, "Ferrari Motoru" felsefemizi görselleştirecektir.
*   **Açıklanabilir Yapay Zeka (XAI) Entegrasyonu:** Raporlara, modelin bir tahmini "neden" yaptığını açıklayan (örn: SHAP, LIME entegrasyonu ile) bir bölüm eklemek.
*   **Sürekli Entegrasyon/Sürekli Dağıtım (CI/CD) Olgunluğu:** Kubernetes gibi konteyner orkestrasyon araçları üzerine dağıtımı otomatikleştirmek, Kanarya dağıtımları ve A/B testleri için altyapı hazırlamak.
*   **AzuraForge Marketplace/Hub:** Topluluk tarafından geliştirilen eklentilerin, veri dönüşümlerinin ve callback'lerin paylaşılabileceği ve kolayca sisteme entegre edilebileceği merkezi bir platform oluşturmak.
*   **Çoklu-Modalite Desteği:** Platformun hem metin hem de görüntü gibi farklı türde girdileri aynı anda alarak tahmin yapabildiği mimarileri desteklemek.

---

### **Çalışma Prensipleri (Nasıl İlerleyeceğiz):**

*   **`PERPETUAL_REVIEW_GUIDE.md`'e Bağlılık:** Her önemli toplantı ve iterasyon başlangıcında bu rehberi aktif olarak kullanmaya devam edeceğiz.
*   **Teknik Borcun Azaltılması:** Belirlenen mimari ayrıştırma ve altyapı iyileştirmeleri, teknik borç olarak ele alınacak ve düzenli olarak önceliklendirilecektir.
*   **İteratif Gelişim:** Büyük hedefler, küçük, yönetilebilir ve değer üreten iterasyonlara bölünecektir.
*   **Test Kapsamının Artırılması:** Özellikle `core` ve `learner` katmanlarındaki birim ve entegrasyon testlerinin kapsamını artırmaya devam edeceğiz.
*   **Açık İletişim:** Ekip içi ve paydaşlarla şeffaf ve düzenli iletişim kurmaya devam edeceğiz.

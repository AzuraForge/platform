# 🚀 AzuraForge: Sonraki Adımlar ve İterasyon Odak Noktaları

**Bu belge, AzuraForge projesinin `PERPETUAL_REVIEW_GUIDE.md` rehberinde tanımlanan tüm uzmanlık perspektiflerinin sentezlenmiş halidir. Amacı, projenin mevcut durumunu değerlendirerek önümüzdeki dönem (örn: bir sonraki 1-2 iterasyon) için en kritik ve etkili odak noktalarını belirlemek ve "The AzuraForge Way" felsefesini operasyonel adımlara dönüştürmektir.**

Bu, projenin yaşayan ve nefes alan, **canlı yol haritasıdır**.

---

### **Genel Değerlendirme ve Mevcut Durum:**

AzuraForge platformu, "sıfırdan inşa edilmiş" AI motoru (`azuraforge-core`, `learner`), olay güdümlü mimarisi (Redis Pub/Sub) ve Docker tabanlı dağıtık yapısıyla sağlam bir MVP (Minimum Viable Product) aşamasındadır. Canlı takip ve dinamik raporlama gibi ana yetenekler başarıyla gösterilmiştir. Dokümantasyonun olgunluğu, projenin iyi yönetildiğini göstermektedir.

Ancak, derinlemesine incelemelerimiz sonucunda, platformun daha da olgunlaşması, ölçeklenmesi, güvenliği ve geliştirici deneyimi için atılması gereken kritik adımlar bulunmaktadır. Önceliğimiz, mimari borçları temizlemek ve temeli endüstriyel standartlara taşımaktır.

---

### **Öncelikli Odak Alanları: FAZ 0 - TEMELİ SAĞLAMLAŞTIRMA (Foundation Hardening)**

Bu alanlar, projenin mevcut mimari ve operasyonel risklerini azaltmaya, temel kararlılığı artırmaya ve kritik eksiklikleri gidermeye yöneliktir.

| Öncelik | Görev                                      | Durum     | Neden? (İş Değeri)                                                              | İlgili Repo(lar)           |
| :------ | :----------------------------------------- | :-------- | :------------------------------------------------------------------------------ | :------------------------- |
| **1**   | **API & Worker'ı Ayırma (Decoupling)**     | `[⬜]`    | Ölçeklenmenin ve bağımsız dağıtımın önündeki en büyük engeli kaldırır.           | `api`, `worker`            |
| **2**   | **Veritabanı Modelini Tekilleştirme**      | `[⬜]`    | Kod tekrarını önler, gelecekteki hataları engeller, merkezi yönetim sağlar.      | `api`, `worker`, `(yeni)`  |
| **3**   | **Merkezi Log Yönetimi (Loki+Promtail)**   | `[⬜]`    | Sorun gidermeyi 10 kat hızlandırır ve sistemin sağlığını izlenebilir kılar.         | `platform` (docker-compose)|
| **4**   | **Hassas Bilgilerin Güvenliği (Secrets)**  | `[⬜]`    | Üretim ortamı için temel güvenlik standardını sağlar.                             | `platform` (docker-compose)|
| **5**   | **Frontend CSS Modülerleşmesi**            | `[⬜]`    | `App.css`'i yönetilebilir hale getirir, stil çakışmalarını önler.               | `dashboard`                |
| **6**   | **Kapsamlı Test Kapsamı**                  | `[⬜]`    | `core` ve `learner` motorunun güvenilirliğini garanti altına alır.                  | `core`, `learner`         |
| **7**   | **Kullanıcı Odaklı Hata Yönetimi**         | `[⬜]`    | Kullanıcı deneyimini doğrudan ve olumlu yönde etkileyen, düşük eforlu/yüksek etkili bir iştir. | `api`, `dashboard`     |

---

### **Orta Vadeli Hedefler: FAZ 1 - DENEYİMİ DERİNLEŞTİRME (MLOps Capability Expansion)**

Temel sağlamlaştıktan sonra, platformun ana MLOps yeteneklerini genişletmeye ve kullanıcıya daha fazla değer sunmaya odaklanacağız.

*   **Model Kayıt Defteri ve Model Sunumu:** Eğitilen en iyi modellerin kalıcı olarak saklanabileceği bir "Model Kayıt Defteri" (`Model Registry`) sistemi geliştirmek. Bu modeller üzerinden tahmin yapmak için `API`'ye yeni endpoint'ler (`/models/{model_id}/predict`) eklemek. Dashboard'a da bir "Model Kütüphanesi" sayfası eklemek.
*   **Hiperparametre Optimizasyonu (AutoML-light):** Dashboard üzerinden parametre aralıkları tanımlayarak hiperparametre optimizasyon görevleri başlatma arayüzü geliştirmek. Worker'ın bu görevleri paralel çalıştırmasını sağlamak ve sonuçları Dashboard'da interaktif grafiklerle görselleştirmek.
*   **Frontend State Yönetimi:** Daha kompleks hale gelen frontend state'i yönetmek için Redux veya Zustand gibi bir kütüphane entegrasyonunu araştırmak ve başlamak.

---

### **Uzun Vadeli Hedefler: FAZ 2 - EVRENİ GENİŞLETME (Genesis)**

Bu hedefler, AzuraForge'u sektörde lider, ileri görüşlü ve kapsamlı bir AI/ML platformu haline getirmeyi amaçlamaktadır.

*   **Görsel Model Genetik Laboratuvarı:** Kullanıcıların `azuraforge-core`'daki AI katmanlarını sürükle-bırak arayüzü ile birleştirerek kendi nöral ağ mimarilerini görsel olarak tasarlayabildiği ve otomatik kod üretebildiği bir modül geliştirmek.
*   **Açıklanabilir Yapay Zeka (XAI) Entegrasyonu:** Raporlara, modelin bir tahmini "neden" yaptığını açıklayan (örn: SHAP, LIME entegrasyonu ile) bir bölüm eklemek.
*   **VocaForge'un Yeniden Doğuşu:** Platformun yeteneklerini ses üretimi için kullanarak projenin kökenine dönmek.

`[✔️] Tamamlandı` `[🟢 Aktif]` `[⏳ Devam Ediyor]` `[⬜ Planlanıyor]`

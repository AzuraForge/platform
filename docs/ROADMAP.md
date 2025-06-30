# 🗺️ AzuraForge Stratejik Yol Haritası

Bu belge, AzuraForge platformunun stratejik hedeflerini, tamamlanan kilometre taşlarını ve gelecekteki geliştirme fazlarını özetlemektedir. Bu yol haritası, projenin nereye gittiğini gösteren canlı bir dokümandır.

**Daha detaylı proje geçmişi ve evrimi için [Proje Yolculuğu](./PROJECT_JOURNEY.md) belgesini inceleyebilirsiniz.**

---

### **📍 MEVCUT DURUM: Faz 0 - "Checkpoint Echo" (Fonksiyonel MVP)**

Platform, temel MLOps yeteneklerine sahip, çalışan ve kararlı bir MVP (Minimum Viable Product) aşamasındadır.

*   **Tamamlananlar:**
    *   Sıfırdan inşa edilmiş `core` ve `learner` motorları.
    *   Olay güdümlü mimari ile asenkron görev işleme.
    *   `docker-compose` ile tam orkestrasyon.
    *   Canlı deney takibi (WebSocket & Redis Pub/Sub).
    *   Dinamik ve interaktif raporlama arayüzü.
    *   Genişletilebilir eklenti sistemi (`entry_points`).

---

### **➡️ FAZ 1: TEMELİ SAĞLAMLAŞTIRMA (Foundation Hardening)**

*   **Hedef:** Projeyi üretim kalitesine, endüstri standardı geliştirme pratiklerine ve yüksek güvenilirliğe taşımak.
*   **Durum:** `🟢 Aktif`
*   **Ana Başlıklar:**
    *   `[✔️]` Kapsamlı Dokümantasyon (`VISION.md`, `ROADMAP.md`, `ARCHITECTURE.md`).
    *   `[⏳]` Anlamsal Versiyonlama ve Git Etiketleme Stratejisi.
    *   `[⏳]` Test Kapsamının Artırılması (Unit & Integration Tests).
    *   `[⏳]` Sürekli Entegrasyon (CI) Pipeline'ları Kurulumu (GitHub Actions).
    *   `[⬜]` Deney Verilerinin Dosya Sisteminden Veritabanına (PostgreSQL) Taşınması.

---

### **➡️ FAZ 2: DENEYİMİ DERİNLEŞTİRME (MLOps Capability Expansion)**

*   **Hedef:** Platformu, temel bir araçtan daha profesyonel ve güçlü bir MLOps çözümüne dönüştürmek.
*   **Durum:** `⬜ Planlanıyor`
*   **Ana Başlıklar:**
    *   `[⬜]` **Model Kayıt Defteri (Model Registry):** Eğitilen en iyi modellerin kalıcı olarak saklanması ve yönetilmesi.
    *   `[⬜]` **Model Sunumu (Model Serving):** Kayıtlı modeller üzerinden tahmin yapmak için API endpoint'leri (`/models/{id}/predict`).
    *   `[⬜]` **Hiperparametre Optimizasyonu:** `Dashboard` üzerinden optimizasyon görevleri başlatma ve sonuçları görselleştirme.
    *   `[⬜]` **Kimlik Doğrulama ve Yetkilendirme:** Çok kullanıcılı ortamlar için güvenlik katmanı (JWT).
    *   `[⬜]` **GPU Desteği:** Uyumlu donanımlarda `CuPy` ile eğitimi hızlandırma.

---

### **➡️ FAZ 3: EVRENİ GENİŞLETME (New Data Modalities)**

*   **Hedef:** Platformun yeteneklerini zaman serilerinin ötesine taşıyarak farklı veri türleri ile çalışabildiğini kanıtlamak.
*   **Durum:** `⬜ Planlanıyor`
*   **Ana Başlıklar:**
    *   `[⬜]` **Görüntü İşleme Desteği:** `Conv2D`, `MaxPool2D` katmanları ve `ImageClassificationPipeline` eklentisi.
    *   `[⬜]` **Doğal Dil İşleme Temelleri:** `Embedding`, `Attention` katmanları ve metin sınıflandırma pipeline'ı.
    *   `[⬜]` **Açıklanabilir Yapay Zeka (XAI):** Raporlara, modelin tahminlerini "neden" yaptığını açıklayan SHAP/LIME gibi görseller eklemek.

---
`[✔️] Tamamlandı` `[🟢 Aktif]` `[⏳ Devam Ediyor]` `[⬜ Planlanıyor]`

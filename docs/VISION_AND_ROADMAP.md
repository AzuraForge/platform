# 🏛️ AzuraForge: Proje Vizyonu ve Stratejik Yol Haritası

**Bu belge, AzuraForge platformunun felsefesini, başlangıcından bugüne evrimini ve geleceğe yönelik stratejik hedeflerini tek bir çatı altında birleştiren ana manifestodur.**

Bu proje, basit bir MLOps aracından daha fazlasıdır; modern yapay zeka sistemlerinin nasıl inşa edilmesi gerektiğine dair bir felsefeyi temsil eder.

---

## 🚀 Proje Felsefesi: Ferrari Motoru ve Uzay Gemisi Şasisi

Vizyonumuzu basit bir metaforla özetliyoruz: **Ferrari motorunu (kanıtlanmış, sıfırdan inşa edilmiş AI gücü) alıp, modüler ve genişletilebilir bir uzay gemisi şasisine (dağıtık MLOps mimarisi) monte etmek.** Bu uzay gemisi, yeni eklentilerle (`app-xx`) sürekli geliştirilerek farklı görevleri yerine getirebilen, akıllı ve kendi kendini yönetebilen bir yapıya dönüşür.

Bu vizyonu gerçekleştirmek için dört temel anayasal prensibe bağlıyız: **"The AzuraForge Way"**

1.  **Sıfırdan İnşa ve Derin Anlayış:** Temel algoritmaları (`Tensor`, `LSTM`, `Conv2D`, `Attention` vb.) sıfırdan yazarak sistem üzerinde tam kontrol, şeffaflık ve derinlemesine bir "know-how" sağlıyoruz.
2.  **Modüler ve Ölçeklenebilir Ekosistem:** Her bileşen (`api`, `worker`, `dbmodels` vb.) kendi bağımsız reposunda yaşar ve bağımsız olarak geliştirilebilir.
3.  **Olay Güdümlü ve Asenkron Akış:** `Celery` ve `Redis Pub/Sub` üzerine kurulu mimari sayesinde, yoğun model eğitimleri bile sistemi bloklamaz.
4.  **Genişletilebilir Eklenti Sistemi:** Python `entry_points` mekanizması, yeni AI uygulamalarının platforma "eklenti" olarak dahil edilmesini sağlar.

---

## 🗺️ Proje Yolculuğu: Tamamlanan Fazlar

*   **Faz 0: Fikir ve Prototip ("Smart Learner"):** Monolitik bir yapıda, sıfırdan yazılmış `LSTM` ile zaman serisi tahmininde yüksek başarı elde edildi. Ölçeklenme ihtiyacı doğdu.
*   **Faz 1: Temeli Sağlamlaştırma ("Foundation Hardening"):**
    *   **Mimari Dönüşüm:** Proje, bağımsız "kardeş repo" yapısına sahip bir mikroservis mimarisine geçirildi.
    *   **Servislerin Ayrıştırılması (Decoupling):** `api` ve `worker` servisleri, Redis üzerinden haberleşerek tamamen bağımsız hale getirildi.
    *   **Merkezi Veritabanı Modelleri:** `dbmodels` reposu oluşturularak tüm servisler için tek bir veri şeması kaynağı sağlandı.
    *   **Frontend Modülerleşmesi:** `Dashboard`'daki global `App.css`, bileşen bazlı CSS Modüllerine ayrılarak daha yönetilebilir hale getirildi.
*   **Faz 2: Yetenek Setini Genişletme ("Capability Expansion"):**
    *   **AI Motorunun Geliştirilmesi:** `core` ve `learner` kütüphanelerine `Conv2D`, `MaxPool2D`, `Embedding`, `Attention`, `CrossEntropyLoss` gibi yeni temel yapı taşları eklendi.
    *   **Uçtan Uca Yetenekler:** `Model Kaydetme/Sunma`, `Hiperparametre Optimizasyonu`, `Kullanıcı Yönetimi (JWT)` ve `İnteraktif Raporlama` gibi özellikler platforma kazandırıldı.
*   **Faz 3: Stabilizasyon ve Optimizasyon (Stability & Optimization) [Tamamlandı]:**
    *   **Kaynak Yönetimi:** `worker` servisindeki bellek patlaması sorunu, paylaşımlı bellek (shared-memory) optimizasyonu ile çözüldü.
    *   **GPU Entegrasyonu:** GPU kullanımının garantilenmesi ve sessiz hata (silent failure) durumlarının önlenmesi sağlandı.
    *   **Gözlemlenebilirlik Altyapısı:** Loki, Promtail ve Grafana yığını tam entegre edilerek merkezi loglama sistemi tamamen işlevsel hale getirildi.
    *   **Uygulama Tutarlılığı:** Tüm eklentilerin (`app-*`) `worker` tarafından doğru şekilde kurulması ve dinamik formlarının `dashboard`'da gösterilmesi sağlandı.
    *   **Hata Ayıklama:** `Pydantic ValidationError` ve `KeyError` gibi kritik çalışma zamanı (runtime) hataları giderildi.

---

## 🧭 Gelecek Vizyonu ve Stratejik Fazlar

**Projenin anlık ve detaylı eylem planı için lütfen [Sonraki Adımlar ve Öncelikler](./NEXT_STEPS_PRIORITIES.md) belgemize bakın.**

### **AKTİF FAZ: FAZ 4 - ÜRÜNLEŞTİRME VE CİLALAMA (Productization & Polish)**
*   **Hedef:** Mevcut güçlü ve stabil altyapıyı, son kullanıcının kolayca kullanabileceği, "cilalı" ve zengin özelliklere sahip bir ürüne dönüştürmek.
*   **Ana Başlıklar:** Gelişmiş UI/UX (Daha iyi grafikler, daha basit tahmin arayüzü), Kapsamlı Kullanıcı Dokümantasyonu, CI/CD Otomasyonu.

### **FAZ 5: DERİNLEŞME VE İNOVASYON (Deep Tech & Innovation) [Gelecek]**
*   **Hedef:** AI motorunu, Transformer gibi en modern mimarileri destekleyecek şekilde derinleştirmek ve platforma benzersiz, yenilikçi özellikler katmak.
*   **Ana Başlıklar:** `Multi-Head Attention`, `LayerNorm`, Veri Artırma (Data Augmentation), Açıklanabilir Yapay Zeka (XAI).

### **FAZ 6: EKOSİSTEM VE TOPLULUK (Ecosystem & Community) [Uzun Vadeli Vizyon]**
*   **Hedef:** AzuraForge'u, topluluğun katkıda bulunabileceği, eklentilerini paylaşabileceği canlı bir ekosisteme dönüştürmek.
*   **Ana Başlıklar:** "AzuraForge Hub" (Eklenti Pazarı), Sertifikasyon Programları, Stratejik Ortaklıklar.
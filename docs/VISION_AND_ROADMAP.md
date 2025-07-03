# 🏛️ AzuraForge: Proje Vizyonu ve Yol Haritası

**Bu belge, AzuraForge platformunun felsefesini, başlangıcından bugüne evrimini ve geleceğe yönelik stratejik hedeflerini tek bir çatı altında birleştiren ana manifestodur.**

Bu proje, basit bir MLOps aracından daha fazlasıdır; modern yapay zeka sistemlerinin nasıl inşa edilmesi gerektiğine dair bir felsefeyi temsil eder.

---

## 🚀 Proje Felsefesi: Ferrari Motoru ve Uzay Gemisi Şasisi

Vizyonumuzu basit bir metaforla özetliyoruz: **Ferrari motorunu (kanıtlanmış, sıfırdan inşa edilmiş AI gücü) alıp, modüler ve genişletilebilir bir uzay gemisi şasisine (dağıtık MLOps mimarisi) monte etmek.** Bu uzay gemisi, yeni eklentilerle (`app-xx`) sürekli geliştirilerek farklı görevleri yerine getirebilen, akıllı ve kendi kendini yönetebilen bir yapıya dönüşür.

Bu vizyonu gerçekleştirmek için dört temel anayasal prensibe bağlıyız: **"The AzuraForge Way"**

1.  **Sıfırdan İnşa ve Derin Anlayış:** Temel algoritmaları (`Tensor`, `LSTM` vb.) sıfırdan yazarak sistem üzerinde tam kontrol, şeffaflık ve derinlemesine bir "know-how" sağlıyoruz. Dış dünyaya minimal bağımlılıkla, "kara kutu"lardan arınmış bir yapı hedefliyoruz.

2.  **Modüler ve Ölçeklenebilir Ekosistem:** Her bileşen (`api`, `worker`, `learner` vb.) kendi bağımsız reposunda yaşar, bağımsız olarak geliştirilebilir ve kurulabilir. Bu mikroservis yaklaşımı, projenin bakımını ve ölçeklenmesini kolaylaştırır.

3.  **Olay Güdümlü ve Asenkron Akış:** `Celery` ve `Redis Pub/Sub` üzerine kurulu mimari sayesinde, yoğun model eğitimleri bile sistemi bloklamaz. Bu, platformun en kritik gücüdür ve kullanıcıya akıcı, gerçek zamanlı bir deneyim sunar.

4.  **Genişletilebilir Eklenti Sistemi:** Yeni AI uygulamaları, platformun çekirdek koduna dokunmadan, Python'un `entry_points` mekanizması kullanılarak sisteme "eklenti" olarak dahil edilebilir.

---

## 🗺️ Proje Yolculuğu: Gelişim Hikayemiz

*   **Faz 0: Fikir ve Prototip ("Smart Learner" Projesi):** Monolitik bir yapıda, sıfırdan yazılmış `LSTM` mimarisi ile `R² > 0.98` gibi etkileyici başarılar elde edildi. Ancak bu yapının ölçeklenemeyeceği anlaşıldı.
*   **Faz 1-3: Mikroservis Mimarisine Geçiş:** Proje, bağımsız repolara (`core`, `learner`, `api`, `worker` vb.) sahip bir mikroservis mimarisine dönüştürüldü. Temel görev akışı çalışır hale getirildi.
*   **Faz 4-8: Gerçek Zamanlı Akış (Pub/Sub Mimarisi - Dönüm Noktası):** CPU-yoğun görevlerin arayüzü bloklaması sorunu, Redis Pub/Sub ile çözüldü. Bu, anlık ve akıcı bir canlı takip deneyimini mümkün kıldı.
*   **Faz 9-17: Mimari Olgunluk ("Checkpoint Echo"):** Standart `BasePipeline` soyut sınıfı, merkezi önbellekleme (caching), akıllı ön işleme ve interaktif raporlama gibi yeteneklerle platform kararlı bir MVP aşamasına ulaştı.

---

## 🧭 Gelecek Vizyonu ve Stratejik Yol Haritası

Bu sağlam temel üzerine inşa edilecek adımlar, AzuraForge'u daha da zenginleştirmeyi ve kapsamını genişletmeyi hedefleyecektir. **Projenin anlık ve detaylı eylem planı için lütfen [Sonraki Adımlar ve Öncelikler](./NEXT_STEPS_PRIORITIES.md) belgemize bakın.**

### **FAZ 0: TEMELİ SAĞLAMLAŞTIRMA (Foundation Hardening) [Aktif]**
*   **Hedef:** Projeyi üretim kalitesine, endüstri standardı geliştirme pratiklerine ve yüksek güvenilirliğe taşımak.
*   **Ana Başlıklar:** Servisleri tamamen bağımsız hale getirme, merkezi loglama, test kapsamını artırma ve güvenlik iyileştirmeleri.

### **FAZ 1: DENEYİMİ DERİNLEŞTİRME (MLOps Capability Expansion) [Planlanıyor]**
*   **Hedef:** Platformu, temel bir araçtan daha profesyonel ve güçlü bir MLOps çözümüne dönüştürmek.
*   **Ana Başlıklar:** Model Kayıt Defteri (Model Registry), Model Sunumu (Model Serving), Hiperparametre Optimizasyonu.

### **FAZ 2: EVRENİ GENİŞLETME (New Data Modalities & Genesis) [Gelecek Vizyonu]**
*   **Hedef:** Platformun yeteneklerini zaman serilerinin ötesine taşıyarak farklı veri türleri ile çalışabilmek ve projenin kökeni olan ses üretimi gibi hedeflere ulaşmak.
*   **Ana Başlıklar:** Görüntü İşleme Desteği, Doğal Dil İşleme Temelleri, Açıklanabilir Yapay Zeka (XAI), Vektör Veritabanları ve nihayetinde `app-tts-generator`.

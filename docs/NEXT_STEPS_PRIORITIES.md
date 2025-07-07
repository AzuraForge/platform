# 🚀 AzuraForge: Sonraki Adımlar ve Öncelikler (FAZ 4)

**Bu belge, projenin "FAZ 4 - ÜRÜNLEŞTİRME VE CİLALAMA" aşamasındaki öncelikli yol haritasıdır.** Amacımız, mevcut sağlam altyapıyı, son kullanıcı için değeri yüksek, sezgisel ve profesyonel özelliklerle zenginleştirmektir.

---

## 🎯 FAZ 4 Odak Alanları

| Öncelik | Tema | Görev Adı | Neden Önemli? (İş Değeri) | Etkilenecek Repolar | Durum |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | **Akıllı Arayüzler** | **Deney Karşılaştırma Arayüzünü İyileştirme** | Toplu deney sonuçlarını (batch experiments) anlamlandırmak için **paralel koordinat grafiği** eklemek. Bu, en iyi hiperparametreyi bulmayı saniyeler meselesi haline getirir ve platformun analiz gücünü sergiler. | `dashboard` | `[✅]` |
| **2** | **Daha Derin Analiz** | **Hata Analizi ve Tahmin Dağılımı Grafiği** | Regresyon modelleri için hataların dağılımını (histogram) ve gerçek vs. tahmin grafiğini (scatter plot) eklemek. Bu, modelin zayıf ve güçlü yönlerini ortaya çıkararak basit bir MLOps aracından öteye taşır. | `worker`, `dashboard` | `[✅]` |
| **3** | **Sağlam Altyapı** | **Alembic ile Veritabanı Geçişlerini Yönetme** | Veritabanı şema değişikliklerini (örn: yeni sütun ekleme) güvenli, otomatik ve versiyon kontrollü hale getirmek. Bu, projenin uzun vadeli sağlığı ve ekip çalışması için kritik bir yatırımdır. | `dbmodels`, `api`, `worker` | `[⬜]` |
| **4. (Düşük Öncelik)** | **Akıllı Arayüzler** | **Tahmin Görevi İçin İptal Butonu** | Özellikle gelecekte eklenecek karmaşık ve yavaş çalışabilecek modeller için, kullanıcının başlattığı bir tahmin görevini iptal edebilmesi (revoke) kullanıcı deneyimini iyileştirir. | `api`, `dashboard` | `[⬜]` |

---

### **Sonraki Adım: Görev #3'e Odaklanma**

Bir numaralı ve iki numaralı görev başarıyla tamamlandı. Artık sıradaki önceliğimiz **3 numaralı görev olan Alembic ile Veritabanı Geçişlerini Yönetme** özelliğini eklemektir.
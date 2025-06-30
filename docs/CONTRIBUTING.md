========== FILE: docs/CONTRIBUTING.md ==========
# 🤝 AzuraForge Platformuna Katkıda Bulunma Rehberi

AzuraForge projesine gösterdiğiniz ilgi ve katkılarınız için teşekkür ederiz! Bu proje, modern yapay zeka sistemlerinin nasıl inşa edilmesi gerektiğine dair bir vizyonu hayata geçirmeyi amaçlamaktadır. Bu rehber, kod tabanının tutarlı, okunabilir, sürdürülebilir ve yüksek kalitede kalmasını sağlamak için benimsediğimiz çalışma prensiplerini ve standartlarını açıklamaktadır.

## 🚀 Hızlı Başlangıç

Eğer henüz geliştirme ortamınızı kurmadıysanız, lütfen platformun ana [Geliştirme Rehberi](./DEVELOPMENT_GUIDE.md) belgesindeki "Geliştirme Ortamı Kurulumu" bölümünü takip edin.

## 🛠️ Kodlama Standartları

Projeye eklenen her kodun aşağıdaki standartları karşılaması beklenmektedir. Bu standartların birçoğu, ilgili reponun kök dizinindeki `pre-commit` hook'ları ile otomatik olarak kontrol edilir.

1.  **Stil Kılavuzu (PEP8 & Black):**
    *   Tüm Python kodları, PEP8 stil kurallarına uymalıdır.
    *   Kodunuzu `black` ile otomatik formatlayın.
    *   **Kontrol:** `black .` komutunu çalıştırın.
    *   **Otomasyon:** `pre-commit` hook'ları bu formatlamayı zorunlu kılar.

2.  **Linting (`flake8`):**
    *   Tüm Python kodları, `flake8` denetiminden hatasız geçmelidir.
    *   **Kontrol:** İlgili repo içinde `flake8 src` komutunu çalıştırın.
    *   **Otomasyon:** `pre-commit` hook'ları linting'i zorunlu kılar.

3.  **Tip İpuçları (Type Hinting & Mypy):**
    *   Tüm fonksiyon ve metod imzaları, parametreler ve dönüş değerleri için tip ipuçları (`typing` modülü kullanılarak) içermelidir.
    *   **Kontrol:** İlgili repo içinde `mypy src` komutunu çalıştırın.
    *   **Otomasyon:** `pre-commit` hook'ları statik tip denetimini zorunlu kılar.

4.  **Dokümantasyon (Docstrings):**
    *   Tüm public modüller, sınıflar ve fonksiyonlar, ne işe yaradıklarını, aldıkları argümanları (`Args:`) ve ne döndürdüklerini (`Returns:`) açıklayan Google-style docstring'ler içermelidir.

5.  **Testler (`pytest`):**
    *   Eklenen her yeni özellik veya fonksiyon için ilgili birim testleri (`unit tests`) `tests/` klasörüne eklenmelidir.
    *   Yapılan bir hata düzeltmesi (bug fix) için, o hatanın tekrar oluşmasını engelleyecek bir regresyon testi yazılmalıdır.
    *   **Kontrol:** İlgili reponun kök dizinindeyken `pytest` komutunu çalıştırın.

## 📝 Commit Mesajları

Commit mesajları, yapılan değişikliği net bir şekilde açıklamalıdır ve [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) standardına uymalıdır. Bu, otomatik versiyonlama ve değişiklik günlüğü oluşturmak için hayati önem taşır.

**Format:**
```
<tip>(<kapsam>): <açıklama>

[opsiyonel gövde]
```

**Örnek Tipler:**
*   `feat`: Yeni bir özellik ekler (Minor versiyon artırımı).
*   `fix`: Bir hata düzeltmesi (Patch versiyon artırımı).
*   `docs`: Sadece dokümantasyon değişiklikleri.
*   `style`: Kod formatı, eksik noktalı virgül gibi stil düzeltmeleri.
*   `refactor`: Kodu yeniden yapılandırma, davranış değişikliği yok.
*   `perf`: Performans iyileştirmesi yapan kod değişikliği.
*   `test`: Eksik testlerin eklenmesi veya mevcut testlerin düzeltilmesi.
*   `build`: Build sistemi veya dış bağımlılık değişiklikleri.
*   `ci`: CI/CD yapılandırma dosyaları ve script'leri değişiklikleri.

**Örnekler:**
*   `feat(learner): Add LSTM backward pass implementation`
*   `fix(api): Handle null values from experiment results`
*   `docs(platform): Update development guide with Pub/Sub architecture`
*   `refactor(worker): Extract Redis publishing logic into a callback`

## 🔄 Pull Request (PR) Süreci

1.  **Branch Oluşturma:** `main` branch'inden kendi feature branch'inizi (`feat/yeni-ozellik` veya `fix/hata-adi` gibi) oluşturun.
2.  **Değişikliklerinizi Yapın:** Yukarıdaki standartlara uyduğunuzdan emin olun.
3.  **Test Edin:** Yerel testlerinizi (`pytest`) çalıştırın ve geçtiğinden emin olun.
4.  **Commit ve Push:** Değişikliklerinizi anlamlı commit mesajlarıyla branch'inize `push` edin.
5.  **Pull Request Açın:** GitHub üzerinden `main` branch'ine bir "Pull Request" (PR) açın.
6.  **CI Kontrolleri:** PR'ınızın CI kontrollerinden (testler, linting) başarıyla geçtiğinden emin olun.
7.  **Kod İncelemesi:** Kodunuz incelenecek ve gerekli geri bildirimler sağlanacaktır.

Bu standartlara uyarak, AzuraForge platformunun uzun vadede sağlıklı, sürdürülebilir ve yüksek kalitede kalmasına yardımcı olursunuz.

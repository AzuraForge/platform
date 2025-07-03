# 🤝 AzuraForge Platformuna Katkıda Bulunma Rehberi

AzuraForge projesine gösterdiğiniz ilgi ve katkılarınız için teşekkür ederiz! Bu proje, modern yapay zeka sistemlerinin nasıl inşa edilmesi gerektiğine dair bir vizyonu hayata geçirmeyi amaçlamaktadır. Bu rehber, kod tabanının tutarlı, okunabilir, sürdürülebilir ve yüksek kalitede kalmasını sağlamak için benimsediğimiz çalışma prensiplerini ve standartlarını açıklamaktadır.

## 🚀 Hızlı Başlangıç

Eğer henüz geliştirme ortamınızı kurmadıysanız, lütfen platformun ana [Geliştirme Rehberi](./DEVELOPMENT_GUIDE.md) belgesindeki adımları takip edin.

## 🛠️ Kodlama Standartları

1.  **Stil Kılavuzu (PEP8 & Black):** Tüm Python kodları `black` ile formatlanmalıdır.
2.  **Linting (`flake8`):** Tüm Python kodları `flake8` denetiminden hatasız geçmelidir.
3.  **Tip İpuçları (Type Hinting & Mypy):** Tüm fonksiyon ve metod imzaları tip ipuçları içermelidir.
4.  **Dokümantasyon (Docstrings):** Tüm public modüller, sınıflar ve fonksiyonlar ne işe yaradıklarını açıklayan docstring'ler içermelidir.
5.  **Testler (`pytest`):** Eklenen her yeni özellik veya hata düzeltmesi için ilgili birim testleri `tests/` klasörüne eklenmelidir.

## 📝 Commit Mesajları

Commit mesajları, yapılan değişikliği net bir şekilde açıklamalıdır ve **[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)** standardına uymalıdır. **Bu, otomatik versiyonlama ve değişiklik günlüğü oluşturmak için hayati önem taşır.**

**Format:** `<tip>(<kapsam>): <açıklama>`

**Örnekler:**
*   `feat(learner): Implement Adam optimizer`
*   `fix(api): Correct dependency version for azuraforge-learner`
*   `docs(platform): Update contributing guide with automated release process`
*   `ci(worker): Add semantic-release to CI pipeline`
*   `refactor(dashboard): Modularize App.css into CSS Modules`

## 🔄 Pull Request (PR) Süreci

1.  **Branch Oluşturma:** `main` branch'inden kendi feature branch'inizi (`feat/yeni-ozellik` veya `fix/hata-adi` gibi) oluşturun.
2.  **Değişikliklerinizi Yapın:** Yukarıdaki standartlara uyduğunuzdan emin olun.
3.  **Test Edin:** Yerel testlerinizi (`pytest`, `npm run lint`) çalıştırın ve geçtiğinden emin olun.
4.  **Commit ve Push:** Değişikliklerinizi Conventional Commit formatında anlamlı commit mesajlarıyla branch'inize `push` edin.
5.  **Pull Request Açın:** GitHub üzerinden `main` branch'ine bir "Pull Request" (PR) açın.
6.  **CI Kontrolleri:** PR'ınızın otomatik CI kontrollerinden (testler, linting) başarıyla geçtiğinden emin olun.

## 📦 Otomatik Versiyonlama ve Bağımlılık Yönetimi (CI/CD)

Platformun kararlılığını sağlamak için Python ve Node.js paketlerimizde **otomatik Semantic Versioning ve Git etiketleme** kullanıyoruz. Bu süreç, GitHub Actions ve `semantic-release` araçlarıyla yönetilir. Bir kütüphane (`learner` gibi) güncellendiğinde, bu kütüphaneyi kullanan diğer depolardaki (`api` gibi) bağımlılıkların otomatik olarak güncellenmesi orta vadeli hedeflerimizdendir.

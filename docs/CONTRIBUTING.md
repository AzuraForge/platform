# 🤝 AzuraForge Platformuna Katkıda Bulunma Rehberi

AzuraForge projesine gösterdiğiniz ilgi ve katkılarınız için teşekkür ederiz! Bu proje, modern yapay zeka sistemlerinin nasıl inşa edilmesi gerektiğine dair bir vizyonu hayata geçirmeyi amaçlamaktadır. Bu rehber, kod tabanının tutarlı, okunabilir, sürdürülebilir ve yüksek kalitede kalmasını sağlamak için benimsediğimiz çalışma prensiplerini ve standartlarını açıklamaktadır.

## 🚀 Hızlı Başlangıç

Eğer henüz geliştirme ortamınızı kurmadıysanız, lütfen platformun ana [Geliştirme Rehberi](./DEVELOPMENT_GUIDE.md) belgesindeki "Geliştirme Ortamı Kurulumu" bölümünü takip edin.

## 🛠️ Kodlama Standartları

Projeye eklenen her kodun aşağıdaki standartları karşılaması beklenmektedir. Bu standartların birçoğu, ilgili reponun kök dizinindeki `pre-commit` hook'ları veya CI pipeline'ları ile otomatik olarak kontrol edilir.

1.  **Stil Kılavuzu (PEP8 & Black):**
    *   Tüm Python kodları, PEP8 stil kurallarına uymalıdır.
    *   Kodunuzu `black` ile otomatik formatlayın.
    *   **Kontrol:** `black .` komutunu çalıştırın.
    *   **Otomasyon:** CI pipeline'ları bu formatlamayı kontrol eder.

2.  **Linting (`flake8`):**
    *   Tüm Python kodları, `flake8` denetiminden hatasız geçmelidir.
    *   **Kontrol:** İlgili repo içinde `flake8 src` komutunu çalıştırın.
    *   **Otomasyon:** CI pipeline'ları linting'i kontrol eder.

3.  **Tip İpuçları (Type Hinting & Mypy):**
    *   Tüm fonksiyon ve metod imzaları, parametreler ve dönüş değerleri için tip ipuçları (`typing` modülü kullanılarak) içermelidir.
    *   **Kontrol:** İlgili repo içinde `mypy src` komutunu çalıştırın.
    *   **Otomasyon:** CI pipeline'ları statik tip denetimini kontrol eder.

4.  **Dokümantasyon (Docstrings):**
    *   Tüm public modüller, sınıflar ve fonksiyonlar, ne işe yaradıklarını, aldıkları argümanları (`Args:`) ve ne döndürdüklerini (`Returns:`) açıklayan Google-style docstring'ler içermelidir.

5.  **Testler (`pytest`):**
    *   Eklenen her yeni özellik veya fonksiyon için ilgili birim testleri (`unit tests`) `tests/` klasörüne eklenmelidir.
    *   Yapılan bir hata düzeltmesi (bug fix) için, o hatanın tekrar oluşmasını engelleyecek bir regresyon testi yazılmalıdır.
    *   **Kontrol:** İlgili reponun kök dizinindeyken `pytest` komutunu çalıştırın.
    *   **Otomasyon:** CI pipeline'ları testleri çalıştırır.

## 📝 Commit Mesajları

Commit mesajları, yapılan değişikliği net bir şekilde açıklamalıdır ve **[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)** standardına uymalıdır. **Bu, otomatik versiyonlama ve değişiklik günlüğü oluşturmak için hayati önem taşır.**

**Format:**
```
<tip>(<kapsam>): <açıklama>

[opsiyonel gövde]
```

**Örnek Tipler (semantic-release tarafından tanınan ve versiyonlamayı etkileyen):**
*   `feat`: Yeni bir özellik ekler (Minor versiyon artırımı: `0.1.x` -> `0.2.0`).
*   `fix`: Bir hata düzeltmesi (Patch versiyon artırımı: `0.1.3` -> `0.1.4`).
*   `docs`: Sadece dokümantasyon değişiklikleri (Versiyonu etkilemez).
*   `style`: Kod formatı, eksik noktalı virgül gibi stil düzeltmeleri (Versiyonu etkilemez).
*   `refactor`: Kodu yeniden yapılandırma, davranış değişikliği yok (Versiyonu etkilemez).
*   `perf`: Performans iyileştirmesi yapan kod değişikliği (Versiyonu etkilemez).
*   `test`: Eksik testlerin eklenmesi veya mevcut testlerin düzeltilmesi (Versiyonu etkilemez).
*   `build`: Build sistemi veya dış bağımlılık değişiklikleri (Versiyonu etkilemez).
*   `ci`: CI/CD yapılandırma dosyaları ve script'leri değişiklikleri (Versiyonu etkilemez).
*   `chore`: Rutin görevler, araç güncellemeleri (Versiyonu etkilemez).

**Örnekler:**
*   `feat(learner): Implement Adam optimizer`
*   `fix(api): Correct dependency version for azuraforge-learner`
*   `docs(platform): Update contributing guide with automated release process`
*   `ci(worker): Add semantic-release to CI pipeline`

## 🔄 Pull Request (PR) Süreci

1.  **Branch Oluşturma:** `main` branch'inden kendi feature branch'inizi (`feat/yeni-ozellik` veya `fix/hata-adi` gibi) oluşturun.
2.  **Değişikliklerinizi Yapın:** Yukarıdaki standartlara uyduğunuzdan emin olun.
3.  **Test Edin:** Yerel testlerinizi (`pytest`, `npm run lint`) çalıştırın ve geçtiğinden emin olun.
4.  **Commit ve Push:** Değişikliklerinizi **Conventional Commit formatında** anlamlı commit mesajlarıyla branch'inize `push` edin.
5.  **Pull Request Açın:** GitHub üzerinden `main` branch'ine bir "Pull Request" (PR) açın.
6.  **CI Kontrolleri:** PR'ınızın otomatik CI kontrollerinden (testler, linting) başarıyla geçtiğinden emin olun.
7.  **Kod İncelemesi:** Kodunuz incelenecek ve gerekli geri bildirimler sağlanacaktır.

Bu standartlara uyarak, AzuraForge platformunun uzun vadede sağlıklı, sürdürülebilir ve yüksek kalitede kalmasına yardımcı olursunuz.

## 📦 Otomatik Versiyonlama ve Bağımlılık Yönetimi (CI/CD)

Platformun kararlılığını sağlamak ve geliştirici deneyimini iyileştirmek için Python ve Node.js paketlerimizde **otomatik Semantic Versioning ve Git etiketleme** kullanıyoruz. Bu süreç, GitHub Actions ve `semantic-release` (Python için `python-semantic-release`, Node.js için `semantic-release`) araçlarıyla yönetilir.

### Nasıl Çalışır?

1.  **Commit Mesajları:** Herhangi bir Python veya Node.js deposunun `main` branch'ine yapılan her commit, **Conventional Commits standardına uygun** bir mesaj içermelidir (örn: `fix(...)`, `feat(...)`).
2.  **CI Tetiklenmesi:** Bu commit, ilgili deponun GitHub Actions CI workflow'unu tetikler.
3.  **Test ve Lint Kontrolleri:** CI pipeline'ı normal testleri ve lint kontrollerini çalıştırır.
4.  **Otomatik Versiyonlama:** Eğer tüm kontroller başarılı olursa, ve commit mesajı versiyon artışını tetikleyen bir türdeyse (`feat` için `minor`, `fix` için `patch`):
    *   `semantic-release` aracı, `pyproject.toml` (Python için) veya `package.json` (Node.js için) dosyasındaki `version` numarasını otomatik olarak artırır.
    *   Git deposunda yeni bir versiyon etiketi (örn: `v0.1.5`) oluşturur.
    *   Bu yeni versiyon etiketini ve güncellenmiş dosyaları (örn: `CHANGELOG.md`) GitHub'a otomatik olarak push eder.
    *   Yeni bir "Release" (Yayın) GitHub'da oluşturulur.
5.  **Otomatik Bağımlılık Güncellemesi (Mevcut Durum - Yarı Otomatik):**
    *   Şu an için, bir kütüphane (örn: `learner`) yeni bir versiyonla yayınlandığında, bu kütüphaneyi bağımlılık olarak kullanan diğer depoların (örn: `api`, `app-stock-predictor`) `pyproject.toml` veya `package.json` dosyalarındaki ilgili Git referanslarını (örn: `@v0.1.5`) **manuel olarak güncellemeleri** gerekmektedir.
    *   Bu manuel güncelleme yapıldıktan sonra, bu depolarda da bir `fix` commit'i atılarak kendi CI/CD'lerinin tetiklenmesi ve yeni bağımlılığı alması sağlanır.
    *   **Gelecek Hedefi:** Bu çapraz depo bağımlılık güncellemelerini tamamen otomatikleştirmek (örn: özel bir bot veya GitHub Actions `repository_dispatch` ile otomatik PR açma).

### Versiyonlama ve Bağımlılık Güncelleme Akışı (Katkıda Bulunan İçin)

Yeni bir özellik veya hata düzeltmesi üzerinde çalışırken izlemeniz gereken genel akış:

1.  **Değişikliği yapacağınız deponuzu klonlayın ve bir `feature` veya `fix` branch'i oluşturun.**
2.  **Kodunuzu yazın, yerel testlerinizi çalıştırın.**
3.  **Değişikliklerinizi Conventional Commit formatında commit'leyin.** Örnek: `fix(learner): Fix live prediction data flow` veya `feat(api): Add new endpoint for model serving`.
4.  **Commit'lerinizi `main` branch'ine push edin (veya bir PR açıp birleştirin).**
5.  **GitHub Actions'ta CI pipeline'ının başarıyla tamamlandığını doğrulayın.** Eğer commit'iniz `fix` veya `feat` ise, yeni bir versiyonun otomatik olarak etiketlendiğini ve yayınlandığını Releases sayfasından kontrol edin.
6.  **Bağımlı Repoları Kontrol Edin:** Eğer değiştirdiğiniz depo başka bir deponun bağımlılığı ise (örn: `learner`'ı değiştirdiyseniz `api` veya `app-stock-predictor`'a gidin), o depoların `pyproject.toml` veya `package.json` dosyalarını açın.
    *   Bağımlılığın eski versiyon etiketine işaret ettiğini göreceksiniz (örn: `learner.git@v0.1.5`).
    *   Bu versiyon etiketini, yeni yayınlanan versiyonla (örn: `v0.1.6`) **manuel olarak güncelleyin.**
    *   Bu güncelleme için de Conventional Commit (örn: `build(api): Update azuraforge-learner dependency to v0.1.6`) yapın ve `main` branch'ine push edin. Bu, bağımlı deponun kendi CI'ını tetikler ve yeni bağımlılığı içeren Docker imajının oluşturulmasını sağlar.

Bu dokümantasyon, projenin büyümesiyle birlikte daha da genişleyecek ve detaylanacaktır.


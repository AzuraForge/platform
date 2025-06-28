# 🤝 AzuraForge Platforma Katkıda Bulunma Rehberi

AzuraForge projesine gösterdiğiniz ilgi ve katkılarınız için teşekkür ederiz! Bu rehber, kod tabanının tutarlı, okunabilir, sürdürülebilir ve yüksek kalitede kalmasını sağlamak için benimsediğimiz çalışma prensiplerini ve standartlarını açıklamaktadır.

## 🚀 Hızlı Başlangıç

Eğer henüz geliştirme ortamınızı kurmadıysanız, lütfen platform repo'sunun ana `README.md` dosyasındaki "Geliştirme Ortamı Kurulumu" bölümünü takip edin.

## 🛠️ Kodlama Standartları

Projeye eklenen her kodun aşağıdaki standartları karşılaması beklenmektedir:

1.  **Stil Kılavuzu (PEP8 & Black):**
    *   Tüm Python kodları, PEP8 stil kurallarına uymalıdır.
    *   Kodunuzu `black` ile otomatik formatlayın.
    *   **Kontrol:** Değişikliklerinizi göndermeden önce `black <dosya_adı>` veya `black .` komutunu çalıştırın.
    *   **Otomasyon:** `pre-commit` hook'ları otomatik formatlamayı zorunlu kılar.

2.  **Linting (`flake8`):**
    *   Tüm Python kodları, `flake8` denetiminden hatasız geçmelidir.
    *   **Kontrol:** `flake8 <dosya_adı>` veya `flake8 src` komutunu çalıştırın.
    *   **Otomasyon:** `pre-commit` hook'ları linting'i zorunlu kılar.

3.  **Tip İpuçları (Type Hinting & Mypy):**
    *   Tüm fonksiyon ve metod imzaları, parametreler ve dönüş değerleri için tip ipuçları (`typing` modülü kullanılarak) içermelidir.
    *   **Kontrol:** `mypy src` komutunu çalıştırın.
    *   **Otomasyon:** `pre-commit` hook'ları statik tip denetimini zorunlu kılar.

4.  **Dokümantasyon (Docstrings):**
    *   Tüm modüller, sınıflar, fonksiyonlar ve metodlar, ne işe yaradıklarını, aldıkları argümanları (`Args:`) ve ne döndürdüklerini (`Returns:`) açıklayan Google-style docstring'ler içermelidir.
    *   Arayüz (API) repoları için `FastAPI`'nin otomatik dokümantasyonunu besleyecek docstring'ler kullanılmalıdır.

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
*   `docs`: Sadece dokümantasyon değişiklikleri (Versiyon artırımı yok).
*   `style`: Kod formatı, eksik noktalı virgül gibi stil düzeltmeleri (Kod mantığı değişmez, versiyon artırımı yok).
*   `refactor`: Kodu yeniden yapılandırma, davranış değişikliği yok (Versiyon artırımı yok).
*   `perf`: Performans iyileştirmesi yapan kod değişikliği (Versiyon artırımı olabilir).
*   `test`: Eksik testlerin eklenmesi veya mevcut testlerin düzeltilmesi (Versiyon artırımı yok).
*   `build`: Build sistemi veya dış bağımlılık değişiklikleri (Versiyon artırımı olabilir).
*   `ci`: CI/CD yapılandırma dosyaları ve script'leri değişiklikleri (Versiyon artırımı yok).

**Örnekler:**
*   `feat(core): Add exponential function to Tensor`
*   `fix(api): Resolve 307 redirect issues for endpoints`
*   `docs(platform): Add initial development guide`
*   `refactor(learner): Separate Layer and Model definitions`

## 🔄 Pull Request (PR) Süreci

1.  **Branch Oluşturma:** `main` branch'inden kendi feature branch'inizi (`feat/yeni-ozellik` veya `fix/hata-adi` gibi) oluşturun.
2.  **Değişikliklerinizi Yapın:** Yukarıdaki standartlara uyduğunuzdan emin olun.
3.  **Test Edin:** Yerel testlerinizi (`pytest`) çalıştırın ve geçtiğinden emin olun.
4.  **Commit ve Push:** Değişikliklerinizi anlamlı commit mesajlarıyla branch'inize `push` edin.
5.  **Pull Request Açın:** GitHub üzerinden `main` branch'ine bir "Pull Request" (PR) açın.
6.  **CI Kontrolleri:** PR'ınızın CI kontrollerinden (testler, linting) başarıyla geçtiğinden emin olun.
7.  **Kod İncelemesi:** Kodunuz incelenecek ve gerekli geri bildirimler sağlanacaktır.

Bu standartlara uyarak, AzuraForge platformunun uzun vadede sağlıklı, sürdürülebilir ve yüksek kalitede kalmasına yardımcı olursunuz.

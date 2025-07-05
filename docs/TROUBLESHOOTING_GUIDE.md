# 🩺 AzuraForge: Sorun Giderme Rehberi

Bu belge, AzuraForge platformunda geliştirme yaparken karşılaşılan yaygın sorunları, nedenlerini ve kanıtlanmış çözümlerini içerir. Bir sorunla karşılaştığınızda ilk başvuracağınız yer burası olmalıdır.

## 1. Konteyner Başlatma veya Çalışma Zamanı Hataları

### Vaka Analizi 1: `docker-compose up` sırasında `loki: plugin not found` Hatası
*   **Hata Logu:** `Error response from daemon: error looking up logging plugin loki: plugin "loki" not found`
*   **Neden:** `docker-compose.yml` dosyasında bir veya daha fazla servis, `logging` sürücüsü olarak `loki` kullanmaya ayarlanmıştır. Ancak, ana makinedeki Docker motorunda bu eklenti kurulu değildir. Bu, özellikle Docker'ın standart dağıtımlarında yaygındır.
*   **Çözüm:** Logları doğrudan Loki'ye gönderme yönteminden vazgeçin. Bunun yerine, tüm servislerin `logging` ayarını varsayılan `json-file` olarak bırakın. Logları toplama görevini `promtail` servisine verin. `promtail-config.yml` dosyasının, `docker_sd_configs` kullanarak Docker soketinden logları okuyacak şekilde yapılandırıldığından emin olun. Bu yöntem, ek Docker eklentisi gerektirmediği için daha taşınabilir ve güvenilirdir.

### Vaka Analizi 2: Grafana'da "No data" veya "Datasource not found" Hatası
*   **Hata Logu:** Grafana arayüzünde "No data" yazısı. `grafana` konteyner loglarında `Failed to upgrade legacy queries Datasource ... was not found` veya `failed to save dashboard ... could not resolve dashboards:uid:...`
*   **Neden:** Bu sorunun birden çok katmanı olabilir:
    1.  **Promtail-Loki Bağlantısı:** `promtail` logları toplayıp Loki'ye gönderemiyor olabilir. `promtail-config.yml` dosyasındaki `scrape_configs` bölümünün, ağ adına bağımlı olmadan tüm konteynerleri keşfettiğinden emin olun.
    2.  **Grafana-Loki Bağlantısı:** Grafana, Loki veri kaynağını bulamıyor. Grafana'nın modern sürümleri, `provisioning` ile oluşturulan veri kaynakları ve dashboard'lar arasında tutarlı bir bağlantı için `uid` alanına güvenir.
*   **Çözüm:**
    1.  `platform/config/grafana/provisioning/datasources/loki-datasource.yml` dosyasında, `datasource` için sabit bir `uid` (örn: `azuraforge_loki_ds`) tanımlayın.
    2.  `platform/config/grafana/provisioning/dashboards/system-overview.json` dosyasında, tüm panellerin ve değişkenlerin `datasource` alanının, bu sabit `uid`'yi kullandığından emin olun.
    3.  `docker-compose down -v` komutuyla sistemi volümleriyle birlikte tamamen indirip yeniden başlatarak Grafana'nın provisioning işlemini sıfırdan yapmasını sağlayın.

### Vaka Analizi 3: Worker'da `KeyError` veya `pydantic.ValidationError`
*   **Hata Logu:** Batch (toplu) deneyler çalıştırırken `KeyError: 'some_column'` veya Pydantic'in `Field required` hatası.
*   **Neden:** Hiperparametre optimizasyonu sırasında her bir alt deney, konfigürasyonun sadece değişen kısmını alır. Ancak pipeline, tam konfigürasyona (örn: hedef sütun adı) ihtiyaç duyar. Veya, `worker` içindeki bir yardımcı fonksiyon (örn: `get_shared_data`), Pydantic doğrulaması için pipeline'a eksik bir konfigürasyon gönderiyordur.
*   **Çözüm:** Pipeline kodunu daha sağlam hale getirin.
    *   `_get_target_and_feature_cols` gibi metotlar, hedef sütunun her zaman özellikler listesinde olduğundan emin olmalıdır.
    *   `worker`'da, bir pipeline'ın geçici bir örneğini oluşturmanız gerekiyorsa, ona her zaman `full_config` (kullanıcıdan gelen orijinal konfigürasyonla zenginleştirilmiş tam yapı) gönderin, sadece bir parçasını değil.

### Vaka Analizi 4: Sistemin Kilitlenmesi ve Aşırı Kaynak Tüketimi
*   **Semptomlar:** Sistem yavaşlar, Docker komutları yanıt vermez, bellek kullanımı %90'ları aşar.
*   **Neden:** Genellikle, Celery'nin `prefork` modelinde çalışan birden çok worker alt sürecinin, her birinin büyük bir veri setini kendi bellek alanına yüklemesinden kaynaklanır (`Concurrency Sayısı * Veri Seti Boyutu`).
*   **Çözüm:** Veri yükleme mantığını pipeline'ların içinden çıkarın. `worker`'ın ana görev modülünde (`training_tasks.py` gibi), Python'un `@lru_cache` dekoratörünü kullanarak paylaşımlı bir bellek içi önbellek oluşturun. Veriyi, görevi alan alt süreçler yerine, bu paylaşımlı fonksiyonda **sadece bir kez** yükleyin. Alt süreçler bu önbelleğe kopyalama yapmadan (copy-on-write) erişir, bu da bellek kullanımını dramatik şekilde düşürür.

---
Bu güncellemelerle birlikte, projenizin dokümantasyonu da mevcut stabil durumunu yansıtacak ve gelecekteki geliştirmeler için sağlam bir zemin oluşturacaktır.
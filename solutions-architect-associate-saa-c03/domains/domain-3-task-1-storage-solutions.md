# Domain 3 — Task Statement 1: Determine High-Performing and/or Scalable Storage Solutions

---

## 2. Önemli Keywords (Sınav Odaklı)

### 3 Depolama Türü

| Tür | Servis | Kullanım |
|-----|--------|----------|
| **Object (Nesne)** | S3 | Büyük veri, yedekleme, statik içerik, medya |
| **Block (Blok)** | EBS | EC2 kalıcı depolama, düşük gecikme, IOPS |
| **File (Dosya)** | EFS, FSx | Paylaşımlı dosya sistemi, NFS/SMB |

### Depolama Seçim Kriterleri

| Kriter | Açıklama |
|--------|----------|
| **Access method** | Nasıl erişilecek (API, mount, attach) |
| **Access patterns** | Sıralı mı rastgele mi, sık mı nadir mi |
| **Throughput** | Gerekli veri aktarım hızı |
| **Frequency of access** | Ne sıklıkla erişilecek |
| **Frequency of updates** | Ne sıklıkla güncellenecek |
| **Availability & Durability** | Kullanılabilirlik ve dayanıklılık gereksinimleri |

### Amazon S3 (Object Storage)

| Keyword | Sınav Notu |
|---------|-----------|
| **Globally resilient** | AZ arızasını tolere eder, Region'lar arası replikasyon mümkün |
| **S3 Storage Classes** | Farklı erişim sıklıkları için farklı sınıflar |
| **Multi-part uploads** | Büyük dosya yükleme performansı |
| **S3 Transfer Accelerator** | CloudFront edge location'ları üzerinden hızlı upload |
| **CloudFront + S3** | Veri alımını (retrieval) hızlandırma — caching |
| **API calls / CLI** | Temel S3 operasyonlarını bilin |

### Amazon EBS (Block Storage)

| Keyword | Sınav Notu |
|---------|-----------|
| **EBS Volume Types** | gp3, gp2, io2, io1, st1, sc1 — IOPS ve throughput farklarını bilin |
| **IOPS** | Performans yapılandırması — hacim türüne göre değişir |
| **Live configuration changes** | Üretimde çalışırken hacim türü/boyut/IOPS değiştirilebilir |
| **Manuel ölçeklendirme** | EBS otomatik ölçeklenmez — manuel müdahale gerekir |
| **Instance Store** | Geçici (ephemeral) yerel depolama — kalıcı değil |
| **EBS Snapshots** | S3'te saklanır → Region dayanıklı, DR için kullanılır |
| **DAS, SAN, persistent storage** | Senaryo sorularında blok depolama anahtar kelimeleri |

### Amazon EFS (File Storage — Linux)

| Keyword | Sınav Notu |
|---------|-----------|
| **Otomatik ölçeklendirme** | Dosya eklendikçe/silindikçe otomatik büyür/küçülür |
| **NFS protokolü** | Linux örnekleri için |
| **Hibrit erişim** | VPN veya Direct Connect üzerinden |
| **General Purpose vs Max IO** | Performans modları |
| **Lifecycle policies** | Maliyet optimizasyonu — seyrek erişilen dosyalar ucuz katmana |
| **En az operasyonel yük** | EBS'ye kıyasla ölçeklendirme için müdahale gerekmez |

### Amazon FSx

| Keyword | Sınav Notu |
|---------|-----------|
| **FSx for Windows** | SMB protokolü, NTFS, Active Directory entegrasyonu |
| **FSx for Lustre** | Linux, HPC, yüksek performanslı iş yükleri |
| **Hibrit depolama** | On-premises + AWS |

### Ölçeklendirme Karşılaştırması

| Servis | Ölçeklendirme | Operasyonel Yük |
|--------|---------------|-----------------|
| **EBS** | Manuel (hacim boyutu/IOPS değiştir) | Yüksek |
| **EFS** | Otomatik | Düşük |
| **S3** | Otomatik (sınırsıza yakın) | Çok düşük |

### Performans İyileştirme

| Senaryo | Çözüm |
|---------|-------|
| S3 upload performansı | Multi-part uploads, S3 Transfer Accelerator |
| S3 retrieval performansı | CloudFront caching |
| Düşük gecikme blok depolama | EBS (doğru hacim türü + IOPS yapılandırması) |
| Paylaşımlı dosya sistemi (Linux) | EFS |
| Paylaşımlı dosya sistemi (Windows) | FSx for Windows |
| Hibrit depolama | Storage Gateway, FSx |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 3'ün ilk görev ifadesini ele alıyor: **yüksek performanslı ve ölçeklenebilir depolama çözümleri seçimi**.

### 3 Depolama Biçimi

AWS'de depolama 3 temel biçimde sunulur: **Object (S3)**, **Block (EBS)** ve **File (EFS/FSx)**. Sınavda doğru seçimi yapmak için erişim yöntemi, erişim kalıbı, throughput, erişim sıklığı ve dayanıklılık gereksinimlerini değerlendirmelisiniz.

### EBS vs EFS Ölçeklendirme Farkı

Videonun en güçlü vurgusu bu karşılaştırma. **EBS manuel ölçeklenir** — hacim türü, boyutu ve IOPS'yi siz değiştirirsiniz (canlı değişiklik desteklenir). **EFS otomatik ölçeklenir** — dosya ekledikçe büyür, sildikçe küçülür. Sınavda "en az operasyonel yük ile ölçeklendirme" sorulursa → EFS veya S3.

### S3 Dayanıklılığı

S3 **globally resilient** bir servistir. Veriler belirli bir Region'da saklanır, AZ arızasını tolere eder ve Region'lar arası replike edilebilir. EBS snapshot'ları S3'te saklandığı için Region dayanıklıdır.

### Dosya Depolama Seçimi

Sınavda anahtar kelimelere göre seçim yapın:
- **NFS, Linux, paylaşımlı** → EFS
- **SMB, NTFS, Windows, Active Directory** → FSx for Windows
- **HPC, Lustre, Linux** → FSx for Lustre
- **Hibrit** → Storage Gateway veya FSx

### Performans İyileştirme

S3 için: büyük dosyalarda **multi-part uploads**, hızlı upload için **S3 Transfer Accelerator**, hızlı okuma için **CloudFront caching**. EBS için: doğru **hacim türü ve IOPS yapılandırması** kritik.

> **Sınav İpucu:** Soruda "düşük gecikme" görürseniz → EBS. "Otomatik ölçeklenen dosya sistemi" → EFS. "Windows dosya paylaşımı" → FSx. "Büyük veri yedekleme" → S3. Anahtar kelimelere göre eşleştirin.

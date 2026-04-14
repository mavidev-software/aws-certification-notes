# Domain 4 — Task Statement 1: Design Cost-Optimized Storage Solutions

---

## 2. Önemli Keywords (Sınav Odaklı)

### Depolama Maliyet Hiyerarşisi

| Depolama Türü | Maliyet | Özellik |
|---------------|---------|---------|
| **Ephemeral (Instance Store)** | En düşük (EC2 fiyatına dahil) | Geçici, kalıcı değil |
| **EBS** | Ek maliyet | Kalıcı, blok depolama, snapshot desteği |
| **S3** | Düşük (sınırsız) | 11 nines dayanıklılık, sınırsız kapasite |
| **S3 Glacier** | En düşük depolama maliyeti | Arşivleme, yüksek alma maliyeti |

### EBS Maliyet Optimizasyonu

| Strateji | Açıklama |
|----------|----------|
| **Right-sizing** | Hacim kapasitesini gereğinden fazla sağlamama |
| **Volume type değiştirme** | Provisioned IOPS → GP2/GP3 (bursting yeterliyse) |
| **IOPS izleme** | Kullanılan IOPS vs ödenen IOPS karşılaştırması |
| **Bağlı olmayan hacimleri silme** | Trusted Advisor ile tespit → sil |
| **Data Lifecycle Manager** | Eski snapshot'ları otomatik silme |
| **AWS Backup** | Yedekleme yaşam döngüsü yönetimi |

### S3 Depolama Sınıfları ve Maliyet

| Sınıf | Kullanım | Depolama Maliyeti | Alma Maliyeti |
|-------|----------|-------------------|---------------|
| **S3 Standard** | Sık erişilen veri | Yüksek | Düşük |
| **S3 Standard-IA** | Seyrek erişilen | Orta | Orta |
| **S3 One Zone-IA** | Seyrek, tek AZ yeterli | Düşük | Orta |
| **S3 Intelligent-Tiering** | Erişim kalıbı değişken | Otomatik optimize | — |
| **S3 Glacier Instant** | Arşiv, anında erişim | Düşük | Orta-yüksek |
| **S3 Glacier Flexible** | Arşiv, dakikalar-saatler | Çok düşük | Yüksek |
| **S3 Glacier Deep Archive** | Uzun süreli arşiv | En düşük | En yüksek |

### S3 Lifecycle vs Intelligent Tiering

| Özellik | S3 Lifecycle | Intelligent Tiering |
|---------|-------------|---------------------|
| **Nasıl çalışır** | Süreye dayalı kural → tüm nesneleri geçiş | Erişim kalıbına göre → her nesneyi bireysel taşı |
| **Ne zaman** | Bilinen erişim kalıbı, zamana dayalı | Bilinmeyen/değişken erişim kalıbı |
| **Otomasyon** | Kural bazlı | Tamamen otomatik |

### S3 Ek Maliyet Özellikleri

| Özellik | Açıklama |
|---------|----------|
| **Requester Pays** | İstekte bulunan, istek ve indirme maliyetini öder (kova sahibi değil) |
| **S3 Lifecycle Policies** | Verileri otomatik arşivle veya sil |
| **Cross-Region Replication** | Ek transfer maliyeti — gerçekten gerekli mi değerlendir |

### Maliyet Yönetim Araçları

| Araç | Ne İşe Yarar |
|------|-------------|
| **Cost Explorer** | Yüksek seviyeli etkileşimli görünüm, drill-down |
| **Cost and Usage Reports** | Saat/gün/ay bazında detaylı maliyet ayrıştırması |
| **AWS Budgets** | Bütçe eşiklerine dayalı alarmlar ve otomatik eylemler |
| **Trusted Advisor** | Bağlı olmayan EBS, yetersiz kullanılan kaynaklar tespiti |
| **Cost Allocation Tags** | Cost Explorer'da filtreleme, maliyet analizi |
| **Consolidated Billing** | Organizations ile merkezi faturalandırma, hacim indirimleri |
| **CloudWatch** | Metrik izleme, alarmlar, otomatik aksiyonlar |

### Veri Transferi Maliyet Optimizasyonu

| Senaryo | Çözüm |
|---------|-------|
| 250 TB arşiv veri → S3 | **Snowball** (birden fazla cihaz) |
| Kullanıcıya yakın cache | **CloudFront** |
| Dedike on-prem bağlantı | **Direct Connect** |
| Hibrit dosya transferi | **DataSync, Transfer Family, Storage Gateway** |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 4'ün ilk görev ifadesini ele alıyor: **maliyet optimize edilmiş depolama çözümleri tasarlama**.

### Depolama Maliyet Prensipleri

En ucuz depolama **ephemeral (instance store)** — EC2 fiyatına dahil ama kalıcı değil. Kalıcı depolama gerekiyorsa EBS veya S3. Anahtar prensip: **right-sizing** — kullandığınızdan fazlasını sağlamayın.

### EBS Optimizasyonu

- Provisioned IOPS gerçekten gerekli mi? Bursting yeterliyse GP2/GP3'e geç
- Kullanılan IOPS vs ödenen IOPS'u sürekli karşılaştır
- **Trusted Advisor** bağlı olmayan hacimleri tespit eder → sil
- **Data Lifecycle Manager** ile eski snapshot'ları otomatik sil

### S3 Maliyet Optimizasyonu

İki kritik kavram:
1. **Lifecycle Policies:** Zamana dayalı → "30 gün sonra IA'ya, 90 gün sonra Glacier'a taşı"
2. **Intelligent Tiering:** Erişim kalıbına dayalı → her nesneyi otomatik en uygun katmana taşır

Farkı bilin: Lifecycle = süre bazlı toplu geçiş, Intelligent Tiering = bireysel nesne bazlı otomatik optimizasyon.

### Maliyet Yönetim Araçları

- **Cost Explorer** = yüksek seviye görünüm + drill-down
- **Cost and Usage Reports** = detaylı ayrıştırma
- **AWS Budgets** = eşik alarmları + otomatik eylemler
- **Cost Allocation Tags** = maliyet filtreleme ve analiz
- **Consolidated Billing** = Organizations ile hacim indirimleri

### Veri Transferi

250 TB gibi büyük verilerde **Snowball** en hızlı ve uygun maliyetli. CloudFront ile kullanıcıya yakın cache, Direct Connect ile dedike bağlantı maliyeti azaltır.

> **Sınav İpucu:**
> - "Erişim kalıbı değişken + S3 maliyet" → Intelligent Tiering
> - "Zamana dayalı arşivleme" → S3 Lifecycle
> - "Bağlı olmayan EBS" → Trusted Advisor + sil
> - "IOPS fazla ödeme" → volume type değiştir (io → gp)
> - "250 TB on-prem → S3" → Snowball
> - "Requester pays" → kova sahibi maliyet ödemez

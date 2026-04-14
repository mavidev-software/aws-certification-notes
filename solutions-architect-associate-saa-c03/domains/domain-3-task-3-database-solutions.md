# Domain 3 — Task Statement 3: Determine High-Performing Database Solutions

---

## 2. Önemli Keywords (Sınav Odaklı)

### AWS Veritabanı Motorları

| Tür | Servis | Kullanım |
|-----|--------|----------|
| **Relational** | RDS, Aurora | Yapılandırılmış veri, SQL sorguları, ACID |
| **Key-Value** | DynamoDB | Düşük gecikme, yüksek hacim, NoSQL |
| **Document** | DocumentDB | JSON benzeri belgeler |
| **In-Memory** | ElastiCache (Redis/Memcached) | Önbellekleme, ultra düşük gecikme |
| **Graph** | Neptune | İlişki ağları, sosyal medya |
| **Time Series** | Timestream | IoT, metrikler, zaman bazlı veri |
| **Ledger** | QLDB | Değiştirilemez, doğrulanabilir geçmiş |

### Veritabanı Seçim Kriterleri

| Kriter | Açıklama |
|--------|----------|
| **Availability** | Kullanılabilirlik gereksinimleri |
| **Consistency** | Veri tutarlılığı |
| **Partition Tolerance** | Bölüm toleransı (CAP teoremi) |
| **Latency** | Gecikme gereksinimleri |
| **Durability** | Dayanıklılık |
| **Scalability** | Ölçeklenebilirlik |
| **Query Capability** | Sorgu yeteneği |

### RDS vs Aurora — Kritik Farklar

| Özellik | RDS | Aurora |
|---------|-----|--------|
| **Mimari** | Tek instance + standby | Cluster (primary + 0-n replica) |
| **Depolama** | Instance'a bağlı yerel depolama | Paylaşımlı cluster volume |
| **Read Replica amacı** | Sadece okuma performansı | Okuma performansı + HA (Multi-AZ etkisi) |
| **Multi-Region** | Sınırlı | Aurora Global Database |
| **Veri kopyası** | — | 3 AZ'de 6 kopya |
| **Dayanıklılık** | Regional (tek AZ kaybı) | Regional + Global seçeneği |

### DynamoDB

| Keyword | Sınav Notu |
|---------|-----------|
| **Tek haneli milisaniye gecikme** | Yük ne olursa olsun tutarlı — ayarlama gerekmez |
| **SSD destekli** | Hızlı |
| **Otomatik replikasyon** | Birden fazla depolama düğümüne varsayılan |
| **Point-in-time recovery** | Yedekleme özelliği |
| **Beklemede şifreleme (encryption at rest)** | Yerleşik güvenlik |
| **Event-driven entegrasyon** | DynamoDB Streams — tablo değişikliğinde aksiyon |

### Caching Çözümleri

| Servis | Motor | Kullanım |
|--------|-------|----------|
| **ElastiCache Redis** | Redis | Yönetilen in-memory cache, pub/sub, persistence |
| **ElastiCache Memcached** | Memcached | Basit caching, çoklu iş parçacığı |
| **DAX** | DynamoDB Accelerator | DynamoDB'ye özel, milisaniye altı gecikme, item + query cache |

### RDS Proxy

| Keyword | Sınav Notu |
|---------|-----------|
| **Bağlantı havuzlama** | Çok sayıda bağlantıyı verimli yönetir |
| **Lambda + RDS** | Lambda'nın çok bağlantı açma sorununu çözer |
| **Performans** | Compute/memory stresini azaltır |
| **Failover** | Aurora/RDS için %66'ya kadar hızlı failover |

### Aurora Serverless

| Keyword | Sınav Notu |
|---------|-----------|
| **ACU (Aurora Capacity Units)** | Compute + memory birimi, min-max arası ölçeklenir |
| **Sıfıra inebilir** | Hareketsizlikte duraklatılır → yalnızca depolama ücreti |
| **Kullanım senaryosu** | Seyrek, aralıklı, tahmin edilemez iş yükleri |
| **Paylaşımlı depolama** | 3 AZ'de 6 kopya — standart Aurora ile aynı |
| **Maliyet optimizasyonu** | Kullanılmadığında kapanır |

### RDS Storage Auto Scaling

| Detay | Açıklama |
|-------|----------|
| **Açılabilir** | Tahmin edilemez iş yükleri için |
| **Sınır** | Maksimum depolama eşiği aşılacaksa ölçeklendirme gerçekleşmez |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 3'ün üçüncü görev ifadesini ele alıyor: **yüksek performanslı veritabanı çözümlerini belirleme**.

### Purpose-Built Databases

AWS'nin temel felsefesi: **her iş yüküne özel veritabanı**. İlişkisel her şeye uymaz. Grafik verileri için Neptune, zaman serisi için Timestream, anahtar-değer için DynamoDB kullanın. Sınavda "hangi veritabanı" sorusu geldiğinde, kullanım senaryosundaki anahtar kelimelere göre eşleştirin.

### RDS vs Aurora — En Kritik Fark

Aurora, RDS ailesinin parçası olmasına rağmen mimari olarak çok farklıdır:
- Aurora **paylaşımlı cluster volume** kullanır (3 AZ'de 6 kopya)
- Aurora **read replica'ları hem okuma performansı hem HA** sağlar (RDS'te read replica sadece okuma içindir)
- **Aurora Global Database** birden fazla Region'ı kapsayabilir — regional failover gerekiyorsa Aurora

### DynamoDB vs Aurora Seçimi

Soruda "tutarlı tek haneli milisaniye, yüksek hacim, ayarlama gerekmez" görürseniz → **DynamoDB**. "İlişkisel veri, SQL sorguları, regional failover" → **Aurora**.

### Caching Stratejisi

- **ElastiCache** = genel amaçlı in-memory cache (Redis veya Memcached)
- **DAX** = sadece DynamoDB için, milisaniye altı gecikme
- **Read Replica ≠ Cache** — önceki dersten hatırlayın

### Aurora Serverless — Maliyet Optimizasyonu

Tahmin edilemez/seyrek iş yükleri için ideal. ACU (Aurora Capacity Units) ile min-max arası otomatik ölçeklenir. **Sıfıra inebilir ve duraklatılabilir** — yalnızca depolama ücreti. Bu özellik sınavda maliyet optimizasyonu soruları ile birlikte çıkabilir.

### Lambda + RDS Sorunu

Lambda çok sayıda bağlantı açıp kapatır → veritabanı kaynaklarını tüketir. Çözüm: **RDS Proxy** ile bağlantı havuzlama.

> **Sınav İpucu:**
> - "Tek haneli ms, yüksek hacim, NoSQL" → DynamoDB
> - "Regional failover + ilişkisel" → Aurora Global Database
> - "Lambda + RDS bağlantı sorunu" → RDS Proxy
> - "Tahmin edilemez iş yükü + maliyet" → Aurora Serverless
> - "Seyrek erişilen veri + cache" → ElastiCache veya DAX

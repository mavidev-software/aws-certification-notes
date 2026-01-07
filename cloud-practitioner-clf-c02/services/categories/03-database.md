# Database Services (In-Scope)

> **Kategori:** Veritabanı Servisleri

## Amazon RDS (Relational Database Service)

**Ne İşe Yarar:** Yönetilen ilişkisel veritabanı hizmeti sunar.

**Desteklenen Veritabanları:**
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- Microsoft SQL Server
- Amazon Aurora

**Temel Özellikler:**
- Otomatik yedekleme
- Multi-AZ dağıtımı (yüksek erişilebilirlik)
- Read Replica (okuma performansı)
- Otomatik yazılım yamaları

**Ne Zaman Kullanılır:**
- Geleneksel web uygulamaları
- ERP sistemleri
- CRM uygulamaları

---

## Amazon Aurora

**Ne İşe Yarar:** AWS'nin yüksek performanslı ilişkisel veritabanı.

**Temel Özellikler:**
- MySQL'den 5x, PostgreSQL'den 3x daha hızlı
- 6 kopya, 3 AZ'de otomatik replikasyon
- Otomatik ölçekleme (10GB'dan 128TB'a)
- Serverless seçeneği mevcut

**Aurora vs RDS:**
| Özellik | Aurora | RDS |
|---------|--------|-----|
| Performans | Daha yüksek | Standart |
| Maliyet | Daha yüksek | Daha düşük |
| Replikasyon | 6 kopya | Multi-AZ |

---

## Amazon DynamoDB

**Ne İşe Yarar:** Tam yönetilen NoSQL veritabanı.

**Temel Özellikler:**
- Milisaniye gecikme süresi
- Sınırsız ölçekleme
- Key-value ve document veri modelleri
- DAX (DynamoDB Accelerator) ile mikrosaniye gecikme
- Global Tables ile çoklu bölge replikasyonu

**Ne Zaman Kullanılır:**
- Yüksek trafikli web uygulamaları
- Gaming leaderboard'ları
- IoT veri depolama
- Session yönetimi

**DynamoDB vs RDS:**
| Özellik | DynamoDB | RDS |
|---------|----------|-----|
| Tür | NoSQL | SQL |
| Şema | Esnek | Sabit |
| Ölçekleme | Otomatik | Manuel |

---

## Amazon ElastiCache

**Ne İşe Yarar:** In-memory cache hizmeti sunar.

**Desteklenen Motorlar:**
- **Redis:** Gelişmiş veri yapıları, persistence, replikasyon
- **Memcached:** Basit caching, çoklu thread

**Ne Zaman Kullanılır:**
- Session caching
- Veritabanı sorgu sonuçlarını cache'leme
- Gerçek zamanlı analitik

---

## Amazon DocumentDB

**Ne İşe Yarar:** MongoDB uyumlu yönetilen document veritabanı.

**Temel Özellikler:**
- MongoDB 3.6, 4.0, 5.0 API uyumlu
- Otomatik ölçekleme
- 6 kopya, 3 AZ replikasyon

**Ne Zaman Kullanılır:**
- Content management
- Katalog yönetimi
- Kullanıcı profilleri

---

## Amazon Neptune

**Ne İşe Yarar:** Yönetilen graph veritabanı.

**Desteklenen Modeller:**
- Property Graph (Gremlin)
- RDF (SPARQL)

**Ne Zaman Kullanılır:**
- Sosyal ağlar
- Öneri motorları
- Fraud detection
- Knowledge graphs

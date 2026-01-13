# Task 3.4: AWS Database Services (Veritabanı Servisleri)

> **Resmi Tanım:** Identify AWS database services.
>
> **Kaynak:** [AWS CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

## İçindekiler

1. [Genel Bakış](#genel-bakış)
2. [Veritabanı Türleri](#veritabanı-türleri)
3. [Amazon RDS](#amazon-rds)
4. [Amazon Aurora](#amazon-aurora)
5. [Amazon DynamoDB](#amazon-dynamodb)
6. [Amazon ElastiCache](#amazon-elasticache)
7. [Amazon Redshift](#amazon-redshift)
8. [Diğer Veritabanı Servisleri](#diğer-veritabanı-servisleri)
9. [Karşılaştırma Tabloları](#karşılaştırma-tabloları)
10. [Sınav Senaryoları](#sınav-senaryoları)

---

## Genel Bakış

AWS, farklı kullanım senaryoları için çeşitli veritabanı çözümleri sunar. Her veritabanı türü belirli ihtiyaçlara yönelik optimize edilmiştir.

### Veritabanı Seçim Kriterleri

```
┌─────────────────────────────────────────────────────────────────────┐
│                    VERİTABANI SEÇİM AĞACI                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  İlişkisel veri mi?                                                 │
│       │                                                             │
│       ├── EVET ──► Yüksek performans + ölçeklenebilirlik?          │
│       │                 │                                           │
│       │                 ├── EVET ──► Amazon Aurora                  │
│       │                 │                                           │
│       │                 └── HAYIR ──► Amazon RDS                    │
│       │                                                             │
│       └── HAYIR ──► Key-Value / Document?                          │
│                          │                                          │
│                          ├── EVET ──► Amazon DynamoDB               │
│                          │                                          │
│                          └── HAYIR ──► Özel veri modeli?           │
│                                            │                        │
│                                            ├── Graph ──► Neptune    │
│                                            ├── Cache ──► ElastiCache│
│                                            └── Analytics ──► Redshift│
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Veritabanı Türleri

### 1. İlişkisel Veritabanları (Relational - SQL)

```
┌─────────────────────────────────────────────────────────────────┐
│                    İLİŞKİSEL VERİTABANI                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Tablolar      ┌──────────┐   ┌──────────┐   ┌──────────┐      │
│                │ Müşteri  │   │ Sipariş  │   │  Ürün    │      │
│                ├──────────┤   ├──────────┤   ├──────────┤      │
│                │ ID       │◄──┤ MüşteriID│   │ ID       │      │
│                │ Ad       │   │ ÜrünID   │──►│ Ad       │      │
│                │ Email    │   │ Miktar   │   │ Fiyat    │      │
│                │ Telefon  │   │ Tarih    │   │ Stok     │      │
│                └──────────┘   └──────────┘   └──────────┘      │
│                                                                 │
│  Özellikler:                                                    │
│  ✓ Şema zorunlu (Schema-based)                                 │
│  ✓ ACID uyumlu (Atomicity, Consistency, Isolation, Durability) │
│  ✓ SQL sorgu dili                                              │
│  ✓ JOIN işlemleri                                              │
│  ✓ Karmaşık sorgular                                           │
│                                                                 │
│  AWS Servisleri: RDS, Aurora                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**ACID Özellikleri:**
| Özellik | Açıklama | Örnek |
|---------|----------|-------|
| **A**tomicity | Tüm işlem başarılı olur veya hiçbiri olmaz | Para transferi: hem çekme hem yatırma |
| **C**onsistency | Veri her zaman tutarlı | Negatif bakiye olamaz |
| **I**solation | İşlemler birbirini etkilemez | Eşzamanlı okuma/yazma |
| **D**urability | Onaylanan veri kalıcı | Sistem çökmesinde veri korunur |

### 2. NoSQL Veritabanları

```
┌─────────────────────────────────────────────────────────────────┐
│                      NoSQL VERİTABANI TÜRLERİ                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   Key-Value     │  │    Document     │  │     Graph       │ │
│  ├─────────────────┤  ├─────────────────┤  ├─────────────────┤ │
│  │                 │  │                 │  │                 │ │
│  │  Key: user123   │  │ {               │  │    (A)──────(B) │ │
│  │  Value: {...}   │  │   "name": "Ali",│  │     │\    /│    │ │
│  │                 │  │   "age": 25,    │  │     │ \  / │    │ │
│  │  Key: order456  │  │   "orders": []  │  │     │  \/  │    │ │
│  │  Value: {...}   │  │ }               │  │     │  /\  │    │ │
│  │                 │  │                 │  │     │ /  \ │    │ │
│  │                 │  │                 │  │    (C)──────(D) │ │
│  ├─────────────────┤  ├─────────────────┤  ├─────────────────┤ │
│  │ DynamoDB        │  │ DocumentDB      │  │ Neptune         │ │
│  │ ElastiCache     │  │ DynamoDB        │  │                 │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
│                                                                 │
│  Ortak Özellikler:                                             │
│  ✓ Şemasız veya esnek şema                                     │
│  ✓ Yatay ölçekleme (Horizontal Scaling)                        │
│  ✓ Yüksek performans                                           │
│  ✓ Büyük veri hacimleri                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon RDS

> **Amazon Relational Database Service** - Yönetilen ilişkisel veritabanı servisi.

### RDS Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                        AMAZON RDS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Yönetilen (Managed) İlişkisel Veritabanı Servisi"            │
│                                                                 │
│  AWS Yönetir:                    Siz Yönetirsiniz:              │
│  ┌────────────────────────┐     ┌────────────────────────┐     │
│  │ ✓ Donanım sağlama      │     │ ✓ Veritabanı şeması    │     │
│  │ ✓ İşletim sistemi      │     │ ✓ Tablo yapısı         │     │
│  │ ✓ Veritabanı kurulumu  │     │ ✓ Sorgular (queries)   │     │
│  │ ✓ Otomatik yedekleme   │     │ ✓ Uygulama mantığı     │     │
│  │ ✓ Yazılım güncelleme   │     │ ✓ Kullanıcı yönetimi   │     │
│  │ ✓ Güvenlik yamaları    │     │ ✓ Bağlantı optimizasyonu│    │
│  │ ✓ Yüksek erişilebilirlik│    │                        │     │
│  │ ✓ Ölçekleme           │     │                        │     │
│  └────────────────────────┘     └────────────────────────┘     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Desteklenen Veritabanı Motorları

```
┌─────────────────────────────────────────────────────────────────┐
│                    RDS MOTOR SEÇENEKLERİ                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   MySQL     │  │ PostgreSQL  │  │  MariaDB    │             │
│  │  ───────    │  │  ─────────  │  │  ───────    │             │
│  │  Açık kaynak│  │  Açık kaynak│  │  Açık kaynak│             │
│  │  En popüler │  │  Gelişmiş   │  │  MySQL fork │             │
│  │  Web apps   │  │  Enterprise │  │  Uyumlu     │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   Oracle    │  │ SQL Server  │  │   Aurora    │             │
│  │  ───────    │  │  ──────────  │  │  ───────   │             │
│  │  Ticari     │  │  Microsoft  │  │  AWS Native │             │
│  │  Enterprise │  │  Ticari     │  │  Bulut için │             │
│  │  BYOL/Lisans│  │  BYOL/Lisans│  │  optimize   │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│                                                                 │
│  BYOL = Bring Your Own License (Kendi Lisansını Getir)         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### RDS Multi-AZ Deployment

```
┌─────────────────────────────────────────────────────────────────┐
│                    RDS MULTI-AZ YAPISI                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│              Region (eu-west-1)                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                                                         │   │
│  │   AZ-a                         AZ-b                     │   │
│  │   ┌─────────────┐             ┌─────────────┐          │   │
│  │   │   PRIMARY   │  Senkron    │  STANDBY    │          │   │
│  │   │   (Master)  │  Replikasyon│  (Replica)  │          │   │
│  │   │             │◄───────────►│             │          │   │
│  │   │  ┌───────┐  │             │  ┌───────┐  │          │   │
│  │   │  │  DB   │  │             │  │  DB   │  │          │   │
│  │   │  └───────┘  │             │  └───────┘  │          │   │
│  │   └──────▲──────┘             └─────────────┘          │   │
│  │          │                           ▲                  │   │
│  │          │ Yazma/Okuma               │ Otomatik         │   │
│  │          │                           │ Failover         │   │
│  │   ┌──────┴──────┐                    │                  │   │
│  │   │ Uygulama    │────────────────────┘                  │   │
│  │   │ (Endpoint)  │  Primary arızalanırsa                 │   │
│  │   └─────────────┘  Standby devreye girer                │   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ÖNEMLİ:                                                       │
│  • Multi-AZ = Yüksek Erişilebilirlik (HA) için                │
│  • Standby okuma için KULLANILMAZ                              │
│  • Otomatik failover: 60-120 saniye                           │
│  • DNS endpoint değişmez                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### RDS Read Replicas

```
┌─────────────────────────────────────────────────────────────────┐
│                    RDS READ REPLICAS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                     ┌─────────────┐                             │
│                     │   PRIMARY   │                             │
│                     │   (Master)  │                             │
│                     │  Yazma+Okuma│                             │
│                     └──────┬──────┘                             │
│                            │                                    │
│               Asenkron Replikasyon                              │
│            ┌───────────────┼───────────────┐                    │
│            │               │               │                    │
│            ▼               ▼               ▼                    │
│     ┌───────────┐   ┌───────────┐   ┌───────────┐              │
│     │  REPLICA  │   │  REPLICA  │   │  REPLICA  │              │
│     │    #1     │   │    #2     │   │    #3     │              │
│     │ Sadece    │   │ Sadece    │   │ Sadece    │              │
│     │ Okuma     │   │ Okuma     │   │ Okuma     │              │
│     └───────────┘   └───────────┘   └───────────┘              │
│        Aynı AZ        Farklı AZ     Farklı Region               │
│                                                                 │
│  FARKLAR:                                                       │
│  ┌──────────────────────┬──────────────────────┐               │
│  │      Multi-AZ        │    Read Replica      │               │
│  ├──────────────────────┼──────────────────────┤               │
│  │ HA (Yedeklilik)      │ Performans (Okuma)   │               │
│  │ Senkron replikasyon  │ Asenkron replikasyon │               │
│  │ Otomatik failover    │ Manuel promote       │               │
│  │ Okuma yapılamaz      │ Okuma yapılır        │               │
│  │ Aynı Region içinde   │ Cross-Region olabilir│               │
│  └──────────────────────┴──────────────────────┘               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### RDS Backup ve Snapshot

```
┌─────────────────────────────────────────────────────────────────┐
│                    RDS YEDEKLEME SEÇENEKLERİ                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Automated Backups (Otomatik Yedekleme)                     │
│  ──────────────────────────────────────────                    │
│  • Varsayılan: 7 gün retention                                 │
│  • Maksimum: 35 gün                                            │
│  • Point-in-time recovery (PITR)                               │
│  • Backup window sırasında I/O askıya alınabilir               │
│                                                                 │
│  2. Manual Snapshots (Manuel Anlık Görüntü)                    │
│  ──────────────────────────────────────────                    │
│  • Kullanıcı tarafından tetiklenir                             │
│  • Silene kadar saklanır                                       │
│  • Farklı Region'a kopyalanabilir                              │
│  • Paylaşılabilir                                              │
│                                                                 │
│  Timeline:                                                      │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Gün1    Gün2    Gün3    Gün4    Gün5    Gün6    Gün7    │  │
│  │  ▼       ▼       ▼       ▼       ▼       ▼       ▼      │  │
│  │  █       █       █       █       █       █       █      │  │
│  │  Otomatik Backup'lar (7 günlük pencere)                 │  │
│  │                    ▲                                     │  │
│  │                    │                                     │  │
│  │              Herhangi bir                                │  │
│  │              noktaya dönüş                               │  │
│  │              (PITR)                                      │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon Aurora

> **Amazon Aurora** - AWS'nin bulut için optimize edilmiş ilişkisel veritabanı.

### Aurora Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                        AMAZON AURORA                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Bulut için sıfırdan tasarlanmış, MySQL ve PostgreSQL         │
│   uyumlu, yüksek performanslı ilişkisel veritabanı"            │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                    PERFORMANS KARŞILAŞTIRMASI              │ │
│  │                                                            │ │
│  │  MySQL Standard:    █████                    100%          │ │
│  │  Aurora:            █████████████████████████ 500%        │ │
│  │                     (5x daha hızlı)                        │ │
│  │                                                            │ │
│  │  PostgreSQL:        █████████                 100%         │ │
│  │  Aurora:            ███████████████████████████ 300%      │ │
│  │                     (3x daha hızlı)                        │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Temel Özellikler:                                             │
│  ✓ 5x MySQL, 3x PostgreSQL performansı                        │
│  ✓ Otomatik 6 kopya (3 AZ'de)                                 │
│  ✓ 10GB - 128TB otomatik ölçekleme                           │
│  ✓ 15 Read Replica desteği                                    │
│  ✓ Anlık failover (<30 saniye)                                │
│  ✓ Continuous backup to S3                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Aurora Mimarisi

```
┌─────────────────────────────────────────────────────────────────┐
│                    AURORA MİMARİSİ                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                    Region                                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                                                         │   │
│  │  ┌──────────────────────────────────────────────────┐  │   │
│  │  │              AURORA CLUSTER                       │  │   │
│  │  │                                                   │  │   │
│  │  │   ┌─────────────┐     ┌─────────────────────┐    │  │   │
│  │  │   │   PRIMARY   │     │   READ REPLICAS     │    │  │   │
│  │  │   │   WRITER    │     │  (15 adete kadar)   │    │  │   │
│  │  │   │             │     │  ┌────┐ ┌────┐      │    │  │   │
│  │  │   │  Yazma+Okuma│     │  │ R1 │ │ R2 │ ...  │    │  │   │
│  │  │   └──────┬──────┘     │  └────┘ └────┘      │    │  │   │
│  │  │          │            └─────────┬───────────┘    │  │   │
│  │  │          │                      │                │  │   │
│  │  │          └──────────┬───────────┘                │  │   │
│  │  │                     │                            │  │   │
│  │  │          ┌──────────▼──────────┐                 │  │   │
│  │  │          │  SHARED STORAGE     │                 │  │   │
│  │  │          │  (Cluster Volume)   │                 │  │   │
│  │  │          │                     │                 │  │   │
│  │  │          │  Auto-replicating   │                 │  │   │
│  │  │          │  Self-healing       │                 │  │   │
│  │  │          │  10GB - 128TB       │                 │  │   │
│  │  │          └─────────────────────┘                 │  │   │
│  │  │                     │                            │  │   │
│  │  │     ┌───────────────┼───────────────┐            │  │   │
│  │  │     ▼               ▼               ▼            │  │   │
│  │  │  ┌─────┐         ┌─────┐         ┌─────┐        │  │   │
│  │  │  │ AZ-a│         │ AZ-b│         │ AZ-c│        │  │   │
│  │  │  │ ██  │         │ ██  │         │ ██  │        │  │   │
│  │  │  │     │         │     │         │     │        │  │   │
│  │  │  └─────┘         └─────┘         └─────┘        │  │   │
│  │  │  2 kopya         2 kopya         2 kopya        │  │   │
│  │  │                                                  │  │   │
│  │  └──────────────────────────────────────────────────┘  │   │
│  │                                                         │   │
│  │  6 kopya = 3 AZ x 2 kopya                               │   │
│  │  4/6 yazma için, 3/6 okuma için yeterli                 │   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Aurora Serverless

```
┌─────────────────────────────────────────────────────────────────┐
│                    AURORA SERVERLESS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Öngörülemeyen veya aralıklı iş yükleri için"                 │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  Kapasite                                                  │ │
│  │     ▲                                                      │ │
│  │     │         ┌──────┐                                     │ │
│  │     │        ╱        ╲                                    │ │
│  │     │       ╱          ╲         ┌────┐                    │ │
│  │     │      ╱            ╲       ╱      ╲                   │ │
│  │     │  ───╱──────────────╲─────╱────────╲──────            │ │
│  │     │                                                      │ │
│  │     │   Min ACU            Otomatik         Max ACU        │ │
│  │     └──────────────────────────────────────────────►       │ │
│  │                        Zaman                               │ │
│  │                                                            │ │
│  │  ACU = Aurora Capacity Units                               │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  ✓ Otomatik ölçekleme (0.5 - 128 ACU)                        │
│  ✓ Kullanılmadığında duraklatılabilir (pause)                 │
│  ✓ Saniye bazlı ücretlendirme                                 │
│  ✓ Kapasite yönetimi gerekmez                                 │
│                                                                 │
│  Kullanım Senaryoları:                                         │
│  • Geliştirme ve test ortamları                               │
│  • Aralıklı kullanılan uygulamalar                            │
│  • Öngörülemeyen trafik                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Aurora Global Database

```
┌─────────────────────────────────────────────────────────────────┐
│                    AURORA GLOBAL DATABASE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │     PRIMARY REGION              SECONDARY REGIONS        │   │
│  │       (us-east-1)              (eu-west-1, ap-southeast) │   │
│  │                                                          │   │
│  │    ┌─────────────┐            ┌─────────────┐           │   │
│  │    │   PRIMARY   │            │  SECONDARY  │           │   │
│  │    │   CLUSTER   │    <1s     │   CLUSTER   │           │   │
│  │    │             │◄──────────►│             │           │   │
│  │    │  Writer +   │  Replik    │  Read Only  │           │   │
│  │    │  Readers    │            │  Readers    │           │   │
│  │    └─────────────┘            └─────────────┘           │   │
│  │          │                           │                   │   │
│  │          ▼                           ▼                   │   │
│  │    ┌─────────────┐            ┌─────────────┐           │   │
│  │    │   Storage   │            │   Storage   │           │   │
│  │    │   Volume    │            │   Volume    │           │   │
│  │    └─────────────┘            └─────────────┘           │   │
│  │                                                          │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  Özellikler:                                                   │
│  • 1 saniyeden kısa replikasyon gecikmesi                     │
│  • 5 Secondary Region'a kadar                                  │
│  • Her Region'da 16 Read Replica                               │
│  • Cross-Region disaster recovery                              │
│  • Global reads için düşük latency                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon DynamoDB

> **Amazon DynamoDB** - Tam yönetilen NoSQL key-value ve document veritabanı.

### DynamoDB Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                       AMAZON DYNAMODB                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Tam yönetilen, serverless, NoSQL veritabanı"                 │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                      TEMEL ÖZELLİKLER                      │ │
│  ├───────────────────────────────────────────────────────────┤ │
│  │                                                            │ │
│  │  • Milisaniye altı gecikme (single-digit millisecond)      │ │
│  │  • Saniyede milyonlarca istek                              │ │
│  │  • Otomatik ölçekleme                                      │ │
│  │  • Şemasız (Schema-less)                                   │ │
│  │  • Serverless - altyapı yönetimi yok                      │ │
│  │  • 3 AZ'de otomatik replikasyon                           │ │
│  │  • Built-in güvenlik, yedekleme, geri yükleme             │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Veri Modeli:                                                   │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Tablo: Users                                              │ │
│  │  ┌─────────────┬──────────────┬──────────────────────┐    │ │
│  │  │ Partition   │  Sort Key    │    Attributes        │    │ │
│  │  │ Key (PK)    │  (SK) Opt.   │    (Flexible)        │    │ │
│  │  ├─────────────┼──────────────┼──────────────────────┤    │ │
│  │  │ user#123    │ order#001    │ {name, email, ...}   │    │ │
│  │  │ user#123    │ order#002    │ {name, email, ...}   │    │ │
│  │  │ user#456    │ profile      │ {name, age, ...}     │    │ │
│  │  └─────────────┴──────────────┴──────────────────────┘    │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### DynamoDB Kapasite Modelleri

```
┌─────────────────────────────────────────────────────────────────┐
│                    DynamoDB KAPASİTE MODELLERİ                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Provisioned Capacity (Sağlanan Kapasite)                   │
│  ─────────────────────────────────────────────                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  • Önceden belirlenen okuma/yazma kapasitesi              │ │
│  │  • RCU (Read Capacity Units) - Okuma birimi               │ │
│  │  • WCU (Write Capacity Units) - Yazma birimi              │ │
│  │  • Ölçeklenebilir (Auto Scaling ile)                      │ │
│  │  • Öngörülebilir iş yükleri için ideal                    │ │
│  │  • Reserved Capacity ile %75'e kadar indirim              │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  2. On-Demand Capacity (İstek Başına)                          │
│  ─────────────────────────────────────                         │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  • Kapasite planlaması gerekmez                           │ │
│  │  • Otomatik ölçeklenir                                    │ │
│  │  • Kullandığın kadar öde                                  │ │
│  │  • Öngörülemeyen iş yükleri için ideal                    │ │
│  │  • Provisioned'a göre daha pahalı olabilir                │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Karşılaştırma:                                                │
│  ┌────────────────────┬────────────────────┐                   │
│  │    Provisioned     │     On-Demand      │                   │
│  ├────────────────────┼────────────────────┤                   │
│  │ Öngörülebilir      │ Öngörülemeyen      │                   │
│  │ Maliyet optimize   │ Basitlik öncelikli │                   │
│  │ Planlama gerekli   │ Planlama yok       │                   │
│  │ Auto Scaling ekle  │ Otomatik ölçekleme │                   │
│  └────────────────────┴────────────────────┘                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### DynamoDB Global Tables

```
┌─────────────────────────────────────────────────────────────────┐
│                    DynamoDB GLOBAL TABLES                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Multi-Region, multi-active veritabanı replikasyonu"          │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │       us-east-1              eu-west-1             │ │
│  │    ┌─────────────┐        ┌─────────────┐               │ │
│  │    │  DynamoDB   │◄──────►│  DynamoDB   │               │ │
│  │    │   Table     │ 2-way  │   Table     │               │ │
│  │    │  (Active)   │ Replik │  (Active)   │               │ │
│  │    └──────┬──────┘        └──────┬──────┘               │ │
│  │           │                      │                       │ │
│  │    ┌──────▼──────┐        ┌──────▼──────┐               │ │
│  │    │  US Users   │        │  EU Users   │               │ │
│  │    │  Read/Write │        │  Read/Write │               │ │
│  │    └─────────────┘        └─────────────┘               │ │
│  │                                                            │ │
│  │       ap-northeast-1                                       │ │
│  │    ┌─────────────┐                                         │ │
│  │    │  DynamoDB   │                                         │ │
│  │    │   Table     │◄───── Tüm region'lar arasında          │ │
│  │    │  (Active)   │       otomatik senkronizasyon          │ │
│  │    └──────┬──────┘                                         │ │
│  │           │                                                │ │
│  │    ┌──────▼──────┐                                         │ │
│  │    │ APAC Users  │                                         │ │
│  │    │  Read/Write │                                         │ │
│  │    └─────────────┘                                         │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Multi-Region, Multi-Active                                  │
│  • Her region'da yazma yapılabilir                            │
│  • Tipik replikasyon: 1 saniyeden az                          │
│  • DR (Disaster Recovery) için ideal                          │
│  • Global uygulamalar için düşük gecikme                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### DynamoDB Accelerator (DAX)

```
┌─────────────────────────────────────────────────────────────────┐
│                    DynamoDB ACCELERATOR (DAX)                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "DynamoDB için in-memory cache"                               │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  DAX Olmadan:                                              │ │
│  │  ┌──────────┐  Milisaniye  ┌─────────────┐                │ │
│  │  │ Uygulama │─────────────►│  DynamoDB   │                │ │
│  │  └──────────┘              └─────────────┘                │ │
│  │                                                            │ │
│  │  DAX İle:                                                  │ │
│  │  ┌──────────┐ Mikrosaniye ┌─────┐ Cache  ┌─────────────┐ │ │
│  │  │ Uygulama │────────────►│ DAX │ Miss  ►│  DynamoDB   │ │ │
│  │  └──────────┘             └─────┘        └─────────────┘ │ │
│  │                              │                            │ │
│  │                           Cache                           │ │
│  │                            Hit                            │ │
│  │                              ▼                            │ │
│  │                         Sonuç dön                         │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Performans:                                                   │
│  • DynamoDB: Milisaniye (tipik 1-9 ms)                        │
│  • DAX: Mikrosaniye (tipik 400 μs)                            │
│  • 10x performans artışı                                      │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Okuma ağırlıklı iş yükleri                                 │
│  • Aynı verinin tekrar tekrar okunması                        │
│  • Mikrosaniye seviyesinde gecikme gerekliliği                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon ElastiCache

> **Amazon ElastiCache** - Yönetilen in-memory cache servisi.

### ElastiCache Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                      AMAZON ELASTICACHE                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Yönetilen in-memory caching servisi"                         │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │         CACHE KULLANIM SENARYOsu                          │ │
│  │                                                            │ │
│  │  ┌──────────┐    Cache     ┌─────────────┐               │ │
│  │  │          │    Miss?     │             │               │ │
│  │  │ Uygulama │─────────────►│ ElastiCache │               │ │
│  │  │          │◄─────────────│  (Redis/    │               │ │
│  │  └────┬─────┘   Cache Hit  │  Memcached) │               │ │
│  │       │                    └─────────────┘               │ │
│  │       │                                                   │ │
│  │       │ Cache Miss                                        │ │
│  │       ▼                                                   │ │
│  │  ┌──────────┐                                            │ │
│  │  │ Database │  Sonucu cache'e yaz                        │ │
│  │  │  (RDS)   │  ve uygulamaya dön                        │ │
│  │  └──────────┘                                            │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Faydaları:                                                    │
│  • Veritabanı yükünü azaltır                                  │
│  • Mikrosaniye seviyesinde gecikme                            │
│  • Uygulamanın ölçeklenmesini kolaylaştırır                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Redis vs Memcached

```
┌─────────────────────────────────────────────────────────────────┐
│                    REDIS vs MEMCACHED                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────┬─────────────────────────┐         │
│  │         REDIS           │       MEMCACHED         │         │
│  ├─────────────────────────┼─────────────────────────┤         │
│  │ Zengin veri tipleri     │ Sadece string key-value │         │
│  │ (string, list, set,     │                         │         │
│  │  sorted set, hash)      │                         │         │
│  ├─────────────────────────┼─────────────────────────┤         │
│  │ Persistence desteği     │ Persistence yok         │         │
│  │ (AOF, RDB snapshots)    │ (sadece cache)          │         │
│  ├─────────────────────────┼─────────────────────────┤         │
│  │ Replikasyon + failover  │ Multi-threaded          │         │
│  │ (Multi-AZ)              │                         │         │
│  ├─────────────────────────┼─────────────────────────┤         │
│  │ Pub/Sub messaging       │ Basit caching           │         │
│  ├─────────────────────────┼─────────────────────────┤         │
│  │ Cluster mode            │ Cluster mode            │         │
│  │ (sharding)              │ (sharding)              │         │
│  ├─────────────────────────┼─────────────────────────┤         │
│  │ Geospatial desteği      │ Geospatial yok          │         │
│  └─────────────────────────┴─────────────────────────┘         │
│                                                                 │
│  Seçim Rehberi:                                                │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │ Redis Seç:                   │ Memcached Seç:             │ │
│  │ • Karmaşık veri yapıları     │ • Basit caching            │ │
│  │ • Yedeklilik/persistence     │ • Çok thread'li işlem      │ │
│  │ • Pub/Sub gerekli            │ • Basit key-value yeterli  │ │
│  │ • HA (High Availability)     │ • Maliyet optimize         │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon Redshift

> **Amazon Redshift** - Bulut veri ambarı (Data Warehouse) servisi.

### Redshift Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                       AMAZON REDSHIFT                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Petabayt ölçeğinde veri ambarı (Data Warehouse)"             │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                    OLAP vs OLTP                            │ │
│  │                                                            │ │
│  │  OLTP (RDS, Aurora)        OLAP (Redshift)                │ │
│  │  ┌─────────────────┐       ┌─────────────────┐            │ │
│  │  │ Online Trans.   │       │ Online Analytic │            │ │
│  │  │ Processing      │       │ Processing      │            │ │
│  │  │                 │       │                 │            │ │
│  │  │ • Küçük işlemler│       │ • Büyük sorgular│            │ │
│  │  │ • Çok kullanıcı │       │ • Karmaşık      │            │ │
│  │  │ • Yazma ağırlık │       │   analizler     │            │ │
│  │  │ • Satır bazlı   │       │ • Okuma ağırlık │            │ │
│  │  │                 │       │ • Sütun bazlı   │            │ │
│  │  └─────────────────┘       └─────────────────┘            │ │
│  │                                                            │ │
│  │  Örnek:                    Örnek:                         │ │
│  │  "Sipariş oluştur"         "Geçen yılın satış analizi"    │ │
│  │  "Ürün güncelle"           "Müşteri segmentasyonu"        │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Redshift Özellikleri:                                         │
│  • Columnar storage (sütun bazlı depolama)                    │
│  • Paralel sorgu işleme (MPP)                                 │
│  • Petabayt ölçeğinde veri                                    │
│  • SQL uyumlu                                                  │
│  • S3'ten veri yükleme (COPY komutu)                          │
│  • BI araçları ile entegrasyon                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Redshift Mimarisi

```
┌─────────────────────────────────────────────────────────────────┐
│                    REDSHIFT MİMARİSİ                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                   REDSHIFT CLUSTER                         │ │
│  │                                                            │ │
│  │  ┌─────────────────────────────────────────────────────┐  │ │
│  │  │                  LEADER NODE                         │  │ │
│  │  │                                                      │  │ │
│  │  │  • SQL bağlantılarını kabul eder                    │  │ │
│  │  │  • Sorgu planlaması yapar                           │  │ │
│  │  │  • Sonuçları birleştirir                           │  │ │
│  │  │                                                      │  │ │
│  │  └──────────────────────┬──────────────────────────────┘  │ │
│  │                         │                                  │ │
│  │           ┌─────────────┼─────────────┐                   │ │
│  │           │             │             │                   │ │
│  │           ▼             ▼             ▼                   │ │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐         │ │
│  │  │   COMPUTE   │ │   COMPUTE   │ │   COMPUTE   │         │ │
│  │  │    NODE 1   │ │    NODE 2   │ │    NODE N   │         │ │
│  │  │             │ │             │ │             │         │ │
│  │  │ • Veri sakla│ │ • Veri sakla│ │ • Veri sakla│         │ │
│  │  │ • Sorgu çalş│ │ • Sorgu çalş│ │ • Sorgu çalş│         │ │
│  │  │ • Paralel   │ │ • Paralel   │ │ • Paralel   │         │ │
│  │  └─────────────┘ └─────────────┘ └─────────────┘         │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Redshift Spectrum:                                            │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  ┌─────────────┐       ┌─────────────┐                    │ │
│  │  │  Redshift   │◄─────►│   Amazon    │                    │ │
│  │  │  Cluster    │ Query │     S3      │                    │ │
│  │  └─────────────┘       └─────────────┘                    │ │
│  │                                                            │ │
│  │  S3'teki veriyi doğrudan sorgula (yüklemeden)             │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Redshift Serverless

```
┌─────────────────────────────────────────────────────────────────┐
│                    REDSHIFT SERVERLESS                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Cluster yönetmeden veri ambarı"                              │
│                                                                 │
│  Geleneksel Redshift:       Redshift Serverless:               │
│  ┌─────────────────┐       ┌─────────────────┐                 │
│  │ ✗ Cluster boyutu│       │ ✓ Otomatik      │                 │
│  │   seçimi        │       │   ölçekleme     │                 │
│  │ ✗ Node yönetimi │       │ ✓ Node yönetimi │                 │
│  │ ✗ Kapasite      │       │   yok           │                 │
│  │   planlama      │       │ ✓ Kullandığın   │                 │
│  │                 │       │   kadar öde     │                 │
│  └─────────────────┘       └─────────────────┘                 │
│                                                                 │
│  Fiyatlandırma:                                                │
│  • RPU (Redshift Processing Units) bazlı                       │
│  • Sorgu çalıştırma süresi + veri tarama                       │
│  • Kullanılmadığında ücret yok                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Diğer Veritabanı Servisleri

### Amazon DocumentDB

```
┌─────────────────────────────────────────────────────────────────┐
│                      AMAZON DOCUMENTDB                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "MongoDB uyumlu document database"                            │
│                                                                 │
│  Özellikler:                                                   │
│  • MongoDB 3.6/4.0 uyumlu                                      │
│  • Tam yönetilen                                               │
│  • 3 AZ'de replikasyon                                         │
│  • 15 read replica                                             │
│  • Storage auto-scaling (10GB - 64TB)                          │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • MongoDB uygulamalarını AWS'e taşıma                         │
│  • Content management                                          │
│  • Profil yönetimi                                             │
│  • Katalog yönetimi                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon Neptune

```
┌─────────────────────────────────────────────────────────────────┐
│                       AMAZON NEPTUNE                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Yönetilen graph database"                                    │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                    GRAPH DATABASE                          │ │
│  │                                                            │ │
│  │        (Ahmet)──────arkadaş──────(Mehmet)                  │ │
│  │           │                          │                      │ │
│  │        beğendi                    takip                     │ │
│  │           │                          │                      │ │
│  │           ▼                          ▼                      │ │
│  │      [Ürün X]                   (Ayşe)                     │ │
│  │           │                          │                      │ │
│  │        kategori                   beğendi                   │ │
│  │           │                          │                      │ │
│  │           ▼                          ▼                      │ │
│  │     <Elektronik>                 [Ürün Y]                  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Sorgu Dilleri:                                                │
│  • Gremlin (Apache TinkerPop)                                 │
│  • SPARQL (W3C RDF)                                           │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Sosyal ağlar                                                │
│  • Öneri motorları                                             │
│  • Fraud detection                                             │
│  • Knowledge graphs                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon Keyspaces

```
┌─────────────────────────────────────────────────────────────────┐
│                     AMAZON KEYSPACES                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Apache Cassandra uyumlu yönetilen veritabanı"                │
│                                                                 │
│  Özellikler:                                                   │
│  • Cassandra Query Language (CQL) uyumlu                       │
│  • Serverless                                                  │
│  • Otomatik ölçekleme                                          │
│  • Multi-Region replikasyon                                    │
│  • 3 AZ'de replikasyon                                         │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Cassandra uygulamalarını taşıma                            │
│  • IoT veri depolama                                           │
│  • Zaman serisi verileri                                       │
│                                                                 │
│  NOT: CLF-C02 sınavı için out-of-scope olabilir               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon QLDB

```
┌─────────────────────────────────────────────────────────────────┐
│                        AMAZON QLDB                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Quantum Ledger Database - Değiştirilemez kayıt defteri"      │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                    LEDGER YAPISI                           │ │
│  │                                                            │ │
│  │  Block 1     Block 2     Block 3     Block 4              │ │
│  │  ┌─────┐    ┌─────┐    ┌─────┐    ┌─────┐                │ │
│  │  │ TX1 │───►│ TX2 │───►│ TX3 │───►│ TX4 │                │ │
│  │  │ TX2 │    │ TX3 │    │ TX4 │    │ TX5 │                │ │
│  │  └─────┘    └─────┘    └─────┘    └─────┘                │ │
│  │                                                            │ │
│  │  Cryptographic Hash Chain                                  │ │
│  │  (Her değişiklik doğrulanabilir)                          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Immutable (değiştirilemez)                                  │
│  • Cryptographically verifiable                                │
│  • Merkezi (Blockchain değil)                                  │
│  • SQL-like PartiQL sorgu dili                                 │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Finansal işlem kayıtları                                   │
│  • Tedarik zinciri takibi                                     │
│  • HR kayıtları                                                │
│  • Düzenleyici uyumluluk                                      │
│                                                                 │
│  NOT: CLF-C02 sınavı için out-of-scope olabilir               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Karşılaştırma Tabloları

### Veritabanı Seçim Matrisi

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        VERİTABANI SEÇİM MATRİSİ                             │
├───────────────┬──────────┬──────────┬──────────┬──────────┬──────────┬─────┤
│   İhtiyaç     │   RDS    │  Aurora  │ DynamoDB │ Redshift │ElastiCache│Doc │
├───────────────┼──────────┼──────────┼──────────┼──────────┼──────────┼─────┤
│ İlişkisel veri│    ✓     │    ✓     │    ✗     │    ✓     │    ✗     │  ✗  │
│ NoSQL         │    ✗     │    ✗     │    ✓     │    ✗     │    ✓     │  ✓  │
│ ACID uyumlu   │    ✓     │    ✓     │   △*     │    ✓     │    ✗     │  ✓  │
│ Serverless    │    ✗     │    ✓     │    ✓     │    ✓     │    ✗     │  ✗  │
│ Multi-AZ HA   │    ✓     │    ✓     │    ✓     │    ✓     │    ✓     │  ✓  │
│ Global Tables │    ✗     │    ✓     │    ✓     │    ✗     │    ✗     │  ✗  │
│ In-Memory     │    ✗     │    ✗     │  DAX     │    ✗     │    ✓     │  ✗  │
│ Analytics     │    ✗     │    ✗     │    ✗     │    ✓     │    ✗     │  ✗  │
│ Max Storage   │  64TB    │  128TB   │ Sınırsız │   PB     │   GB     │ 64TB│
└───────────────┴──────────┴──────────┴──────────┴──────────┴──────────┴─────┘

* DynamoDB Transactions ile ACID benzeri garantiler sağlar
```

### RDS vs Aurora vs Redshift

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      RDS vs AURORA vs REDSHIFT                              │
├─────────────────────┬─────────────────────┬─────────────────────────────────┤
│        RDS          │       Aurora        │           Redshift              │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ İşlemsel (OLTP)     │ İşlemsel (OLTP)     │ Analitik (OLAP)                 │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ 6 motor seçeneği    │ MySQL/PostgreSQL    │ PostgreSQL tabanlı              │
│ (MySQL, PostgreSQL, │ uyumlu              │ (ama farklı)                    │
│ MariaDB, Oracle,    │                     │                                 │
│ SQL Server, Aurora) │                     │                                 │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ 64TB max            │ 128TB auto-scale    │ Petabyte ölçeği                 │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ 5 read replica      │ 15 read replica     │ Compute nodes                   │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ Standart performans │ 5x MySQL            │ MPP (Massively Parallel         │
│                     │ 3x PostgreSQL       │ Processing)                     │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ Multi-AZ için       │ Built-in 6 kopya    │ Cluster bazlı                   │
│ ek yapılandırma     │ (3 AZ)              │ (Leader + Compute)              │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ Geleneksel uyg.     │ Yüksek performanslı │ BI, raporlama,                  │
│ migration için      │ cloud-native uyg.   │ veri analizi için               │
└─────────────────────┴─────────────────────┴─────────────────────────────────┘
```

### Maliyet ve Ölçekleme Karşılaştırması

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      MALİYET VE ÖLÇEKLEME                                   │
├─────────────────────┬───────────────────────────────────────────────────────┤
│      Servis         │                    Fiyatlandırma                      │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ RDS                 │ Instance saat + Storage GB + I/O                      │
│                     │ Reserved Instance ile %70 indirim                     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Aurora              │ Instance saat + Storage GB + I/O                      │
│                     │ Serverless: ACU-saniye bazlı                          │
│                     │ Storage I/O optimize edilmiş                          │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ DynamoDB            │ Provisioned: RCU/WCU saat                             │
│                     │ On-Demand: İstek başına                               │
│                     │ Reserved ile %75 indirim                              │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Redshift            │ Node saat + Storage                                   │
│                     │ Serverless: RPU bazlı                                 │
│                     │ Reserved ile %75 indirim                              │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ ElastiCache         │ Node saat (Redis/Memcached)                           │
│                     │ Reserved ile indirim                                  │
└─────────────────────┴───────────────────────────────────────────────────────┘
```

---

## Sınav Senaryoları

### Senaryo 1: E-ticaret Uygulaması

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Bir e-ticaret şirketi sipariş yönetimi için veritabanı   │
│ seçiyor. İlişkisel veri yapısı, yüksek erişilebilirlik ve      │
│ yüksek performans gerekiyor. Hangi servis?                     │
├─────────────────────────────────────────────────────────────────┤
│ A) DynamoDB                                                     │
│ B) Aurora                                                       │
│ C) Redshift                                                     │
│ D) ElastiCache                                                  │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) Aurora                                                │
│                                                                 │
│ Neden?                                                          │
│ • "İlişkisel veri" = RDS veya Aurora                           │
│ • "Yüksek performans" = Aurora (5x MySQL)                      │
│ • "Yüksek erişilebilirlik" = Aurora (6 kopya, 3 AZ)            │
│ • DynamoDB NoSQL, Redshift analytics, ElastiCache cache        │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 2: Mobil Oyun

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Bir mobil oyun şirketi, milyonlarca oyuncunun skorlarını │
│ milisaniye altı gecikme ile saklamak istiyor. Yönetim          │
│ overhead'i minimumda tutmak isteniyor. Hangi servis?           │
├─────────────────────────────────────────────────────────────────┤
│ A) RDS Multi-AZ                                                 │
│ B) Aurora Serverless                                            │
│ C) DynamoDB                                                     │
│ D) Redshift                                                     │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: C) DynamoDB                                              │
│                                                                 │
│ Neden?                                                          │
│ • "Milisaniye altı gecikme" = DynamoDB single-digit ms         │
│ • "Milyonlarca kullanıcı" = DynamoDB ölçeklenebilirlik         │
│ • "Yönetim overhead minimum" = DynamoDB serverless             │
│ • Skor tablosu = key-value yapısı için ideal                   │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 3: İş Zekası

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Bir şirket, 5 yıllık satış verilerini analiz etmek için  │
│ SQL tabanlı bir çözüm arıyor. Veriler terabayt boyutunda ve    │
│ karmaşık raporlar oluşturmak isteniyor. Hangi servis?          │
├─────────────────────────────────────────────────────────────────┤
│ A) RDS                                                          │
│ B) Aurora                                                       │
│ C) DynamoDB                                                     │
│ D) Redshift                                                     │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: D) Redshift                                              │
│                                                                 │
│ Neden?                                                          │
│ • "Analiz" = OLAP = Redshift                                   │
│ • "Terabayt boyutunda" = Redshift (petabyte ölçeği)            │
│ • "SQL tabanlı" = Redshift SQL uyumlu                          │
│ • "Karmaşık raporlar" = Redshift veri ambarı                   │
│ • RDS/Aurora işlemsel (OLTP), DynamoDB NoSQL                   │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 4: Session Caching

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Web uygulaması veritabanı sorgularını azaltmak ve        │
│ kullanıcı oturumlarını saklamak için in-memory çözüm arıyor.   │
│ Hangi servis?                                                   │
├─────────────────────────────────────────────────────────────────┤
│ A) RDS                                                          │
│ B) DynamoDB                                                     │
│ C) ElastiCache                                                  │
│ D) Redshift                                                     │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: C) ElastiCache                                           │
│                                                                 │
│ Neden?                                                          │
│ • "In-memory" = ElastiCache (Redis/Memcached)                  │
│ • "Session storage" = ElastiCache tipik kullanım alanı         │
│ • "Sorgu azaltma" = Cache kullanım amacı                       │
│ • DynamoDB DAX da in-memory ama sadece DynamoDB için           │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 5: Cross-Region DR

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Küresel bir uygulama için veritabanı gerekiyor. Her      │
│ region'da hem okuma hem yazma yapılabilmeli ve otomatik        │
│ senkronizasyon sağlanmalı. Hangi çözüm?                        │
├─────────────────────────────────────────────────────────────────┤
│ A) RDS Multi-AZ                                                 │
│ B) Aurora Global Database                                       │
│ C) DynamoDB Global Tables                                       │
│ D) ElastiCache Global Datastore                                 │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: C) DynamoDB Global Tables                                │
│                                                                 │
│ Neden?                                                          │
│ • "Her region'da hem okuma hem yazma" = Multi-Active            │
│ • Aurora Global DB = bir region'da yazma (diğerleri read-only) │
│ • DynamoDB Global Tables = Multi-Region, Multi-Active          │
│ • RDS Multi-AZ = Aynı region içi                               │
│                                                                 │
│ NOT: Eğer soru "relational" deseydi Aurora Global DB olurdu    │
│ ama "her region'da yazma" dendiği için DynamoDB Global Tables  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Özet Kartları

### Hızlı Referans

```
┌─────────────────────────────────────────────────────────────────┐
│                    HIZLI REFERANS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  İLİŞKİSEL + STANDART        → RDS                             │
│  İLİŞKİSEL + YÜKSEK PERF.    → Aurora                          │
│  NOSQL + SERVERLESS          → DynamoDB                        │
│  ANALİTİK + BİG DATA         → Redshift                        │
│  IN-MEMORY CACHE             → ElastiCache                     │
│  DOCUMENT + MONGODB          → DocumentDB                      │
│  GRAPH                       → Neptune                         │
│                                                                 │
│  ─────────────────────────────────────────────────────         │
│                                                                 │
│  OLTP = Online Transaction Processing = RDS, Aurora            │
│  OLAP = Online Analytical Processing = Redshift                │
│                                                                 │
│  ─────────────────────────────────────────────────────         │
│                                                                 │
│  Multi-AZ = Yüksek Erişilebilirlik (HA) - Failover için       │
│  Read Replica = Okuma Performansı - Ölçekleme için            │
│  Global Tables = Multi-Region Active-Active                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

- [Amazon RDS User Guide](https://docs.aws.amazon.com/rds/)
- [Amazon Aurora User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/)
- [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/dynamodb/)
- [Amazon ElastiCache User Guide](https://docs.aws.amazon.com/elasticache/)
- [Amazon Redshift Getting Started](https://docs.aws.amazon.com/redshift/)
- [CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

> **Sınav İpucu:** Veritabanı sorularında önce OLTP vs OLAP ayrımını yap, sonra SQL vs NoSQL'i belirle. Bu iki soru genellikle doğru servisi bulmak için yeterli!

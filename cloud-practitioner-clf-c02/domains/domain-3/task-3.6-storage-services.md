# Task 3.6: AWS Storage Services (Depolama Servisleri)

> **Resmi Tanım:** Identify AWS storage services.
>
> **Kaynak:** [AWS CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

## İçindekiler

1. [Genel Bakış](#genel-bakış)
2. [Depolama Türleri](#depolama-türleri)
3. [Amazon S3](#amazon-s3)
4. [Amazon EBS](#amazon-ebs)
5. [Amazon EFS](#amazon-efs)
6. [Amazon FSx](#amazon-fsx)
7. [AWS Storage Gateway](#aws-storage-gateway)
8. [AWS Snow Family](#aws-snow-family)
9. [AWS Backup](#aws-backup)
10. [Karşılaştırma Tabloları](#karşılaştırma-tabloları)
11. [Sınav Senaryoları](#sınav-senaryoları)

---

## Genel Bakış

AWS, farklı kullanım senaryoları için çeşitli depolama çözümleri sunar. Doğru depolama türünü seçmek, performans, maliyet ve erişilebilirlik açısından kritiktir.

### Depolama Seçim Haritası

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        AWS DEPOLAMA SERVİSLERİ                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌────────────────────────────────────────────────────────────────────────┐│
│  │                       DEPOLAMA TÜRLERİ                                 ││
│  │                                                                        ││
│  │   Object Storage       Block Storage        File Storage              ││
│  │   ┌─────────────┐     ┌─────────────┐      ┌─────────────┐           ││
│  │   │    ████     │     │   █ █ █ █   │      │  📁 📁 📁    │           ││
│  │   │   █    █    │     │   █ █ █ █   │      │  /home/user  │           ││
│  │   │   ██████    │     │   █ █ █ █   │      │  /var/log    │           ││
│  │   │             │     │             │      │             │           ││
│  │   │   Amazon    │     │  Amazon     │      │  Amazon     │           ││
│  │   │     S3      │     │    EBS      │      │  EFS / FSx  │           ││
│  │   └─────────────┘     └─────────────┘      └─────────────┘           ││
│  │                                                                        ││
│  │   Web üzerinden      Disk gibi            Dosya sistemi              ││
│  │   HTTP/S erişim      (EC2'ye bağlı)       (Paylaşımlı)               ││
│  │                                                                        ││
│  └────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
│  ┌────────────────────────────────────────────────────────────────────────┐│
│  │                    HİBRİT VE TRANSFER                                   ││
│  │                                                                        ││
│  │   ┌─────────────┐     ┌─────────────┐      ┌─────────────┐           ││
│  │   │  Storage    │     │    Snow     │      │   AWS       │           ││
│  │   │  Gateway    │     │   Family    │      │   Backup    │           ││
│  │   │             │     │             │      │             │           ││
│  │   │ On-prem ↔   │     │ Fiziksel    │      │ Merkezi     │           ││
│  │   │ AWS köprüsü │     │ veri        │      │ yedekleme   │           ││
│  │   │             │     │ transferi   │      │             │           ││
│  │   └─────────────┘     └─────────────┘      └─────────────┘           ││
│  │                                                                        ││
│  └────────────────────────────────────────────────────────────────────────┘│
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Depolama Türleri

### Object vs Block vs File Storage

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         DEPOLAMA TÜRLERİ KARŞILAŞTIRMA                      │
├──────────────────────┬───────────────────┬───────────────────┬──────────────┤
│                      │  Object Storage   │  Block Storage    │ File Storage │
├──────────────────────┼───────────────────┼───────────────────┼──────────────┤
│ AWS Servisi          │ S3                │ EBS               │ EFS, FSx     │
├──────────────────────┼───────────────────┼───────────────────┼──────────────┤
│ Veri Yapısı          │ Nesneler          │ Bloklar (sabit)   │ Dosya/Klasör │
│                      │ (key-value)       │                   │ hiyerarşisi  │
├──────────────────────┼───────────────────┼───────────────────┼──────────────┤
│ Erişim               │ HTTP/S API        │ EC2 instance'a    │ NFS/SMB      │
│                      │ (REST)            │ mount             │ protokolü    │
├──────────────────────┼───────────────────┼───────────────────┼──────────────┤
│ Ölçekleme            │ Sınırsız          │ Volume boyutu     │ Otomatik     │
│                      │ (otomatik)        │ (manuel)          │              │
├──────────────────────┼───────────────────┼───────────────────┼──────────────┤
│ Paylaşım             │ Web üzerinden     │ Tek EC2           │ Çoklu EC2    │
│                      │ herkes            │ (aynı anda)       │ (aynı anda)  │
├──────────────────────┼───────────────────┼───────────────────┼──────────────┤
│ Değiştirme           │ Tüm nesne         │ Blok seviyesinde  │ Dosya        │
│                      │ yeniden yazılır   │ güncelleme        │ seviyesinde  │
├──────────────────────┼───────────────────┼───────────────────┼──────────────┤
│ Kullanım             │ Statik içerik     │ İşletim sistemi   │ Paylaşımlı   │
│                      │ Backup, Arşiv     │ Veritabanı        │ dosyalar     │
│                      │ Büyük veri        │ Boot volume       │ CMS, web     │
└──────────────────────┴───────────────────┴───────────────────┴──────────────┘
```

---

## Amazon S3

> **Amazon Simple Storage Service (S3)** - Nesne depolama servisi.

### S3 Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                         AMAZON S3                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Sınırsız ölçeklenebilir nesne depolama"                      │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                        S3 BUCKET                           │ │
│  │                     (my-bucket-name)                       │ │
│  │                                                            │ │
│  │   ┌─────────────────────────────────────────────────────┐ │ │
│  │   │                     OBJECTS                          │ │ │
│  │   │                                                      │ │ │
│  │   │  ┌──────────────┐  ┌──────────────┐                 │ │ │
│  │   │  │   image.jpg  │  │   data.csv   │                 │ │ │
│  │   │  │ Key: images/ │  │ Key: data/   │                 │ │ │
│  │   │  │    photo.jpg │  │    file.csv  │                 │ │ │
│  │   │  │ Value: binary│  │ Value: text  │                 │ │ │
│  │   │  │ Metadata:    │  │ Metadata:    │                 │ │ │
│  │   │  │  - size      │  │  - size      │                 │ │ │
│  │   │  │  - type      │  │  - type      │                 │ │ │
│  │   │  └──────────────┘  └──────────────┘                 │ │ │
│  │   │                                                      │ │ │
│  │   │  Max object size: 5TB                               │ │ │
│  │   │  Single PUT limit: 5GB (multipart için)             │ │ │
│  │   │                                                      │ │ │
│  │   └─────────────────────────────────────────────────────┘ │ │
│  │                                                            │ │
│  │  Bucket Rules:                                            │ │
│  │  • İsim globally unique (tüm AWS'de benzersiz)           │ │
│  │  • Region'a özel oluşturulur                              │ │
│  │  • DNS uyumlu isim (küçük harf, tire, rakam)             │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Temel Özellikler:                                             │
│  • %99.999999999 (11 9's) dayanıklılık                        │
│  • %99.99 erişilebilirlik (Standard)                          │
│  • Otomatik 3+ AZ replikasyonu                                │
│  • Sınırsız depolama                                           │
│  • Versioning desteği                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### S3 Storage Classes

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          S3 STORAGE CLASSES                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    SIKLIK ve MALİYET MATRİSİ                         │   │
│  │                                                                      │   │
│  │  Erişim     Maliyet                                                 │   │
│  │  Sıklığı    (Storage)                                               │   │
│  │     ▲                                                                │   │
│  │     │                                                                │   │
│  │ SIK │  S3 Standard                                                  │   │
│  │     │  ────────────                                                 │   │
│  │     │  • Sık erişim                                                 │   │
│  │     │  • En yüksek maliyet                                          │   │
│  │     │  • Retrieval ücreti yok                                       │   │
│  │     │                                                                │   │
│  │     │  S3 Intelligent-Tiering                                       │   │
│  │     │  ─────────────────────────                                    │   │
│  │     │  • Otomatik katman değiştirme                                 │   │
│  │     │  • Monitoring ücreti var                                      │   │
│  │     │  • Retrieval ücreti yok                                       │   │
│  │     │                                                                │   │
│  │ ARA │  S3 Standard-IA (Infrequent Access)                          │   │
│  │ SIRA│  ─────────────────────────────────────                        │   │
│  │     │  • Nadir erişim (30+ gün)                                     │   │
│  │     │  • Düşük depolama ücreti                                      │   │
│  │     │  • Retrieval ücreti var                                       │   │
│  │     │  • Min 30 gün ücretlendirme                                   │   │
│  │     │                                                                │   │
│  │     │  S3 One Zone-IA                                               │   │
│  │     │  ─────────────────                                            │   │
│  │     │  • Tek AZ (daha düşük dayanıklılık)                          │   │
│  │     │  • En düşük IA fiyatı                                         │   │
│  │     │  • Yeniden oluşturulabilir veriler için                      │   │
│  │     │                                                                │   │
│  │NADİR│  S3 Glacier Instant Retrieval                                │   │
│  │     │  ───────────────────────────────                              │   │
│  │     │  • Milisaniye erişim                                          │   │
│  │     │  • Çeyreklik erişilen arşiv                                  │   │
│  │     │  • Min 90 gün                                                 │   │
│  │     │                                                                │   │
│  │     │  S3 Glacier Flexible Retrieval                               │   │
│  │     │  ─────────────────────────────────                            │   │
│  │     │  • Dakikalar - 12 saat erişim                                │   │
│  │     │  • Yıllık erişim için                                        │   │
│  │     │  • Min 90 gün                                                 │   │
│  │     │                                                                │   │
│  │ ÇOK │  S3 Glacier Deep Archive                                     │   │
│  │NADİR│  ────────────────────────────                                 │   │
│  │     │  • 12-48 saat erişim                                          │   │
│  │     │  • En düşük maliyet                                           │   │
│  │     │  • Uzun vadeli arşiv                                          │   │
│  │     │  • Min 180 gün                                                │   │
│  │     ▼                                                                │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### S3 Storage Classes Detaylı Karşılaştırma

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                              S3 STORAGE CLASSES KARŞILAŞTIRMA                               │
├─────────────────────┬──────────┬─────────────┬───────────────┬──────────────┬───────────────┤
│ Storage Class       │ AZ Sayısı│ Dayanıklılık│ Erişilebilirlik│ Min Süre    │ Retrieval     │
├─────────────────────┼──────────┼─────────────┼───────────────┼──────────────┼───────────────┤
│ Standard            │ ≥3       │ 11 9's      │ 99.99%        │ Yok         │ Ücretsiz      │
├─────────────────────┼──────────┼─────────────┼───────────────┼──────────────┼───────────────┤
│ Intelligent-Tiering │ ≥3       │ 11 9's      │ 99.9%         │ Yok         │ Ücretsiz      │
├─────────────────────┼──────────┼─────────────┼───────────────┼──────────────┼───────────────┤
│ Standard-IA         │ ≥3       │ 11 9's      │ 99.9%         │ 30 gün      │ GB başına     │
├─────────────────────┼──────────┼─────────────┼───────────────┼──────────────┼───────────────┤
│ One Zone-IA         │ 1        │ 11 9's      │ 99.5%         │ 30 gün      │ GB başına     │
├─────────────────────┼──────────┼─────────────┼───────────────┼──────────────┼───────────────┤
│ Glacier Instant     │ ≥3       │ 11 9's      │ 99.9%         │ 90 gün      │ GB başına     │
├─────────────────────┼──────────┼─────────────┼───────────────┼──────────────┼───────────────┤
│ Glacier Flexible    │ ≥3       │ 11 9's      │ 99.99%        │ 90 gün      │ GB başına     │
│                     │          │             │ (restored)    │             │ + request     │
├─────────────────────┼──────────┼─────────────┼───────────────┼──────────────┼───────────────┤
│ Glacier Deep Archive│ ≥3       │ 11 9's      │ 99.99%        │ 180 gün     │ GB başına     │
│                     │          │             │ (restored)    │             │ + request     │
└─────────────────────┴──────────┴─────────────┴───────────────┴──────────────┴───────────────┘

11 9's = %99.999999999 = 10,000 obje için 10,000 yılda 1 kayıp
```

### S3 Lifecycle Rules

```
┌─────────────────────────────────────────────────────────────────┐
│                     S3 LIFECYCLE RULES                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Nesneleri otomatik olarak farklı storage class'lara taşı     │
│   veya sil"                                                     │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  Gün 0          Gün 30         Gün 90         Gün 365     │ │
│  │    │              │              │              │          │ │
│  │    ▼              ▼              ▼              ▼          │ │
│  │ ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐       │ │
│  │ │  S3  │ ───► │ S3   │ ───► │Glacier│ ───► │ Deep │       │ │
│  │ │Stand.│      │  IA  │      │Flex.  │      │Archive│      │ │
│  │ └──────┘      └──────┘      └──────┘      └──────┘       │ │
│  │                                              │             │ │
│  │                                              ▼             │ │
│  │                                          ┌──────┐          │ │
│  │                           Gün 730+       │DELETE│          │ │
│  │                                          └──────┘          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Transition Actions (Geçiş):                                   │
│  • Standard → IA (30 gün sonra)                               │
│  • IA → Glacier (90 gün sonra)                                │
│  • Glacier → Deep Archive (180 gün sonra)                     │
│                                                                 │
│  Expiration Actions (Silme):                                   │
│  • Nesneyi sil                                                 │
│  • Eski versiyonları sil                                       │
│  • Tamamlanmamış multipart upload'ları sil                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### S3 Güvenlik

```
┌─────────────────────────────────────────────────────────────────┐
│                       S3 GÜVENLİK                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Bucket Policies (Bucket Bazlı)                             │
│  ─────────────────────────────────                             │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  {                                                         │ │
│  │    "Effect": "Allow",                                      │ │
│  │    "Principal": "*",                                       │ │
│  │    "Action": "s3:GetObject",                              │ │
│  │    "Resource": "arn:aws:s3:::my-bucket/*"                 │ │
│  │  }                                                         │ │
│  │                                                            │ │
│  │  Kullanım: Public access, cross-account access            │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  2. IAM Policies (Kullanıcı Bazlı)                             │
│  ─────────────────────────────────                             │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  IAM User/Role'a S3 izinleri ata                          │ │
│  │  Örnek: AmazonS3FullAccess, AmazonS3ReadOnlyAccess        │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  3. Access Control Lists (ACL) - Eski Yöntem                   │
│  ─────────────────────────────────────────────                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  • Bucket ve object seviyesinde                           │ │
│  │  • Artık önerilmiyor (Bucket Policies kullan)             │ │
│  │  • Varsayılan: ACL devre dışı (2023+)                     │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  4. Block Public Access                                        │
│  ─────────────────────                                         │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  ✓ Block all public access (varsayılan AÇIK)              │ │
│  │                                                            │ │
│  │  Account seviyesinde veya bucket seviyesinde              │ │
│  │  Public bucket'lar için KAPALI olmalı                     │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  5. Encryption                                                 │
│  ──────────────                                                │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  Server-Side Encryption (SSE):                            │ │
│  │  • SSE-S3: AWS yönetilen anahtarlar (varsayılan)         │ │
│  │  • SSE-KMS: AWS KMS ile (audit, key yönetimi)            │ │
│  │  • SSE-C: Müşteri sağladığı anahtarlar                   │ │
│  │                                                            │ │
│  │  Client-Side Encryption:                                  │ │
│  │  • Müşteri tarafında şifreleme                           │ │
│  │                                                            │ │
│  │  Varsayılan: SSE-S3 (2023+)                               │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### S3 Versioning

```
┌─────────────────────────────────────────────────────────────────┐
│                       S3 VERSIONING                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Aynı anahtarla birden fazla versiyon sakla"                  │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  Key: photo.jpg                                            │ │
│  │                                                            │ │
│  │  Version 3 (current) ←── En güncel                        │ │
│  │       │                                                    │ │
│  │  Version 2                                                 │ │
│  │       │                                                    │ │
│  │  Version 1                                                 │ │
│  │                                                            │ │
│  │  GET photo.jpg → Version 3 döner                          │ │
│  │  GET photo.jpg?versionId=v1 → Version 1 döner             │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Silme Davranışı:                                              │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  DELETE photo.jpg                                          │ │
│  │       │                                                    │ │
│  │       ▼                                                    │ │
│  │  Delete Marker (yeni "versiyon")                          │ │
│  │       │                                                    │ │
│  │  Version 3                                                 │ │
│  │  Version 2                                                 │ │
│  │  Version 1                                                 │ │
│  │                                                            │ │
│  │  Gerçek silme için: DELETE photo.jpg?versionId=v3         │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Faydaları:                                                    │
│  • Yanlışlıkla silme koruması                                 │
│  • Önceki versiyonlara geri dönüş                             │
│  • MFA Delete ile ek güvenlik                                 │
│                                                                 │
│  ÖNEMLİ:                                                       │
│  • Versioning açıldıktan sonra kapatılamaz (sadece askıya)    │
│  • Tüm versiyonlar için depolama ücreti                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### S3 Replication

```
┌─────────────────────────────────────────────────────────────────┐
│                       S3 REPLICATION                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Cross-Region Replication (CRR)                             │
│  ─────────────────────────────────                             │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Region A                          Region B               │ │
│  │   (us-east-1)                       (eu-west-1)           │ │
│  │   ┌─────────────┐                  ┌─────────────┐        │ │
│  │   │   Source    │  Asynchronous   │ Destination │        │ │
│  │   │   Bucket    │ ──────────────► │   Bucket    │        │ │
│  │   │             │   Replication   │             │        │ │
│  │   └─────────────┘                  └─────────────┘        │ │
│  │                                                            │ │
│  │   Kullanım:                                               │ │
│  │   • Compliance (farklı coğrafyada kopya)                  │ │
│  │   • Düşük latency erişim                                  │ │
│  │   • DR (Disaster Recovery)                                │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  2. Same-Region Replication (SRR)                              │
│  ─────────────────────────────────                             │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Region A (us-east-1)                                    │ │
│  │   ┌─────────────┐                  ┌─────────────┐        │ │
│  │   │   Source    │ ──────────────► │ Destination │        │ │
│  │   │   Bucket    │   Replication   │   Bucket    │        │ │
│  │   │  (Prod)     │                 │   (Test)    │        │ │
│  │   └─────────────┘                  └─────────────┘        │ │
│  │                                                            │ │
│  │   Kullanım:                                               │ │
│  │   • Log aggregation                                       │ │
│  │   • Test ortamına kopya                                   │ │
│  │   • Hesaplar arası kopya                                  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Gereksinimler:                                                │
│  • Her iki bucket'ta versioning AÇIK olmalı                   │
│  • Kaynak ve hedef farklı bucket olmalı                       │
│  • Uygun IAM izinleri                                          │
│                                                                 │
│  NOT: Sadece yeni objeler replicate edilir                    │
│       (var olanlar için S3 Batch Replication)                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### S3 Kullanım Senaryoları

```
┌─────────────────────────────────────────────────────────────────┐
│                  S3 KULLANIM SENARYOLARI                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────────┬────────────────────────────────┐ │
│  │       Senaryo            │          Storage Class         │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Web sitesi hosting       │ S3 Standard                    │ │
│  │ (static website)         │                                │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Sık erişilen dosyalar    │ S3 Standard                    │ │
│  │ (images, CSS, JS)        │                                │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Erişim paterni belirsiz  │ S3 Intelligent-Tiering        │ │
│  │                          │                                │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Aylık raporlar           │ S3 Standard-IA                 │ │
│  │ (nadir erişim)           │                                │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Thumbnail (yeniden       │ S3 One Zone-IA                 │ │
│  │ oluşturulabilir)         │                                │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Medikal görüntüler       │ S3 Glacier Instant Retrieval   │ │
│  │ (çeyreklik erişim)       │                                │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Yıllık arşiv             │ S3 Glacier Flexible Retrieval  │ │
│  │ (saatler içinde erişim)  │                                │ │
│  ├──────────────────────────┼────────────────────────────────┤ │
│  │ Uyumluluk arşivi         │ S3 Glacier Deep Archive        │ │
│  │ (7+ yıl saklama)         │                                │ │
│  └──────────────────────────┴────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon EBS

> **Amazon Elastic Block Store (EBS)** - EC2 için blok depolama.

### EBS Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                         AMAZON EBS                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "EC2 instance'lara bağlanan sanal disk"                       │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │       EC2 Instance                                        │ │
│  │       ┌────────────────────────────────────────────┐      │ │
│  │       │                                            │      │ │
│  │       │     ┌──────────────┐  ┌──────────────┐    │      │ │
│  │       │     │  Root Volume │  │  Data Volume │    │      │ │
│  │       │     │   (EBS)      │  │    (EBS)     │    │      │ │
│  │       │     │   /dev/xvda  │  │   /dev/xvdb  │    │      │ │
│  │       │     └──────────────┘  └──────────────┘    │      │ │
│  │       │                                            │      │ │
│  │       └────────────────────────────────────────────┘      │ │
│  │                    │                   │                   │ │
│  │                    │ Network           │                   │ │
│  │                    ▼                   ▼                   │ │
│  │       ┌────────────────┐  ┌────────────────┐              │ │
│  │       │   EBS Volume   │  │   EBS Volume   │              │ │
│  │       │    (gp3)       │  │    (io2)       │              │ │
│  │       │    100 GB      │  │    500 GB      │              │ │
│  │       └────────────────┘  └────────────────┘              │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Temel Özellikler:                                             │
│  • AZ'e özgü (aynı AZ'deki EC2'ye bağlanır)                   │
│  • Network attached (EC2'den bağımsız yaşam döngüsü)          │
│  • Snapshot desteği (S3'e yedekleme)                          │
│  • Şifreleme desteği (rest + transit)                         │
│  • Instance durdurulduğunda veri korunur                      │
│                                                                 │
│  ÖNEMLİ:                                                       │
│  • EBS = Tek AZ                                                │
│  • Başka AZ'e taşımak için: Snapshot → Yeni AZ'de restore    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### EBS Volume Types

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          EBS VOLUME TYPES                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  SSD-based (IOPS ağırlıklı):                                               │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  gp3 (General Purpose SSD)          io2 Block Express               │   │
│  │  ──────────────────────────         ─────────────────────────────   │   │
│  │  • Genel amaçlı, varsayılan         • En yüksek performans          │   │
│  │  • 3,000 IOPS baseline              • 256,000 IOPS                   │   │
│  │  • 125 MB/s throughput              • 99.999% dayanıklılık          │   │
│  │  • IOPS ve throughput bağımsız      • Mission-critical uygulamalar  │   │
│  │  • Boot volume için uygun           • Büyük veritabanları           │   │
│  │                                                                      │   │
│  │  gp2 (Önceki nesil)                 io1 (Önceki nesil)              │   │
│  │  ──────────────────────             ─────────────────────────────   │   │
│  │  • Burst performans                 • io2'nin önceki versiyonu      │   │
│  │  • IOPS = Volume size x 3           • 64,000 IOPS                   │   │
│  │  • gp3'e geçiş önerilir             • io2'ye geçiş önerilir        │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  HDD-based (Throughput ağırlıklı):                                         │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  st1 (Throughput Optimized)         sc1 (Cold HDD)                  │   │
│  │  ────────────────────────────       ─────────────────────────────   │   │
│  │  • Sık erişilen büyük veri          • Nadir erişilen veri           │   │
│  │  • 500 MB/s throughput              • 250 MB/s throughput           │   │
│  │  • Big data, data warehouse         • Arşiv, nadir erişim           │   │
│  │  • Boot volume OLAMAZ               • Boot volume OLAMAZ            │   │
│  │  • En düşük maliyet (HDD)           • En düşük maliyet              │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  Özet Tablo:                                                               │
│  ┌────────────┬──────────────┬─────────────┬──────────────┬─────────────┐ │
│  │   Type     │   Max IOPS   │ Max Thruput │   Use Case   │ Boot Volume │ │
│  ├────────────┼──────────────┼─────────────┼──────────────┼─────────────┤ │
│  │ gp3        │    16,000    │  1,000 MB/s │ Genel amaçlı │     ✓       │ │
│  │ io2        │   256,000    │  4,000 MB/s │ Kritik DB    │     ✓       │ │
│  │ st1        │      500     │    500 MB/s │ Big Data     │     ✗       │ │
│  │ sc1        │      250     │    250 MB/s │ Arşiv        │     ✗       │ │
│  └────────────┴──────────────┴─────────────┴──────────────┴─────────────┘ │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### EBS Snapshots

```
┌─────────────────────────────────────────────────────────────────┐
│                       EBS SNAPSHOTS                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "EBS volume'ların point-in-time yedeği"                       │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   EBS Volume                        S3 (Managed)           │ │
│  │   (us-east-1a)                                            │ │
│  │   ┌─────────────┐    Snapshot      ┌─────────────┐        │ │
│  │   │             │ ──────────────►  │   Snapshot  │        │ │
│  │   │    Data     │                  │   (S3'te)   │        │ │
│  │   │             │                  │             │        │ │
│  │   └─────────────┘                  └──────┬──────┘        │ │
│  │                                           │                │ │
│  │                      ┌────────────────────┼────────────┐  │ │
│  │                      │                    │            │  │ │
│  │                      ▼                    ▼            ▼  │ │
│  │              ┌─────────────┐      ┌─────────────┐     Copy│ │
│  │              │ Restore to  │      │ Restore to  │     to  │ │
│  │              │ New Volume  │      │ New Volume  │   other │ │
│  │              │ (Same AZ)   │      │ (Diff. AZ)  │  Region │ │
│  │              └─────────────┘      └─────────────┘         │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Incremental (sadece değişiklikler)                         │
│  • S3'te saklanır (managed, görünmez)                         │
│  • Farklı AZ veya Region'a restore                            │ │
│  • Encryption desteği                                          │
│  • Cross-Region copy                                          │
│                                                                 │
│  Snapshot Archive:                                             │
│  • 90 gün+ saklanacak snapshot'lar için                       │
│  • %75 daha ucuz                                              │
│  • Restore süresi: 24-72 saat                                 │
│                                                                 │
│  Recycle Bin:                                                  │
│  • Yanlışlıkla silinen snapshot'ları kurtar                   │
│  • 1 gün - 1 yıl retention                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### EBS Multi-Attach

```
┌─────────────────────────────────────────────────────────────────┐
│                      EBS MULTI-ATTACH                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Bir io1/io2 volume'u birden fazla EC2'ye bağla"              │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   EC2 Instance A        EC2 Instance B       EC2 Inst. C  │ │
│  │   ┌─────────────┐      ┌─────────────┐     ┌───────────┐  │ │
│  │   │             │      │             │     │           │  │ │
│  │   └──────┬──────┘      └──────┬──────┘     └─────┬─────┘  │ │
│  │          │                    │                  │         │ │
│  │          └────────────────────┼──────────────────┘         │ │
│  │                               │                             │ │
│  │                        ┌──────▼──────┐                      │ │
│  │                        │ EBS Volume  │                      │ │
│  │                        │   (io2)     │                      │ │
│  │                        │             │                      │ │
│  │                        └─────────────┘                      │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Kısıtlamalar:                                                 │
│  • Sadece io1/io2 volume types                                │
│  • Aynı AZ içinde                                              │
│  • Max 16 EC2 instance                                         │
│  • Cluster-aware dosya sistemi gerekli                        │
│                                                                 │
│  Kullanım: Clustered databases, high availability             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon EFS

> **Amazon Elastic File System (EFS)** - Yönetilen NFS dosya sistemi.

### EFS Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                         AMAZON EFS                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Elastic, paylaşımlı dosya sistemi (NFS)"                     │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                          VPC                               │ │
│  │                                                            │ │
│  │   AZ-a                    AZ-b                    AZ-c    │ │
│  │   ┌─────────────┐        ┌─────────────┐        ┌───────┐ │ │
│  │   │ EC2 Inst. 1 │        │ EC2 Inst. 2 │        │EC2 3  │ │ │
│  │   └──────┬──────┘        └──────┬──────┘        └───┬───┘ │ │
│  │          │                      │                   │      │ │
│  │   ┌──────▼──────┐        ┌──────▼──────┐     ┌─────▼─────┐│ │
│  │   │ Mount Target│        │ Mount Target│     │Mount Tgt  ││ │
│  │   └──────┬──────┘        └──────┬──────┘     └─────┬─────┘│ │
│  │          │                      │                  │       │ │
│  │          └──────────────────────┼──────────────────┘       │ │
│  │                                 │                          │ │
│  │                          ┌──────▼──────┐                   │ │
│  │                          │    EFS      │                   │ │
│  │                          │ File System │                   │ │
│  │                          │             │                   │ │
│  │                          │  /data      │                   │ │
│  │                          │  /logs      │                   │ │
│  │                          │  /shared    │                   │ │
│  │                          │             │                   │ │
│  │                          └─────────────┘                   │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Temel Özellikler:                                             │
│  • Multi-AZ erişim (Regional)                                 │
│  • NFSv4 protokolü                                            │
│  • POSIX uyumlu                                               │
│  • Otomatik ölçekleme (petabyte'lara kadar)                  │
│  • Binlerce eşzamanlı bağlantı                               │
│  • Pay-per-use (kullandığın kadar öde)                       │
│                                                                 │
│  EBS vs EFS:                                                   │
│  ┌───────────────────────┬───────────────────────┐            │
│  │         EBS           │          EFS          │            │
│  ├───────────────────────┼───────────────────────┤            │
│  │ Tek AZ                │ Multi-AZ              │            │
│  │ Tek EC2 (genellikle)  │ Çoklu EC2            │            │
│  │ Block storage         │ File storage (NFS)    │            │
│  │ Önceden boyut belirle │ Otomatik ölçekleme    │            │
│  └───────────────────────┴───────────────────────┘            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### EFS Storage Classes

```
┌─────────────────────────────────────────────────────────────────┐
│                    EFS STORAGE CLASSES                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   EFS Standard                    EFS Standard-IA          │ │
│  │   ────────────────                ─────────────────        │ │
│  │   • Sık erişim                    • Nadir erişim           │ │
│  │   • Multi-AZ                      • Multi-AZ               │ │
│  │   • En yüksek maliyet             • %92 daha ucuz storage  │ │
│  │                                   • Erişim ücreti var      │ │
│  │                                                            │ │
│  │   EFS One Zone                    EFS One Zone-IA          │ │
│  │   ────────────────                ─────────────────        │ │
│  │   • Tek AZ                        • Tek AZ + nadir erişim  │ │
│  │   • Standard'dan %47 ucuz         • En düşük maliyet       │ │
│  │   • Dev/test için ideal           • %92 daha ucuz          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  EFS Intelligent-Tiering:                                      │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  Lifecycle Management ile otomatik tier geçişi:           │ │
│  │                                                            │ │
│  │  Standard ──(30 gün erişim yok)──► Standard-IA           │ │
│  │                                                            │ │
│  │  Erişilince otomatik Standard'a geri döner               │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon FSx

> **Amazon FSx** - Yönetilen üçüncü parti dosya sistemleri.

### FSx Türleri

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           AMAZON FSx                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. FSx for Windows File Server                                            │
│  ──────────────────────────────                                            │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │                                                                        │ │
│  │  • Native Windows dosya sistemi (NTFS)                                │ │
│  │  • SMB protokolü                                                       │ │
│  │  • Active Directory entegrasyonu                                      │ │
│  │  • Windows uygulamaları için                                          │ │
│  │  • SQL Server, SharePoint, .NET uygulamaları                          │ │
│  │  • DFS (Distributed File System) desteği                              │ │
│  │                                                                        │ │
│  │  Kullanım: Windows workloads, enterprise file sharing                 │ │
│  │                                                                        │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  2. FSx for Lustre                                                         │
│  ─────────────────                                                         │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │                                                                        │ │
│  │  • High-Performance Computing (HPC)                                   │ │
│  │  • Machine Learning workloads                                         │ │
│  │  • Video processing                                                    │ │
│  │  • Yüzlerce GB/s throughput                                           │ │
│  │  • Milyonlarca IOPS                                                   │ │
│  │  • S3 ile seamless entegrasyon                                        │ │
│  │  • Sub-millisecond latency                                            │ │
│  │                                                                        │ │
│  │  Kullanım: HPC, ML training, financial modeling                       │ │
│  │                                                                        │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  3. FSx for NetApp ONTAP                                                   │
│  ────────────────────────                                                  │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │                                                                        │ │
│  │  • NetApp ONTAP dosya sistemi                                         │ │
│  │  • NFS, SMB, iSCSI protokolleri                                       │ │
│  │  • On-premises NetApp'tan migration                                   │ │
│  │  • Linux, Windows, macOS desteği                                      │ │
│  │  • Snapshot, replication, compression                                 │ │
│  │                                                                        │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  4. FSx for OpenZFS                                                        │
│  ──────────────────                                                        │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │                                                                        │ │
│  │  • OpenZFS dosya sistemi                                              │ │
│  │  • NFS protokolü                                                       │ │
│  │  • ZFS'ten migration                                                  │ │
│  │  • Up to 1 million IOPS                                               │ │
│  │  • Snapshots, cloning, compression                                    │ │
│  │                                                                        │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│  Özet:                                                                     │
│  ┌────────────────────┬────────────────────────────────────────────────┐  │
│  │     FSx Türü       │              Kullanım Alanı                    │  │
│  ├────────────────────┼────────────────────────────────────────────────┤  │
│  │ Windows File Server│ Windows uygulamaları, AD, SMB                  │  │
│  │ Lustre             │ HPC, ML, büyük veri işleme                     │  │
│  │ NetApp ONTAP       │ Enterprise, hybrid, multi-protocol             │  │
│  │ OpenZFS            │ ZFS migration, Linux workloads                 │  │
│  └────────────────────┴────────────────────────────────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## AWS Storage Gateway

> **AWS Storage Gateway** - On-premises ile AWS storage arasında köprü.

### Storage Gateway Türleri

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        AWS STORAGE GATEWAY                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  "Hybrid cloud storage - On-prem ↔ AWS"                                    │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │   On-Premises                            AWS Cloud                   │   │
│  │   ┌──────────────────┐                 ┌──────────────────┐         │   │
│  │   │                  │                 │                  │         │   │
│  │   │   Applications   │                 │       S3         │         │   │
│  │   │                  │                 │      EBS         │         │   │
│  │   └────────┬─────────┘                 │     Glacier      │         │   │
│  │            │                           │                  │         │   │
│  │   ┌────────▼─────────┐                 └────────▲─────────┘         │   │
│  │   │  Storage Gateway │◄───────────────────────►│                    │   │
│  │   │    (VM/HW)       │       Secure            │                    │   │
│  │   │                  │      Connection         │                    │   │
│  │   │   Local Cache    │                                              │   │
│  │   └──────────────────┘                                              │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  Gateway Türleri:                                                          │
│                                                                             │
│  1. S3 File Gateway                                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  • NFS / SMB ile S3'e erişim                                        │   │
│  │  • Dosyalar S3 object olarak saklanır                               │   │
│  │  • Local cache ile düşük latency                                    │   │
│  │  • Active Directory entegrasyonu                                    │   │
│  │                                                                      │   │
│  │  On-prem (NFS/SMB) ──► Gateway ──► S3 Bucket                        │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  2. FSx File Gateway                                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  • On-prem'den FSx for Windows'a SMB erişimi                        │   │
│  │  • Local cache ile low latency                                      │   │
│  │  • Windows dosya paylaşımları için                                  │   │
│  │                                                                      │   │
│  │  On-prem (SMB) ──► Gateway ──► FSx for Windows                      │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  3. Volume Gateway                                                         │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  • iSCSI block storage                                              │   │
│  │  • EBS snapshot'lara yedekleme                                      │   │
│  │                                                                      │   │
│  │  Cached Volumes:                                                     │   │
│  │  • S3'te sakla, sık erişileni cache'le                             │   │
│  │  • Düşük latency erişim                                             │   │
│  │                                                                      │   │
│  │  Stored Volumes:                                                     │   │
│  │  • Tüm veri on-prem, S3'e async backup                             │   │
│  │  • Tam yerel performans                                             │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  4. Tape Gateway                                                           │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  • Sanal tape library (VTL)                                         │   │
│  │  • Mevcut backup yazılımları ile uyumlu                             │   │
│  │  • S3 ve Glacier'a yedekleme                                        │   │
│  │  • Physical tape'lerden migration                                   │   │
│  │                                                                      │   │
│  │  Backup Software ──► Tape Gateway ──► S3/Glacier                    │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## AWS Snow Family

> **AWS Snow Family** - Fiziksel veri transfer cihazları.

### Snow Family Cihazları

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          AWS SNOW FAMILY                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  "Fiziksel cihazlarla büyük veri transferi"                                │
│                                                                             │
│  Network transferi için çok yavaş/pahalı olduğunda kullan!                 │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  1. Snowcone                                                         │   │
│  │  ────────────                                                        │   │
│  │  ┌─────────────┐                                                    │   │
│  │  │   ┌─────┐   │  • En küçük ve taşınabilir                        │   │
│  │  │   │     │   │  • 8 TB HDD / 14 TB SSD                           │   │
│  │  │   └─────┘   │  • 2.1 kg                                          │   │
│  │  └─────────────┘  • Edge computing destekler                        │   │
│  │                   • DataSync agent içerir                           │   │
│  │                                                                      │   │
│  │  2. Snowball Edge                                                    │   │
│  │  ────────────────                                                    │   │
│  │  ┌─────────────────┐                                                │   │
│  │  │   ┌─────────┐   │  Storage Optimized:                           │   │
│  │  │   │         │   │  • 80 TB HDD + 28 TB NVMe                     │   │
│  │  │   │         │   │  • 40 vCPU, 80 GB RAM                         │   │
│  │  │   └─────────┘   │                                                │   │
│  │  └─────────────────┘  Compute Optimized:                            │   │
│  │                       • 42 TB HDD + 28 TB NVMe                      │   │
│  │                       • 104 vCPU, 416 GB RAM                        │   │
│  │                       • Optional GPU                                 │   │
│  │                       • EC2 & Lambda destekler                      │   │
│  │                                                                      │   │
│  │  3. Snowmobile                                                       │   │
│  │  ─────────────                                                       │   │
│  │  ┌───────────────────────────────────────────────────────────────┐  │   │
│  │  │                                                                │  │   │
│  │  │   🚚 45 feet konteyner (TIR)                                   │  │   │
│  │  │   • 100 PB kapasitesi                                          │  │   │
│  │  │   • GPS tracking, 24/7 video surveillance                      │  │   │
│  │  │   • Multiple security layers                                   │  │   │
│  │  │   • Veri merkezi taşıma için                                   │  │   │
│  │  │                                                                │  │   │
│  │  └───────────────────────────────────────────────────────────────┘  │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  Kullanım Süreci:                                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  1. AWS Console'dan sipariş ver                                     │   │
│  │  2. AWS cihazı gönderir                                             │   │
│  │  3. Veriyi cihaza yükle (şifreli)                                   │   │
│  │  4. AWS'e geri gönder                                               │   │
│  │  5. AWS veriyi S3'e yükler                                          │   │
│  │  6. Cihaz güvenli şekilde silinir                                   │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  Ne Zaman Kullan?                                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                      │   │
│  │  Kural: Network üzerinden 1 hafta+ sürecekse → Snow Family          │   │
│  │                                                                      │   │
│  │  Örnek: 100 TB veri, 100 Mbps bağlantı                              │   │
│  │  Network: 100 TB / 100 Mbps ≈ 100 gün                               │   │
│  │  Snowball: ~1 hafta (kargo dahil)                                   │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  Karşılaştırma:                                                            │
│  ┌────────────────┬───────────┬─────────────────┬─────────────────────┐   │
│  │    Cihaz       │  Storage  │  Edge Compute   │    Kullanım         │   │
│  ├────────────────┼───────────┼─────────────────┼─────────────────────┤   │
│  │ Snowcone       │  8-14 TB  │ Sınırlı         │ Uzak lokasyonlar    │   │
│  │ Snowball Edge  │  80+ TB   │ Tam (EC2/Lambda)│ Büyük veri transfer │   │
│  │ Snowmobile     │  100 PB   │ Hayır           │ Veri merkezi taşıma │   │
│  └────────────────┴───────────┴─────────────────┴─────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## AWS Backup

> **AWS Backup** - Merkezi yedekleme servisi.

```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS BACKUP                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Merkezi, policy-based yedekleme yönetimi"                    │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                      AWS BACKUP                            │ │
│  │                                                            │ │
│  │  ┌─────────────────────────────────────────────────────┐  │ │
│  │  │               Backup Plans (Policies)               │  │ │
│  │  │                                                     │  │ │
│  │  │  • Backup frequency (hourly, daily, weekly)        │  │ │
│  │  │  • Retention period (30 days, 1 year, 7 years)     │  │ │
│  │  │  • Transition to cold storage                      │  │ │
│  │  │  • Cross-Region copy                               │  │ │
│  │  │                                                     │  │ │
│  │  └────────────────────────┬────────────────────────────┘  │ │
│  │                           │                                │ │
│  │         ┌─────────────────┼─────────────────┐              │ │
│  │         │                 │                 │              │ │
│  │         ▼                 ▼                 ▼              │ │
│  │    ┌─────────┐      ┌─────────┐      ┌─────────┐          │ │
│  │    │   EBS   │      │   RDS   │      │   DynamoDB│         │ │
│  │    └─────────┘      └─────────┘      └─────────┘          │ │
│  │                                                            │ │
│  │    ┌─────────┐      ┌─────────┐      ┌─────────┐          │ │
│  │    │   EFS   │      │   FSx   │      │   EC2   │          │ │
│  │    └─────────┘      └─────────┘      └─────────┘          │ │
│  │                                                            │ │
│  │    ┌─────────┐      ┌─────────┐      ┌─────────┐          │ │
│  │    │   S3    │      │ Aurora  │      │Document │          │ │
│  │    │         │      │         │      │   DB    │          │ │
│  │    └─────────┘      └─────────┘      └─────────┘          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Tek yerden tüm AWS kaynaklarını yedekle                    │
│  • Cross-Region ve Cross-Account backup                       │
│  • Compliance (WORM - Write Once Read Many)                   │
│  • Audit ve reporting                                          │
│  • Point-in-time recovery                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Karşılaştırma Tabloları

### Depolama Servisleri Karşılaştırma

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                              DEPOLAMA SERVİSLERİ KARŞILAŞTIRMA                              │
├─────────────────┬───────────────┬────────────────┬──────────────┬──────────────┬────────────┤
│     Servis      │     Tür       │    Protokol    │   Ölçekleme  │   Paylaşım   │   Erişim   │
├─────────────────┼───────────────┼────────────────┼──────────────┼──────────────┼────────────┤
│ S3              │ Object        │ HTTP/S API     │ Sınırsız     │ Web üzerinden│ Global     │
├─────────────────┼───────────────┼────────────────┼──────────────┼──────────────┼────────────┤
│ EBS             │ Block         │ AWS Internal   │ 64 TB/volume │ Tek EC2      │ Tek AZ     │
├─────────────────┼───────────────┼────────────────┼──────────────┼──────────────┼────────────┤
│ EFS             │ File (NFS)    │ NFSv4          │ Otomatik     │ Çoklu EC2    │ Multi-AZ   │
├─────────────────┼───────────────┼────────────────┼──────────────┼──────────────┼────────────┤
│ FSx Windows     │ File (SMB)    │ SMB            │ Otomatik     │ Çoklu EC2    │ Multi-AZ   │
├─────────────────┼───────────────┼────────────────┼──────────────┼──────────────┼────────────┤
│ FSx Lustre      │ File          │ Lustre         │ Otomatik     │ Çoklu EC2    │ Tek AZ     │
├─────────────────┼───────────────┼────────────────┼──────────────┼──────────────┼────────────┤
│ Storage Gateway │ Hybrid        │ NFS/SMB/iSCSI  │ Değişken     │ On-prem      │ Hybrid     │
├─────────────────┼───────────────┼────────────────┼──────────────┼──────────────┼────────────┤
│ Snow Family     │ Transfer      │ Fiziksel       │ Device bağlı │ Offline      │ Offline    │
└─────────────────┴───────────────┴────────────────┴──────────────┴──────────────┴────────────┘
```

### Kullanım Senaryoları

```
┌─────────────────────────────────────────────────────────────────┐
│                    SENARYO BAZLI SEÇİM                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────────────┬────────────────────────────┐ │
│  │          Senaryo             │          Servis            │ │
│  ├──────────────────────────────┼────────────────────────────┤ │
│  │ Web sitesi static files      │ S3 + CloudFront            │ │
│  │ EC2 boot volume              │ EBS (gp3)                  │ │
│  │ EC2 yüksek IOPS veritabanı   │ EBS (io2)                  │ │
│  │ Paylaşımlı Linux files       │ EFS                        │ │
│  │ Windows file share           │ FSx for Windows            │ │
│  │ HPC/ML training data         │ FSx for Lustre + S3        │ │
│  │ On-prem → S3 entegrasyonu    │ Storage Gateway            │ │
│  │ 100 TB veri migrasyonu       │ Snowball Edge              │ │
│  │ Veri merkezi taşıma          │ Snowmobile                 │ │
│  │ Uzun vadeli arşiv            │ S3 Glacier Deep Archive    │ │
│  │ Merkezi yedekleme            │ AWS Backup                 │ │
│  │ Big data analytics           │ S3 + Athena                │ │
│  │ Log saklama                  │ S3 Standard-IA             │ │
│  │ Backup offsite kopyası       │ S3 Cross-Region Replication│ │
│  └──────────────────────────────┴────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Sınav Senaryoları

### Senaryo 1: Statik Web Sitesi

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Şirket, statik web sitesi barındırmak istiyor. Sunucu    │
│ yönetimi olmamalı, global erişim hızlı olmalı. En uygun çözüm? │
├─────────────────────────────────────────────────────────────────┤
│ A) EC2 + EBS                                                    │
│ B) S3 + CloudFront                                              │
│ C) EFS + ALB                                                    │
│ D) FSx for Windows                                              │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) S3 + CloudFront                                       │
│                                                                 │
│ Neden?                                                          │
│ • "Statik web sitesi" = S3 Static Website Hosting              │
│ • "Sunucu yönetimi yok" = S3 serverless                        │
│ • "Global hızlı erişim" = CloudFront CDN                       │
│ • Maliyet etkin çözüm                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 2: Paylaşımlı Dosya Sistemi

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Birden fazla EC2 Linux instance'ın aynı dosyalara        │
│ erişmesi gerekiyor. Dosyalar farklı AZ'lerden erişilecek.      │
│ Hangi depolama çözümü?                                          │
├─────────────────────────────────────────────────────────────────┤
│ A) EBS                                                          │
│ B) S3                                                           │
│ C) EFS                                                          │
│ D) Instance Store                                               │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: C) EFS                                                   │
│                                                                 │
│ Neden?                                                          │
│ • "Birden fazla EC2" = Paylaşımlı dosya sistemi gerekli        │
│ • "Farklı AZ'ler" = Multi-AZ desteği gerekli                   │
│ • "Linux" = NFS protokolü = EFS                                │
│ • EBS tek AZ ve genellikle tek EC2                             │
│ • S3 object storage, dosya sistemi değil                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 3: Arşiv Depolama

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Yasal uyumluluk için 7 yıl boyunca veri saklanmalı.      │
│ Veriye çok nadir erişilecek (yılda 1-2 kez) ve erişim 12       │
│ saat içinde olabilir. En ucuz çözüm?                           │
├─────────────────────────────────────────────────────────────────┤
│ A) S3 Standard                                                  │
│ B) S3 Glacier Instant Retrieval                                 │
│ C) S3 Glacier Flexible Retrieval                                │
│ D) S3 Glacier Deep Archive                                      │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: D) S3 Glacier Deep Archive                               │
│                                                                 │
│ Neden?                                                          │
│ • "7 yıl" = Uzun vadeli arşiv                                  │
│ • "Yılda 1-2 kez" = Çok nadir erişim                           │
│ • "12 saat içinde" = Deep Archive uygun (12-48 saat)           │
│ • "En ucuz" = Deep Archive en düşük maliyet                    │
│ • Instant Retrieval milisaniye erişim (gereksiz)               │
│ • Flexible Retrieval daha pahalı                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 4: Büyük Veri Transferi

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Şirket 50 TB veriyi AWS'e taşımak istiyor. Mevcut        │
│ internet bağlantısı 100 Mbps. En verimli yöntem?               │
├─────────────────────────────────────────────────────────────────┤
│ A) Direct Connect                                               │
│ B) AWS Snowball                                                 │
│ C) Internet üzerinden S3 upload                                 │
│ D) AWS DataSync                                                 │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) AWS Snowball                                          │
│                                                                 │
│ Neden?                                                          │
│ Hesaplama:                                                      │
│ • 50 TB = 50,000 GB = 400,000 Gb                               │
│ • 100 Mbps = 1 GB / ~85 saniye                                 │
│ • Toplam: 50,000 GB x 85s ≈ 50 gün                             │
│                                                                 │
│ • "50 gün" > 1 hafta = Snowball daha verimli                   │
│ • Direct Connect kurulum süresi uzun                           │
│ • DataSync internet üzerinden, yine 50 gün sürer               │
│                                                                 │
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
│  DEPOLAMA TÜRLERİ:                                             │
│  • Object = S3 (HTTP API, sınırsız)                            │
│  • Block = EBS (disk, tek AZ)                                  │
│  • File = EFS (NFS, multi-AZ), FSx (Windows/Lustre)            │
│                                                                 │
│  S3 STORAGE CLASSES (ucuzdan pahalıya):                        │
│  Deep Archive < Glacier < IA < Standard                        │
│                                                                 │
│  S3 ÖNEMLİ ÖZELLİKLER:                                         │
│  • 11 9's dayanıklılık                                         │
│  • Versioning = Silme koruması                                 │
│  • Lifecycle = Otomatik tier geçişi                            │
│  • Replication = CRR (cross-region), SRR (same-region)         │
│                                                                 │
│  EBS VOLUME TYPES:                                             │
│  • gp3 = Genel amaçlı SSD (varsayılan)                        │
│  • io2 = Yüksek IOPS (kritik veritabanları)                   │
│  • st1 = Throughput HDD (big data)                            │
│  • sc1 = Cold HDD (arşiv)                                      │
│                                                                 │
│  TRANSFER:                                                      │
│  • 1 hafta+ sürecek = Snow Family kullan                       │
│  • Snowcone < Snowball Edge < Snowmobile                       │
│                                                                 │
│  HİBRİT:                                                       │
│  • Storage Gateway = On-prem ↔ AWS köprüsü                     │
│  • File/Volume/Tape Gateway türleri                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

- [Amazon S3 User Guide](https://docs.aws.amazon.com/s3/)
- [Amazon EBS User Guide](https://docs.aws.amazon.com/ebs/)
- [Amazon EFS User Guide](https://docs.aws.amazon.com/efs/)
- [Amazon FSx Documentation](https://docs.aws.amazon.com/fsx/)
- [AWS Storage Gateway User Guide](https://docs.aws.amazon.com/storagegateway/)
- [AWS Snow Family Documentation](https://docs.aws.amazon.com/snowball/)
- [CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

> **Sınav İpucu:** S3 storage class soruları çok sık çıkar! Erişim sıklığı ve retrieval süresi anahtar kelimelerdir. "Nadir erişim" = IA, "Yılda birkaç kez" = Glacier, "7+ yıl arşiv" = Deep Archive.

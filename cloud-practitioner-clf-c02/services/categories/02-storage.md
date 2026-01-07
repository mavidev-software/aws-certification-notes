# Storage Services (In-Scope)

> **Kategori:** Depolama Servisleri

## Amazon S3 (Simple Storage Service)

**Ne İşe Yarar:** Sınırsız nesne (object) depolama hizmeti sunar.

**Temel Özellikler:**
- %99.999999999 (11 9's) dayanıklılık
- Sınırsız depolama kapasitesi
- Tek dosya maksimum 5TB
- Versiyonlama desteği
- Lifecycle policy'ler ile otomatik yönetim

**Depolama Sınıfları:**
| Sınıf | Kullanım | Maliyet |
|-------|----------|---------|
| S3 Standard | Sık erişilen veriler | En yüksek |
| S3 Intelligent-Tiering | Değişken erişim | Otomatik optimize |
| S3 Standard-IA | Nadir erişim (aylık) | Düşük |
| S3 One Zone-IA | Nadir erişim, tek AZ | Daha düşük |
| S3 Glacier Instant | Arşiv, anında erişim | Düşük |
| S3 Glacier Flexible | Arşiv, dakikalar/saatler | Çok düşük |
| S3 Glacier Deep Archive | Uzun süreli arşiv | En düşük |

**Ne Zaman Kullanılır:**
- Statik web sitesi barındırma
- Yedekleme ve arşivleme
- Veri gölü (data lake)

---

## Amazon S3 Glacier

**Ne İşe Yarar:** Uzun süreli arşiv depolama için düşük maliyetli çözüm.

**Retrieval Seçenekleri:**
| Seçenek | Süre | Maliyet |
|---------|------|---------|
| Expedited | 1-5 dakika | En yüksek |
| Standard | 3-5 saat | Orta |
| Bulk | 5-12 saat | En düşük |

---

## Amazon EBS (Elastic Block Store)

**Ne İşe Yarar:** EC2 instance'ları için kalıcı blok depolama sağlar.

**Volume Tipleri:**
| Tip | Kullanım | IOPS |
|-----|----------|------|
| gp3 (General Purpose SSD) | Genel kullanım | 16,000 |
| io2 (Provisioned IOPS SSD) | Yüksek performans | 256,000 |
| st1 (Throughput Optimized HDD) | Büyük veri, streaming | 500 |
| sc1 (Cold HDD) | Nadir erişilen veriler | 250 |

**Temel Özellikler:**
- Snapshot ile yedekleme (S3'e kaydedilir)
- Şifreleme desteği
- Tek AZ içinde replike edilir

---

## Amazon EFS (Elastic File System)

**Ne İşe Yarar:** Paylaşımlı dosya sistemi (NFS) sağlar.

**Temel Özellikler:**
- Birden fazla EC2 instance'ı aynı anda erişebilir
- Otomatik ölçeklenir
- Linux tabanlı workload'lar için

**EBS vs EFS:**
| Özellik | EBS | EFS |
|---------|-----|-----|
| Erişim | Tek EC2 | Çoklu EC2 |
| Protokol | Block | NFS |
| Ölçekleme | Manuel | Otomatik |

---

## Amazon FSx

**Ne İşe Yarar:** Yönetilen dosya sistemi hizmeti sunar.

**Türleri:**
- **FSx for Windows File Server:** Windows uygulamaları için SMB
- **FSx for Lustre:** Yüksek performanslı computing (HPC)
- **FSx for NetApp ONTAP:** Enterprise özellikler
- **FSx for OpenZFS:** Linux workload'lar

---

## AWS Storage Gateway

**Ne İşe Yarar:** On-premises uygulamalarınızı AWS depolamaya bağlar.

**Gateway Türleri:**
| Tür | Kullanım |
|-----|----------|
| File Gateway | S3'e NFS/SMB erişimi |
| Volume Gateway | iSCSI blok depolama |
| Tape Gateway | Sanal tape kütüphanesi |

---

## AWS Backup

**Ne İşe Yarar:** Merkezi yedekleme yönetimi sağlar.

**Desteklenen Servisler:**
- EC2, EBS, EFS, FSx
- RDS, DynamoDB, Aurora
- Storage Gateway

---

## AWS Elastic Disaster Recovery

**Ne İşe Yarar:** Felaket kurtarma (DR) çözümü sunar.

**Temel Özellikler:**
- Dakikalar içinde kurtarma
- Sürekli veri replikasyonu

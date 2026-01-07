# AWS Cloud Practitioner (CLF-C02) - Tüm In-Scope Servisler

Bu doküman, AWS Cloud Practitioner sınavında (CLF-C02) çıkabilecek tüm servislerin detaylı açıklamalarını içermektedir.

> **Kaynak:** [AWS Resmi Sınav Rehberi](https://docs.aws.amazon.com/aws-certification/latest/examguides/clf-02-in-scope-services.html)

---

## İçindekiler

1. [Compute (Hesaplama)](#1-compute-hesaplama)
2. [Storage (Depolama)](#2-storage-depolama)
3. [Database (Veritabanı)](#3-database-veritabanı)
4. [Networking & Content Delivery (Ağ)](#4-networking--content-delivery-ağ)
5. [Security, Identity & Compliance (Güvenlik)](#5-security-identity--compliance-güvenlik)
6. [Management & Governance (Yönetim)](#6-management--governance-yönetim)
7. [Cloud Financial Management (Maliyet)](#7-cloud-financial-management-maliyet)
8. [Machine Learning (Makine Öğrenmesi)](#8-machine-learning-makine-öğrenmesi)
9. [Analytics (Analitik)](#9-analytics-analitik)
10. [Application Integration (Uygulama Entegrasyonu)](#10-application-integration-uygulama-entegrasyonu)
11. [Developer Tools (Geliştirici Araçları)](#11-developer-tools-geliştirici-araçları)
12. [Containers (Konteynerler)](#12-containers-konteynerler)
13. [Serverless (Sunucusuz)](#13-serverless-sunucusuz)
14. [Migration & Transfer (Göç)](#14-migration--transfer-göç)
15. [End User Computing](#15-end-user-computing)
16. [Frontend Web & Mobile](#16-frontend-web--mobile)
17. [Business Applications](#17-business-applications)
18. [Internet of Things (IoT)](#18-internet-of-things-iot)
19. [Customer Enablement](#19-customer-enablement)

---

## 1. Compute (Hesaplama)

### Amazon EC2 (Elastic Compute Cloud)

**Ne İşe Yarar:** AWS'de sanal sunucular (instance) oluşturmanızı sağlar.

**Temel Özellikler:**
- İstediğiniz işletim sistemini seçebilirsiniz (Linux, Windows, macOS)
- Farklı instance tipleri: General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, Accelerated Computing
- On-Demand, Reserved, Spot, Dedicated Host gibi fiyatlandırma seçenekleri

**Ne Zaman Kullanılır:**
- Web sunucuları barındırmak
- Uygulama sunucuları çalıştırmak
- Geliştirme/test ortamları oluşturmak

**Fiyatlandırma Modelleri:**
| Model | Açıklama | İndirim |
|-------|----------|---------|
| On-Demand | Kullandığın kadar öde | - |
| Reserved Instance | 1-3 yıllık taahhüt | %72'ye kadar |
| Spot Instance | Boşta kalan kapasiteyi kullan | %90'a kadar |
| Savings Plans | Esnek taahhüt | %72'ye kadar |
| Dedicated Host | Fiziksel sunucu kiralama | Lisans uyumluluğu |

---

### AWS Lambda

**Ne İşe Yarar:** Sunucu yönetmeden kod çalıştırmanızı sağlar (Serverless).

**Temel Özellikler:**
- Sadece kod çalışma süresi kadar ödeme yaparsınız
- Otomatik ölçeklenir
- Python, Node.js, Java, Go, C#, Ruby destekler
- Maksimum 15 dakika çalışma süresi

**Ne Zaman Kullanılır:**
- Event-driven (olay tabanlı) uygulamalar
- API backend'leri
- Veri işleme pipeline'ları
- Zamanlanmış görevler (cron jobs)

**Örnek Kullanım:**
```
S3'e dosya yüklendi → Lambda tetiklendi → Dosya işlendi → DynamoDB'ye kaydedildi
```

---

### AWS Elastic Beanstalk

**Ne İşe Yarar:** Web uygulamalarını kolayca dağıtmanızı ve yönetmenizi sağlar (PaaS).

**Temel Özellikler:**
- Altyapıyı otomatik olarak yönetir (EC2, Load Balancer, Auto Scaling)
- Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker destekler
- Ücret sadece kullandığınız kaynaklar için alınır

**Ne Zaman Kullanılır:**
- Hızlı uygulama dağıtımı gerektiğinde
- Altyapı yönetimi ile uğraşmak istemediğinizde
- Geliştiricilerin koda odaklanması gerektiğinde

**Elastic Beanstalk vs EC2:**
| Özellik | EC2 | Elastic Beanstalk |
|---------|-----|-------------------|
| Kontrol | Tam kontrol | Otomatik yönetim |
| Kurulum | Manuel | Otomatik |
| Esneklik | Yüksek | Orta |
| Öğrenme eğrisi | Dik | Düşük |

---

### Amazon Lightsail

**Ne İşe Yarar:** Basit sanal özel sunucu (VPS) hizmeti sunar.

**Temel Özellikler:**
- Sabit aylık fiyatlandırma
- Önceden yapılandırılmış uygulamalar (WordPress, LAMP, Node.js vb.)
- Kolay kullanım arayüzü
- Küçük ve orta ölçekli projeler için ideal

**Ne Zaman Kullanılır:**
- Basit web siteleri
- Blog platformları
- Küçük e-ticaret siteleri
- Geliştirme/test ortamları

**Lightsail vs EC2:**
| Özellik | Lightsail | EC2 |
|---------|-----------|-----|
| Fiyatlandırma | Sabit aylık | Kullandıkça öde |
| Karmaşıklık | Basit | Karmaşık |
| Özelleştirme | Sınırlı | Tam |
| Hedef kitle | Başlangıç | İleri düzey |

---

### AWS Batch

**Ne İşe Yarar:** Büyük ölçekli toplu işleme (batch processing) işlerini çalıştırır.

**Temel Özellikler:**
- Binlerce işi paralel çalıştırabilir
- Otomatik kaynak ölçekleme
- Spot Instance desteği ile maliyet optimizasyonu

**Ne Zaman Kullanılır:**
- Büyük veri analizi
- Video/görüntü işleme
- Finansal modelleme
- Bilimsel simülasyonlar

---

### AWS Outposts

**Ne İşe Yarar:** AWS altyapısını kendi veri merkezinizde çalıştırmanızı sağlar.

**Temel Özellikler:**
- Hybrid cloud çözümü
- AWS servisleri on-premises'te çalışır
- Düşük gecikme süresi gerektiren uygulamalar için

**Ne Zaman Kullanılır:**
- Veri yerelliği gereksinimleri
- Düşük gecikme süresi gerektiğinde
- Mevcut on-premises yatırımlarla entegrasyon

---

## 2. Storage (Depolama)

### Amazon S3 (Simple Storage Service)

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
- Medya depolama

---

### Amazon S3 Glacier

**Ne İşe Yarar:** Uzun süreli arşiv depolama için düşük maliyetli çözüm.

**Retrieval (Geri Alma) Seçenekleri:**
| Seçenek | Süre | Maliyet |
|---------|------|---------|
| Expedited | 1-5 dakika | En yüksek |
| Standard | 3-5 saat | Orta |
| Bulk | 5-12 saat | En düşük |

**Ne Zaman Kullanılır:**
- Uyumluluk arşivleri
- Medya arşivleri
- Nadir erişilen eski veriler

---

### Amazon EBS (Elastic Block Store)

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

**Ne Zaman Kullanılır:**
- İşletim sistemi diskleri
- Veritabanı depolama
- Yüksek IOPS gerektiren uygulamalar

---

### Amazon EFS (Elastic File System)

**Ne İşe Yarar:** Paylaşımlı dosya sistemi (NFS) sağlar.

**Temel Özellikler:**
- Birden fazla EC2 instance'ı aynı anda erişebilir
- Otomatik ölçeklenir
- Bölgeler arası replikasyon
- Linux tabanlı workload'lar için

**EBS vs EFS:**
| Özellik | EBS | EFS |
|---------|-----|-----|
| Erişim | Tek EC2 | Çoklu EC2 |
| Protokol | Block | NFS |
| Ölçekleme | Manuel | Otomatik |
| OS | Linux/Windows | Linux |

**Ne Zaman Kullanılır:**
- Paylaşımlı dosya depolama
- Content management sistemleri
- Web sunucusu dosyaları
- Konteyner depolama

---

### Amazon FSx

**Ne İşe Yarar:** Yönetilen dosya sistemi hizmeti sunar.

**Türleri:**
- **FSx for Windows File Server:** Windows uygulamaları için SMB protokolü
- **FSx for Lustre:** Yüksek performanslı computing (HPC) için
- **FSx for NetApp ONTAP:** Enterprise özellikler
- **FSx for OpenZFS:** Linux workload'lar

**Ne Zaman Kullanılır:**
- Windows tabanlı uygulamalar (FSx for Windows)
- Machine learning, HPC (FSx for Lustre)
- Enterprise dosya sistemleri

---

### AWS Storage Gateway

**Ne İşe Yarar:** On-premises uygulamalarınızı AWS depolamaya bağlar.

**Gateway Türleri:**
| Tür | Kullanım |
|-----|----------|
| File Gateway | S3'e NFS/SMB erişimi |
| Volume Gateway | iSCSI blok depolama |
| Tape Gateway | Sanal tape kütüphanesi |

**Ne Zaman Kullanılır:**
- Hybrid cloud senaryoları
- On-premises yedekleme
- Veri göçü

---

### AWS Backup

**Ne İşe Yarar:** Merkezi yedekleme yönetimi sağlar.

**Desteklenen Servisler:**
- EC2, EBS, EFS, FSx
- RDS, DynamoDB, Aurora
- Storage Gateway

**Ne Zaman Kullanılır:**
- Merkezi yedekleme politikası
- Uyumluluk gereksinimleri
- Cross-region yedekleme

---

### AWS Elastic Disaster Recovery

**Ne İşe Yarar:** Felaket kurtarma (DR) çözümü sunar.

**Temel Özellikler:**
- Dakikalar içinde kurtarma
- Sürekli veri replikasyonu
- Düşük TCO

**Ne Zaman Kullanılır:**
- İş sürekliliği planları
- RTO/RPO gereksinimlerini karşılama

---

## 3. Database (Veritabanı)

### Amazon RDS (Relational Database Service)

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

### Amazon Aurora

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
| Ölçekleme | Otomatik | Manuel |

---

### Amazon DynamoDB

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
- Alışveriş sepeti

**DynamoDB vs RDS:**
| Özellik | DynamoDB | RDS |
|---------|----------|-----|
| Tür | NoSQL | SQL |
| Şema | Esnek | Sabit |
| Ölçekleme | Otomatik | Manuel |
| Sorgulama | Key-based | SQL |

---

### Amazon ElastiCache

**Ne İşe Yarar:** In-memory cache hizmeti sunar.

**Desteklenen Motorlar:**
- **Redis:** Gelişmiş veri yapıları, persistence, replikasyon
- **Memcached:** Basit caching, çoklu thread

**Ne Zaman Kullanılır:**
- Session caching
- Veritabanı sorgu sonuçlarını cache'leme
- Gerçek zamanlı analitik
- Gaming leaderboard'ları

---

### Amazon DocumentDB

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

### Amazon Neptune

**Ne İşe Yarar:** Yönetilen graph veritabanı.

**Desteklenen Modeller:**
- Property Graph (Gremlin)
- RDF (SPARQL)

**Ne Zaman Kullanılır:**
- Sosyal ağlar
- Öneri motorları
- Fraud detection
- Knowledge graphs

---

## 4. Networking & Content Delivery (Ağ)

### Amazon VPC (Virtual Private Cloud)

**Ne İşe Yarar:** AWS'de izole sanal ağ oluşturmanızı sağlar.

**Temel Bileşenler:**
| Bileşen | Açıklama |
|---------|----------|
| Subnet | VPC içinde IP aralığı bölümü |
| Route Table | Trafik yönlendirme kuralları |
| Internet Gateway | İnternete çıkış |
| NAT Gateway | Private subnet'ten internete çıkış |
| Security Group | Instance seviyesinde güvenlik duvarı (stateful) |
| Network ACL | Subnet seviyesinde güvenlik duvarı (stateless) |

**Public vs Private Subnet:**
| Özellik | Public Subnet | Private Subnet |
|---------|---------------|----------------|
| Internet erişimi | Doğrudan (IGW) | NAT Gateway üzerinden |
| Kullanım | Web sunucuları | Veritabanları, backend |

---

### Amazon Route 53

**Ne İşe Yarar:** DNS (Domain Name System) web hizmeti.

**Routing Policy'leri:**
| Policy | Açıklama |
|--------|----------|
| Simple | Tek kaynak |
| Weighted | Ağırlıklı dağıtım |
| Latency-based | En düşük gecikme |
| Failover | Aktif/pasif |
| Geolocation | Coğrafi konum |
| Multi-value | Birden fazla değer |

**Ne Zaman Kullanılır:**
- Domain kayıt
- DNS yönetimi
- Sağlık kontrolü ve failover
- Traffic routing

---

### Amazon CloudFront

**Ne İşe Yarar:** Global CDN (Content Delivery Network) hizmeti.

**Temel Özellikler:**
- 400+ Edge Location
- DDoS koruması (AWS Shield entegrasyonu)
- SSL/TLS sertifikası
- Lambda@Edge ile edge computing

**Ne Zaman Kullanılır:**
- Statik içerik dağıtımı
- Video streaming
- API hızlandırma
- Web sitesi performansı

---

### AWS Direct Connect

**Ne İşe Yarar:** Veri merkezinizden AWS'e özel bağlantı sağlar.

**Temel Özellikler:**
- İnternet üzerinden değil, özel hat
- Tutarlı ağ performansı
- 1Gbps veya 10Gbps bağlantı seçenekleri

**Ne Zaman Kullanılır:**
- Büyük veri transferleri
- Hybrid cloud mimarileri
- Düşük gecikme gereksinimleri

**Direct Connect vs VPN:**
| Özellik | Direct Connect | VPN |
|---------|----------------|-----|
| Bağlantı | Özel hat | İnternet üzerinden |
| Kurulum süresi | Haftalar | Dakikalar |
| Maliyet | Yüksek | Düşük |
| Performans | Tutarlı | Değişken |

---

### AWS VPN (Site-to-Site VPN & Client VPN)

**Ne İşe Yarar:** Şifreli VPN bağlantısı sağlar.

**Türleri:**
- **Site-to-Site VPN:** Veri merkezi ile AWS arasında
- **Client VPN:** Bireysel kullanıcılar için uzaktan erişim

**Ne Zaman Kullanılır:**
- Hızlı hybrid bağlantı kurulumu
- Yedek bağlantı (Direct Connect ile birlikte)
- Uzaktan çalışan erişimi

---

### Amazon API Gateway

**Ne İşe Yarar:** API oluşturma, yayınlama ve yönetme hizmeti.

**Temel Özellikler:**
- RESTful API ve WebSocket API desteği
- Throttling ve rate limiting
- API key yönetimi
- Lambda, EC2, diğer AWS servisleriyle entegrasyon

**Ne Zaman Kullanılır:**
- Serverless API backend
- Microservice mimarileri
- Mobile app backend

---

### AWS Global Accelerator

**Ne İşe Yarar:** AWS global ağını kullanarak uygulama performansını artırır.

**Temel Özellikler:**
- Anycast IP adresleri
- AWS backbone ağı üzerinden trafik
- Otomatik failover

**CloudFront vs Global Accelerator:**
| Özellik | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| Kullanım | Statik/dinamik içerik | TCP/UDP uygulamaları |
| Cache | Var | Yok |
| IP tipi | DNS-based | Static Anycast IP |

---

### AWS Transit Gateway

**Ne İşe Yarar:** VPC'leri ve on-premises ağlarını merkezi olarak bağlar.

**Ne Zaman Kullanılır:**
- Çok sayıda VPC bağlantısı
- Hub-and-spoke ağ mimarisi
- Karmaşık hybrid ağlar

---

### AWS PrivateLink

**Ne İşe Yarar:** AWS servislerine özel bağlantı sağlar (internet üzerinden değil).

**Ne Zaman Kullanılır:**
- S3, DynamoDB gibi servislere güvenli erişim
- VPC içinden AWS servislerine erişim

---

## 5. Security, Identity & Compliance (Güvenlik)

### AWS IAM (Identity and Access Management)

**Ne İşe Yarar:** AWS kaynaklarına erişimi güvenli bir şekilde yönetir.

**Temel Kavramlar:**
| Kavram | Açıklama |
|--------|----------|
| User | Bireysel kullanıcı |
| Group | Kullanıcı grubu |
| Role | Geçici kimlik (EC2, Lambda için) |
| Policy | İzin belgeleri (JSON) |

**Policy Türleri:**
- **AWS Managed:** AWS tarafından oluşturulan
- **Customer Managed:** Müşteri tarafından oluşturulan
- **Inline:** Tek kaynağa bağlı

**Best Practices:**
- Root hesabı kullanmayın
- MFA etkinleştirin
- En az yetki ilkesi (Least Privilege)
- Access Key'leri düzenli döndürün

---

### AWS IAM Identity Center (eski adı: AWS SSO)

**Ne İşe Yarar:** Tek oturum açma (SSO) ve merkezi erişim yönetimi.

**Temel Özellikler:**
- Birden fazla AWS hesabına tek giriş
- Active Directory entegrasyonu
- SAML 2.0 uyumlu uygulamalar

**Ne Zaman Kullanılır:**
- Multi-account ortamları
- Kurumsal SSO gereksinimleri

---

### Amazon Cognito

**Ne İşe Yarar:** Web ve mobil uygulamalar için kullanıcı kimlik doğrulama.

**Bileşenleri:**
- **User Pools:** Kullanıcı dizini, kayıt/giriş
- **Identity Pools:** AWS servislerine geçici erişim

**Ne Zaman Kullanılır:**
- Mobil uygulama kullanıcı yönetimi
- Sosyal medya ile giriş (Google, Facebook)
- Guest erişimi

---

### AWS KMS (Key Management Service)

**Ne İşe Yarar:** Şifreleme anahtarlarını oluşturur ve yönetir.

**Anahtar Türleri:**
| Tür | Yönetim |
|-----|---------|
| AWS Managed Keys | AWS yönetir |
| Customer Managed Keys | Siz yönetirsiniz |
| AWS Owned Keys | AWS servisleri için |

**Ne Zaman Kullanılır:**
- S3 şifreleme
- EBS şifreleme
- RDS şifreleme
- Uygulama seviyesi şifreleme

---

### AWS Secrets Manager

**Ne İşe Yarar:** Gizli bilgileri (şifreler, API anahtarları) güvenle saklar.

**Temel Özellikler:**
- Otomatik şifre döndürme
- RDS, Redshift, DocumentDB entegrasyonu
- Şifrelenmiş depolama

**Secrets Manager vs Parameter Store:**
| Özellik | Secrets Manager | Parameter Store |
|---------|-----------------|-----------------|
| Otomatik rotation | Var | Yok |
| Maliyet | Ücretli | Ücretsiz tier var |
| Kullanım | Şifreler, credentials | Konfigürasyon değerleri |

---

### AWS WAF (Web Application Firewall)

**Ne İşe Yarar:** Web uygulamalarını yaygın saldırılardan korur.

**Korunan Saldırı Türleri:**
- SQL Injection
- Cross-Site Scripting (XSS)
- IP tabanlı engelleme
- Rate limiting

**Entegrasyonlar:**
- CloudFront
- Application Load Balancer
- API Gateway

---

### AWS Shield

**Ne İşe Yarar:** DDoS saldırılarına karşı koruma sağlar.

**Versiyonları:**
| Versiyon | Özellik | Maliyet |
|----------|---------|---------|
| Shield Standard | L3/L4 DDoS koruması | Ücretsiz |
| Shield Advanced | L7 koruması, 24/7 destek, maliyet koruması | $3,000/ay |

---

### Amazon GuardDuty

**Ne İşe Yarar:** Akıllı tehdit algılama hizmeti.

**Analiz Kaynakları:**
- VPC Flow Logs
- CloudTrail Logs
- DNS Logs

**Ne Zaman Kullanılır:**
- Anormal davranış tespiti
- Cryptocurrency mining algılama
- Yetkisiz erişim tespiti

---

### Amazon Inspector

**Ne İşe Yarar:** Otomatik güvenlik açığı taraması.

**Tarama Türleri:**
- EC2 instance güvenlik açıkları
- Container image taraması
- Lambda fonksiyon taraması
- Network erişilebilirlik

---

### AWS Artifact

**Ne İşe Yarar:** AWS güvenlik ve uyumluluk belgelerine erişim sağlar.

**İçerik:**
- SOC raporları
- PCI DSS raporları
- ISO sertifikaları
- GDPR belgeleri

---

### Amazon Macie

**Ne İşe Yarar:** S3'teki hassas verileri (PII) otomatik keşfeder ve korur.

**Ne Zaman Kullanılır:**
- GDPR uyumluluğu
- Veri sınıflandırma
- Veri sızıntısı önleme

---

### AWS Security Hub

**Ne İşe Yarar:** Merkezi güvenlik görünümü ve uyumluluk kontrolü.

**Entegrasyonlar:**
- GuardDuty
- Inspector
- Macie
- IAM Access Analyzer
- Firewall Manager

---

### AWS Certificate Manager (ACM)

**Ne İşe Yarar:** SSL/TLS sertifikalarını yönetir.

**Temel Özellikler:**
- Ücretsiz public sertifikalar
- Otomatik yenileme
- CloudFront, ALB, API Gateway entegrasyonu

---

### AWS CloudHSM

**Ne İşe Yarar:** Donanım güvenlik modülü (HSM) hizmeti.

**KMS vs CloudHSM:**
| Özellik | KMS | CloudHSM |
|---------|-----|----------|
| Yönetim | AWS shared | Dedicated |
| FIPS 140-2 | Level 2 | Level 3 |
| Anahtar kontrolü | AWS | Müşteri |

---

### Amazon Detective

**Ne İşe Yarar:** Güvenlik olaylarının kök nedenini araştırır.

**Ne Zaman Kullanılır:**
- GuardDuty bulgularını araştırma
- Güvenlik olayı analizi

---

### AWS Directory Service

**Ne İşe Yarar:** Microsoft Active Directory hizmeti sunar.

**Türleri:**
- AWS Managed Microsoft AD
- AD Connector
- Simple AD

---

### AWS Firewall Manager

**Ne İşe Yarar:** Merkezi güvenlik duvarı kuralları yönetimi.

**Yönetilen Servisler:**
- WAF kuralları
- Shield Advanced korumaları
- Security Group'lar
- Network Firewall

---

### AWS RAM (Resource Access Manager)

**Ne İşe Yarar:** AWS kaynaklarını hesaplar arasında paylaşır.

**Paylaşılabilir Kaynaklar:**
- Subnets
- Transit Gateway
- License Manager
- Route 53 Resolver

---

### AWS Audit Manager

**Ne İşe Yarar:** Sürekli denetim ve uyumluluk kanıtı toplama.

**Ne Zaman Kullanılır:**
- SOX, PCI DSS, HIPAA denetimleri
- Otomatik kanıt toplama

---

## 6. Management & Governance (Yönetim)

### AWS CloudFormation

**Ne İşe Yarar:** Infrastructure as Code (IaC) - Altyapıyı kod olarak tanımlar.

**Temel Kavramlar:**
| Kavram | Açıklama |
|--------|----------|
| Template | JSON/YAML altyapı tanımı |
| Stack | Bir template'ten oluşturulan kaynaklar |
| Change Set | Değişiklikleri önizleme |
| Drift Detection | Konfigürasyon değişikliklerini algılama |

**Ne Zaman Kullanılır:**
- Tekrarlanabilir altyapı dağıtımı
- Multi-region dağıtım
- Disaster recovery

---

### AWS CloudTrail

**Ne İşe Yarar:** AWS API çağrılarını kaydeder ve izler.

**Log İçeriği:**
- Kim (hangi kullanıcı/rol)
- Ne zaman
- Hangi kaynak
- Hangi işlem
- Nereden (IP adresi)

**Ne Zaman Kullanılır:**
- Güvenlik denetimi
- Uyumluluk
- Sorun giderme
- "Kim bu EC2'yi sildi?" sorularına cevap

---

### Amazon CloudWatch

**Ne İşe Yarar:** AWS kaynaklarını izler ve log toplar.

**Bileşenleri:**
| Bileşen | Açıklama |
|---------|----------|
| Metrics | CPU, Network, Disk vb. metrikleri |
| Alarms | Eşik aşımında uyarı |
| Logs | Uygulama ve sistem logları |
| Events | Kaynak değişikliği olayları |
| Dashboards | Görsel panolar |

**CloudTrail vs CloudWatch:**
| Özellik | CloudTrail | CloudWatch |
|---------|------------|------------|
| Amaç | API aktivite logu | Performans izleme |
| Soru | Kim ne yaptı? | Sistem nasıl çalışıyor? |

---

### AWS Config

**Ne İşe Yarar:** Kaynak konfigürasyonlarını izler ve denetler.

**Temel Özellikler:**
- Konfigürasyon geçmişi
- Compliance rules ile uyumluluk kontrolü
- Remediation (otomatik düzeltme)

**Örnek Config Rules:**
- "Tüm S3 bucket'ları şifrelenmiş olmalı"
- "Security Group'larda 0.0.0.0/0 SSH olmamalı"

---

### AWS Organizations

**Ne İşe Yarar:** Birden fazla AWS hesabını merkezi yönetir.

**Temel Özellikler:**
- Consolidated billing (birleşik faturalama)
- Service Control Policies (SCP)
- Organizational Units (OU)

**Ne Zaman Kullanılır:**
- Multi-account strateji
- Merkezi güvenlik politikaları
- Maliyet yönetimi

---

### AWS Control Tower

**Ne İşe Yarar:** Multi-account ortamı için best-practice landing zone kurar.

**Temel Özellikler:**
- Guardrails (korkuluklar)
- Account Factory (otomatik hesap oluşturma)
- Dashboard

---

### AWS Trusted Advisor

**Ne İşe Yarar:** AWS best practice önerilerini sunar.

**Kontrol Kategorileri:**
| Kategori | Örnek |
|----------|-------|
| Cost Optimization | Kullanılmayan EC2, EBS |
| Performance | High utilization instances |
| Security | MFA, açık security group'lar |
| Fault Tolerance | Multi-AZ olmayan RDS |
| Service Limits | Limit yaklaşanlar |

**Support Plan Gereksinimleri:**
- Basic/Developer: 7 temel kontrol
- Business/Enterprise: Tüm kontroller

---

### AWS Systems Manager

**Ne İşe Yarar:** AWS ve on-premises kaynakları merkezi yönetir.

**Önemli Özellikleri:**
- **Session Manager:** Bastion host olmadan EC2 erişimi
- **Parameter Store:** Konfigürasyon ve secret saklama
- **Patch Manager:** Otomatik yamalama
- **Run Command:** Uzaktan komut çalıştırma

---

### AWS Auto Scaling

**Ne İşe Yarar:** Kaynakları otomatik olarak ölçeklendirir.

**Ölçekleme Türleri:**
| Tür | Tetikleyici |
|-----|-------------|
| Target Tracking | CPU %70'e ulaşınca |
| Step Scaling | Aşamalı ölçekleme |
| Scheduled | Zamanlanmış |

---

### AWS Well-Architected Tool

**Ne İşe Yarar:** Mimarinizi Well-Architected Framework'e göre değerlendirir.

**6 Sütun:**
1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability

---

### AWS Health Dashboard

**Ne İşe Yarar:** AWS servis durumunu ve hesabınızı etkileyen olayları gösterir.

**İki Görünüm:**
- **Service Health:** Genel AWS servis durumu
- **Your Account Health:** Sizin kaynaklarınızı etkileyen olaylar

---

### Service Quotas

**Ne İşe Yarar:** AWS servis limitlerini görüntüler ve artış talep eder.

---

### AWS Compute Optimizer

**Ne İşe Yarar:** Kaynak boyutlandırma önerileri sunar.

**Analiz Edilen Kaynaklar:**
- EC2 instances
- EBS volumes
- Lambda functions
- ECS on Fargate

---

### AWS License Manager

**Ne İşe Yarar:** Yazılım lisanslarını yönetir.

---

### AWS Service Catalog

**Ne İşe Yarar:** Onaylı IT hizmetlerinin kataloğunu oluşturur.

---

## 7. Cloud Financial Management (Maliyet)

### AWS Cost Explorer

**Ne İşe Yarar:** Maliyet verilerini görselleştirir ve analiz eder.

**Özellikleri:**
- 13 aylık geçmiş veri
- Gelecek 12 ay tahmin
- Filtreleme ve gruplama
- Savings Plans önerileri

---

### AWS Budgets

**Ne İşe Yarar:** Bütçe oluşturur ve uyarılar gönderir.

**Bütçe Türleri:**
- Cost Budget
- Usage Budget
- Reservation Budget
- Savings Plans Budget

**Örnek:**
- "Aylık EC2 harcaması $1000'ı geçerse uyar"

---

### AWS Cost and Usage Reports

**Ne İşe Yarar:** En detaylı maliyet verilerini CSV/Parquet olarak sunar.

**Ne Zaman Kullanılır:**
- Detaylı maliyet analizi
- Chargeback raporları
- Third-party araçlarla entegrasyon

---

### AWS Marketplace

**Ne İşe Yarar:** Üçüncü parti yazılım ve hizmetlerin mağazası.

**Kategoriler:**
- Security
- Networking
- Machine Learning
- DevOps
- Database

---

## 8. Machine Learning (Makine Öğrenmesi)

### Amazon SageMaker AI

**Ne İşe Yarar:** ML modellerini geliştirme, eğitme ve dağıtma platformu.

**Bileşenleri:**
- SageMaker Studio (IDE)
- SageMaker Training
- SageMaker Inference
- SageMaker Autopilot (AutoML)

---

### Amazon Rekognition

**Ne İşe Yarar:** Görüntü ve video analizi.

**Yetenekleri:**
- Yüz tanıma
- Nesne algılama
- Metin algılama (OCR)
- İçerik moderasyonu
- Ünlü tanıma

---

### Amazon Comprehend

**Ne İşe Yarar:** Doğal dil işleme (NLP) hizmeti.

**Yetenekleri:**
- Duygu analizi (sentiment)
- Entity extraction
- Dil algılama
- Konu modelleme

---

### Amazon Lex

**Ne İşe Yarar:** Chatbot oluşturma hizmeti (Alexa teknolojisi).

---

### Amazon Polly

**Ne İşe Yarar:** Metinden sese dönüştürme (Text-to-Speech).

**Özellikler:**
- 60+ dil ve lehçe
- Neural TTS (doğal ses)
- SSML desteği

---

### Amazon Transcribe

**Ne İşe Yarar:** Sesten metne dönüştürme (Speech-to-Text).

---

### Amazon Translate

**Ne İşe Yarar:** Yapay zeka ile çeviri.

---

### Amazon Textract

**Ne İşe Yarar:** Belgelerden metin ve veri çıkarma (gelişmiş OCR).

**Yetenekleri:**
- Form alanları çıkarma
- Tablo çıkarma
- El yazısı tanıma

---

### Amazon Kendra

**Ne İşe Yarar:** Kurumsal akıllı arama motoru.

---

### Amazon Q

**Ne İşe Yarar:** İş için yapay zeka asistanı.

---

## 9. Analytics (Analitik)

### Amazon Athena

**Ne İşe Yarar:** S3'teki verileri SQL ile sorgulama (Serverless).

**Özellikler:**
- Sunucu yönetimi yok
- Sadece sorgulanan veri için ödeme
- Presto/Trino tabanlı

---

### Amazon Redshift

**Ne İşe Yarar:** Veri ambarı (Data Warehouse) hizmeti.

**Özellikler:**
- Petabyte ölçeğinde veri
- Columnar storage
- Massively Parallel Processing (MPP)
- Redshift Serverless seçeneği

---

### Amazon EMR (Elastic MapReduce)

**Ne İşe Yarar:** Büyük veri işleme (Hadoop, Spark, Hive).

---

### AWS Glue

**Ne İşe Yarar:** ETL (Extract, Transform, Load) hizmeti.

**Bileşenleri:**
- Glue Data Catalog (metadata deposu)
- Glue ETL Jobs
- Glue Crawlers (otomatik şema keşfi)

---

### Amazon Kinesis

**Ne İşe Yarar:** Gerçek zamanlı veri akışı işleme.

**Servisleri:**
| Servis | Kullanım |
|--------|----------|
| Kinesis Data Streams | Ham veri akışı |
| Kinesis Data Firehose | S3, Redshift'e yükleme |
| Kinesis Data Analytics | SQL ile analiz |
| Kinesis Video Streams | Video akışı |

---

### Amazon OpenSearch Service

**Ne İşe Yarar:** Arama ve log analizi (Elasticsearch tabanlı).

---

### Amazon QuickSight

**Ne İşe Yarar:** İş zekası (BI) dashboard ve görselleştirme.

---

## 10. Application Integration (Uygulama Entegrasyonu)

### Amazon SQS (Simple Queue Service)

**Ne İşe Yarar:** Tam yönetilen mesaj kuyruğu.

**Kuyruk Türleri:**
| Tür | Özellik |
|-----|---------|
| Standard | Yüksek throughput, sıra garantisi yok |
| FIFO | Sıralı teslim, 300 msg/sn |

**Ne Zaman Kullanılır:**
- Microservice'ler arası iletişim
- İş yükü dengeleme
- Asenkron işleme

---

### Amazon SNS (Simple Notification Service)

**Ne İşe Yarar:** Pub/Sub mesajlaşma ve push bildirimleri.

**Aboneler:**
- Email
- SMS
- HTTP/HTTPS
- SQS
- Lambda

**SQS vs SNS:**
| Özellik | SQS | SNS |
|---------|-----|-----|
| Model | Queue (pull) | Pub/Sub (push) |
| Tüketici | Tek | Çoklu |
| Kalıcılık | Mesaj kuyrukta kalır | Anında iletilir |

---

### Amazon EventBridge

**Ne İşe Yarar:** Serverless event bus (olay yönlendirme).

**Olay Kaynakları:**
- AWS servisleri
- SaaS uygulamaları
- Custom uygulamalar

---

### AWS Step Functions

**Ne İşe Yarar:** Görsel workflow orchestration.

**Ne Zaman Kullanılır:**
- Lambda fonksiyonlarını zincirleme
- Karmaşık iş akışları
- ETL pipeline'ları

---

## 11. Developer Tools (Geliştirici Araçları)

### AWS CLI (Command Line Interface)

**Ne İşe Yarar:** Komut satırından AWS yönetimi.

---

### AWS CodePipeline

**Ne İşe Yarar:** CI/CD pipeline hizmeti.

**Pipeline Aşamaları:**
Source → Build → Test → Deploy

---

### AWS CodeBuild

**Ne İşe Yarar:** Tam yönetilen build hizmeti.

---

### AWS X-Ray

**Ne İşe Yarar:** Dağıtık uygulama izleme ve debugging.

---

## 12. Containers (Konteynerler)

### Amazon ECR (Elastic Container Registry)

**Ne İşe Yarar:** Docker container image'ları depolar.

---

### Amazon ECS (Elastic Container Service)

**Ne İşe Yarar:** Container orchestration hizmeti.

**Launch Türleri:**
| Tür | Yönetim |
|-----|---------|
| EC2 Launch Type | EC2 instance'ları yönetirsiniz |
| Fargate Launch Type | Serverless, AWS yönetir |

---

### Amazon EKS (Elastic Kubernetes Service)

**Ne İşe Yarar:** Yönetilen Kubernetes hizmeti.

**ECS vs EKS:**
| Özellik | ECS | EKS |
|---------|-----|-----|
| Platform | AWS native | Kubernetes |
| Öğrenme | Daha kolay | Daha zor |
| Portability | AWS'e bağlı | Multi-cloud |

---

## 13. Serverless (Sunucusuz)

### AWS Lambda

(Compute bölümünde detaylandırıldı)

---

### AWS Fargate

**Ne İşe Yarar:** Serverless container çalıştırma.

**Özellikler:**
- EC2 instance yönetimi yok
- ECS ve EKS ile kullanılır
- Kullandığın kadar öde

---

## 14. Migration & Transfer (Göç)

### AWS Migration Hub

**Ne İşe Yarar:** Göç projelerini merkezi izleme.

---

### AWS Application Migration Service

**Ne İşe Yarar:** Sunucuları AWS'e göç ettirme (lift-and-shift).

---

### AWS Database Migration Service (DMS)

**Ne İşe Yarar:** Veritabanı göçü ve replikasyonu.

**Desteklenen Senaryolar:**
- Aynı veritabanı motoru (MySQL → MySQL)
- Farklı motor (Oracle → Aurora)
- Sürekli replikasyon

---

### AWS Schema Conversion Tool (SCT)

**Ne İşe Yarar:** Veritabanı şemasını dönüştürme (Oracle → PostgreSQL gibi).

---

### AWS Snow Family

**Ne İşe Yarar:** Fiziksel veri transfer cihazları.

| Cihaz | Kapasite | Kullanım |
|-------|----------|----------|
| Snowcone | 8-14 TB | Küçük transferler, edge |
| Snowball Edge | 80 TB | Orta ölçekli transferler |
| Snowmobile | 100 PB | Çok büyük transferler (tır) |

---

### AWS Application Discovery Service

**Ne İşe Yarar:** On-premises altyapı keşfi.

---

### Migration Evaluator

**Ne İşe Yarar:** Göç maliyet analizi.

---

## 15. End User Computing

### Amazon WorkSpaces

**Ne İşe Yarar:** Sanal masaüstü (VDI) hizmeti.

---

### Amazon AppStream 2.0

**Ne İşe Yarar:** Uygulama streaming hizmeti.

---

### Amazon WorkSpaces Secure Browser

**Ne İşe Yarar:** Yönetilen güvenli web tarayıcı.

---

## 16. Frontend Web & Mobile

### AWS Amplify

**Ne İşe Yarar:** Full-stack web ve mobil uygulama geliştirme platformu.

**Bileşenleri:**
- Amplify Hosting
- Amplify Studio
- Amplify CLI

---

### AWS AppSync

**Ne İşe Yarar:** Yönetilen GraphQL API hizmeti.

---

## 17. Business Applications

### Amazon Connect

**Ne İşe Yarar:** Bulut tabanlı çağrı merkezi.

---

### Amazon SES (Simple Email Service)

**Ne İşe Yarar:** E-posta gönderme hizmeti.

---

## 18. Internet of Things (IoT)

### AWS IoT Core

**Ne İşe Yarar:** IoT cihazlarını buluta bağlama.

**Protokoller:**
- MQTT
- HTTPS
- WebSocket

---

## 19. Customer Enablement

### AWS Support

**Ne İşe Yarar:** Teknik destek hizmetleri.

**Support Planları:**

| Plan | Özellikler | Fiyat |
|------|------------|-------|
| **Basic** | Dokümantasyon, forum, Service Health | Ücretsiz |
| **Developer** | Business hours email, 1 kişi | $29/ay~ |
| **Business** | 24/7 telefon, tüm Trusted Advisor, 1 saat yanıt | $100/ay~ |
| **Enterprise On-Ramp** | TAM pool, 30 dk kritik yanıt | $5,500/ay~ |
| **Enterprise** | Dedicated TAM, 15 dk kritik yanıt, Concierge | $15,000/ay~ |

**TAM:** Technical Account Manager

---

## Hızlı Referans Tablosu

### Servis Seçimi Rehberi

| İhtiyaç | Servis |
|---------|--------|
| Sanal sunucu | EC2 |
| Serverless fonksiyon | Lambda |
| Container orchestration | ECS veya EKS |
| İlişkisel veritabanı | RDS veya Aurora |
| NoSQL veritabanı | DynamoDB |
| Nesne depolama | S3 |
| Blok depolama | EBS |
| Dosya sistemi | EFS veya FSx |
| CDN | CloudFront |
| DNS | Route 53 |
| VPN | Site-to-Site VPN |
| Özel bağlantı | Direct Connect |
| Kimlik yönetimi | IAM |
| Secret saklama | Secrets Manager |
| Şifreleme anahtarları | KMS |
| DDoS koruması | Shield |
| Web güvenlik duvarı | WAF |
| Tehdit algılama | GuardDuty |
| Log izleme | CloudWatch |
| API aktivite logu | CloudTrail |
| Konfigürasyon denetimi | Config |
| IaC | CloudFormation |
| Maliyet analizi | Cost Explorer |
| ML platform | SageMaker |
| Görüntü analizi | Rekognition |
| Chatbot | Lex |
| Text-to-Speech | Polly |
| Speech-to-Text | Transcribe |
| Mesaj kuyruğu | SQS |
| Push bildirimi | SNS |
| Veri ambarı | Redshift |
| S3 SQL sorgusu | Athena |
| ETL | Glue |
| Real-time streaming | Kinesis |
| CI/CD | CodePipeline |

---

## Sınav İpuçları

1. **Her servisin temel amacını bilin** - Detaylara değil, "ne zaman kullanılır" sorusuna odaklanın.

2. **Benzer servisleri karşılaştırın:**
   - EBS vs EFS vs S3
   - SQS vs SNS
   - CloudTrail vs CloudWatch vs Config
   - RDS vs DynamoDB
   - EC2 vs Lambda vs Fargate

3. **Shared Responsibility Model'i unutmayın:**
   - AWS: Altyapı güvenliği
   - Müşteri: Veri, erişim, konfigürasyon güvenliği

4. **Well-Architected Framework 6 sütununu bilin**

5. **Fiyatlandırma modellerini anlayın:**
   - On-Demand vs Reserved vs Spot
   - Serverless = Kullandığın kadar öde

---

> **Son Güncelleme:** Bu doküman AWS CLF-C02 sınav rehberine göre hazırlanmıştır.
>
> **Resmi Kaynak:** [AWS Certification Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

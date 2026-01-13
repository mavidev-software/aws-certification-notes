# Task 3.2: AWS Global Infrastructure

> **Resmi Tanım:** Define the AWS global infrastructure
>
> **Kaynak:** [AWS CLF-C02 Exam Guide - Domain 3](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain3.html)

---

## Bu Task'ta Öğrenilecekler

### Bilgi Alanları (Knowledge)
- AWS Regions, Availability Zones ve edge locations
- High availability (yüksek erişilebilirlik)
- Birden fazla Region kullanımı
- Edge location'ların faydaları

### Beceriler (Skills)
- Region'lar, AZ'ler ve edge location'lar arasındaki ilişkileri tanımlama
- Birden fazla AZ kullanarak yüksek erişilebilirlik sağlama
- AZ'lerin single point of failure paylaşmadığını anlama
- Birden fazla Region kullanım senaryolarını belirleme

---

## 1. AWS Global Infrastructure Genel Bakış

```
┌─────────────────────────────────────────────────────────────────┐
│                 AWS GLOBAL INFRASTRUCTURE                        │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                      REGION                              │   │
│  │              (Coğrafi bölge, 2+ AZ)                      │   │
│  │                                                          │   │
│  │   ┌─────────────┐ ┌─────────────┐ ┌─────────────┐       │   │
│  │   │    AZ-a     │ │    AZ-b     │ │    AZ-c     │       │   │
│  │   │             │ │             │ │             │       │   │
│  │   │  ┌───────┐  │ │  ┌───────┐  │ │  ┌───────┐  │       │   │
│  │   │  │ Data  │  │ │  │ Data  │  │ │  │ Data  │  │       │   │
│  │   │  │Center │  │ │  │Center │  │ │  │Center │  │       │   │
│  │   │  │  (1+) │  │ │  │  (1+) │  │ │  │  (1+) │  │       │   │
│  │   │  └───────┘  │ │  └───────┘  │ │  └───────┘  │       │   │
│  │   └──────┬──────┘ └──────┬──────┘ └──────┬──────┘       │   │
│  │          │               │               │               │   │
│  │          └───────────────┼───────────────┘               │   │
│  │              Yüksek hızlı, düşük latency                 │   │
│  │                   fiber bağlantı                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐       │
│  │   Edge    │ │   Edge    │ │   Edge    │ │   Edge    │       │
│  │ Location  │ │ Location  │ │ Location  │ │ Location  │       │
│  │  (450+)   │ │           │ │           │ │           │       │
│  └───────────┘ └───────────┘ └───────────┘ └───────────┘       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Güncel Sayılar (2024)

| Bileşen | Sayı |
|---------|------|
| **Regions** | 33+ |
| **Availability Zones** | 105+ |
| **Edge Locations** | 450+ |
| **Local Zones** | 30+ |
| **Wavelength Zones** | 30+ |

---

## 2. AWS Regions

### 2.1 Region Nedir?

**Tanım:** Coğrafi olarak ayrı bir alanda bulunan, birden fazla Availability Zone içeren fiziksel konum.

**Özellikler:**
- Her Region en az 2 AZ içerir (çoğunlukla 3+)
- Region'lar birbirinden tamamen izole
- Veriler Region'dan çıkmaz (siz taşımadıkça)
- Her Region'un kendine özgü servisleri olabilir

**Region İsimlendirme:**
```
Region Code          Region Name
───────────────────────────────────────
us-east-1           N. Virginia (ABD)
us-west-2           Oregon (ABD)
eu-west-1           Ireland (Avrupa)
eu-central-1        Frankfurt (Avrupa)
ap-northeast-1      Tokyo (Asya)
ap-southeast-1      Singapore (Asya)
me-south-1          Bahrain (Orta Doğu)
sa-east-1           São Paulo (Güney Amerika)
```

### 2.2 Region Seçim Kriterleri

```
┌─────────────────────────────────────────────────────────────────┐
│                   REGION SEÇİM KRİTERLERİ                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. COMPLIANCE (Uyumluluk)                                     │
│     └─► Veri hangi ülkede kalmalı?                             │
│     └─► GDPR, KVKK, HIPAA gereksinimleri                       │
│                                                                 │
│  2. LATENCY (Gecikme)                                          │
│     └─► Kullanıcılara en yakın Region                          │
│     └─► Milisaniyeler önemli                                   │
│                                                                 │
│  3. SERVICE AVAILABILITY (Servis Mevcudiyeti)                  │
│     └─► İstenen servis o Region'da var mı?                     │
│     └─► Yeni servisler önce us-east-1'de                       │
│                                                                 │
│  4. PRICING (Fiyatlandırma)                                    │
│     └─► Region'lar arası fiyat farkı olabilir                  │
│     └─► São Paulo genellikle daha pahalı                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Sınav Sorusu Örneği:**
```
Soru: "Bir şirket GDPR uyumluluğu için müşteri verilerinin
       Avrupa'da kalmasını sağlamalı. Hangi Region kullanılmalı?"

Cevap: eu-west-1 (Ireland) veya eu-central-1 (Frankfurt)
       Avrupa Region'larından biri seçilmeli.
```

---

## 3. Availability Zones (AZ)

### 3.1 AZ Nedir?

**Tanım:** Bir Region içinde bulunan, bir veya daha fazla fiziksel veri merkezinden oluşan izole altyapı.

```
┌─────────────────────────────────────────────────────────────────┐
│                    AVAILABILITY ZONE                             │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                      AZ-a                                │   │
│   │                                                          │   │
│   │   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │   │
│   │   │ Data Center 1│  │ Data Center 2│  │ Data Center 3│  │   │
│   │   │              │  │              │  │              │  │   │
│   │   │ • Sunucular  │  │ • Sunucular  │  │ • Sunucular  │  │   │
│   │   │ • Storage    │  │ • Storage    │  │ • Storage    │  │   │
│   │   │ • Network    │  │ • Network    │  │ • Network    │  │   │
│   │   │              │  │              │  │              │  │   │
│   │   │ Güç: Ayrı    │  │ Güç: Ayrı    │  │ Güç: Ayrı    │  │   │
│   │   │ Soğutma: Ayrı│  │ Soğutma: Ayrı│  │ Soğutma: Ayrı│  │   │
│   │   └──────────────┘  └──────────────┘  └──────────────┘  │   │
│   │                                                          │   │
│   │   Tüm data center'lar aynı AZ'de, birbirine çok yakın   │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 3.2 AZ Özellikleri

| Özellik | Açıklama |
|---------|----------|
| **İzolasyon** | Her AZ bağımsız güç, soğutma, ağ |
| **Mesafe** | AZ'ler birbirinden km'lerce uzakta |
| **Bağlantı** | Yüksek hızlı, düşük latency fiber |
| **Latency** | AZ'ler arası < 2ms |
| **Tanımlama** | use1-az1, use1-az2 (hesaba özgü mapping) |

### 3.3 Single Point of Failure (SPOF)

```
TEK AZ KULLANIMI (Kötü Uygulama):
─────────────────────────────────
┌─────────────┐
│    AZ-a     │
│  ┌───────┐  │
│  │  EC2  │  │  ← AZ-a çökerse = Uygulama çöker
│  │  RDS  │  │
│  └───────┘  │
└─────────────┘
⚠️ SPOF VAR!


MULTI-AZ KULLANIMI (İyi Uygulama):
──────────────────────────────────
┌─────────────┐     ┌─────────────┐
│    AZ-a     │     │    AZ-b     │
│  ┌───────┐  │     │  ┌───────┐  │
│  │  EC2  │  │     │  │  EC2  │  │
│  │  RDS  │◄─┼─────┼─►│  RDS  │  │
│  │(Primary)│  │     │  │(Standby)│  │
│  └───────┘  │     │  └───────┘  │
└─────────────┘     └─────────────┘
✅ AZ-a çökerse = AZ-b devralır
✅ SPOF YOK!
```

**⚠️ Kritik Bilgi:**
```
AZ'ler single point of failure PAYLAŞMAZ:
• Ayrı güç kaynakları
• Ayrı soğutma sistemleri
• Ayrı ağ bağlantıları
• Farklı fiziksel konumlar
```

---

## 4. Edge Locations

### 4.1 Edge Location Nedir?

**Tanım:** İçerik önbelleğe alma (caching) ve son kullanıcıya yakın servis sunumu için kullanılan AWS altyapısı.

```
┌─────────────────────────────────────────────────────────────────┐
│                      EDGE LOCATIONS                              │
│                                                                 │
│   Kullanıcı (İstanbul)                                         │
│        │                                                        │
│        ▼                                                        │
│   ┌─────────────┐                                              │
│   │    Edge     │◄─── CloudFront cache                         │
│   │  Location   │     (İstanbul yakını)                        │
│   │  (İstanbul) │                                              │
│   └──────┬──────┘                                              │
│          │                                                      │
│          │ Cache MISS ise                                       │
│          ▼                                                      │
│   ┌─────────────┐                                              │
│   │   Origin    │                                              │
│   │  (S3/EC2)   │                                              │
│   │ eu-west-1   │                                              │
│   └─────────────┘                                              │
│                                                                 │
│   İlk istek: Origin'den al, edge'de cache'le                   │
│   Sonraki istekler: Edge'den hızlı teslim                      │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 Edge Location Kullanan Servisler

| Servis | Kullanım |
|--------|----------|
| **CloudFront** | CDN - İçerik dağıtımı |
| **Route 53** | DNS çözümleme |
| **AWS Global Accelerator** | Uygulama hızlandırma |
| **AWS WAF** | Web güvenlik duvarı |
| **AWS Shield** | DDoS koruması |
| **Lambda@Edge** | Edge'de kod çalıştırma |

---

## 5. Local Zones ve Wavelength

### 5.1 Local Zones

**Tanım:** Region'ın bir uzantısı, büyük şehirlere yakın altyapı.

```
┌─────────────────────────────────────────────────────────────────┐
│                       LOCAL ZONES                                │
│                                                                 │
│   ┌─────────────────┐        ┌─────────────────┐               │
│   │    Region       │        │   Local Zone    │               │
│   │   (us-west-2)   │◄──────►│   (Los Angeles) │               │
│   │    Oregon       │        │                 │               │
│   └─────────────────┘        └─────────────────┘               │
│                                                                 │
│   Kullanım: Düşük latency gerektiren uygulamalar               │
│   - Gerçek zamanlı oyunlar                                     │
│   - Video streaming                                             │
│   - AR/VR uygulamaları                                         │
└─────────────────────────────────────────────────────────────────┘
```

### 5.2 Wavelength Zones

**Tanım:** 5G operatör ağlarının içine yerleştirilmiş AWS altyapısı.

```
┌─────────────────────────────────────────────────────────────────┐
│                     WAVELENGTH ZONES                             │
│                                                                 │
│   Mobil Cihaz ──► 5G Ağı ──► Wavelength Zone ──► AWS Region    │
│                                    │                            │
│                              Ultra düşük                        │
│                               latency                           │
│                            (< 10ms)                             │
│                                                                 │
│   Kullanım:                                                    │
│   - IoT uygulamaları                                           │
│   - Mobil oyunlar                                              │
│   - Gerçek zamanlı analitik                                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 6. Servis Dayanıklılık Seviyeleri

AWS servisleri farklı dayanıklılık seviyelerinde çalışır:

```
┌─────────────────────────────────────────────────────────────────┐
│                 SERVİS DAYANIKLILIK SEVİYELERİ                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  GLOBALLY RESILIENT (Global Dayanıklı)                         │
│  ─────────────────────────────────────                         │
│  • Region çökse bile çalışır                                   │
│  • Veriler global olarak replike                               │
│  • Örnekler: IAM, Route 53, CloudFront                         │
│                                                                 │
│  REGIONALLY RESILIENT (Bölgesel Dayanıklı)                     │
│  ─────────────────────────────────────────                     │
│  • Tek Region içinde çalışır                                   │
│  • AZ'ler arası replikasyon                                    │
│  • Örnekler: S3, DynamoDB, EFS, Lambda                         │
│                                                                 │
│  AZ RESILIENT (AZ Dayanıklı)                                   │
│  ────────────────────────────                                   │
│  • Tek AZ'de çalışır                                           │
│  • AZ çökerse servis çöker                                     │
│  • Örnekler: EBS, EC2 (varsayılan), RDS (single-AZ)           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Servis Dayanıklılık Tablosu

| Servis | Dayanıklılık | Açıklama |
|--------|--------------|----------|
| **IAM** | Global | Tüm Region'larda aynı |
| **Route 53** | Global | DNS global servisi |
| **CloudFront** | Global | CDN edge location'lar |
| **S3** | Regional | 3 AZ'de otomatik replikasyon |
| **DynamoDB** | Regional | Multi-AZ varsayılan |
| **Lambda** | Regional | Multi-AZ otomatik |
| **EFS** | Regional | Multi-AZ dosya sistemi |
| **EC2** | AZ | Belirli AZ'de çalışır |
| **EBS** | AZ | Belirli AZ'de depolama |
| **RDS (Single)** | AZ | Tek AZ'de veritabanı |
| **RDS (Multi-AZ)** | Regional | Multi-AZ yüksek erişilebilirlik |

---

## 7. High Availability (Yüksek Erişilebilirlik)

### 7.1 HA Konsepti

```
High Availability = Sistemin sürekli çalışır durumda olması

Ölçüm: Uptime yüzdesi
───────────────────────────────────────────────
99%      = 3.65 gün/yıl kesinti
99.9%    = 8.76 saat/yıl kesinti
99.99%   = 52.6 dakika/yıl kesinti
99.999%  = 5.26 dakika/yıl kesinti (Five 9's)
```

### 7.2 Multi-AZ ile HA

```
┌─────────────────────────────────────────────────────────────────┐
│              MULTI-AZ HIGH AVAILABILITY                          │
│                                                                 │
│                    ┌─────────────────┐                         │
│                    │  Load Balancer  │                         │
│                    │   (Multi-AZ)    │                         │
│                    └────────┬────────┘                         │
│                             │                                   │
│            ┌────────────────┼────────────────┐                 │
│            ▼                ▼                ▼                 │
│     ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│     │    AZ-a     │  │    AZ-b     │  │    AZ-c     │         │
│     │  ┌───────┐  │  │  ┌───────┐  │  │  ┌───────┐  │         │
│     │  │  EC2  │  │  │  │  EC2  │  │  │  │  EC2  │  │         │
│     │  └───────┘  │  │  └───────┘  │  │  └───────┘  │         │
│     │  ┌───────┐  │  │  ┌───────┐  │  │  ┌───────┐  │         │
│     │  │  RDS  │  │  │  │  RDS  │  │  │             │         │
│     │  │(Primary)│  │  │(Standby)│  │  │             │         │
│     │  └───────┘  │  │  └───────┘  │  │             │         │
│     └─────────────┘  └─────────────┘  └─────────────┘         │
│                                                                 │
│   ✅ Herhangi bir AZ çökerse sistem çalışmaya devam eder       │
└─────────────────────────────────────────────────────────────────┘
```

---

## 8. Multi-Region Kullanım Senaryoları

### 8.1 Ne Zaman Multi-Region?

| Senaryo | Açıklama |
|---------|----------|
| **Disaster Recovery** | Tüm Region çökse bile çalışma |
| **Business Continuity** | İş sürekliliği garantisi |
| **Low Latency** | Global kullanıcılara yakınlık |
| **Data Sovereignty** | Farklı ülkelerde veri tutma |

### 8.2 Multi-Region Mimari

```
┌─────────────────────────────────────────────────────────────────┐
│                    MULTI-REGION ARCHITECTURE                     │
│                                                                 │
│                    ┌─────────────────┐                         │
│                    │    Route 53     │                         │
│                    │  (DNS Failover) │                         │
│                    └────────┬────────┘                         │
│                             │                                   │
│            ┌────────────────┴────────────────┐                 │
│            ▼                                 ▼                 │
│   ┌─────────────────┐              ┌─────────────────┐        │
│   │   eu-west-1     │              │   us-east-1     │        │
│   │   (Primary)     │              │   (Secondary)   │        │
│   │                 │              │                 │        │
│   │  ┌───────────┐  │   S3 Cross   │  ┌───────────┐  │        │
│   │  │    App    │  │    Region    │  │    App    │  │        │
│   │  │  Servers  │  │  Replication │  │  Servers  │  │        │
│   │  └───────────┘  │◄────────────►│  └───────────┘  │        │
│   │  ┌───────────┐  │              │  ┌───────────┐  │        │
│   │  │    RDS    │  │   Aurora     │  │    RDS    │  │        │
│   │  │           │◄─┼──Global DB──►│  │ (Replica) │  │        │
│   │  └───────────┘  │              │  └───────────┘  │        │
│   └─────────────────┘              └─────────────────┘        │
│                                                                 │
│   Primary Region çökerse → Route 53 trafiği Secondary'ye yönlendirir
└─────────────────────────────────────────────────────────────────┘
```

---

## 9. CloudFront vs Global Accelerator

Her ikisi de edge location kullanır ancak farklı amaçlara hizmet eder:

```
┌─────────────────────────────────────────────────────────────────┐
│            CLOUDFRONT vs GLOBAL ACCELERATOR                      │
├────────────────────────────┬────────────────────────────────────┤
│        CLOUDFRONT          │       GLOBAL ACCELERATOR           │
├────────────────────────────┼────────────────────────────────────┤
│                            │                                    │
│  Amaç: İçerik dağıtımı     │  Amaç: Uygulama hızlandırma       │
│        (CDN)               │                                    │
│                            │                                    │
│  Cache: ✅ VAR             │  Cache: ❌ YOK                     │
│                            │                                    │
│  Protokol: HTTP/HTTPS      │  Protokol: TCP/UDP                │
│                            │                                    │
│  IP: DNS tabanlı           │  IP: Static Anycast               │
│      (değişebilir)         │      (sabit 2 IP)                 │
│                            │                                    │
│  Kullanım:                 │  Kullanım:                        │
│  • Web siteleri            │  • Gaming                         │
│  • Video streaming         │  • IoT                            │
│  • Statik içerik           │  • VoIP                           │
│  • API hızlandırma         │  • Non-HTTP apps                  │
│                            │                                    │
└────────────────────────────┴────────────────────────────────────┘
```

**Sınav Sorusu Örneği:**
```
Soru: "Bir şirket statik web sitesi içeriğini global kullanıcılara
       hızlı teslim etmek istiyor. Hangi servis kullanılmalı?"

Cevap: CloudFront (CDN, caching ile içerik dağıtımı)

Soru: "Bir oyun şirketi UDP tabanlı oyun trafiğini optimize etmek
       ve sabit IP adresi kullanmak istiyor. Hangi servis?"

Cevap: Global Accelerator (TCP/UDP, static IP)
```

---

## 10. Sınav Senaryoları

| Senaryo | Çözüm |
|---------|-------|
| "GDPR uyumluluğu, veri AB'de kalmalı" | EU Region seçimi |
| "Global kullanıcılar için düşük latency" | CloudFront + Multi-Region |
| "Tüm Region çökse bile çalışmalı" | Multi-Region + Route 53 Failover |
| "Tek AZ çökse bile çalışmalı" | Multi-AZ deployment |
| "Sabit IP ile global hızlandırma" | Global Accelerator |
| "Statik içerik caching" | CloudFront |
| "5G mobil için ultra düşük latency" | Wavelength Zone |
| "Büyük şehre yakın compute" | Local Zone |

---

## 11. Anahtar Terimler Sözlüğü

| Terim | Tanım |
|-------|-------|
| **Region** | Coğrafi bölge, 2+ AZ içerir |
| **AZ** | Availability Zone, 1+ data center |
| **Edge Location** | CDN cache noktası |
| **Local Zone** | Region uzantısı, şehirlere yakın |
| **Wavelength Zone** | 5G ağı içinde AWS altyapısı |
| **HA** | High Availability, yüksek erişilebilirlik |
| **DR** | Disaster Recovery, felaket kurtarma |
| **SPOF** | Single Point of Failure |
| **Latency** | Gecikme süresi |

---

## 12. Sınav İpuçları

```
✓ Region = 2+ AZ içerir
✓ AZ = 1+ Data Center içerir
✓ AZ'ler SPOF PAYLAŞMAZ (ayrı güç, soğutma, ağ)
✓ Veri Region'dan çıkmaz (siz taşımadıkça)

✓ Global servisler: IAM, Route 53, CloudFront
✓ Regional servisler: S3, Lambda, DynamoDB
✓ AZ servisler: EC2, EBS

✓ Multi-AZ = High Availability
✓ Multi-Region = Disaster Recovery

✓ CloudFront = Cache VAR, HTTP/HTTPS, içerik dağıtımı
✓ Global Accelerator = Cache YOK, TCP/UDP, sabit IP

✓ Region seçimi: Compliance > Latency > Service > Price
```

---

## Referanslar

- [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
- [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)
- [AWS Local Zones](https://aws.amazon.com/about-aws/global-infrastructure/localzones/)
- [AWS Wavelength](https://aws.amazon.com/wavelength/)
- [Amazon CloudFront](https://docs.aws.amazon.com/cloudfront/)
- [AWS Global Accelerator](https://docs.aws.amazon.com/global-accelerator/)


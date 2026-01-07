# Domain 1: Cloud Concepts (Bulut Kavramları)

> **Sınav Ağırlığı: %24**
>
> **Kaynak:** [AWS Official Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain1.html)

## Resmi Task Statement'lar

| Task    | Konu                                          |
| ------- | --------------------------------------------- |
| **1.1** | Define the benefits of the AWS Cloud          |
| **1.2** | Identify design principles (Well-Architected) |
| **1.3** | Migration strategies (AWS CAF)                |
| **1.4** | Cloud economics                               |

---

## Task 1.1: AWS Cloud'un Faydaları

### Cloud'un 6 Temel Avantajı

| Avantaj                        | Açıklama                                     | Anahtar Kelime  |
| ------------------------------ | -------------------------------------------- | --------------- |
| **Trade CapEx for OpEx**       | Sermaye harcaması yerine operasyonel harcama | CapEx → OpEx    |
| **Economies of Scale**         | AWS'in büyük ölçeği sayesinde düşük fiyat    | Ölçek ekonomisi |
| **Stop Guessing Capacity**     | Kapasite tahmini yapmaya gerek yok           | Tahmin yok      |
| **Increase Speed & Agility**   | Dakikalar içinde kaynak oluşturma            | Çeviklik        |
| **No Data Center Maintenance** | Veri merkezi yönetimi yok                    | Bakım yok       |
| **Go Global in Minutes**       | Dakikalar içinde global dağıtım              | Global erişim   |

---

### Elasticity vs Scalability - KRİTİK!

```
┌─────────────────────────────────────────────────────────────┐
│  SINAV SORUSU: "scale up AND down" → ELASTICITY             │
│  SINAV SORUSU: "handle increased load" → SCALABILITY        │
└─────────────────────────────────────────────────────────────┘
```

| Kavram          | Tanım                             | Yön          | Anahtar İfade                    |
| --------------- | --------------------------------- | ------------ | -------------------------------- |
| **Elasticity**  | Talebe göre kaynak ekleme/ÇIKARMA | ↑↓ İki yönlü | "variable demand", "up AND down" |
| **Scalability** | Artan yükü karşılama kapasitesi   | ↑ Tek yönlü  | "grow", "increased load"         |

### Görsel Açıklama

```
ELASTICITY (Esneklik):
══════════════════════
Talep ↑ → Kaynak ↑  (scale up)
Talep ↓ → Kaynak ↓  (scale down) ← BU ÖNEMLİ!

Örnek: Black Friday'de 100 sunucu, normal günde 10 sunucu


SCALABILITY (Ölçeklenebilirlik):
════════════════════════════════
Sistem büyüyebilir mi? → Evet/Hayır
Kapasite artırma yeteneği

Örnek: 10 sunucudan 100 sunucuya çıkabilme kapasitesi
```

### Agility, Elasticity, Scalability Karşılaştırması

| Kavram          | Anlam                                        | Örnek                          |
| --------------- | -------------------------------------------- | ------------------------------ |
| **Agility**     | Hızlı geliştirme ve dağıtım (zaman)          | Dakikalar içinde sunucu kurma  |
| **Elasticity**  | Kaynak miktarını dinamik ayarlama (kapasite) | Otomatik ölçekleme             |
| **Scalability** | Büyüme kapasitesi                            | 10'dan 1000 sunucuya çıkabilme |

---

### AWS Global Altyapısı

#### Region Seçim Kriterleri (Öncelik Sırasına Göre)

```
1. COMPLIANCE (Uyumluluk)     ← İLK ÖNCELİK!
   └── Veri yerelliği, GDPR, yasal gereksinimler

2. LATENCY (Gecikme)
   └── Kullanıcılara yakınlık

3. SERVICE AVAILABILITY
   └── İstenen servisler mevcut mu?

4. PRICING
   └── Region'lar arası fiyat farkları
```

#### Global Altyapı Hiyerarşisi

```
AWS Global Infrastructure
│
├── Region (Bölge) - Örn: eu-west-1 (İrlanda)
│   │
│   ├── Availability Zone (AZ) - En az 2-3 AZ per Region
│   │   └── Data Center(s) - Fiziksel veri merkezleri
│   │
│   ├── Availability Zone
│   │   └── Data Center(s)
│   │
│   └── Availability Zone
│       └── Data Center(s)
│
├── Edge Location (400+)
│   └── CloudFront CDN için içerik cache
│
├── Regional Edge Cache
│   └── Edge Location'lar için ara cache
│
├── Local Zone
│   └── Büyük şehirlerde düşük gecikme
│
└── Wavelength Zone
    └── 5G ağlarında ultra düşük gecikme
```

#### Yüksek Erişilebilirlik (High Availability)

| Strateji            | Açıklama                           | Kullanım           |
| ------------------- | ---------------------------------- | ------------------ |
| **Multi-AZ**        | Aynı region içinde birden fazla AZ | RDS, ELB           |
| **Multi-Region**    | Farklı region'larda dağıtım        | Global uygulamalar |
| **Fault Tolerance** | Hata durumunda otomatik devir      | Kritik sistemler   |

#### AZ (Availability Zone) Özellikleri

- Her AZ **fiziksel olarak ayrı** veri merkezleri
- AZ'ler arası **düşük gecikmeli** bağlantı
- AZ'ler birbirinden **onlarca km uzakta**
- Bir AZ çökse bile diğerleri çalışmaya devam eder

---

### Cloud Dağıtım ve Hizmet Modelleri

#### 3 Dağıtım Modeli

| Model             | Açıklama                  | Ne Zaman                   | Örnek                |
| ----------------- | ------------------------- | -------------------------- | -------------------- |
| **Public Cloud**  | Tamamen AWS üzerinde      | Yeni projeler, startup'lar | Tüm kaynaklar AWS'de |
| **Private Cloud** | On-premises               | Düzenleyici gereksinimler  | VMware, OpenStack    |
| **Hybrid Cloud**  | Public + Private birlikte | Geçiş dönemleri            | AWS + On-prem DC     |

#### Cloud Hizmet Modelleri (IaaS, PaaS, SaaS)

```
┌─────────────────────────────────────────────────────────────┐
│              SORUMLULUK DAĞILIMI                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  On-Premises    IaaS         PaaS         SaaS             │
│  ───────────    ────         ────         ────             │
│  Applications   Applications Applications │AWS│            │
│  Data           Data         Data         │   │            │
│  Runtime        Runtime      │AWS│        │   │            │
│  Middleware     Middleware   │   │        │   │            │
│  OS             OS           │   │        │   │            │
│  Virtualization │AWS│        │   │        │   │            │
│  Servers        │   │        │   │        │   │            │
│  Storage        │   │        │   │        │   │            │
│  Networking     │   │        │   │        │   │            │
│                                                             │
│  █ = Müşteri Yönetir    │AWS│ = AWS Yönetir               │
└─────────────────────────────────────────────────────────────┘
```

| Model    | AWS Örneği                 | Müşteri Yönetir         | Kontrol |
| -------- | -------------------------- | ----------------------- | ------- |
| **IaaS** | EC2, VPC                   | OS, runtime, data, apps | En çok  |
| **PaaS** | Elastic Beanstalk, RDS     | Data, apps              | Orta    |
| **SaaS** | WorkSpaces, Chime, Connect | Sadece kullanım         | En az   |

---

## Task 1.2: Well-Architected Framework - 6 Sütun

```
┌──────────────────────────────────────────────────────────┐
│  O-S-R-P-C-S (Ezberle!)                                  │
│  Operational - Security - Reliability -                  │
│  Performance - Cost - Sustainability                     │
└──────────────────────────────────────────────────────────┘
```

| #   | Sütun                      | Odak                        | Örnek Uygulama                    |
| --- | -------------------------- | --------------------------- | --------------------------------- |
| 1   | **Operational Excellence** | Operasyonları iyileştirme   | IaC, otomasyon, monitoring        |
| 2   | **Security**               | Veri ve sistem koruma       | IAM, şifreleme, least privilege   |
| 3   | **Reliability**            | Hatalardan kurtulma         | Multi-AZ, yedekleme, auto-scaling |
| 4   | **Performance Efficiency** | Kaynakları verimli kullanma | Doğru instance tipi seçimi        |
| 5   | **Cost Optimization**      | Gereksiz harcamayı önleme   | Reserved Instance, right-sizing   |
| 6   | **Sustainability**         | Çevresel etki azaltma       | Verimli kaynak kullanımı          |

### Her Sütun İçin Anahtar Sorular

| Sütun                  | Soru                                     |
| ---------------------- | ---------------------------------------- |
| Operational Excellence | "Operasyonları nasıl iyileştirebiliriz?" |
| Security               | "Veri ve sistemleri nasıl koruruz?"      |
| Reliability            | "Hatalardan nasıl kurtuluruz?"           |
| Performance Efficiency | "Kaynakları nasıl verimli kullanırız?"   |
| Cost Optimization      | "Gereksiz harcamayı nasıl önleriz?"      |
| Sustainability         | "Çevresel etkiyi nasıl azaltırız?"       |

---

## Task 1.3: Migration to AWS Cloud

> AWS resmi dokümanından: _"Understanding the benefits of and strategies for migration to the AWS Cloud"_

### AWS Cloud Adoption Framework (AWS CAF)

AWS CAF, bulut göçü için rehberlik sağlayan bir framework'tür.

#### CAF'ın 6 Perspektifi

```
┌─────────────────────────────────────────────────────────────┐
│                    AWS CAF PERSPEKTİFLERİ                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  İŞ ODAKLI (Business)          TEKNİK ODAKLI (Technical)   │
│  ═══════════════════           ════════════════════════     │
│  • Business                    • Platform                   │
│  • People                      • Security                   │
│  • Governance                  • Operations                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

| Perspektif     | Odak                 | Stakeholder               |
| -------------- | -------------------- | ------------------------- |
| **Business**   | İş değeri, ROI       | CEO, CFO, CIO             |
| **People**     | Organizasyon, eğitim | HR, Yöneticiler           |
| **Governance** | Risk, uyumluluk      | CIO, Enterprise Architect |
| **Platform**   | Altyapı, mimari      | CTO, Architects           |
| **Security**   | Güvenlik kontrolü    | CISO, Security Team       |
| **Operations** | Operasyonel süreçler | IT Operations             |

#### CAF'ın Faydaları

AWS resmi dokümanından:

- **Reduced business risk** (İş riskini azaltma)
- **Improved ESG performance** (Environmental, Social, Governance)
- **Increased revenue** (Gelir artışı)
- **Increased operational efficiency** (Operasyonel verimlilik)

### Migration Strategies - 7 R's

```
┌─────────────────────────────────────────────────────────────┐
│                      7 R's of MIGRATION                     │
└─────────────────────────────────────────────────────────────┘
```

| Strateji       | Açıklama                                 | Örnek                           |
| -------------- | ---------------------------------------- | ------------------------------- |
| **Retire**     | Kullanılmayanları kapat                  | Eski, kullanılmayan uygulamalar |
| **Retain**     | Şimdilik yerinde bırak                   | Karmaşık legacy sistemler       |
| **Rehost**     | "Lift and shift" - Olduğu gibi taşı      | VM'leri EC2'ye taşıma           |
| **Relocate**   | VMware Cloud on AWS'e taşı               | vSphere workload'ları           |
| **Repurchase** | SaaS'a geç                               | CRM → Salesforce                |
| **Replatform** | "Lift and reshape" - Küçük değişiklikler | DB → RDS                        |
| **Refactor**   | Cloud-native olarak yeniden yaz          | Monolith → Microservices        |

### Migration Araçları

| Araç                                     | Kullanım                     |
| ---------------------------------------- | ---------------------------- |
| **AWS Migration Hub**                    | Merkezi göç izleme           |
| **AWS Application Migration Service**    | Sunucu göçü (lift-and-shift) |
| **AWS Database Migration Service (DMS)** | Veritabanı göçü              |
| **AWS Snow Family**                      | Fiziksel veri transferi      |
| **AWS DataSync**                         | Online veri transferi        |

### AWS Snow Family

| Cihaz             | Kapasite | Kullanım                          |
| ----------------- | -------- | --------------------------------- |
| **Snowcone**      | 8-14 TB  | Küçük transferler, edge computing |
| **Snowball Edge** | 80 TB    | Orta ölçekli transferler          |
| **Snowmobile**    | 100 PB   | Exabyte ölçeğinde veri (kamyon!)  |

---

## Task 1.4: Cloud Economics

> AWS resmi dokümanından: _"Understanding aspects of cloud economics and cost savings of moving to the cloud"_

### CapEx vs OpEx - KRİTİK!

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  ON-PREMISES (CapEx)              CLOUD (OpEx)              │
│  ═══════════════════              ════════════              │
│                                                             │
│  • Sunucu satın alma              • Kullandığın kadar öde   │
│  • Veri merkezi kurma             • Ön yatırım yok          │
│  • Kapasite tahmini               • Esnek ölçekleme         │
│  • Bakım maliyetleri              • AWS bakımı yapar        │
│  • 3-5 yıllık amortisman          • Aylık fatura            │
│                                                             │
│  Capital Expenditure              Operational Expenditure   │
│  (Sermaye Harcaması)              (İşletme Gideri)          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Fixed Costs vs Variable Costs

| Maliyet Tipi            | On-Premises          | Cloud          |
| ----------------------- | -------------------- | -------------- |
| **Fixed (Sabit)**       | Sunucu, DC, personel | Minimal        |
| **Variable (Değişken)** | Elektrik, soğutma    | Kullanım bazlı |

**Cloud'un avantajı:** Sabit maliyetleri değişken maliyetlere dönüştürür!

### Right-sizing

```
┌─────────────────────────────────────────────────────────────┐
│  RIGHT-SIZING: Doğru kaynak boyutunu seçme                  │
│                                                             │
│  YANLIŞ: Her ihtimale karşı büyük instance al               │
│  DOĞRU:  İhtiyaca uygun en küçük instance'ı seç            │
│                                                             │
│  Araçlar: AWS Compute Optimizer, Cost Explorer              │
└─────────────────────────────────────────────────────────────┘
```

### Licensing Strategies

AWS resmi dokümanından: _"Understanding differences between licensing strategies"_

| Strateji                          | Açıklama                     | Örnek                      |
| --------------------------------- | ---------------------------- | -------------------------- |
| **BYOL** (Bring Your Own License) | Mevcut lisansı AWS'de kullan | Windows Server, SQL Server |
| **License Included**              | AWS lisansı dahil eder       | Amazon Linux, bazı RDS     |

### Automation ile Maliyet Tasarrufu

- **Auto Scaling:** Boşta kaynakları kapat
- **Scheduled scaling:** Mesai saatlerinde açık, gece kapalı
- **Lambda:** Sadece çalışırken öde
- **Spot Instances:** %90'a kadar tasarruf

### Economies of Scale (Ölçek Ekonomisi)

```
AWS'in büyük ölçeği = Düşük birim maliyet

AWS milyonlarca müşteriye hizmet veriyor
     ↓
Toplu donanım alımı = Düşük maliyet
     ↓
Tasarruf müşterilere yansıtılıyor
```

### Total Cost of Ownership (TCO)

On-premises'in gizli maliyetleri:

- Sunucu donanımı
- Veri merkezi alanı
- Elektrik ve soğutma
- Ağ altyapısı
- IT personeli
- Güvenlik
- Bakım ve destek

**AWS Pricing Calculator:** Maliyet tahmini için

---

## Domain 1 - Sınav İçin Kritik Noktalar

### Kesinlikle Ezberle!

```
┌─────────────────────────────────────────────────────────────┐
│  1. ELASTICITY = UP and DOWN (iki yönlü, variable demand)  │
│  2. SCALABILITY = GROW (tek yönlü, increased load)         │
│  3. Well-Architected = 6 sütun (O-S-R-P-C-S)              │
│  4. Region seçimi = COMPLIANCE önce gelir!                 │
│  5. Multi-AZ = High Availability                           │
│  6. CapEx → OpEx = Cloud'un temel avantajı                │
│  7. IaaS = En çok kontrol, SaaS = En az kontrol           │
│  8. AWS CAF = 6 perspektif (B-P-G-P-S-O)                  │
│  9. Migration 7 R's = Retire, Retain, Rehost...           │
│ 10. Right-sizing = Doğru kaynak boyutu seçimi             │
│ 11. BYOL = Bring Your Own License                          │
└─────────────────────────────────────────────────────────────┘
```

### Sık Çıkan Soru Kalıpları

| Soru Kalıbı                                 | Cevap                        |
| ------------------------------------------- | ---------------------------- |
| "Scale up AND down to meet variable demand" | **Elasticity**               |
| "Handle increased load"                     | Scalability                  |
| "Reduce capital expenditure"                | CapEx → OpEx (Cloud benefit) |
| "Deploy globally in minutes"                | Cloud benefit                |
| "Which region to choose first?"             | Compliance requirements      |
| "How to achieve high availability?"         | Multi-AZ deployment          |
| "Lift and shift migration"                  | **Rehost**                   |
| "Move to SaaS"                              | **Repurchase**               |
| "Rewrite for cloud-native"                  | **Refactor**                 |
| "Physical data transfer device"             | **AWS Snow Family**          |
| "Fixed costs to variable costs"             | Cloud benefit                |
| "Use existing Windows license on AWS"       | **BYOL**                     |
| "Optimize instance size"                    | **Right-sizing**             |
| "Framework for cloud migration planning"    | **AWS CAF**                  |

---

## Hızlı Referans Kartı

```
┌─────────────────────────────────────────────────────────────┐
│                   DOMAIN 1 ÖZET                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  CLOUD BENEFITS (6 tane):                                   │
│  • CapEx → OpEx                                            │
│  • Economies of scale                                       │
│  • Stop guessing capacity                                   │
│  • Speed & agility                                         │
│  • No DC maintenance                                       │
│  • Go global in minutes                                     │
│                                                             │
│  KEY CONCEPTS:                                              │
│  • Elasticity = UP + DOWN                                  │
│  • Scalability = GROW                                      │
│  • Agility = SPEED                                         │
│                                                             │
│  WELL-ARCHITECTED (6 Pillar):                              │
│  • Operational Excellence                                   │
│  • Security                                                │
│  • Reliability                                             │
│  • Performance Efficiency                                   │
│  • Cost Optimization                                       │
│  • Sustainability                                          │
│                                                             │
│  AWS CAF (6 Perspectives):                                  │
│  • Business, People, Governance (İş odaklı)                │
│  • Platform, Security, Operations (Teknik odaklı)          │
│                                                             │
│  MIGRATION 7 R's:                                           │
│  • Retire, Retain, Rehost, Relocate                        │
│  • Repurchase, Replatform, Refactor                        │
│                                                             │
│  CLOUD ECONOMICS:                                           │
│  • CapEx → OpEx (sabit → değişken)                        │
│  • Right-sizing = Doğru boyut                              │
│  • BYOL = Kendi lisansını getir                            │
│  • TCO = Total Cost of Ownership                           │
│                                                             │
│  GLOBAL INFRASTRUCTURE:                                     │
│  • Region > AZ > Data Center                               │
│  • Edge Location = CloudFront                              │
│  • Multi-AZ = High Availability                            │
│                                                             │
│  SERVICE MODELS:                                            │
│  • IaaS (EC2) = Most control                               │
│  • PaaS (Beanstalk, RDS) = Medium                          │
│  • SaaS (WorkSpaces) = Least control                       │
│                                                             │
│  MIGRATION TOOLS:                                           │
│  • Migration Hub = Merkezi izleme                          │
│  • DMS = Database migration                                │
│  • Snow Family = Physical transfer                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

- [AWS CLF-C02 Domain 1 Official Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain1.html)
- [AWS Cloud Adoption Framework](https://aws.amazon.com/cloud-adoption-framework/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Migration Strategies](https://docs.aws.amazon.com/prescriptive-guidance/latest/large-migration-guide/migration-strategies.html)

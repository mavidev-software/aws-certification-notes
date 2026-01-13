# Domain 4: Billing, Pricing, and Support (Faturalandırma, Fiyatlandırma ve Destek)

> **Sınav Ağırlığı: %12**
>
> **Kaynak:** [AWS Official Exam Guide - Domain 4](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain4.html)

---

## Genel Bakış

Domain 4, AWS fiyatlandırma modellerini, maliyet yönetim araçlarını ve AWS destek seçeneklerini kapsar. Bu domain sınavın en küçük bölümü olsa da, pratik ve anlaşılması kolay konular içerir.

```
┌─────────────────────────────────────────────────────────────────┐
│                    DOMAIN 4 KAPSAMI                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Toplam 3 Task Statement:                                      │
│                                                                 │
│  ├── Task 4.1: AWS Pricing Models (Fiyatlandırma Modelleri)   │
│  ├── Task 4.2: Billing & Cost Management (Maliyet Yönetimi)   │
│  └── Task 4.3: Technical Resources & Support (Destek)          │
│                                                                 │
│  Tahmini Soru Sayısı: ~8 soru (65 x 0.12)                      │
│                                                                 │
│  ÖNEMLİ: Spesifik fiyatları ezberlemeniz GEREKMEZ!            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Task Statement 4.1: AWS Pricing Models

> **Resmi Tanım:** "Compare AWS pricing models"

### AWS Fiyatlandırma Temel Prensipleri

```
┌─────────────────────────────────────────────────────────────────┐
│              AWS FİYATLANDIRMA PRENSİPLERİ                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Pay-as-you-go (Kullandığın kadar öde)                      │
│     └── Ön ödeme yok, kullandığın kadar fatura                 │
│                                                                 │
│  2. Save when you reserve (Rezerve ederek tasarruf)            │
│     └── 1-3 yıllık taahhüt ile indirim                         │
│                                                                 │
│  3. Pay less by using more (Daha fazla kullan, daha az öde)    │
│     └── Hacim indirimleri (S3 gibi)                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### EC2 Fiyatlandırma Modelleri - KRİTİK!

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      EC2 FİYATLANDIRMA MODELLERİ                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Model              İndirim    Taahhüt    Kullanım                         │
│  ─────────────────  ─────────  ─────────  ─────────────────────────────    │
│                                                                             │
│  On-Demand          %0         Yok        Kısa süreli, test, değişken yük  │
│                                                                             │
│  Reserved Instance  %72'ye     1-3 yıl    Kararlı, sürekli iş yükleri      │
│                     kadar                                                   │
│                                                                             │
│  Savings Plans      %72'ye     1-3 yıl    Esnek, EC2/Fargate/Lambda        │
│                     kadar                                                   │
│                                                                             │
│  Spot Instance      %90'a      Yok        Kesintiye toleranslı işler       │
│                     kadar                                                   │
│                                                                             │
│  Dedicated Host     Değişken   Yok/1-3yıl Lisans, uyumluluk gereksinimleri │
│                                                                             │
│  Dedicated Instance Değişken   Yok        Fiziksel izolasyon               │
│                                                                             │
│  Capacity           Değişken   Yok        Kapasite garantisi               │
│  Reservation                                                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Detaylı Model Açıklamaları

##### 1. On-Demand Instances

```
┌─────────────────────────────────────────────────────────────────┐
│                      ON-DEMAND                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ✓ Ön ödeme yok                                                │
│  ✓ Uzun vadeli taahhüt yok                                     │
│  ✓ Saniye/saat bazında faturalandırma                          │
│  ✓ İstediğin zaman başlat/durdur                               │
│                                                                 │
│  Ne zaman kullanılır:                                          │
│  • Kısa süreli iş yükleri                                      │
│  • Geliştirme/test ortamları                                   │
│  • Tahmin edilemeyen iş yükleri                                │
│  • İlk kez denenen uygulamalar                                 │
│                                                                 │
│  Anahtar kelimeler: "flexibility", "no commitment",            │
│                     "unpredictable", "short-term"              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

##### 2. Reserved Instances (RI)

```
┌─────────────────────────────────────────────────────────────────┐
│                   RESERVED INSTANCES                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Taahhüt Süresi: 1 yıl veya 3 yıl                              │
│  İndirim: %72'ye kadar (On-Demand'e göre)                      │
│                                                                 │
│  Ödeme Seçenekleri:                                            │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ All Upfront      │ Tamamı peşin    │ En yüksek indirim  │   │
│  │ Partial Upfront  │ Kısmi peşin     │ Orta indirim       │   │
│  │ No Upfront       │ Peşin ödeme yok │ En düşük indirim   │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  RI Türleri:                                                   │
│  • Standard RI: Daha yüksek indirim, az esneklik              │
│  • Convertible RI: Daha düşük indirim, değiştirilebilir        │
│                                                                 │
│  Ne zaman kullanılır:                                          │
│  • Kararlı, sürekli iş yükleri                                 │
│  • Veritabanları, backend sunucuları                           │
│  • Bilinen minimum kullanım                                    │
│                                                                 │
│  Anahtar kelimeler: "steady-state", "predictable",            │
│                     "1 year", "3 year", "commitment"           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

##### 3. Savings Plans - ÖNEMLİ!

```
┌─────────────────────────────────────────────────────────────────┐
│                     SAVINGS PLANS                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  RI'dan farkı: Daha ESNEK!                                     │
│                                                                 │
│  Taahhüt: Saatlik harcama taahhüdü ($/saat)                   │
│           Örn: "Saatte $10 harcama taahhüt ediyorum"           │
│                                                                 │
│  Türleri:                                                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                                                         │   │
│  │  Compute Savings Plans                                  │   │
│  │  ─────────────────────                                  │   │
│  │  • EC2, Fargate, Lambda için geçerli                   │   │
│  │  • Region, instance family, OS değişebilir            │   │
│  │  • En esnek seçenek                                    │   │
│  │                                                         │   │
│  │  EC2 Instance Savings Plans                             │   │
│  │  ───────────────────────────                            │   │
│  │  • Belirli Region + Instance family                    │   │
│  │  • Daha yüksek indirim (%72'ye kadar)                  │   │
│  │  • Daha az esneklik                                    │   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  Anahtar kelimeler: "flexible commitment", "hourly",          │
│                     "across services", "Lambda + EC2"          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

##### Reserved Instance vs Savings Plans Karşılaştırması

| Özellik | Reserved Instance | Savings Plans |
|---------|-------------------|---------------|
| **Taahhüt** | Instance bazlı | Harcama bazlı ($/saat) |
| **Esneklik** | Sınırlı | Yüksek |
| **Kapsam** | EC2 only | EC2, Fargate, Lambda |
| **Region değişikliği** | Standard RI'da yok | Compute SP'de var |
| **Instance değişikliği** | Convertible RI'da var | Her zaman var |

##### 4. Spot Instances - KRİTİK!

```
┌─────────────────────────────────────────────────────────────────┐
│                      SPOT INSTANCES                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ⚠️  DİKKAT: AWS istediği zaman geri alabilir! (2 dk uyarı)    │
│                                                                 │
│  İndirim: %90'a kadar (En yüksek!)                             │
│                                                                 │
│  Nasıl çalışır:                                                │
│  • AWS'in kullanılmayan kapasitesini kullanırsın              │
│  • Fiyat arz-talebe göre değişir                              │
│  • Spot fiyatı > senin teklifin → Instance sonlandırılır      │
│                                                                 │
│  ✅ NE ZAMAN KULLANILIR:                                       │
│  • Batch processing                                            │
│  • Big data analizi                                            │
│  • CI/CD build işleri                                          │
│  • Kesintiye toleranslı iş yükleri                            │
│  • Stateless uygulamalar                                       │
│                                                                 │
│  ❌ NE ZAMAN KULLANILMAZ:                                      │
│  • Veritabanları                                               │
│  • Kritik uygulamalar                                          │
│  • Kesintiye toleranssız işler                                │
│                                                                 │
│  Anahtar kelimeler: "fault-tolerant", "flexible",             │
│                     "batch", "interruptible", "stateless"      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

##### 5. Dedicated Hosts vs Dedicated Instances

```
┌─────────────────────────────────────────────────────────────────┐
│            DEDICATED HOST vs DEDICATED INSTANCE                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  DEDICATED HOST                                                 │
│  ──────────────                                                 │
│  • Fiziksel sunucunun tamamı sizin                            │
│  • Socket/core görünürlüğü var                                │
│  • BYOL (Bring Your Own License) için ideal                   │
│  • Uyumluluk gereksinimleri için                              │
│                                                                 │
│  DEDICATED INSTANCE                                             │
│  ──────────────────                                             │
│  • Instance seviyesinde izolasyon                              │
│  • Aynı hesaptaki instance'larla paylaşabilir                 │
│  • Socket/core görünürlüğü yok                                │
│  • Dedicated Host'tan daha ucuz                               │
│                                                                 │
│  KARŞILAŞTIRMA:                                                │
│  ┌──────────────────┬─────────────────┬───────────────────┐   │
│  │ Özellik          │ Dedicated Host  │ Dedicated Instance│   │
│  ├──────────────────┼─────────────────┼───────────────────┤   │
│  │ BYOL desteği     │ ✅              │ ❌                │   │
│  │ Socket visibility│ ✅              │ ❌                │   │
│  │ Maliyet          │ Daha yüksek     │ Daha düşük        │   │
│  │ Fiziksel kontrol │ Tam             │ Kısmi             │   │
│  └──────────────────┴─────────────────┴───────────────────┘   │
│                                                                 │
│  Anahtar kelimeler:                                            │
│  • "BYOL", "licensing" → Dedicated Host                       │
│  • "compliance", "regulatory" → Her ikisi de                  │
│  • "socket", "core" → Dedicated Host                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Veri Transfer Maliyetleri

```
┌─────────────────────────────────────────────────────────────────┐
│                   VERİ TRANSFER MALİYETLERİ                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  INBOUND (AWS'e gelen):                                        │
│  ═══════════════════════                                        │
│  İnternetten AWS'e veri transferi → ÜCRETSİZ                   │
│                                                                 │
│  OUTBOUND (AWS'den çıkan):                                     │
│  ════════════════════════                                       │
│  AWS'den internete veri transferi → ÜCRETLİ                    │
│                                                                 │
│  AYNI REGION İÇİNDE:                                           │
│  ════════════════════                                           │
│  • Aynı AZ içinde: Genellikle ücretsiz                        │
│  • Farklı AZ'ler arası: Düşük ücret                           │
│                                                                 │
│  FARKLI REGION'LAR ARASI:                                      │
│  ════════════════════════                                       │
│  • Her zaman ücretli                                           │
│  • Kaynak ve hedef Region'a göre değişir                      │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                                                         │   │
│  │  INTERNET ───► AWS (Inbound)   = FREE                  │   │
│  │  AWS ───► INTERNET (Outbound)  = PAID                  │   │
│  │  AWS ───► AWS (Same Region)    = LOW/FREE              │   │
│  │  AWS ───► AWS (Cross Region)   = PAID                  │   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### S3 Storage Classes ve Fiyatlandırma

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      S3 STORAGE CLASSES                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Class                    Kullanım                    Maliyet               │
│  ─────────────────────    ────────────────────────    ─────────────────     │
│                                                                             │
│  S3 Standard              Sık erişilen veriler       En yüksek             │
│                                                                             │
│  S3 Intelligent-Tiering   Değişken erişim paterni    Otomatik optimize     │
│                                                                             │
│  S3 Standard-IA           Nadir erişim (aylık)       Düşük (30 gün min)    │
│                                                                             │
│  S3 One Zone-IA           Nadir + tek AZ yeterli    Daha düşük            │
│                                                                             │
│  S3 Glacier Instant       Arşiv, anında erişim       Çok düşük             │
│  Retrieval                                                                  │
│                                                                             │
│  S3 Glacier Flexible      Arşiv, dakika/saat        Çok düşük (90 gün)    │
│  Retrieval                                                                  │
│                                                                             │
│  S3 Glacier Deep Archive  Uzun süreli arşiv         En düşük (180 gün)    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Sınav İpucu:**
```
"Sık erişim"                    → S3 Standard
"Nadir erişim"                  → S3 Standard-IA
"Erişim paterni bilinmiyor"     → S3 Intelligent-Tiering
"Uzun süreli arşiv"             → Glacier Deep Archive
"Arşiv ama hızlı erişim"        → Glacier Instant Retrieval
```

---

## Task Statement 4.2: Billing, Budget ve Cost Management

> **Resmi Tanım:** "Understand resources for billing, budget, and cost management"

### AWS Maliyet Yönetim Araçları

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    MALİYET YÖNETİM ARAÇLARI                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                     │   │
│  │  AWS COST EXPLORER                                                  │   │
│  │  ════════════════════                                               │   │
│  │  • Maliyet görselleştirme ve analiz                                │   │
│  │  • Geçmiş 13 ay veri                                               │   │
│  │  • Gelecek 12 ay tahmin                                            │   │
│  │  • Filtreleme ve gruplama                                          │   │
│  │  • Savings Plans önerileri                                         │   │
│  │                                                                     │   │
│  │  Soru: "Where can I visualize my AWS spending?"                    │   │
│  │  Cevap: AWS Cost Explorer                                          │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                     │   │
│  │  AWS BUDGETS                                                        │   │
│  │  ═══════════════                                                    │   │
│  │  • Bütçe oluşturma ve izleme                                       │   │
│  │  • Uyarı gönderme (email, SNS)                                     │   │
│  │  • Otomatik eylemler (EC2/RDS durdurma)                           │   │
│  │                                                                     │   │
│  │  Bütçe Türleri:                                                    │   │
│  │  • Cost Budget: Harcama limiti                                     │   │
│  │  • Usage Budget: Kullanım limiti                                   │   │
│  │  • Reservation Budget: RI kullanım takibi                          │   │
│  │  • Savings Plans Budget: SP kullanım takibi                        │   │
│  │                                                                     │   │
│  │  Soru: "How can I get alerts when spending exceeds threshold?"     │   │
│  │  Cevap: AWS Budgets                                                │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                     │   │
│  │  AWS PRICING CALCULATOR                                             │   │
│  │  ══════════════════════════                                         │   │
│  │  • Maliyet tahmini yapma                                           │   │
│  │  • Senaryo karşılaştırma                                           │   │
│  │  • Teklif oluşturma                                                │   │
│  │                                                                     │   │
│  │  Soru: "How can I estimate costs BEFORE deploying?"                │   │
│  │  Cevap: AWS Pricing Calculator                                     │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                                                                     │   │
│  │  AWS COST AND USAGE REPORT (CUR)                                   │   │
│  │  ═══════════════════════════════                                    │   │
│  │  • En detaylı maliyet raporu                                       │   │
│  │  • CSV/Parquet formatında S3'e                                     │   │
│  │  • Saatlik/günlük granülarite                                      │   │
│  │  • Third-party araçlarla entegrasyon                               │   │
│  │                                                                     │   │
│  │  Soru: "Most detailed/comprehensive billing report?"               │   │
│  │  Cevap: AWS Cost and Usage Report                                  │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Araç Karşılaştırması

| Araç | Amaç | Ne Zaman Kullanılır |
|------|------|---------------------|
| **Cost Explorer** | Analiz ve görselleştirme | Geçmiş harcamaları anlamak |
| **AWS Budgets** | Bütçe ve uyarılar | Harcama limiti koymak |
| **Pricing Calculator** | Maliyet tahmini | Deploy etmeden önce |
| **Cost and Usage Report** | Detaylı rapor | Chargeback, detaylı analiz |

---

### AWS Organizations ve Consolidated Billing

```
┌─────────────────────────────────────────────────────────────────┐
│                   AWS ORGANIZATIONS                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                    Management Account                           │
│                    (Payer Account)                              │
│                          │                                      │
│          ┌───────────────┼───────────────┐                     │
│          │               │               │                     │
│          ▼               ▼               ▼                     │
│     ┌─────────┐    ┌─────────┐    ┌─────────┐                 │
│     │ Account │    │ Account │    │ Account │                 │
│     │   Dev   │    │  Prod   │    │  Test   │                 │
│     │  $500   │    │ $2000   │    │  $300   │                 │
│     └─────────┘    └─────────┘    └─────────┘                 │
│                          │                                      │
│                          ▼                                      │
│              ┌───────────────────────┐                         │
│              │   TEK FATURA: $2800   │                         │
│              │   + Hacim indirimleri │                         │
│              └───────────────────────┘                         │
│                                                                 │
│  CONSOLIDATED BILLING FAYDALARI:                               │
│  • Tek fatura (tüm hesaplar için)                             │
│  • Hacim indirimleri (combined usage)                          │
│  • Reserved Instance paylaşımı                                 │
│  • Savings Plans paylaşımı                                     │
│  • Merkezi maliyet yönetimi                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Cost Allocation Tags

```
┌─────────────────────────────────────────────────────────────────┐
│                   COST ALLOCATION TAGS                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Amaç: Maliyetleri kategorilere ayırma                         │
│                                                                 │
│  İki tür tag:                                                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                                                         │   │
│  │  AWS-Generated Tags                                     │   │
│  │  ══════════════════                                     │   │
│  │  • AWS tarafından otomatik oluşturulur                 │   │
│  │  • aws: prefix ile başlar                              │   │
│  │  • Örn: aws:createdBy                                  │   │
│  │                                                         │   │
│  │  User-Defined Tags                                      │   │
│  │  ═══════════════════                                    │   │
│  │  • Kullanıcı tanımlı                                   │   │
│  │  • user: prefix ile başlar (isteğe bağlı)             │   │
│  │  • Örn: Environment=Production, Project=WebApp        │   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  Örnek Kullanım:                                               │
│  EC2 Instance Tags:                                            │
│  • Environment: Production                                     │
│  • Department: Engineering                                     │
│  • Project: MobileApp                                         │
│  • CostCenter: CC-12345                                       │
│                                                                 │
│  → Sonra Cost Explorer'da "Department" bazında filtreleme     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Task Statement 4.3: Technical Resources ve AWS Support

> **Resmi Tanım:** "Identify AWS technical resources and AWS Support options"

### Destek Kavramı

```
┌─────────────────────────────────────────────────────────────────┐
│                   DESTEK NEDİR?                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Destek sadece "bir şeyler yanlış gitti, düzelt" değil!        │
│                                                                 │
│  Destek şunları içerir:                                        │
│  • Ortamlarınızı tasarlarken YARDIM                            │
│  • Ortamlarınızı oluştururken REHBERLİK                        │
│  • Proaktif öneriler ve mimari incelemeler                     │
│  • AWS'de başarılı olmanız için danışmanlık                    │
│                                                                 │
│  Cloud Center of Excellence (CCOE) olsa bile, kimse AWS'in    │
│  tüm servislerini, özelliklerini ve araçlarını tam olarak     │
│  bilemez. Bu yüzden destek seçenekleri kritiktir.              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### AWS Support Plans - KRİTİK!

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              AWS SUPPORT PLANS                                           │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                         │
│  Plan                 Fiyat           Yanıt Süresi (Kritik)   Özellikler               │
│  ──────────────────   ──────────────  ────────────────────    ─────────────────────     │
│                                                                                         │
│  BASIC                Ücretsiz        -                       • Dokümantasyon           │
│                                                               • Community forums        │
│                                                               • Service Health          │
│                                                               • 7 Trusted Advisor       │
│                                                               • Personal Health Dashboard│
│                                                                                         │
│  DEVELOPER            $29/ay~         12-24 saat              • Business hours email   │
│                                       (genel rehberlik)       • 1 kişi case açabilir   │
│                                                               • Genel mimari rehberlik │
│                                                               • Sınırsız teknik vaka   │
│                                                                                         │
│  BUSINESS             $100/ay~        1 saat                  • 24/7 telefon + chat    │
│                                       (production down)       • Sınırsız kişi          │
│                                                               • TÜM Trusted Advisor    │
│                                                               • 3rd party yazılım      │
│                                                               • AWS Support API        │
│                                                               • Kullanım senaryosu rehb.│
│                                                                                         │
│  ENTERPRISE           $5,500/ay~      30 dakika               • TAM pool erişimi       │
│  ON-RAMP                              (business critical)     • Proaktif programlar   │
│                                                               • Concierge (fatura)     │
│                                                                                         │
│  ENTERPRISE           $15,000/ay~     15 dakika               • Dedicated TAM          │
│                                       (business critical)     • Infrastructure Event   │
│                                                                 Management             │
│                                                               • Well-Architected       │
│                                                                 Review                 │
│                                                               • Operations Review      │
│                                                               • Concierge Service      │
│                                                               • AWS Konu Uzmanları     │
│                                                                                         │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

#### Support Plan Karşılaştırma Tablosu

| Özellik | Basic | Developer | Business | Enterprise On-Ramp | Enterprise |
|---------|-------|-----------|----------|-------------------|------------|
| **Fiyat** | Ücretsiz | $29/ay~ | $100/ay~ | $5,500/ay~ | $15,000/ay~ |
| **7/24 Destek** | ❌ | ❌ | ✅ | ✅ | ✅ |
| **Telefon** | ❌ | ❌ | ✅ | ✅ | ✅ |
| **Trusted Advisor** | 7 kontrol | 7 kontrol | Tümü | Tümü | Tümü |
| **Support API** | ❌ | ❌ | ✅ | ✅ | ✅ |
| **TAM** | ❌ | ❌ | ❌ | Pool | Dedicated |
| **Concierge** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Well-Architected Review** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Kritik Yanıt** | - | - | 1 saat | 30 dk | 15 dk |
| **Teknik Vaka Sayısı** | Yok | Sınırsız | Sınırsız | Sınırsız | Sınırsız |

---

#### Sınav İçin Kritik Support Plan Soruları

```
┌─────────────────────────────────────────────────────────────────┐
│        SINAV SORU-CEVAP: SUPPORT PLANS                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  S: Support API'ye erişim + en maliyet etkin plan?             │
│  C: BUSINESS (Enterprise'dan ucuz, API erişimi var)            │
│                                                                 │
│  S: Sınırsız teknik vaka açabilen en ucuz plan?                │
│  C: DEVELOPER                                                   │
│                                                                 │
│  S: TAM (Technical Account Manager) hangi planda?              │
│  C: ENTERPRISE (On-Ramp'ta pool, Enterprise'da dedicated)      │
│                                                                 │
│  S: Well-Architected review hangi planda?                      │
│  C: ENTERPRISE (sadece)                                         │
│                                                                 │
│  S: Concierge hizmeti hangi planda?                            │
│  C: ENTERPRISE (sadece)                                         │
│                                                                 │
│  S: 3rd party yazılım desteği hangi planda başlıyor?           │
│  C: BUSINESS                                                    │
│                                                                 │
│  S: Infrastructure Event Management hangi planda?              │
│  C: BUSINESS (ek ücretli), ENTERPRISE (dahil)                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

#### Basic Support - Tüm Müşteriler İçin (Ücretsiz)

```
┌─────────────────────────────────────────────────────────────────┐
│                 BASIC SUPPORT ÖZELLİKLERİ                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Tüm AWS müşterileri otomatik olarak şunlara erişir:          │
│                                                                 │
│  ✓ Customer Service (Müşteri Hizmetleri)                      │
│  ✓ Hesap ve faturalandırma sorularına bire bir yanıtlar       │
│  ✓ Destek forumları                                            │
│  ✓ Hizmet sağlık kontrolleri (Service Health)                 │
│  ✓ Dokümantasyon                                               │
│  ✓ Teknik makaleler (Whitepapers)                             │
│  ✓ En iyi uygulama kılavuzları                                │
│  ✓ Personal Health Dashboard                                   │
│  ✓ 7 temel Trusted Advisor kontrolü                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

#### Business/Enterprise Ek Özellikleri

```
┌─────────────────────────────────────────────────────────────────┐
│           BUSINESS/ENTERPRISE EK ÖZELLİKLER                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  KULLANIM SENARYOSU REHBERLİĞİ:                                │
│  • Hangi AWS ürünlerini kullanacağınız konusunda danışmanlık  │
│  • Spesifik ihtiyaçlarınıza en uygun servis önerileri         │
│                                                                 │
│  TAM TRUSTED ADVISOR ERİŞİMİ:                                  │
│  • Müşteri ortamlarını inceler                                 │
│  • Para tasarrufu fırsatlarını belirler                       │
│  • Güvenlik açıklarını tespit eder                            │
│  • Sistem güvenilirliğini ve performansını iyileştirir        │
│                                                                 │
│  SUPPORT API:                                                   │
│  • Destek merkezi ve Trusted Advisor ile etkileşim            │
│  • Otomatik destek vakası yönetimi                            │
│  • Programatik Trusted Advisor işlemleri                       │
│                                                                 │
│  3. PARTİ YAZILIM DESTEĞİ:                                     │
│  • Amazon EC2 instance işletim sistemleri yapılandırması      │
│  • AWS'deki popüler 3. parti yazılım bileşenlerinin performansı│
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### TAM (Technical Account Manager)

```
┌─────────────────────────────────────────────────────────────────┐
│                   TAM (Technical Account Manager)                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Kim?                                                          │
│  • AWS'de atanmış teknik danışman                             │
│  • Hesabınıza özel                                             │
│                                                                 │
│  Ne yapar?                                                     │
│  • Proaktif rehberlik                                          │
│  • Mimari inceleme                                             │
│  • Optimizasyon önerileri                                      │
│  • AWS ekipleriyle koordinasyon                                │
│  • Event desteği (Infrastructure Event Management)             │
│                                                                 │
│  Hangi planlarda?                                              │
│  • Enterprise On-Ramp: TAM pool (paylaşımlı)                  │
│  • Enterprise: Dedicated TAM (size özel)                       │
│                                                                 │
│  Soru: "Designated technical contact at AWS?"                  │
│  Cevap: TAM (Enterprise Support)                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### AWS Trusted Advisor - KRİTİK!

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS TRUSTED ADVISOR                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Amaç: AWS best practice önerileri sunma                       │
│                                                                 │
│  5 KATEGORİ (Ezberle!):                                        │
│  ═══════════════════════                                        │
│                                                                 │
│  1. COST OPTIMIZATION (Maliyet Optimizasyonu)                  │
│     • Kullanılmayan EC2, EBS                                   │
│     • Idle load balancers                                      │
│     • Underutilized resources                                  │
│                                                                 │
│  2. PERFORMANCE (Performans)                                   │
│     • High utilization instances                               │
│     • CloudFront optimizasyonu                                 │
│     • EC2 to EBS throughput                                    │
│                                                                 │
│  3. SECURITY (Güvenlik)                                        │
│     • MFA on root account                                      │
│     • Açık security group'lar                                  │
│     • Public S3 bucket'lar                                     │
│     • IAM kullanımı                                            │
│                                                                 │
│  4. FAULT TOLERANCE (Hata Toleransı)                           │
│     • Multi-AZ olmayan RDS                                     │
│     • EBS snapshot yaşı                                        │
│     • Auto Scaling grupları                                    │
│                                                                 │
│  5. SERVICE LIMITS (Servis Limitleri)                          │
│     • Limit yaklaşan servisler                                 │
│     • VPC, EC2, IAM limitleri                                  │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  SUPPORT PLAN GEREKSİNİMLERİ:                           │   │
│  │                                                         │   │
│  │  Basic/Developer: 7 temel kontrol                       │   │
│  │  Business/Enterprise: TÜM kontroller                    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### AWS Teknik Kaynaklar

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        AWS TEKNİK KAYNAKLAR                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  AWS DOCUMENTATION (docs.aws.amazon.com)                            │   │
│  │  • User guides, API references                                      │   │
│  │  • Tutorials, getting started                                       │   │
│  │  • Best practices                                                   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  AWS WHITEPAPERS                                                    │   │
│  │  • Well-Architected Framework                                       │   │
│  │  • Security best practices                                          │   │
│  │  • Migration guides                                                 │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  AWS KNOWLEDGE CENTER                                               │   │
│  │  • FAQ formatında çözümler                                         │   │
│  │  • Yaygın sorunların cevapları                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  AWS re:POST                                                        │   │
│  │  • Topluluk Q&A platformu                                          │   │
│  │  • AWS çalışanları da cevaplar                                     │   │
│  │  • Stack Overflow benzeri                                          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  AWS PRESCRIPTIVE GUIDANCE                                          │   │
│  │  • Detaylı mimari rehberler                                        │   │
│  │  • Migration patterns                                              │   │
│  │  • Implementation guides                                           │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  AWS BLOGS                                                          │   │
│  │  • Yeni özellikler                                                 │   │
│  │  • Case studies                                                    │   │
│  │  • Best practices                                                  │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  AWS TRAINING AND CERTIFICATION                                     │   │
│  │  • AWS Skill Builder - dijital eğitim                              │   │
│  │  • Hands-on labs                                                   │   │
│  │  • Sertifikasyon programları                                       │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Kaynak Bulma Rehberi - Nereye Bakmalı?

```
┌─────────────────────────────────────────────────────────────────┐
│               NEREYE BAKMALI? HIZLI REFERANS                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  İHTİYAÇ                          │  KAYNAK                     │
│  ────────────────────────────────────────────────────────────  │
│                                                                 │
│  Hizmeti ilk kez dağıtma          │  AWS Documentation         │
│  Rehberli talimatlar              │                             │
│                                                                 │
│  AWS en iyi uygulamaları          │  AWS Well-Architected      │
│                                   │  Framework + Documentation │
│                                                                 │
│  Spesifik teknik soru             │  AWS re:Post               │
│  Topluluk desteği                 │  (Community Q&A)           │
│                                                                 │
│  Bilgi ve deneyim geliştirme      │  AWS Training &            │
│                                   │  Certification + Skill Builder│
│                                                                 │
│  FAQ formatında çözümler          │  AWS Knowledge Center      │
│                                                                 │
│  Detaylı mimari rehberler         │  AWS Prescriptive Guidance │
│  Migration patterns               │                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Diğer Destek Ekipleri

```
┌─────────────────────────────────────────────────────────────────┐
│                  DİĞER DESTEK EKİPLERİ                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AWS ABUSE EKİBİ                                                │
│  • Kötüye kullanım raporlama                                   │
│  • Spam, DDoS, phishing şikayetleri                           │
│                                                                 │
│  PREMIUM SUPPORT                                                │
│  • Teknik destek (plan seviyesine göre)                       │
│  • 7/24 erişim (Business+)                                     │
│                                                                 │
│  BILLING/ACCOUNT SUPPORT                                        │
│  • Faturalandırma soruları                                     │
│  • Hesap sorunları                                             │
│  • Tüm müşteriler için ücretsiz                               │
│                                                                 │
│  TECHNICAL ACCOUNT MANAGER (TAM)                               │
│  • Ortam koordinasyonu                                         │
│  • Proaktif programlara erişim                                 │
│  • Enterprise Support'ta                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Gerçek Dünya İpucu

```
┌─────────────────────────────────────────────────────────────────┐
│                  GERÇEK DÜNYA İPUCU                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Google kung fu'nuzu geliştirin!"                             │
│                                                                 │
│  İhtiyacınız olan cevapları ve rehberliği nasıl arayacağınızı  │
│  öğrenin. Dokümante edilmiş cevaplarınızı NEREDE bulacağınızı  │
│  anlamak, destek aramanın İLK ADIMIDIR.                        │
│                                                                 │
│  Bu yeteneği geliştirdikten sonra, daha acil konular için      │
│  kullanabileceğiniz destek yollarını değerlendirin:            │
│                                                                 │
│  • AWS Abuse ekibi                                              │
│  • Premium Support                                              │
│  • Billing/Account Support                                      │
│  • Technical Account Manager (Enterprise)                       │
│  • AWS Partner Network                                         │
│                                                                 │
│  Destekleyici bilgiyi nerede bulacağınızı bilmek, sınava      │
│  hazırlanırken öğrenebileceğiniz EN ÖNEMLİ şeylerden biri!    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### AWS Partner Network (APN)

```
┌─────────────────────────────────────────────────────────────────┐
│                   AWS PARTNER NETWORK (APN)                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Partner Türleri:                                               │
│  ═══════════════                                                │
│                                                                 │
│  CONSULTING PARTNERS                                            │
│  • System Integrators (SI)                                     │
│  • Migration yardımı                                           │
│  • Mimari danışmanlık                                          │
│  • Örn: Accenture, Deloitte                                    │
│                                                                 │
│  TECHNOLOGY PARTNERS                                            │
│  • Independent Software Vendors (ISV)                          │
│  • SaaS çözümleri                                              │
│  • AWS üzerinde çalışan yazılımlar                            │
│                                                                 │
│  AWS MARKETPLACE                                                │
│  ═══════════════                                                │
│  • 3rd party yazılım mağazası                                 │
│  • Tek fatura ile ödeme                                        │
│  • AMI, container, SaaS                                        │
│  • Lisans yönetimi                                             │
│                                                                 │
│  Soru: "Where to find 3rd party software for AWS?"            │
│  Cevap: AWS Marketplace                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### AWS Professional Services

```
┌─────────────────────────────────────────────────────────────────┐
│                 AWS PROFESSIONAL SERVICES                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AWS Professional Services:                                    │
│  • AWS'in kendi danışmanları                                   │
│  • Enterprise migration                                         │
│  • Karmaşık projeler                                           │
│                                                                 │
│  AWS Solutions Architects:                                      │
│  • Mimari danışmanlık                                          │
│  • Best practice rehberliği                                    │
│  • POC desteği                                                 │
│                                                                 │
│  AWS Training and Certification:                                │
│  • Resmi eğitimler                                             │
│  • Sertifikasyon programları                                   │
│  • Digital ve classroom                                        │
│                                                                 │
│  AWS IQ:                                                        │
│  • On-demand uzman bul                                         │
│  • AWS sertifikalı uzmanlar                                    │
│  • Proje bazlı çalışma                                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### AWS Health Dashboard

```
┌─────────────────────────────────────────────────────────────────┐
│                   AWS HEALTH DASHBOARD                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  İki Görünüm:                                                  │
│                                                                 │
│  SERVICE HEALTH DASHBOARD                                       │
│  ═════════════════════════                                      │
│  • Genel AWS servis durumu                                     │
│  • Tüm Region'lar için                                         │
│  • Herkes görebilir (public)                                   │
│  • health.aws.amazon.com                                       │
│                                                                 │
│  YOUR ACCOUNT HEALTH (Personal Health Dashboard)               │
│  ════════════════════════════════════════════════               │
│  • Sizin kaynaklarınızı etkileyen olaylar                     │
│  • Hesaba özel bildirimler                                     │
│  • Proaktif uyarılar                                           │
│  • Console'da Health bölümü                                    │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Soru kalıbı:                                           │   │
│  │  "Events that affect YOUR resources?" → Personal Health │   │
│  │  "General AWS service status?" → Service Health         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Domain 4 - Sınav İçin Kritik Noktalar

### Kesinlikle Ezberle!

```
┌─────────────────────────────────────────────────────────────────┐
│              DOMAIN 4 KRİTİK NOKTALAR                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  EC2 FİYATLANDIRMA:                                            │
│  1. On-Demand = Esneklik, taahhüt yok                          │
│  2. Reserved = 1-3 yıl, %72 indirim                            │
│  3. Spot = %90 indirim, kesintiye toleranslı                   │
│  4. Savings Plans = Esnek taahhüt (EC2+Lambda+Fargate)        │
│  5. Dedicated Host = BYOL, lisans                              │
│                                                                 │
│  MALİYET ARAÇLARI:                                             │
│  • Cost Explorer = Analiz, görselleştirme                      │
│  • AWS Budgets = Bütçe, uyarılar                               │
│  • Pricing Calculator = Tahmin (deploy'dan önce)              │
│  • CUR = En detaylı rapor                                      │
│                                                                 │
│  SUPPORT PLANS:                                                │
│  • Basic = Ücretsiz, 7 Trusted Advisor                        │
│  • Developer = $29, email (business hours)                     │
│  • Business = $100, 24/7, TÜM Trusted Advisor                 │
│  • Enterprise = TAM, 15 dk yanıt                               │
│                                                                 │
│  TRUSTED ADVISOR 5 KATEGORİ:                                   │
│  • Cost Optimization                                           │
│  • Performance                                                 │
│  • Security                                                    │
│  • Fault Tolerance                                             │
│  • Service Limits                                              │
│                                                                 │
│  VERİ TRANSFERİ:                                               │
│  • Inbound = ÜCRETSİZ                                          │
│  • Outbound = ÜCRETLİ                                          │
│                                                                 │
│  DİĞER:                                                        │
│  • Consolidated Billing = Tek fatura, hacim indirimi          │
│  • Cost Allocation Tags = Maliyet kategorileme                │
│  • AWS Marketplace = 3rd party yazılım                        │
│  • TAM = Enterprise Support'ta                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Sık Çıkan Soru Kalıpları

| Soru Kalıbı | Cevap |
|-------------|-------|
| "Most cost-effective for steady workload?" | **Reserved Instance** veya **Savings Plans** |
| "Fault-tolerant, can be interrupted?" | **Spot Instance** |
| "No long-term commitment?" | **On-Demand** |
| "BYOL, licensing compliance?" | **Dedicated Host** |
| "Alert when spending exceeds budget?" | **AWS Budgets** |
| "Visualize past spending?" | **AWS Cost Explorer** |
| "Estimate costs before deploying?" | **AWS Pricing Calculator** |
| "Most detailed billing report?" | **Cost and Usage Report** |
| "24/7 phone support?" | **Business** veya **Enterprise** Support |
| "Dedicated technical contact?" | **TAM (Enterprise)** |
| "All Trusted Advisor checks?" | **Business** veya **Enterprise** Support |
| "Best practice recommendations?" | **AWS Trusted Advisor** |
| "Find 3rd party software?" | **AWS Marketplace** |
| "Events affecting my resources?" | **Personal Health Dashboard** |

---

## Hızlı Referans Kartı

```
┌─────────────────────────────────────────────────────────────────┐
│                   DOMAIN 4 ÖZET                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  EC2 PRICING MODELS:                                           │
│  • On-Demand: Esnek, pahalı                                    │
│  • Reserved: 1-3 yıl, %72↓                                     │
│  • Spot: Kesintiye OK, %90↓                                    │
│  • Savings Plans: Esnek taahhüt                                │
│  • Dedicated: BYOL, izolasyon                                  │
│                                                                 │
│  COST TOOLS:                                                   │
│  • Cost Explorer: Geçmiş analiz                                │
│  • Budgets: Bütçe + uyarı                                      │
│  • Pricing Calculator: Tahmin                                  │
│  • CUR: Detaylı rapor                                          │
│                                                                 │
│  SUPPORT PLANS:                                                │
│  • Basic: Free, 7 TA                                           │
│  • Developer: $29, email                                       │
│  • Business: $100, 24/7, all TA                               │
│  • Enterprise On-Ramp: $5.5K, TAM pool                        │
│  • Enterprise: $15K, dedicated TAM                             │
│                                                                 │
│  TRUSTED ADVISOR (5 kategori):                                 │
│  C-P-S-F-S (Cost, Perf, Security, Fault, Service)             │
│                                                                 │
│  DATA TRANSFER:                                                │
│  • IN = Free                                                   │
│  • OUT = Paid                                                  │
│                                                                 │
│  ORGANIZATIONS:                                                │
│  • Consolidated Billing                                        │
│  • Volume discounts                                            │
│  • RI/SP sharing                                               │
│                                                                 │
│  RESOURCES:                                                    │
│  • Docs, Whitepapers, Knowledge Center                        │
│  • re:Post, Prescriptive Guidance                             │
│  • AWS Marketplace, APN Partners                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

### AWS Resmi Dokümantasyon

- [AWS CLF-C02 Domain 4 Official Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain4.html)
- [AWS Pricing](https://aws.amazon.com/pricing/)
- [AWS Support Plans](https://aws.amazon.com/premiumsupport/plans/)
- [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/)
- [AWS Cost Management](https://aws.amazon.com/aws-cost-management/)

### Araçlar

- [AWS Pricing Calculator](https://calculator.aws/)
- [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
- [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/)

---

> **Hedef:** Domain 4 küçük ama önemli. Support planları ve fiyatlandırma modelleri sınavda kesinlikle çıkar!

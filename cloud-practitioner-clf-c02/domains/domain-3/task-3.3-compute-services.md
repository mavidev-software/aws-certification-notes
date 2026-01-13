# Task 3.3: AWS Compute Services

> **Resmi Tanım:** Identify AWS compute services
>
> **Kaynak:** [AWS CLF-C02 Exam Guide - Domain 3](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain3.html)

---

## Bu Task'ta Öğrenilecekler

### Bilgi Alanları (Knowledge)

- AWS compute servisleri

### Beceriler (Skills)

- Farklı EC2 instance türlerinin uygun kullanımını tanıma
- Container seçeneklerinin uygun kullanımını tanıma (ECS, EKS)
- Serverless compute seçeneklerinin uygun kullanımını tanıma (Fargate, Lambda)
- Auto scaling'in elasticity sağladığını anlama
- Load balancer'ların amaçlarını belirleme

---

## 1. AWS Compute Servisleri Genel Bakış

```
┌─────────────────────────────────────────────────────────────────┐
│                   AWS COMPUTE SERVİSLERİ                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   SANAL SUNUCULAR              KONTEYNERLER                    │
│   ─────────────────            ─────────────                    │
│   ┌───────────────┐            ┌───────────────┐               │
│   │    EC2        │            │     ECS       │               │
│   │ (IaaS)        │            │     EKS       │               │
│   └───────────────┘            │    Fargate    │               │
│   ┌───────────────┐            └───────────────┘               │
│   │   Lightsail   │                                            │
│   │ (Basit VPS)   │            SERVERLESS                      │
│   └───────────────┘            ───────────                      │
│                                ┌───────────────┐               │
│   PLATFORM (PaaS)              │    Lambda     │               │
│   ───────────────              │   (FaaS)      │               │
│   ┌───────────────┐            └───────────────┘               │
│   │Elastic Beanstalk│                                          │
│   └───────────────┘            DİĞER                           │
│                                ─────                            │
│   BATCH PROCESSING             ┌───────────────┐               │
│   ────────────────             │   Outposts    │               │
│   ┌───────────────┐            │    Batch      │               │
│   │   AWS Batch   │            └───────────────┘               │
│   └───────────────┘                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 2. Amazon EC2 (Elastic Compute Cloud)

### 2.1 EC2 Nedir?

**Tanım:** AWS'de sanal sunucu (instance) oluşturma ve çalıştırma servisi. Infrastructure as a Service (IaaS).

```
┌─────────────────────────────────────────────────────────────────┐
│                      EC2 MİMARİSİ                                │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                    EC2 HOST                              │   │
│   │               (Fiziksel Sunucu)                          │   │
│   │                                                          │   │
│   │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │   │
│   │  │ EC2 Instance│  │ EC2 Instance│  │ EC2 Instance│      │   │
│   │  │             │  │             │  │             │      │   │
│   │  │ ┌─────────┐ │  │ ┌─────────┐ │  │ ┌─────────┐ │      │   │
│   │  │ │   OS    │ │  │ │   OS    │ │  │ │   OS    │ │      │   │
│   │  │ └─────────┘ │  │ └─────────┘ │  │ └─────────┘ │      │   │
│   │  │ ┌─────────┐ │  │ ┌─────────┐ │  │ ┌─────────┐ │      │   │
│   │  │ │  Apps   │ │  │ │  Apps   │ │  │ │  Apps   │ │      │   │
│   │  │ └─────────┘ │  │ └─────────┘ │  │ └─────────┘ │      │   │
│   │  └─────────────┘  └─────────────┘  └─────────────┘      │   │
│   │                                                          │   │
│   │  Instance Store (Geçici)    EBS (Kalıcı)                │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 EC2 Instance Kategorileri

| Kategori                  | Amaç           | Kullanım Alanı                 | Örnek Tipler |
| ------------------------- | -------------- | ------------------------------ | ------------ |
| **General Purpose**       | Dengeli kaynak | Web sunucuları, geliştirme     | t3, m5, m6i  |
| **Compute Optimized**     | Yüksek CPU     | HPC, ML, gaming, batch         | c5, c6i, c7g |
| **Memory Optimized**      | Yüksek RAM     | Veritabanları, in-memory cache | r5, r6i, x2  |
| **Storage Optimized**     | Yüksek I/O     | Data warehouse, log işleme     | i3, d2, h1   |
| **Accelerated Computing** | GPU/FPGA       | ML training, video encoding    | p4, g5, inf1 |

### 2.3 Instance Tipi Okuma

```
Instance Tipi: m5.2xlarge
               ││ │
               ││ └─► Boyut: 2xlarge (nano → micro → small → medium → large → xlarge → 2xlarge...)
               ││
               │└─► Nesil: 5 (yeni nesiller daha iyi performans/fiyat)
               │
               └─► Aile: m (general purpose)

Aile Kodları:
─────────────
t = Burstable (değişken performans)
m = General Purpose
c = Compute Optimized
r = Memory Optimized
i = Storage Optimized
p, g = GPU
```

### 2.4 EC2 Fiyatlandırma Modelleri

```
┌─────────────────────────────────────────────────────────────────┐
│                EC2 FİYATLANDIRMA MODELLERİ                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ON-DEMAND                      RESERVED INSTANCE               │
│  ─────────                      ─────────────────               │
│  • Kullandıkça öde              • 1-3 yıl taahhüt               │
│  • Taahhüt yok                  • %72'ye kadar indirim          │
│  • En esnek                     • Sabit kapasite                │
│  • En pahalı                    • Standard veya Convertible     │
│                                                                 │
│  SPOT INSTANCE                  SAVINGS PLANS                   │
│  ─────────────                  ─────────────                   │
│  • Boşta kalan kapasite         • Esnek taahhüt                 │
│  • %90'a kadar indirim          • %72'ye kadar indirim          │
│  • Kesintiye uğrayabilir!       • Compute veya EC2              │
│  • Esnek iş yükleri             • Instance tipi değiştirilebilir│
│                                                                 │
│  DEDICATED HOST                 DEDICATED INSTANCE              │
│  ──────────────                 ──────────────────              │
│  • Fiziksel sunucu kiralama     • Dedicated donanım             │
│  • BYOL için                    • Instance seviyesinde          │
│  • Compliance gereksinimleri    • Host kontrolü yok             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Fiyatlandırma Karşılaştırması:**

| Model         | İndirim | Taahhüt | Kesinti Riski | En İyi Kullanım     |
| ------------- | ------- | ------- | ------------- | ------------------- |
| On-Demand     | %0      | Yok     | Yok           | Kısa süreli, test   |
| Reserved      | %72     | 1-3 yıl | Yok           | Sürekli çalışan     |
| Spot          | %90     | Yok     | VAR           | Batch, esnek işler  |
| Savings Plans | %72     | 1-3 yıl | Yok           | Değişken iş yükleri |

### 2.5 EC2 Instance Metadata ve User Data

```
┌─────────────────────────────────────────────────────────────────┐
│              METADATA vs USER DATA                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  INSTANCE METADATA                                              │
│  ─────────────────                                              │
│  URL: http://169.254.169.254/latest/meta-data/                 │
│                                                                 │
│  İçerik:                                                        │
│  • Instance ID                                                  │
│  • Instance Type                                                │
│  • Public IP                                                    │
│  • Private IP                                                   │
│  • IAM Role credentials                                         │
│  • Security Groups                                              │
│                                                                 │
│  INSTANCE USER DATA                                             │
│  ──────────────────                                             │
│  URL: http://169.254.169.254/latest/user-data/                 │
│                                                                 │
│  İçerik:                                                        │
│  • Bootstrap scriptleri                                         │
│  • İlk başlatmada çalışacak komutlar                           │
│  • Yazılım kurulumu                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**User Data Örneği:**

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from EC2</h1>" > /var/www/html/index.html
```

### 2.6 AMI (Amazon Machine Image)

**Tanım:** EC2 instance başlatmak için kullanılan şablon.

```
┌─────────────────────────────────────────────────────────────────┐
│                         AMI                                      │
│                                                                 │
│   İçerir:                                                       │
│   • İşletim sistemi                                             │
│   • Uygulama yazılımları                                        │
│   • Konfigürasyonlar                                            │
│   • Launch permissions                                          │
│                                                                 │
│   AMI Kaynakları:                                               │
│   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│   │AWS Provided │  │ Marketplace │  │   Custom    │            │
│   │   (Free)    │  │   (Paid)    │  │  (Kendi)    │            │
│   └─────────────┘  └─────────────┘  └─────────────┘            │
│                                                                 │
│   Golden AMI: Standartlaştırılmış, güvenlik yamaları,          │
│              kurulu uygulamalar içeren şablon                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 3. Container Servisleri

### 3.1 Container Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│              VM vs CONTAINER KARŞILAŞTIRMASI                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   VIRTUAL MACHINE                CONTAINER                      │
│   ───────────────                ─────────                      │
│   ┌─────────────┐               ┌─────────────┐                │
│   │    App A    │               │    App A    │                │
│   ├─────────────┤               ├─────────────┤                │
│   │   Bins/Libs │               │   Bins/Libs │                │
│   ├─────────────┤               └──────┬──────┘                │
│   │  Guest OS   │               ┌──────┴──────┐                │
│   └──────┬──────┘               │    App B    │                │
│   ┌──────┴──────┐               ├─────────────┤                │
│   │    App B    │               │   Bins/Libs │                │
│   ├─────────────┤               └──────┬──────┘                │
│   │   Bins/Libs │                      │                       │
│   ├─────────────┤               ┌──────┴──────┐                │
│   │  Guest OS   │               │   Docker    │                │
│   └──────┬──────┘               │   Engine    │                │
│          │                      └──────┬──────┘                │
│   ┌──────┴──────┐               ┌──────┴──────┐                │
│   │ Hypervisor  │               │   Host OS   │                │
│   └──────┬──────┘               └──────┬──────┘                │
│   ┌──────┴──────┐               ┌──────┴──────┐                │
│   │  Hardware   │               │  Hardware   │                │
│   └─────────────┘               └─────────────┘                │
│                                                                 │
│   • Her VM ayrı OS              • Tek OS paylaşılır            │
│   • Daha fazla kaynak           • Daha az kaynak               │
│   • Dakikalar içinde başlar     • Saniyeler içinde başlar      │
│   • Tam izolasyon               • Process izolasyonu           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 3.2 Amazon ECS (Elastic Container Service)

**Tanım:** AWS'nin native container orchestration servisi.

```
┌─────────────────────────────────────────────────────────────────┐
│                        ECS MİMARİSİ                              │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                    ECS CLUSTER                           │   │
│   │                                                          │   │
│   │   EC2 LAUNCH TYPE           FARGATE LAUNCH TYPE          │   │
│   │   ────────────────          ──────────────────          │   │
│   │   ┌───────────────┐         ┌───────────────┐          │   │
│   │   │  EC2 Instance │         │    Fargate    │          │   │
│   │   │ ┌───────────┐ │         │  (Serverless) │          │   │
│   │   │ │Task (Container)│      │ ┌───────────┐ │          │   │
│   │   │ └───────────┘ │         │ │   Task    │ │          │   │
│   │   │ ┌───────────┐ │         │ └───────────┘ │          │   │
│   │   │ │Task (Container)│      │ ┌───────────┐ │          │   │
│   │   │ └───────────┘ │         │ │   Task    │ │          │   │
│   │   └───────────────┘         │ └───────────┘ │          │   │
│   │                              └───────────────┘          │   │
│   │   Sen yönetirsin            AWS yönetir                │   │
│   │                                                          │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│   Task Definition: Container konfigürasyonu (image, CPU, RAM)  │
│   Service: Task'ların çalışmasını yönetir                      │
└─────────────────────────────────────────────────────────────────┘
```

### 3.3 Amazon EKS (Elastic Kubernetes Service)

**Tanım:** Yönetilen Kubernetes servisi.

```
┌─────────────────────────────────────────────────────────────────┐
│                        EKS MİMARİSİ                              │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                    EKS CLUSTER                           │   │
│   │                                                          │   │
│   │   ┌─────────────────────────────────────────────────┐   │   │
│   │   │            CONTROL PLANE (AWS Yönetir)          │   │   │
│   │   │         • API Server                             │   │   │
│   │   │         • etcd                                   │   │   │
│   │   │         • Controller Manager                     │   │   │
│   │   └─────────────────────────────────────────────────┘   │   │
│   │                          │                               │   │
│   │   ┌──────────────────────┼──────────────────────┐       │   │
│   │   │                      ▼                       │       │   │
│   │   │   ┌─────────┐  ┌─────────┐  ┌─────────┐    │       │   │
│   │   │   │  Node   │  │  Node   │  │  Node   │    │       │   │
│   │   │   │  (EC2)  │  │  (EC2)  │  │(Fargate)│    │       │   │
│   │   │   │┌───────┐│  │┌───────┐│  │┌───────┐│    │       │   │
│   │   │   ││  Pod  ││  ││  Pod  ││  ││  Pod  ││    │       │   │
│   │   │   │└───────┘│  │└───────┘│  │└───────┘│    │       │   │
│   │   │   └─────────┘  └─────────┘  └─────────┘    │       │   │
│   │   │                DATA PLANE                    │       │   │
│   │   └──────────────────────────────────────────────┘       │   │
│   └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 3.4 ECS vs EKS Karşılaştırması

| Özellik            | ECS            | EKS                |
| ------------------ | -------------- | ------------------ |
| **Platform**       | AWS Native     | Kubernetes         |
| **Öğrenme Eğrisi** | Düşük          | Yüksek             |
| **Portability**    | AWS'e bağlı    | Multi-cloud        |
| **Ekosistem**      | AWS araçları   | K8s ekosistemi     |
| **Maliyet**        | ECS ücretsiz\* | EKS cluster ücreti |
| **Karmaşıklık**    | Basit          | Karmaşık           |

\*ECS cluster yönetimi ücretsiz, sadece altındaki kaynaklar (EC2/Fargate) ücretli.

### 3.5 Amazon ECR (Elastic Container Registry)

**Tanım:** Container image'ları depolamak için yönetilen registry.

```
┌─────────────────────────────────────────────────────────────────┐
│                          ECR                                     │
│                                                                 │
│   Developer ──► docker push ──► ECR Repository                  │
│                                     │                           │
│                                     ▼                           │
│                              ┌─────────────┐                   │
│                              │   Image     │                   │
│                              │ Scanning    │                   │
│                              └─────────────┘                   │
│                                     │                           │
│   ECS/EKS ◄── docker pull ◄─────────┘                          │
│                                                                 │
│   Özellikler:                                                  │
│   • Private registry                                           │
│   • Vulnerability scanning                                     │
│   • Image lifecycle policies                                   │
│   • Cross-region replication                                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4. Serverless Compute

### 4.1 AWS Lambda

**Tanım:** Event-driven, serverless compute servisi. Function as a Service (FaaS).

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS LAMBDA                                  │
│                                                                 │
│   EVENT SOURCES                   LAMBDA FUNCTION               │
│   ─────────────                   ───────────────               │
│   ┌─────────────┐                ┌─────────────────────────┐   │
│   │     S3      │───────────────►│                         │   │
│   │  (Upload)   │                │   def handler(event):   │   │
│   └─────────────┘                │       # Process event   │   │
│   ┌─────────────┐                │       return result     │   │
│   │ API Gateway │───────────────►│                         │   │
│   │   (HTTP)    │                └───────────┬─────────────┘   │
│   └─────────────┘                            │                  │
│   ┌─────────────┐                            ▼                  │
│   │  CloudWatch │                ┌─────────────────────────┐   │
│   │   (Cron)    │───────────────►│      DESTINATIONS       │   │
│   └─────────────┘                │  • S3                   │   │
│   ┌─────────────┐                │  • DynamoDB             │   │
│   │   DynamoDB  │───────────────►│  • SNS/SQS              │   │
│   │  (Streams)  │                │  • Step Functions       │   │
│   └─────────────┘                └─────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Lambda Özellikleri:**

| Özellik                | Değer                                               |
| ---------------------- | --------------------------------------------------- |
| **Max Çalışma Süresi** | 15 dakika                                           |
| **Max Bellek**         | 10,240 MB                                           |
| **Desteklenen Diller** | Python, Node.js, Java, Go, C#, Ruby, Custom Runtime |
| **Fiyatlandırma**      | İstek sayısı + GB-saniye                            |
| **Ölçekleme**          | Otomatik (1000 concurrent varsayılan)               |
| **Cold Start**         | İlk çağrıda gecikme olabilir                        |

**Lambda Kullanım Senaryoları:**

- API backend'leri
- Dosya işleme (S3 trigger)
- Gerçek zamanlı veri işleme
- Scheduled tasks (cron jobs)
- Event-driven automation

### 4.2 AWS Fargate

**Tanım:** Container'lar için serverless compute engine. ECS ve EKS ile kullanılır.

```
┌─────────────────────────────────────────────────────────────────┐
│                    FARGATE vs EC2                                │
├───────────────────────────────┬─────────────────────────────────┤
│          FARGATE              │            EC2                  │
├───────────────────────────────┼─────────────────────────────────┤
│                               │                                 │
│  AWS yönetir:                 │  Sen yönetirsin:               │
│  • Sunucu provisioning        │  • EC2 instance'ları           │
│  • Cluster yönetimi           │  • Scaling                     │
│  • OS patching                │  • OS patching                 │
│                               │  • Capacity planning           │
│  Sen yönetirsin:              │                                 │
│  • Task definition            │  Sen yönetirsin:               │
│  • CPU/Memory                 │  • Her şey                     │
│                               │                                 │
│  ✅ Operasyonel yük düşük     │  ✅ Tam kontrol                │
│  ✅ Saniyeye göre ödeme       │  ✅ Spot instance desteği      │
│  ❌ Daha pahalı olabilir      │  ❌ Daha fazla yönetim         │
│                               │                                 │
└───────────────────────────────┴─────────────────────────────────┘
```

---

## 5. Diğer Compute Servisleri

### 5.1 AWS Elastic Beanstalk

**Tanım:** Platform as a Service (PaaS). Uygulama deployment ve yönetim platformu.

```
┌─────────────────────────────────────────────────────────────────┐
│                    ELASTIC BEANSTALK                             │
│                                                                 │
│   Developer                                                     │
│      │                                                          │
│      │ Upload Code                                              │
│      ▼                                                          │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                  Elastic Beanstalk                       │   │
│   │                                                          │   │
│   │   Otomatik Oluşturur:                                   │   │
│   │   • EC2 Instances                                        │   │
│   │   • Load Balancer                                        │   │
│   │   • Auto Scaling Group                                   │   │
│   │   • Security Groups                                      │   │
│   │   • CloudWatch Alarms                                    │   │
│   │                                                          │   │
│   │   Desteklenen Platformlar:                              │   │
│   │   Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker    │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│   Avantaj: Koda odaklan, altyapıyı Beanstalk yönetsin         │
│   Maliyet: Sadece kullanılan kaynaklar için ödeme             │
└─────────────────────────────────────────────────────────────────┘
```

### 5.2 Amazon Lightsail

**Tanım:** Basit VPS (Virtual Private Server) servisi. Başlangıç için ideal.

**Özellikler:**

- Sabit aylık fiyatlandırma
- Önceden yapılandırılmış uygulamalar (WordPress, LAMP, Node.js)
- Basit arayüz
- Küçük-orta projeler için

| Lightsail            | EC2              |
| -------------------- | ---------------- |
| Sabit fiyat          | Kullandıkça öde  |
| Basit                | Karmaşık         |
| Sınırlı özelleştirme | Tam kontrol      |
| Başlangıç için       | İleri düzey için |

### 5.3 AWS Batch

**Tanım:** Toplu işleme (batch processing) için yönetilen servis.

**Kullanım Senaryoları:**

- Büyük veri analizi
- Video/görüntü işleme
- Finansal modelleme
- Bilimsel simülasyonlar

### 5.4 AWS Outposts

**Tanım:** AWS altyapısını kendi veri merkezinizde çalıştırma.

**Kullanım Senaryoları:**

- Düşük latency gereksinimleri
- Veri yerelliği
- Hybrid cloud mimarileri

---

## 6. Auto Scaling

### 6.1 Auto Scaling Konsepti

```
┌─────────────────────────────────────────────────────────────────┐
│                      AUTO SCALING                                │
│                                                                 │
│   TALEP DÜŞÜK              NORMAL                TALEP YÜKSEK   │
│   ──────────              ──────                 ───────────    │
│   ┌───┐                   ┌───┐ ┌───┐           ┌───┐ ┌───┐    │
│   │EC2│                   │EC2│ │EC2│           │EC2│ │EC2│    │
│   └───┘                   └───┘ └───┘           └───┘ └───┘    │
│                                                  ┌───┐ ┌───┐    │
│   Min: 1                  Desired: 2            │EC2│ │EC2│    │
│                                                  └───┘ └───┘    │
│                                                                 │
│                                                  Max: 4         │
│                                                                 │
│   ◄────────────────── Auto Scaling ──────────────────►         │
│                         (Elasticity)                            │
└─────────────────────────────────────────────────────────────────┘
```

### 6.2 Auto Scaling Türleri

| Tür                 | Tetikleyici      | Örnek                     |
| ------------------- | ---------------- | ------------------------- |
| **Target Tracking** | Metrik hedefi    | CPU %50'de tut            |
| **Step Scaling**    | Aşamalı kurallar | CPU %70 = +1, %90 = +2    |
| **Scheduled**       | Zaman bazlı      | Her Pazartesi 09:00'da +2 |
| **Predictive**      | ML tahminleri    | Geçmiş paternlere göre    |

### 6.3 Auto Scaling Bileşenleri

```
┌─────────────────────────────────────────────────────────────────┐
│               AUTO SCALING GROUP (ASG)                           │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                  LAUNCH TEMPLATE                         │   │
│   │  • AMI ID                                                │   │
│   │  • Instance Type                                         │   │
│   │  • Security Groups                                       │   │
│   │  • Key Pair                                              │   │
│   │  • User Data                                             │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                  SCALING POLICIES                        │   │
│   │  • Min Capacity: 2                                       │   │
│   │  • Desired Capacity: 2                                   │   │
│   │  • Max Capacity: 10                                      │   │
│   │  • Scale Out: CPU > 70%                                  │   │
│   │  • Scale In: CPU < 30%                                   │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                  HEALTH CHECKS                           │   │
│   │  • EC2 Health Check                                      │   │
│   │  • ELB Health Check                                      │   │
│   └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. Load Balancing

### 7.1 Elastic Load Balancing (ELB)

```
┌─────────────────────────────────────────────────────────────────┐
│                   LOAD BALANCING                                 │
│                                                                 │
│                        Users                                    │
│                          │                                      │
│                          ▼                                      │
│                 ┌─────────────────┐                            │
│                 │  Load Balancer  │                            │
│                 │   (ELB Node)    │                            │
│                 └────────┬────────┘                            │
│                          │                                      │
│          ┌───────────────┼───────────────┐                     │
│          ▼               ▼               ▼                     │
│     ┌─────────┐    ┌─────────┐    ┌─────────┐                 │
│     │   EC2   │    │   EC2   │    │   EC2   │                 │
│     │  AZ-a   │    │  AZ-b   │    │  AZ-c   │                 │
│     └─────────┘    └─────────┘    └─────────┘                 │
│                                                                 │
│   Load Balancer trafiği sağlıklı hedeflere dağıtır            │
└─────────────────────────────────────────────────────────────────┘
```

### 7.2 Load Balancer Türleri

| Tür     | Katman    | Protokol       | Kullanım                    |
| ------- | --------- | -------------- | --------------------------- |
| **ALB** | Layer 7   | HTTP/HTTPS     | Web apps, microservices     |
| **NLB** | Layer 4   | TCP/UDP/TLS    | Ultra düşük latency, gaming |
| **GLB** | Layer 3   | IP             | Güvenlik cihazları          |
| **CLB** | Layer 4/7 | HTTP/HTTPS/TCP | Legacy (kullanmayın)        |

### 7.3 ALB vs NLB

```
┌─────────────────────────────────────────────────────────────────┐
│                    ALB vs NLB                                    │
├───────────────────────────────┬─────────────────────────────────┤
│    APPLICATION LB (ALB)       │    NETWORK LB (NLB)             │
├───────────────────────────────┼─────────────────────────────────┤
│                               │                                 │
│  Layer 7 (HTTP/HTTPS)         │  Layer 4 (TCP/UDP)             │
│                               │                                 │
│  Path-based routing:          │  Static IP                     │
│  /api/* → API servers         │  Ultra düşük latency           │
│  /images/* → Image servers    │  Milyonlarca istek/saniye      │
│                               │                                 │
│  Host-based routing:          │  Kullanım:                     │
│  api.example.com → API        │  • Gaming                      │
│  www.example.com → Web        │  • IoT                         │
│                               │  • Real-time                   │
│  WebSocket desteği            │                                 │
│  Lambda hedef                 │  Elastic IP desteği            │
│                               │                                 │
└───────────────────────────────┴─────────────────────────────────┘
```

---

## 8. Sınav Senaryoları

| Senaryo                                | Doğru Seçim       |
| -------------------------------------- | ----------------- |
| "Web sunucusu kurulumu"                | EC2               |
| "Kubernetes kullanmak istiyorum"       | EKS               |
| "AWS native container orchestration"   | ECS               |
| "Sunucu yönetmeden container çalıştır" | Fargate           |
| "Event-driven fonksiyon çalıştır"      | Lambda            |
| "Basit VPS, sabit fiyat"               | Lightsail         |
| "Koda odaklan, altyapıyı yönetme"      | Elastic Beanstalk |
| "HTTP trafiğini dağıt"                 | ALB               |
| "Ultra düşük latency, TCP/UDP"         | NLB               |
| "Taleple birlikte otomatik ölçeklen"   | Auto Scaling      |
| "Batch işleme"                         | AWS Batch         |
| "On-premises'te AWS"                   | Outposts          |

---

## 9. Anahtar Terimler Sözlüğü

| Terim             | Tanım                             |
| ----------------- | --------------------------------- |
| **IaaS**          | Infrastructure as a Service (EC2) |
| **PaaS**          | Platform as a Service (Beanstalk) |
| **FaaS**          | Function as a Service (Lambda)    |
| **Instance**      | EC2 sanal sunucu                  |
| **AMI**           | Amazon Machine Image - şablon     |
| **Container**     | Paketlenmiş uygulama              |
| **Serverless**    | Sunucu yönetimi gerektirmeyen     |
| **Elasticity**    | Talebe göre otomatik ölçekleme    |
| **Auto Scaling**  | Otomatik instance ekleme/çıkarma  |
| **Load Balancer** | Trafik dağıtıcı                   |

---

## 10. Sınav İpuçları

```
✓ EC2 = IaaS, sanal sunucu, tam kontrol
✓ Lambda = Serverless, max 15 dk, event-driven
✓ Fargate = Serverless containers (ECS/EKS ile)
✓ ECS = AWS native container, EKS = Kubernetes

✓ On-Demand = En esnek, en pahalı
✓ Reserved = 1-3 yıl, %72 indirim
✓ Spot = %90 indirim, kesintiye uğrayabilir

✓ ALB = Layer 7, HTTP/HTTPS, path/host routing
✓ NLB = Layer 4, TCP/UDP, ultra düşük latency

✓ Auto Scaling = Elasticity sağlar
✓ Instance metadata = 169.254.169.254
```

---

## Referanslar

- [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [Amazon ECS Documentation](https://docs.aws.amazon.com/ecs/)
- [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/)
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- [Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/)
- [Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/)

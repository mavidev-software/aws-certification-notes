# Domain 3: Cloud Technology and Services

> **Sınav Ağırlığı:** %34 (En yüksek ağırlıklı domain!)
>
> **Kaynak:** [AWS CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

## Genel Bakış

Domain 3, AWS Cloud Practitioner sınavının en geniş ve en ağırlıklı bölümüdür. AWS'nin temel servislerini, global altyapısını ve bu servislerin nasıl kullanılacağını kapsar.

```
┌─────────────────────────────────────────────────────────────────┐
│                    DOMAIN 3 KAPSAMI                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Toplam 8 Task Statement:                                      │
│                                                                 │
│  ├── Task 3.1: Deployment ve Operations                        │
│  ├── Task 3.2: Global Infrastructure                           │
│  ├── Task 3.3: Compute Services                                │
│  ├── Task 3.4: Database Services                               │
│  ├── Task 3.5: Network Services                                │
│  ├── Task 3.6: Storage Services                                │
│  ├── Task 3.7: AI/ML ve Analytics                              │
│  └── Task 3.8: Diğer Servisler                                 │
│                                                                 │
│  Tahmini Soru Sayısı: ~22 soru (65 x 0.34)                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Task Statement Dosyaları

| Task | Başlık | Dosya |
|------|--------|-------|
| 3.1 | Deployment ve Operations | [task-3.1-deployment-operations.md](task-3.1-deployment-operations.md) |
| 3.2 | Global Infrastructure | [task-3.2-global-infrastructure.md](task-3.2-global-infrastructure.md) |
| 3.3 | Compute Services | [task-3.3-compute-services.md](task-3.3-compute-services.md) |
| 3.4 | Database Services | [task-3.4-database-services.md](task-3.4-database-services.md) |
| 3.5 | Network Services | [task-3.5-network-services.md](task-3.5-network-services.md) |
| 3.6 | Storage Services | [task-3.6-storage-services.md](task-3.6-storage-services.md) |
| 3.7 | AI/ML ve Analytics | [task-3.7-ai-ml-analytics-services.md](task-3.7-ai-ml-analytics-services.md) |
| 3.8 | Diğer Servisler | [task-3.8-other-services.md](task-3.8-other-services.md) |

---

## Task Statement Özetleri

### Task 3.1: Deployment ve Operations

AWS'de deployment ve operasyon yöntemlerini tanımlama:
- AWS erişim yöntemleri (Console, CLI, SDK)
- Deployment modelleri (Public, Private, Hybrid Cloud, Multi-cloud)
- AWS managed vs unmanaged servisler
- IaC (CloudFormation, CDK)

**Anahtar Servisler:** AWS Console, CLI, SDK, CloudFormation, CDK, Elastic Beanstalk

---

### Task 3.2: Global Infrastructure

AWS global altyapısını anlama:
- Regions, Availability Zones, Edge Locations
- Yüksek erişilebilirlik tasarımı
- Multi-Region kullanım senaryoları
- Regional vs Global servisler

**Anahtar Kavramlar:** Region, AZ, Edge Location, HA, DR, Latency

---

### Task 3.3: Compute Services

AWS compute servislerini tanımlama:
- EC2 (instance types, pricing, AMI)
- Container servisleri (ECS, EKS, Fargate, ECR)
- Serverless (Lambda)
- Auto Scaling, Load Balancing

**Anahtar Servisler:** EC2, Lambda, ECS, EKS, Fargate, Auto Scaling, ELB

---

### Task 3.4: Database Services

AWS veritabanı servislerini tanımlama:
- Relational (RDS, Aurora)
- NoSQL (DynamoDB)
- In-memory (ElastiCache)
- Data Warehouse (Redshift)

**Anahtar Servisler:** RDS, Aurora, DynamoDB, ElastiCache, Redshift

---

### Task 3.5: Network Services

AWS ağ servislerini tanımlama:
- VPC (Subnet, IGW, NAT, Route Table)
- Güvenlik (Security Groups, NACLs)
- Bağlantı (VPC Peering, VPN, Direct Connect)
- DNS ve CDN (Route 53, CloudFront)

**Anahtar Servisler:** VPC, Route 53, CloudFront, Direct Connect, VPN, ELB

---

### Task 3.6: Storage Services

AWS depolama servislerini tanımlama:
- Object Storage (S3, storage classes)
- Block Storage (EBS)
- File Storage (EFS, FSx)
- Hybrid ve Transfer (Storage Gateway, Snow Family)

**Anahtar Servisler:** S3, EBS, EFS, FSx, Storage Gateway, Snowball

---

### Task 3.7: AI/ML ve Analytics

AWS AI/ML ve analytics servislerini tanımlama:
- ML platformu (SageMaker)
- Pre-trained AI (Rekognition, Comprehend, Polly, etc.)
- Analytics (Athena, Kinesis, QuickSight)
- ETL (Glue)

**Anahtar Servisler:** SageMaker, Rekognition, Comprehend, Athena, Kinesis, QuickSight, Glue

---

### Task 3.8: Diğer Servisler

Diğer in-scope AWS servislerini tanımlama:
- Application Integration (SQS, SNS, EventBridge, Step Functions)
- Developer Tools (CodePipeline, CodeBuild, CodeDeploy)
- Management (CloudWatch, CloudTrail, Config, Systems Manager)
- End User Computing (WorkSpaces, AppStream)

**Anahtar Servisler:** SQS, SNS, EventBridge, CloudWatch, CloudTrail, Config

---

## En Kritik Konular

### Sık Çıkan Karşılaştırmalar

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      SIK ÇIKAN KARŞILAŞTIRMALAR                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. Security Group vs NACL                                                  │
│     • SG: Stateful, instance seviyesi, sadece allow                        │
│     • NACL: Stateless, subnet seviyesi, allow + deny                       │
│                                                                             │
│  2. CloudWatch vs CloudTrail                                               │
│     • CloudWatch: HOW (metrics, logs, alarms)                              │
│     • CloudTrail: WHO (API logs, audit)                                    │
│                                                                             │
│  3. RDS vs Aurora vs Redshift                                              │
│     • RDS: Standard relational (OLTP)                                      │
│     • Aurora: High-performance relational (OLTP)                           │
│     • Redshift: Data warehouse (OLAP)                                      │
│                                                                             │
│  4. SQS vs SNS                                                             │
│     • SQS: Queue, pull model, message retention                            │
│     • SNS: Pub/Sub, push model, no retention                               │
│                                                                             │
│  5. S3 Storage Classes                                                     │
│     • Standard: Sık erişim                                                 │
│     • IA: Nadir erişim (30 gün min)                                        │
│     • Glacier: Arşiv (90-180 gün min)                                      │
│                                                                             │
│  6. EC2 vs Lambda                                                          │
│     • EC2: Sanal sunucu, uzun süren iş yükleri                            │
│     • Lambda: Serverless, kısa süreli (15 dk max)                         │
│                                                                             │
│  7. EBS vs EFS vs S3                                                       │
│     • EBS: Block, tek AZ, tek EC2                                          │
│     • EFS: File, multi-AZ, çoklu EC2                                       │
│     • S3: Object, global, web erişimi                                      │
│                                                                             │
│  8. CloudFront vs Global Accelerator                                       │
│     • CloudFront: CDN, HTTP/HTTPS, caching                                 │
│     • Global Accelerator: TCP/UDP, no cache, static IP                     │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Servis Seçim Rehberi

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        SERVİS SEÇİM REHBERİ                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  COMPUTE:                                                                   │
│  "Sanal sunucu gerekli"           → EC2                                    │
│  "Sunucu yönetmek istemiyorum"    → Lambda / Fargate                       │
│  "Container orchestration"         → ECS / EKS                              │
│  "Otomatik ölçekleme"              → Auto Scaling                           │
│                                                                             │
│  DATABASE:                                                                  │
│  "Relational + yüksek performans" → Aurora                                 │
│  "Relational + standart"           → RDS                                    │
│  "NoSQL + serverless"              → DynamoDB                               │
│  "Analytics / OLAP"                → Redshift                               │
│  "Caching"                         → ElastiCache                            │
│                                                                             │
│  STORAGE:                                                                   │
│  "Web'den erişim / static files"  → S3                                     │
│  "EC2 disk"                        → EBS                                    │
│  "Paylaşımlı dosya (Linux)"       → EFS                                    │
│  "Paylaşımlı dosya (Windows)"     → FSx for Windows                        │
│  "Arşiv (nadir erişim)"           → S3 Glacier                             │
│  "Büyük veri transferi"           → Snow Family                            │
│                                                                             │
│  NETWORK:                                                                   │
│  "DNS"                             → Route 53                               │
│  "CDN"                             → CloudFront                             │
│  "On-prem bağlantı (hızlı)"       → VPN                                    │
│  "On-prem bağlantı (tutarlı)"     → Direct Connect                         │
│  "VPC arası bağlantı"             → VPC Peering / Transit Gateway          │
│                                                                             │
│  AI/ML:                                                                    │
│  "Custom ML model"                 → SageMaker                              │
│  "Görüntü analizi"                 → Rekognition                            │
│  "Metin analizi / NLP"            → Comprehend                             │
│  "Chatbot"                         → Lex                                    │
│  "Text → Speech"                   → Polly                                  │
│  "Speech → Text"                   → Transcribe                             │
│                                                                             │
│  ANALYTICS:                                                                │
│  "S3'te SQL sorgusu"              → Athena                                 │
│  "Real-time streaming"            → Kinesis                                │
│  "BI dashboards"                   → QuickSight                             │
│  "ETL"                             → Glue                                   │
│                                                                             │
│  INTEGRATION:                                                              │
│  "Decoupling / Queue"             → SQS                                    │
│  "Notifications"                   → SNS                                    │
│  "Event routing"                   → EventBridge                            │
│  "Workflow"                        → Step Functions                         │
│                                                                             │
│  MANAGEMENT:                                                               │
│  "Metrics / Alarms"                → CloudWatch                             │
│  "API Audit logs"                  → CloudTrail                             │
│  "Configuration compliance"        → Config                                 │
│  "IaC"                             → CloudFormation                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Çalışma Stratejisi

### Önerilen Sıra

1. **Task 3.2: Global Infrastructure** - Temel kavramları anla
2. **Task 3.3: Compute** - En temel servisler (EC2, Lambda)
3. **Task 3.4: Database** - RDS, DynamoDB farkları
4. **Task 3.6: Storage** - S3 storage classes önemli!
5. **Task 3.5: Network** - VPC, Security Groups
6. **Task 3.1: Deployment** - Erişim yöntemleri
7. **Task 3.7: AI/ML Analytics** - Servis eşleştirme
8. **Task 3.8: Other Services** - SQS/SNS, CloudWatch/CloudTrail

### Pratik İpuçları

```
┌─────────────────────────────────────────────────────────────────┐
│                      PRATİK İPUÇLARI                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Karşılaştırmaları ezberle                                  │
│     • SG vs NACL                                               │
│     • CloudWatch vs CloudTrail                                 │
│     • SQS vs SNS                                               │
│                                                                 │
│  2. Anahtar kelimelere dikkat et                               │
│     • "Serverless" → Lambda, DynamoDB, Fargate                │
│     • "Managed" → RDS, ECS, managed services                  │
│     • "Decoupling" → SQS                                      │
│     • "Real-time" → Kinesis                                   │
│                                                                 │
│  3. Senaryo bazlı düşün                                        │
│     • "Web sitesi barındırma" → S3 + CloudFront               │
│     • "Veritabanı yedekleme" → Automated backups, snapshots   │
│     • "API oluşturma" → API Gateway + Lambda                  │
│                                                                 │
│  4. Fiyatlandırma modellerini öğren                           │
│     • On-Demand vs Reserved vs Spot (EC2)                     │
│     • S3 storage classes                                       │
│     • Serverless = kullandığın kadar öde                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

### AWS Resmi Dokümantasyon

- [AWS CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)
- [In-Scope Services](https://docs.aws.amazon.com/aws-certification/latest/examguides/clf-02-in-scope-services.html)
- [AWS Documentation](https://docs.aws.amazon.com/)

### Eğitim Kaynakları

- [AWS Cloud Practitioner Essentials](https://explore.skillbuilder.aws/learn/course/external/view/elearning/134/aws-cloud-practitioner-essentials)
- [AWS Skill Builder](https://skillbuilder.aws)

---

> **Hedef:** Domain 3 en geniş domain olduğu için en çok zaman ayırılması gereken bölüm. Pratik sorularda %80+ tutturabilene kadar çalış!

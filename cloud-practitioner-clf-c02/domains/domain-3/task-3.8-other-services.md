# Task 3.8: Diğer AWS Servisleri

> **Resmi Tanım:** Identify services from other in-scope AWS service categories.
>
> **Kaynak:** [AWS CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

## İçindekiler

1. [Application Integration](#application-integration)
2. [Developer Tools](#developer-tools)
3. [Management & Governance](#management--governance)
4. [Customer Engagement](#customer-engagement)
5. [End User Computing](#end-user-computing)
6. [Frontend & Mobile](#frontend--mobile)
7. [IoT](#iot)
8. [Karşılaştırma Tabloları](#karşılaştırma-tabloları)
9. [Sınav Senaryoları](#sınav-senaryoları)

---

## Application Integration

### Amazon SQS

```
┌─────────────────────────────────────────────────────────────────┐
│                        AMAZON SQS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Simple Queue Service - Yönetilen mesaj kuyruğu"              │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Producer           SQS Queue           Consumer          │ │
│  │   ┌─────────┐       ┌─────────┐        ┌─────────┐        │ │
│  │   │ App A   │ ───►  │ ████████│  ◄──── │  App B  │        │ │
│  │   │ (Send)  │       │ ████████│  Poll  │ (Receive│        │ │
│  │   │         │       │ Messages│        │  Delete)│        │ │
│  │   └─────────┘       └─────────┘        └─────────┘        │ │
│  │                                                            │ │
│  │   Decoupling: Uygulamaları birbirinden bağımsız hale getirir│
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Queue Türleri:                                                │
│  ┌───────────────────────────┬───────────────────────────────┐ │
│  │     Standard Queue        │       FIFO Queue              │ │
│  ├───────────────────────────┼───────────────────────────────┤ │
│  │ At-least-once delivery    │ Exactly-once delivery         │ │
│  │ Best-effort ordering      │ First-In-First-Out sırası     │ │
│  │ Sınırsız throughput       │ 300 msg/s (batch: 3000)       │ │
│  │ Default seçenek           │ Sıra önemliyse kullan         │ │
│  └───────────────────────────┴───────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Fully managed, serverless                                  │
│  • Unlimited queues and messages                              │
│  • Message size: max 256 KB                                   │
│  • Retention: 1 dakika - 14 gün (default: 4 gün)             │
│  • Visibility timeout: mesaj işlenirken gizle                 │
│  • Dead-letter queue: başarısız mesajlar için                 │
│  • Long polling: maliyet optimizasyonu                        │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Uygulama decoupling                                        │
│  • Async işleme                                                │
│  • Buffer (tepe yük yönetimi)                                 │
│  • Batch işleme                                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon SNS

```
┌─────────────────────────────────────────────────────────────────┐
│                        AMAZON SNS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Simple Notification Service - Pub/Sub mesajlaşma"           │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Publisher              SNS Topic           Subscribers   │ │
│  │   ┌─────────┐           ┌───────┐                         │ │
│  │   │ App     │ ────────► │ TOPIC │ ──────► SQS Queue       │ │
│  │   │         │  Publish  │       │ ──────► Lambda          │ │
│  │   │         │           │       │ ──────► HTTP/S Endpoint │ │
│  │   └─────────┘           │       │ ──────► Email           │ │
│  │                         │       │ ──────► SMS             │ │
│  │                         └───────┘ ──────► Mobile Push     │ │
│  │                                                            │ │
│  │   Fan-out: Bir mesajı birden fazla alıcıya ilet           │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  SNS vs SQS:                                                   │
│  ┌───────────────────────────┬───────────────────────────────┐ │
│  │          SNS              │           SQS                 │ │
│  ├───────────────────────────┼───────────────────────────────┤ │
│  │ Push model (push to subs) │ Pull model (poll messages)   │ │
│  │ Pub/Sub pattern           │ Queue pattern                │ │
│  │ Mesaj saklanmaz           │ Mesaj saklanır (retention)   │ │
│  │ Birden fazla subscriber   │ Genellikle tek consumer      │ │
│  │ Notification/Alert        │ Decoupling/Buffer            │ │
│  └───────────────────────────┴───────────────────────────────┘ │
│                                                                 │
│  SNS + SQS Fan-out Pattern:                                    │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │                      ┌──────► SQS Queue 1 ──► Consumer A  │ │
│  │   Publisher ──► SNS ─┼──────► SQS Queue 2 ──► Consumer B  │ │
│  │                      └──────► SQS Queue 3 ──► Consumer C  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Alarm ve bildirimler                                       │
│  • Email/SMS kampanyaları                                     │
│  • Mobile push notifications                                  │
│  • Fan-out pattern                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon EventBridge

```
┌─────────────────────────────────────────────────────────────────┐
│                    AMAZON EVENTBRIDGE                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Serverless event bus servisi"                                │
│  (Eski adı: CloudWatch Events)                                 │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Event Sources          EventBridge          Targets      │ │
│  │   ┌─────────────┐       ┌─────────────┐                   │ │
│  │   │ AWS Services│ ───►  │             │ ──► Lambda        │ │
│  │   │ (EC2, S3,   │       │   Event     │ ──► SQS           │ │
│  │   │  ECS...)    │       │    Bus      │ ──► SNS           │ │
│  │   ├─────────────┤       │             │ ──► Step Functions│ │
│  │   │ SaaS Apps   │ ───►  │   ┌─────┐   │ ──► Kinesis       │ │
│  │   │ (Zendesk,   │       │   │Rules│   │ ──► API Gateway  │ │
│  │   │  DataDog...)│       │   └─────┘   │ ──► 20+ targets   │ │
│  │   ├─────────────┤       │             │                   │ │
│  │   │ Custom Apps │ ───►  │             │                   │ │
│  │   └─────────────┘       └─────────────┘                   │ │
│  │                                                            │ │
│  │   Rules: Event pattern matching ile routing                │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Serverless                                                  │
│  • Event pattern matching                                     │
│  • Scheduled rules (cron)                                     │
│  • Schema registry                                            │
│  • Archive and replay                                         │
│  • Cross-account events                                       │
│                                                                 │
│  Örnek Kullanımlar:                                            │
│  • EC2 instance state change → Lambda                         │
│  • S3 object created → Step Functions                         │
│  • Scheduled task (her gün 09:00) → Lambda                    │
│  • Custom event → SNS notification                            │
│                                                                 │
│  EventBridge vs SNS:                                           │
│  • EventBridge = Event routing, filtering, transformation      │
│  • SNS = Simple pub/sub notification                          │
│  • EventBridge daha gelişmiş event-driven mimari için         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS Step Functions

```
┌─────────────────────────────────────────────────────────────────┐
│                    AWS STEP FUNCTIONS                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Serverless workflow orkestrasyon servisi"                    │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                    STATE MACHINE                           │ │
│  │                                                            │ │
│  │       ┌─────────────────────────────────────────────┐     │ │
│  │       │                                             │     │ │
│  │       ▼                                             │     │ │
│  │   ┌───────────┐                                     │     │ │
│  │   │   Start   │                                     │     │ │
│  │   └─────┬─────┘                                     │     │ │
│  │         │                                           │     │ │
│  │         ▼                                           │     │ │
│  │   ┌───────────┐     Success    ┌───────────┐       │     │ │
│  │   │  Lambda   │ ─────────────► │  Lambda   │       │     │ │
│  │   │  Step 1   │                │  Step 2   │       │     │ │
│  │   └─────┬─────┘                └─────┬─────┘       │     │ │
│  │         │ Fail                       │             │     │ │
│  │         ▼                            ▼             │     │ │
│  │   ┌───────────┐              ┌───────────┐        │     │ │
│  │   │  Retry /  │              │   Wait    │        │     │ │
│  │   │  Catch    │              │  30 sec   │        │     │ │
│  │   └───────────┘              └─────┬─────┘        │     │ │
│  │                                    │               │     │ │
│  │                                    ▼               │     │ │
│  │                              ┌───────────┐        │     │ │
│  │                              │   Choice  │        │     │ │
│  │                              └─────┬─────┘        │     │ │
│  │                              ┌─────┴─────┐        │     │ │
│  │                              ▼           ▼        │     │ │
│  │                          ┌──────┐    ┌──────┐     │     │ │
│  │                          │Path A│    │Path B│     │     │ │
│  │                          └──┬───┘    └──┬───┘     │     │ │
│  │                             └─────┬─────┘         │     │ │
│  │                                   ▼               │     │ │
│  │                             ┌───────────┐         │     │ │
│  │                             │    End    │         │     │ │
│  │                             └───────────┘         │     │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  State Types:                                                  │
│  • Task: Lambda, ECS, Batch, etc. çalıştır                    │
│  • Choice: Koşullu dallanma                                   │
│  • Wait: Bekle                                                 │
│  • Parallel: Paralel işlem                                    │
│  • Map: Liste üzerinde iterasyon                              │
│  • Pass: Veri geçir                                           │
│  • Succeed/Fail: Bitir                                        │
│                                                                 │
│  Workflow Türleri:                                             │
│  • Standard: Uzun süren (1 yıla kadar)                        │
│  • Express: Kısa süren, yüksek hacimli (5 dakika)             │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Order processing                                           │
│  • Data processing pipelines                                  │
│  • Microservices orchestration                                │
│  • Human approval workflows                                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Developer Tools

### AWS CodePipeline

```
┌─────────────────────────────────────────────────────────────────┐
│                     AWS CODEPIPELINE                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Continuous Delivery (CD) servisi"                            │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐  │ │
│  │   │ SOURCE  │ ► │  BUILD  │ ► │  TEST   │ ► │ DEPLOY  │  │ │
│  │   │         │   │         │   │         │   │         │  │ │
│  │   │CodeCommit│   │CodeBuild│   │CodeBuild│   │CodeDeploy│ │ │
│  │   │ GitHub  │   │         │   │         │   │ ECS     │  │ │
│  │   │ S3      │   │         │   │         │   │ Lambda  │  │ │
│  │   └─────────┘   └─────────┘   └─────────┘   └─────────┘  │ │
│  │                                                            │ │
│  │   Pipeline: Stages → Actions → Artifacts                  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Entegrasyonlar:                                               │
│  • Source: CodeCommit, GitHub, GitLab, S3, ECR               │
│  • Build: CodeBuild, Jenkins                                  │
│  • Deploy: CodeDeploy, ECS, S3, CloudFormation, Lambda       │
│  • Test: CodeBuild, 3rd party tools                          │
│  • Approval: Manual approval stages                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS CodeBuild

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS CODEBUILD                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Fully managed build service"                                 │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Source Code          CodeBuild          Output           │ │
│  │   ┌─────────┐         ┌─────────┐        ┌─────────┐      │ │
│  │   │CodeCommit│ ─────► │  Build  │ ─────► │   S3    │      │ │
│  │   │ GitHub  │         │ Server  │        │  (Artifacts)│   │ │
│  │   │ S3      │         │(Managed)│        │         │      │ │
│  │   └─────────┘         └─────────┘        └─────────┘      │ │
│  │                            │                               │ │
│  │                       buildspec.yml                        │ │
│  │                       (build commands)                     │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Fully managed, serverless                                  │
│  • Pay per build minute                                       │
│  • Docker support                                             │
│  • Pre-built images (Java, Python, Node.js, .NET, Go...)     │
│  • Custom images (ECR)                                        │
│  • Parallel builds                                            │
│  • VPC support                                                │
│  • Local caching                                              │
│                                                                 │
│  buildspec.yml:                                                │
│  • phases: install, pre_build, build, post_build             │
│  • artifacts: output files                                    │
│  • cache: caching settings                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS CodeDeploy

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS CODEDEPLOY                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Automated deployment service"                                │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Application Revision        Deployment Target            │ │
│  │   ┌─────────────────┐        ┌─────────────────┐          │ │
│  │   │  S3 / GitHub    │ ─────► │  EC2 Instances  │          │ │
│  │   │                 │        │  Auto Scaling   │          │ │
│  │   │  appspec.yml    │        │  ECS           │          │ │
│  │   │  (deployment    │        │  Lambda        │          │ │
│  │   │   instructions) │        │  On-premises   │          │ │
│  │   └─────────────────┘        └─────────────────┘          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Deployment Strategies:                                        │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  In-Place (Rolling):                                      │ │
│  │  ┌────┐ ┌────┐ ┌────┐   ┌────┐ ┌────┐ ┌────┐            │ │
│  │  │ V1 │ │ V1 │ │ V1 │ ► │ V2 │ │ V1 │ │ V1 │ ► ...      │ │
│  │  └────┘ └────┘ └────┘   └────┘ └────┘ └────┘            │ │
│  │                                                            │ │
│  │  Blue/Green:                                              │ │
│  │  ┌──────────────┐      ┌──────────────┐                  │ │
│  │  │ Blue (V1)    │      │ Green (V2)   │                  │ │
│  │  │ ┌────┐┌────┐ │  ►   │ ┌────┐┌────┐ │                  │ │
│  │  │ │ V1 ││ V1 │ │      │ │ V2 ││ V2 │ │  ← Traffic      │ │
│  │  │ └────┘└────┘ │      │ └────┘└────┘ │    switch        │ │
│  │  └──────────────┘      └──────────────┘                  │ │
│  │                                                            │ │
│  │  Canary: Trafiğin %'sini yeni versiyona yönlendir        │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  appspec.yml:                                                  │
│  • Lifecycle hooks                                            │
│  • Scripts (BeforeInstall, AfterInstall, etc.)               │
│  • File locations                                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS CodeCommit

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS CODECOMMIT                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Fully managed Git repository"                                │
│                                                                 │
│  NOT: CodeCommit artık yeni kullanıcılara kapalı (2024).      │
│  Mevcut kullanıcılar devam edebilir.                          │
│  Alternatif: GitHub, GitLab, Bitbucket                        │
│                                                                 │
│  Özellikler (referans için):                                   │
│  • Tam Git uyumlu                                             │
│  • AWS IAM entegrasyonu                                       │
│  • Private repositories                                        │
│  • Encryption at rest (KMS)                                   │
│  • CodePipeline entegrasyonu                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### CI/CD Pipeline Özeti

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS CI/CD PIPELINE                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │         ┌─────────────────────────────────────────────┐   │ │
│  │         │              CodePipeline                    │   │ │
│  │         │          (Orchestration)                     │   │ │
│  │         └─────────────────────────────────────────────┘   │ │
│  │                            │                               │ │
│  │     ┌──────────┬───────────┼───────────┬──────────┐       │ │
│  │     ▼          ▼           ▼           ▼          ▼       │ │
│  │ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐   │ │
│  │ │  Code  │ │  Code  │ │  Code  │ │  Code  │ │ Deploy │   │ │
│  │ │ Commit*│ │  Build │ │  Build │ │ Deploy │ │(Manual)│   │ │
│  │ │ GitHub │ │ (Build)│ │ (Test) │ │        │ │Approval│   │ │
│  │ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘   │ │
│  │  Source     Build       Test       Deploy     Prod       │ │
│  │                                                            │ │
│  │  *CodeCommit yeni kullanıcılara kapalı                    │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Code* Servisleri:                                             │
│  ┌───────────────────┬──────────────────────────────────────┐ │
│  │ CodeCommit*       │ Git repository (deprecated)          │ │
│  │ CodeBuild         │ Build/Test (serverless)              │ │
│  │ CodeDeploy        │ Deployment automation                │ │
│  │ CodePipeline      │ CI/CD orchestration                  │ │
│  │ CodeArtifact      │ Package management (npm, Maven, pip) │ │
│  │ CodeGuru          │ Code review + performance profiling  │ │
│  └───────────────────┴──────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Management & Governance

### Amazon CloudWatch

```
┌─────────────────────────────────────────────────────────────────┐
│                     AMAZON CLOUDWATCH                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "AWS monitoring ve observability servisi"                     │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                CLOUDWATCH BİLEŞENLERİ                      │ │
│  │                                                            │ │
│  │  1. CloudWatch Metrics                                    │ │
│  │  ──────────────────────                                   │ │
│  │  • AWS servisleri otomatik metrik gönderir               │ │
│  │  • CPU, Network, Disk, etc.                              │ │
│  │  • Custom metrics (application metrics)                  │ │
│  │  • 15 ay saklama                                          │ │
│  │                                                            │ │
│  │  2. CloudWatch Alarms                                     │ │
│  │  ─────────────────────                                    │ │
│  │  • Metrik eşik değerlerine göre alarm                    │ │
│  │  • Actions: SNS, Auto Scaling, EC2 actions               │ │
│  │  • States: OK, ALARM, INSUFFICIENT_DATA                  │ │
│  │                                                            │ │
│  │  3. CloudWatch Logs                                       │ │
│  │  ───────────────────                                      │ │
│  │  • Application logs toplama                              │ │
│  │  • Log groups → Log streams → Log events                 │ │
│  │  • Log Insights: SQL-like sorgu                          │ │
│  │  • Metric filters: Loglardan metrik oluştur              │ │
│  │  • Export to S3, stream to Kinesis/Lambda                │ │
│  │                                                            │ │
│  │  4. CloudWatch Dashboards                                 │ │
│  │  ────────────────────────                                 │ │
│  │  • Custom görselleştirme                                 │ │
│  │  • Cross-account, cross-region                           │ │
│  │                                                            │ │
│  │  5. CloudWatch Events / EventBridge                      │ │
│  │  ───────────────────────────────────                     │ │
│  │  • Artık EventBridge olarak devam ediyor                 │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  CloudWatch Agent:                                             │
│  • EC2/on-prem'den detaylı metrik ve log toplama             │
│  • Memory, disk utilization (default'ta yok)                 │
│  • Custom log dosyaları                                       │
│                                                                 │
│  CloudWatch Synthetics:                                        │
│  • Canary tests (synthetic monitoring)                        │
│  • Endpoint availability checking                             │
│  • Screenshot and HAR capture                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS CloudTrail

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS CLOUDTRAIL                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "AWS API aktivitelerinin kaydı - WHO did WHAT"               │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   AWS API Call          CloudTrail          Storage        │ │
│  │   ┌─────────────┐      ┌──────────┐      ┌────────────┐  │ │
│  │   │ EC2:        │ ───► │          │ ───► │    S3      │  │ │
│  │   │ RunInstances│      │  Trail   │      │  (Bucket)  │  │ │
│  │   │             │      │          │      │            │  │ │
│  │   │ IAM:        │ ───► │  Logs    │ ───► │ CloudWatch │  │ │
│  │   │ CreateUser  │      │  Events  │      │   Logs     │  │ │
│  │   └─────────────┘      └──────────┘      └────────────┘  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Event Türleri:                                                │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  Management Events (default):                             │ │
│  │  • AWS kaynakları üzerindeki işlemler                    │ │
│  │  • CreateUser, StartInstances, AttachPolicy...           │ │
│  │  • Ücretsiz (90 gün geçmiş)                              │ │
│  │                                                            │ │
│  │  Data Events (opsiyonel):                                 │ │
│  │  • S3 object-level (GetObject, PutObject)                │ │
│  │  • Lambda function invocations                           │ │
│  │  • Ek maliyet                                             │ │
│  │                                                            │ │
│  │  Insights Events (opsiyonel):                             │ │
│  │  • Anormal aktivite tespiti                              │ │
│  │  • Ek maliyet                                             │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  CloudTrail vs CloudWatch:                                     │
│  ┌───────────────────────────┬───────────────────────────────┐ │
│  │       CloudTrail          │        CloudWatch             │ │
│  ├───────────────────────────┼───────────────────────────────┤ │
│  │ WHO did WHAT              │ HOW is it performing          │ │
│  │ API aktivite logları      │ Metrics, alarms, dashboards   │ │
│  │ Audit, compliance         │ Monitoring, troubleshooting   │ │
│  │ Security analysis         │ Performance analysis          │ │
│  └───────────────────────────┴───────────────────────────────┘ │
│                                                                 │
│  ÖNEMLİ: CloudTrail = Güvenlik ve Uyumluluk için              │
│  "Kim, ne zaman, hangi kaynağı değiştirdi?"                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS Config

```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS CONFIG                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "AWS kaynak yapılandırmalarını izle ve değerlendir"          │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   1. Resource Inventory                                   │ │
│  │   ──────────────────────                                  │ │
│  │   • Tüm AWS kaynaklarının envanteri                      │ │
│  │   • Yapılandırma geçmişi                                 │ │
│  │   • "Geçen hafta EC2 güvenlik grubu nasıldı?"           │ │
│  │                                                            │ │
│  │   2. Configuration Recording                              │ │
│  │   ──────────────────────────                              │ │
│  │   • Kaynak değişikliklerini kaydet                       │ │
│  │   • S3'e configuration snapshots                         │ │
│  │   • SNS notifications                                     │ │
│  │                                                            │ │
│  │   3. Config Rules                                         │ │
│  │   ─────────────────                                       │ │
│  │   • Compliance kuralları tanımla                         │ │
│  │   • AWS Managed Rules (150+)                             │ │
│  │   • Custom Rules (Lambda)                                │ │
│  │                                                            │ │
│  │   Örnek Kurallar:                                        │ │
│  │   • restricted-ssh: SSH 0.0.0.0/0'dan açık mı?          │ │
│  │   • encrypted-volumes: EBS şifreli mi?                   │ │
│  │   • required-tags: Gerekli tag'ler var mı?              │ │
│  │                                                            │ │
│  │   4. Remediation                                          │ │
│  │   ─────────────                                           │ │
│  │   • Uyumsuz kaynakları otomatik düzelt                   │ │
│  │   • Systems Manager Automation                           │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Config vs CloudTrail:                                         │
│  ┌───────────────────────────┬───────────────────────────────┐ │
│  │        Config             │        CloudTrail             │ │
│  ├───────────────────────────┼───────────────────────────────┤ │
│  │ WHAT is configured        │ WHO did WHAT                  │ │
│  │ Kaynak durumu             │ API aktivitesi                │ │
│  │ Compliance değerlendirme  │ Aktivite kaydı                │ │
│  │ "EC2 şu an nasıl?"        │ "EC2'yi kim değiştirdi?"     │ │
│  └───────────────────────────┴───────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS Systems Manager

```
┌─────────────────────────────────────────────────────────────────┐
│                  AWS SYSTEMS MANAGER                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "EC2 ve on-premises sunucu yönetimi"                          │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                 SYSTEMS MANAGER BİLEŞENLERİ                │ │
│  │                                                            │ │
│  │  Session Manager                                          │ │
│  │  ───────────────                                          │ │
│  │  • SSH/RDP olmadan EC2'ye bağlan                         │ │
│  │  • Port 22/3389 açmaya gerek yok                         │ │
│  │  • Tüm oturumlar loglanır                                │ │
│  │  • IAM tabanlı erişim kontrolü                           │ │
│  │                                                            │ │
│  │  Run Command                                              │ │
│  │  ───────────                                              │ │
│  │  • Birden fazla instance'da komut çalıştır              │ │
│  │  • SSH gerekmez                                           │ │
│  │  • Rate control, error handling                          │ │
│  │                                                            │ │
│  │  Patch Manager                                            │ │
│  │  ─────────────                                            │ │
│  │  • İşletim sistemi yamaları                              │ │
│  │  • Otomatik zamanlama                                    │ │
│  │  • Compliance raporlama                                  │ │
│  │                                                            │ │
│  │  Parameter Store                                          │ │
│  │  ───────────────                                          │ │
│  │  • Yapılandırma değerlerini sakla                        │ │
│  │  • Secrets (şifreli)                                     │ │
│  │  • Version control                                        │ │
│  │  • Ücretsiz (standard tier)                              │ │
│  │                                                            │ │
│  │  Automation                                               │ │
│  │  ──────────                                               │ │
│  │  • Runbook'lar ile otomasyon                             │ │
│  │  • EC2 golden AMI oluşturma                              │ │
│  │  • Remediation actions                                   │ │
│  │                                                            │ │
│  │  Inventory                                                │ │
│  │  ─────────                                                │ │
│  │  • Yüklü yazılımlar, yapılandırmalar                     │ │
│  │  • Metadata toplama                                       │ │
│  │                                                            │ │
│  │  State Manager                                            │ │
│  │  ─────────────                                            │ │
│  │  • İstenen durumu koru                                   │ │
│  │  • Anti-virus, agent durumu                              │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  SSM Agent:                                                    │
│  • EC2 ve on-premises sunuculara yüklenir                     │
│  • AWS yönetilen AMI'larda pre-installed                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS Trusted Advisor

```
┌─────────────────────────────────────────────────────────────────┐
│                   AWS TRUSTED ADVISOR                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "AWS best practices kontrolü"                                 │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                  5 KONTROL KATEGORİSİ                      │ │
│  │                                                            │ │
│  │  1. Cost Optimization 💰                                  │ │
│  │     • Kullanılmayan kaynaklar                            │ │
│  │     • Idle EC2 instances                                 │ │
│  │     • Underutilized EBS volumes                          │ │
│  │     • Reserved Instance recommendations                  │ │
│  │                                                            │ │
│  │  2. Performance 🚀                                        │ │
│  │     • High utilization instances                         │ │
│  │     • CloudFront optimization                            │ │
│  │     • EBS throughput optimization                        │ │
│  │                                                            │ │
│  │  3. Security 🔒                                           │ │
│  │     • Security groups (open ports)                       │ │
│  │     • IAM use                                             │ │
│  │     • MFA on root                                        │ │
│  │     • Exposed access keys                                │ │
│  │     • S3 bucket permissions                              │ │
│  │                                                            │ │
│  │  4. Fault Tolerance 🔄                                    │ │
│  │     • EBS snapshots                                      │ │
│  │     • RDS backups                                        │ │
│  │     • Multi-AZ                                           │ │
│  │     • Auto Scaling                                       │ │
│  │                                                            │ │
│  │  5. Service Limits 📊                                     │ │
│  │     • %80+ kullanılan limitler                           │ │
│  │     • EC2, EBS, VPC limits                               │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Erişim Seviyesi:                                              │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │  Basic/Developer Support:                                 │ │
│  │  • 6 core security checks (S3 permissions, SG, IAM...)  │ │
│  │  • Service limits                                         │ │
│  │                                                            │ │
│  │  Business/Enterprise Support:                             │ │
│  │  • ALL Trusted Advisor checks                            │ │
│  │  • API access                                             │ │
│  │  • CloudWatch integration                                │ │
│  │  • Weekly email notifications                            │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS CloudFormation

```
┌─────────────────────────────────────────────────────────────────┐
│                   AWS CLOUDFORMATION                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Infrastructure as Code (IaC)"                                │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Template (YAML/JSON)          CloudFormation            │ │
│  │   ┌─────────────────┐          ┌──────────────┐          │ │
│  │   │ AWSTemplateFormat│ ───────►│    Stack     │          │ │
│  │   │ Resources:       │         │              │          │ │
│  │   │   MyEC2:         │         │  ┌────────┐  │          │ │
│  │   │     Type: EC2    │         │  │  EC2   │  │          │ │
│  │   │   MyS3:          │         │  ├────────┤  │          │ │
│  │   │     Type: S3     │         │  │   S3   │  │          │ │
│  │   │   MyRDS:         │         │  ├────────┤  │          │ │
│  │   │     Type: RDS    │         │  │  RDS   │  │          │ │
│  │   └─────────────────┘         │  └────────┘  │          │ │
│  │                                │              │          │ │
│  │                                └──────────────┘          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Temel Kavramlar:                                              │
│  • Template: YAML/JSON kaynak tanımları                       │
│  • Stack: Template'dan oluşturulan kaynak koleksiyonu        │
│  • Change Set: Stack güncellemelerini önizle                 │
│  • Drift Detection: Manuel değişiklikleri tespit et          │
│                                                                 │
│  Özellikler:                                                   │
│  • Deklaratif syntax                                          │
│  • Automatic rollback on failure                              │
│  • Cross-stack references                                     │
│  • StackSets: Multi-account, multi-region                    │
│  • Designer: Visual template builder                          │
│                                                                 │
│  CloudFormation vs Terraform:                                  │
│  • CloudFormation = AWS native                                │
│  • Terraform = Multi-cloud                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS Organizations

```
┌─────────────────────────────────────────────────────────────────┐
│                    AWS ORGANIZATIONS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Multi-account yönetimi"                                      │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │                    ROOT                                    │ │
│  │                     │                                      │ │
│  │           ┌─────────┴─────────┐                           │ │
│  │           │                   │                           │ │
│  │   ┌───────▼───────┐   ┌───────▼───────┐                  │ │
│  │   │      OU       │   │      OU       │                  │ │
│  │   │  (Production) │   │   (Dev/Test)  │                  │ │
│  │   │               │   │               │                  │ │
│  │   │ ┌─────────┐   │   │ ┌─────────┐   │                  │ │
│  │   │ │Account 1│   │   │ │Account 3│   │                  │ │
│  │   │ └─────────┘   │   │ └─────────┘   │                  │ │
│  │   │ ┌─────────┐   │   │ ┌─────────┐   │                  │ │
│  │   │ │Account 2│   │   │ │Account 4│   │                  │ │
│  │   │ └─────────┘   │   │ └─────────┘   │                  │ │
│  │   └───────────────┘   └───────────────┘                  │ │
│  │                                                            │ │
│  │   OU = Organizational Unit                                │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Consolidated billing (tek fatura)                          │
│  • Volume discounts                                           │
│  • Reserved Instance sharing                                  │
│  • Service Control Policies (SCPs)                           │
│  • Tag policies                                               │
│  • Backup policies                                            │
│                                                                 │
│  SCP (Service Control Policies):                               │
│  • IAM üzerinde guardrails                                    │
│  • "Bu OU'da kimse EC2 kapatamaz"                            │
│  • Deny > Allow                                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Customer Engagement

### Amazon Connect

```
┌─────────────────────────────────────────────────────────────────┐
│                      AMAZON CONNECT                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Cloud-based contact center"                                  │
│                                                                 │
│  Özellikler:                                                   │
│  • Self-service setup (dakikalar içinde)                      │
│  • Pay-per-use pricing                                        │
│  • Omnichannel (voice, chat)                                  │
│  • Lex entegrasyonu (IVR bots)                               │
│  • Real-time ve historical analytics                         │
│  • CRM entegrasyonları                                        │
│                                                                 │
│  Contact Lens for Amazon Connect:                              │
│  • Real-time sentiment analysis                               │
│  • Automatic call categorization                              │
│  • Post-call transcription                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon SES

```
┌─────────────────────────────────────────────────────────────────┐
│                        AMAZON SES                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Simple Email Service - Bulk email gönderimi"                │
│                                                                 │
│  Özellikler:                                                   │
│  • Transactional emails                                       │
│  • Marketing campaigns                                        │
│  • High deliverability                                        │
│  • Reputation management                                      │
│  • Email receiving (inbound)                                  │
│  • SMTP interface                                             │
│  • API access                                                  │
│                                                                 │
│  Kullanım:                                                     │
│  • Password reset emails                                      │
│  • Order confirmations                                        │
│  • Newsletter                                                 │
│  • Marketing emails                                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## End User Computing

### Amazon WorkSpaces

```
┌─────────────────────────────────────────────────────────────────┐
│                     AMAZON WORKSPACES                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Desktop as a Service (DaaS)"                                 │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   User Device              AWS Cloud                       │ │
│  │   ┌─────────┐             ┌───────────────────────┐       │ │
│  │   │ Laptop  │ ◄─────────► │    WorkSpaces         │       │ │
│  │   │ Tablet  │    PCoIP    │    ┌─────────────┐    │       │ │
│  │   │ Mobile  │    or WSP   │    │   Windows   │    │       │ │
│  │   └─────────┘             │    │   or Linux  │    │       │ │
│  │                           │    │   Desktop   │    │       │ │
│  │                           │    └─────────────┘    │       │ │
│  │                           └───────────────────────┘       │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Windows veya Linux desktop                                 │
│  • Persistent storage                                         │
│  • Active Directory entegrasyonu                              │
│  • Monthly veya hourly billing                                │
│  • BYOL (Bring Your Own License)                             │
│  • Encryption at rest                                         │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Uzaktan çalışma                                            │
│  • Geçici çalışanlar                                          │
│  • BYOD (Bring Your Own Device)                              │
│  • Secure desktop access                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon AppStream 2.0

```
┌─────────────────────────────────────────────────────────────────┐
│                   AMAZON APPSTREAM 2.0                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Application streaming service"                               │
│                                                                 │
│  Özellikler:                                                   │
│  • Desktop applications browser'dan erişim                    │
│  • Non-persistent (oturum bitince kaybolur)                   │
│  • Per-user pricing                                           │
│  • Any application (Windows)                                  │
│                                                                 │
│  WorkSpaces vs AppStream:                                      │
│  ┌───────────────────────────┬───────────────────────────────┐ │
│  │      WorkSpaces           │       AppStream 2.0           │ │
│  ├───────────────────────────┼───────────────────────────────┤ │
│  │ Full desktop              │ Single application            │ │
│  │ Persistent               │ Non-persistent               │ │
│  │ User-specific            │ Application-specific         │ │
│  │ Monthly/hourly           │ Per-user, per-session        │ │
│  └───────────────────────────┴───────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Frontend & Mobile

### AWS Amplify

```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS AMPLIFY                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Full-stack web ve mobile uygulama geliştirme platformu"      │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Amplify Hosting          Amplify Studio                 │ │
│  │   ────────────────         ──────────────                 │ │
│  │   • Static web hosting     • Visual development           │ │
│  │   • CI/CD                  • UI components                │ │
│  │   • Git-based deployment   • Data models                  │ │
│  │   • SSL                    • Auth                         │ │
│  │   • CDN                    • No-code backend             │ │
│  │                                                            │ │
│  │   Amplify Libraries                                       │ │
│  │   ─────────────────                                       │ │
│  │   • Frontend SDK                                          │ │
│  │   • Auth, Storage, API, Analytics                        │ │
│  │   • React, Vue, Angular, iOS, Android                    │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Backend Integration:                                          │
│  • Cognito (Auth)                                             │
│  • S3 (Storage)                                               │
│  • AppSync (GraphQL)                                          │
│  • Lambda (Functions)                                         │
│  • DynamoDB (Database)                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon API Gateway

```
┌─────────────────────────────────────────────────────────────────┐
│                    AMAZON API GATEWAY                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Fully managed API management service"                       │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Clients          API Gateway          Backends          │ │
│  │   ┌────────┐      ┌──────────┐         ┌─────────────┐   │ │
│  │   │ Mobile │ ───► │          │ ───────► │   Lambda    │   │ │
│  │   │  Apps  │      │   REST   │         ├─────────────┤   │ │
│  │   ├────────┤      │   API    │ ───────► │     EC2     │   │ │
│  │   │  Web   │ ───► │   or     │         ├─────────────┤   │ │
│  │   │  Apps  │      │ WebSocket│ ───────► │   HTTP      │   │ │
│  │   ├────────┤      │   or     │         │  Endpoints  │   │ │
│  │   │  IoT   │ ───► │   HTTP   │ ───────► ├─────────────┤   │ │
│  │   └────────┘      │          │         │    AWS      │   │ │
│  │                   └──────────┘         │  Services   │   │ │
│  │                                         └─────────────┘   │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  API Types:                                                    │
│  • REST API: Full-featured, most flexible                     │
│  • HTTP API: Low latency, lower cost (Lambda, HTTP)          │
│  • WebSocket API: Real-time, two-way communication           │
│                                                                 │
│  Özellikler:                                                   │
│  • Throttling (rate limiting)                                 │
│  • Caching                                                     │
│  • API versioning                                             │
│  • Authentication (Cognito, Lambda authorizers)              │
│  • Request/Response transformation                           │
│  • SDK generation                                             │
│  • Swagger/OpenAPI support                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## IoT

### AWS IoT Core

```
┌─────────────────────────────────────────────────────────────────┐
│                       AWS IoT CORE                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "IoT cihazlarını AWS'e bağlama ve yönetme"                   │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   IoT Devices          IoT Core            AWS Services   │ │
│  │   ┌─────────┐         ┌─────────┐         ┌─────────────┐│ │
│  │   │ Sensors │ ──────► │  MQTT   │ ───────►│   Lambda    ││ │
│  │   │ Cameras │  MQTT   │ Broker  │ Rules  │   Kinesis   ││ │
│  │   │ Devices │         │         │ Engine │   S3        ││ │
│  │   └─────────┘         │ Device  │ ───────►│  DynamoDB   ││ │
│  │                       │ Shadow  │         │   SNS       ││ │
│  │                       └─────────┘         └─────────────┘│ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Temel Kavramlar:                                              │
│  • Thing: IoT cihazının temsili                               │
│  • Device Shadow: Cihaz durumunun sanal kopyası               │
│  • Rules Engine: Mesajları AWS servislerine yönlendir        │
│  • MQTT: Lightweight messaging protocol                       │
│                                                                 │
│  Güvenlik:                                                     │
│  • X.509 certificates                                         │
│  • IAM policies                                                │
│  • TLS encryption                                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Karşılaştırma Tabloları

### Application Integration Karşılaştırma

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                  APPLICATION INTEGRATION KARŞILAŞTIRMA                      │
├─────────────────────┬───────────────────────────────────────────────────────┤
│      Servis         │                    Kullanım Alanı                     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ SQS                 │ Decoupling, async processing, buffer                 │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ SNS                 │ Push notifications, fan-out, alerts                  │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ EventBridge         │ Event-driven architecture, event routing             │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Step Functions      │ Workflow orchestration, state machines               │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ API Gateway         │ REST/HTTP/WebSocket API management                   │
└─────────────────────┴───────────────────────────────────────────────────────┘
```

### Management Servisleri Karşılaştırma

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    MANAGEMENT SERVİSLERİ KARŞILAŞTIRMA                      │
├─────────────────────┬───────────────────────────────────────────────────────┤
│      Servis         │                    Soru / Cevap                       │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ CloudWatch          │ HOW is it performing? (Metrics, logs, alarms)        │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ CloudTrail          │ WHO did WHAT? (API activity logs)                    │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Config              │ WHAT is the configuration? (Resource compliance)     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Systems Manager     │ HOW to manage? (Patching, session, automation)       │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Trusted Advisor     │ WHAT are best practices? (5 pillars check)           │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ CloudFormation      │ HOW to provision? (IaC - Infrastructure as Code)     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Organizations       │ HOW to manage accounts? (Multi-account, SCPs)        │
└─────────────────────┴───────────────────────────────────────────────────────┘
```

---

## Sınav Senaryoları

### Senaryo 1: Decoupling

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Web uygulaması ve backend işleme arasında asenkron       │
│ iletişim gerekiyor. Backend bazen yavaş olabiliyor ve          │
│ mesajlar kaybolmamalı. Hangi servis?                           │
├─────────────────────────────────────────────────────────────────┤
│ A) Amazon SNS                                                   │
│ B) Amazon SQS                                                   │
│ C) Amazon EventBridge                                           │
│ D) AWS Step Functions                                           │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) Amazon SQS                                            │
│                                                                 │
│ Neden?                                                          │
│ • "Asenkron iletişim" + "mesaj kaybolmamalı" = SQS             │
│ • SQS mesajları retention süresince saklar                     │
│ • SNS mesajları saklamaz (push model)                          │
│ • Decoupling için ideal                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 2: CI/CD

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Kaynak kodunu build edip EC2 instance'lara deploy        │
│ etmek isteniyor. Sunucu yönetimi minimum olmalı.               │
│ Hangi servisler kullanılmalı?                                   │
├─────────────────────────────────────────────────────────────────┤
│ A) CodeBuild + CodeDeploy                                       │
│ B) EC2 + Jenkins                                                │
│ C) Lambda + S3                                                  │
│ D) ECS + ECR                                                    │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: A) CodeBuild + CodeDeploy                                │
│                                                                 │
│ Neden?                                                          │
│ • "Build" = CodeBuild (serverless)                             │
│ • "EC2'ye deploy" = CodeDeploy                                 │
│ • "Sunucu yönetimi minimum" = Managed servisler                │
│ • Jenkins = self-managed, sunucu gerekir                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 3: Monitoring

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: EC2 CPU kullanımı %80'i geçtiğinde alarm oluşturmak      │
│ ve SNS ile bildirim göndermek isteniyor. Hangi servis?         │
├─────────────────────────────────────────────────────────────────┤
│ A) AWS CloudTrail                                               │
│ B) Amazon CloudWatch                                            │
│ C) AWS Config                                                   │
│ D) AWS Trusted Advisor                                          │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) Amazon CloudWatch                                     │
│                                                                 │
│ Neden?                                                          │
│ • "CPU kullanımı" = Metric = CloudWatch                        │
│ • "Alarm" = CloudWatch Alarms                                  │
│ • "SNS bildirim" = Alarm action                                │
│ • CloudTrail = API logs, CloudWatch değil                      │
│ • Config = yapılandırma, metrik değil                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 4: Audit

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Şirket, hangi kullanıcının S3 bucket'ı sildiğini         │
│ bulmak istiyor. Hangi servis?                                   │
├─────────────────────────────────────────────────────────────────┤
│ A) Amazon CloudWatch                                            │
│ B) AWS CloudTrail                                               │
│ C) AWS Config                                                   │
│ D) Amazon Inspector                                             │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) AWS CloudTrail                                        │
│                                                                 │
│ Neden?                                                          │
│ • "Kim" = WHO = CloudTrail                                     │
│ • CloudTrail tüm API çağrılarını loglar                       │
│ • DeleteBucket API çağrısı kayıtlı                            │
│ • CloudWatch = performans metrikleri                          │
│ • Config = kaynak yapılandırması                               │
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
│  APPLICATION INTEGRATION:                                       │
│  • SQS = Queue, decoupling, async (pull)                       │
│  • SNS = Pub/Sub, notifications (push)                         │
│  • EventBridge = Event routing, scheduled                      │
│  • Step Functions = Workflow orchestration                     │
│                                                                 │
│  DEVELOPER TOOLS:                                               │
│  • CodePipeline = CI/CD orchestration                          │
│  • CodeBuild = Build (serverless)                              │
│  • CodeDeploy = Deployment                                     │
│                                                                 │
│  MANAGEMENT:                                                    │
│  • CloudWatch = HOW (metrics, logs, alarms)                   │
│  • CloudTrail = WHO (API logs, audit)                         │
│  • Config = WHAT (configuration, compliance)                   │
│  • Systems Manager = Sunucu yönetimi                          │
│  • Trusted Advisor = Best practices                           │
│  • CloudFormation = IaC                                        │
│  • Organizations = Multi-account                               │
│                                                                 │
│  ANAHTAR KELIMELER:                                            │
│  • "Decoupling/Queue" → SQS                                   │
│  • "Notification/Fan-out" → SNS                               │
│  • "Event-driven" → EventBridge                               │
│  • "Workflow" → Step Functions                                │
│  • "Build" → CodeBuild                                        │
│  • "Deploy" → CodeDeploy                                      │
│  • "Metrics/Alarm" → CloudWatch                               │
│  • "API logs/Audit" → CloudTrail                              │
│  • "Compliance" → Config                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

- [Amazon SQS Developer Guide](https://docs.aws.amazon.com/sqs/)
- [Amazon SNS Developer Guide](https://docs.aws.amazon.com/sns/)
- [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/)
- [AWS Step Functions Developer Guide](https://docs.aws.amazon.com/step-functions/)
- [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/cloudwatch/)
- [AWS CloudTrail User Guide](https://docs.aws.amazon.com/cloudtrail/)
- [AWS Config Developer Guide](https://docs.aws.amazon.com/config/)
- [CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

> **Sınav İpucu:** CloudWatch vs CloudTrail sık sorulan bir karşılaştırma! CloudWatch = HOW (performans), CloudTrail = WHO (audit). Ayrıca SQS vs SNS: SQS = pull/queue/retention, SNS = push/pub-sub/no retention.

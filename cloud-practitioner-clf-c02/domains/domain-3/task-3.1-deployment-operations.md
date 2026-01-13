# Task 3.1: AWS'de Deployment ve Operasyon Yöntemleri

> **Resmi Tanım:** Define methods of deploying and operating in the AWS Cloud
>
> **Kaynak:** [AWS CLF-C02 Exam Guide - Domain 3](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain3.html)

---

## Bu Task'ta Öğrenilecekler

### Bilgi Alanları (Knowledge)
- AWS Cloud'da provizyon ve operasyon yapmanın çeşitli yolları
- AWS servislerine erişim yöntemleri
- Cloud deployment modelleri türleri

### Beceriler (Skills)
- Programatik erişim (API, SDK, CLI), AWS Management Console ve IaC arasında karar verme
- Tek seferlik işlemler mi yoksa tekrarlanabilir süreçler mi kullanılacağını değerlendirme
- Deployment modellerini tanımlama (cloud, hybrid, on-premises)

---

## 1. AWS'e Erişim Yöntemleri

AWS ile etkileşim kurmanın dört temel yolu vardır:

```
┌─────────────────────────────────────────────────────────────────┐
│                    AWS'E ERİŞİM YÖNTEMLERİ                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌────────┐│
│  │  Management │  │    CLI      │  │    SDK      │  │  IaC   ││
│  │   Console   │  │             │  │             │  │        ││
│  │   (GUI)     │  │ (Terminal)  │  │  (Code)     │  │(Template)│
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └───┬────┘│
│         │                │                │              │     │
│         └────────────────┴────────────────┴──────────────┘     │
│                              │                                  │
│                              ▼                                  │
│                    ┌─────────────────┐                         │
│                    │    AWS APIs     │                         │
│                    └─────────────────┘                         │
│                              │                                  │
│                              ▼                                  │
│                    ┌─────────────────┐                         │
│                    │  AWS Services   │                         │
│                    └─────────────────┘                         │
└─────────────────────────────────────────────────────────────────┘
```

### 1.1 AWS Management Console (Web GUI)

**Ne İşe Yarar:** Tarayıcı tabanlı grafik arayüz ile AWS servislerini yönetme.

**Özellikler:**
- Görsel ve kullanıcı dostu arayüz
- Hızlı başlangıç için ideal
- Öğrenme ve keşif için mükemmel
- Multi-Factor Authentication (MFA) desteği
- Mobile app mevcut (iOS/Android)

**Ne Zaman Kullanılır:**
| Kullanım | Uygun mu? |
|----------|-----------|
| Yeni servis öğrenme | ✅ Evet |
| Tek seferlik işlemler | ✅ Evet |
| Dashboard izleme | ✅ Evet |
| Tekrarlayan görevler | ❌ Hayır |
| Otomasyon | ❌ Hayır |
| CI/CD pipeline | ❌ Hayır |

**Erişim:** https://console.aws.amazon.com

---

### 1.2 AWS Command Line Interface (CLI)

**Ne İşe Yarar:** Terminal/komut satırından AWS servislerini yönetme.

**Özellikler:**
- Scripting ve otomasyon için ideal
- Tüm AWS servislerini destekler
- Windows, macOS, Linux üzerinde çalışır
- AWS CloudShell ile tarayıcıda kullanılabilir

**Kurulum:**
```bash
# macOS
brew install awscli

# Windows
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi

# Linux
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Yapılandırma:**
```bash
aws configure
# AWS Access Key ID: AKIAXXXXXXXXXXXXXXXX
# AWS Secret Access Key: ********
# Default region name: eu-west-1
# Default output format: json
```

**Örnek Komutlar:**
```bash
# S3 bucket'ları listele
aws s3 ls

# EC2 instance'ları listele
aws ec2 describe-instances

# Yeni S3 bucket oluştur
aws s3 mb s3://my-bucket-name

# Dosya yükle
aws s3 cp myfile.txt s3://my-bucket-name/
```

**Ne Zaman Kullanılır:**
| Kullanım | Uygun mu? |
|----------|-----------|
| Script yazma | ✅ Evet |
| Otomasyon | ✅ Evet |
| Hızlı sorgulama | ✅ Evet |
| Batch işlemler | ✅ Evet |
| CI/CD pipeline | ✅ Evet |

---

### 1.3 AWS Software Development Kits (SDKs)

**Ne İşe Yarar:** Programlama dilleri ile AWS servislerini yönetme.

**Desteklenen Diller:**
| Dil | SDK Adı |
|-----|---------|
| Python | Boto3 |
| JavaScript/Node.js | AWS SDK for JavaScript |
| Java | AWS SDK for Java |
| .NET | AWS SDK for .NET |
| Go | AWS SDK for Go |
| Ruby | AWS SDK for Ruby |
| PHP | AWS SDK for PHP |
| C++ | AWS SDK for C++ |

**Python (Boto3) Örneği:**
```python
import boto3

# S3 client oluştur
s3 = boto3.client('s3')

# Bucket'ları listele
response = s3.list_buckets()
for bucket in response['Buckets']:
    print(bucket['Name'])

# Dosya yükle
s3.upload_file('local_file.txt', 'my-bucket', 'remote_file.txt')
```

**JavaScript Örneği:**
```javascript
const AWS = require('aws-sdk');

// S3 client oluştur
const s3 = new AWS.S3();

// Bucket'ları listele
s3.listBuckets((err, data) => {
    if (err) console.log(err);
    else console.log(data.Buckets);
});
```

**Ne Zaman Kullanılır:**
| Kullanım | Uygun mu? |
|----------|-----------|
| Uygulama geliştirme | ✅ Evet |
| AWS servis entegrasyonu | ✅ Evet |
| Karmaşık otomasyon | ✅ Evet |
| Microservices | ✅ Evet |
| Lambda fonksiyonları | ✅ Evet |

---

### 1.4 Infrastructure as Code (IaC)

**Ne İşe Yarar:** Altyapıyı kod olarak tanımlama ve otomatik dağıtım.

#### AWS CloudFormation

AWS'nin native IaC servisi.

**Template Formatları:** JSON veya YAML

**Örnek CloudFormation Template (YAML):**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple EC2 Instance

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0123456789abcdef0
      Tags:
        - Key: Name
          Value: MyWebServer

  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-unique-bucket-name

Outputs:
  InstanceId:
    Description: Instance ID
    Value: !Ref MyEC2Instance
```

**Temel Kavramlar:**
| Kavram | Açıklama |
|--------|----------|
| **Template** | Altyapı tanımı (JSON/YAML) |
| **Stack** | Template'ten oluşturulan kaynak grubu |
| **Change Set** | Stack değişikliklerini önizleme |
| **Drift Detection** | Manuel değişiklikleri tespit etme |
| **StackSets** | Birden fazla hesap/region'a dağıtım |

#### AWS CDK (Cloud Development Kit)

Programlama dilleri ile IaC.

**Örnek CDK (TypeScript):**
```typescript
import * as cdk from 'aws-cdk-lib';
import * as s3 from 'aws-cdk-lib/aws-s3';

export class MyStack extends cdk.Stack {
  constructor(scope: cdk.App, id: string) {
    super(scope, id);

    new s3.Bucket(this, 'MyBucket', {
      versioned: true,
      encryption: s3.BucketEncryption.S3_MANAGED
    });
  }
}
```

#### Diğer IaC Araçları

| Araç | Tür | Açıklama |
|------|-----|----------|
| **Terraform** | 3rd party | Multi-cloud IaC |
| **Pulumi** | 3rd party | Multi-language IaC |
| **AWS SAM** | AWS | Serverless uygulamalar için |

**IaC Avantajları:**
```
✅ Tekrarlanabilirlik - Aynı altyapıyı birçok kez oluştur
✅ Versiyon kontrolü - Git ile değişiklikleri takip et
✅ Tutarlılık - Dev/Staging/Prod aynı yapıda
✅ Dokümantasyon - Kod = Dokümantasyon
✅ Hız - Dakikalar içinde karmaşık altyapı
✅ Maliyet tasarrufu - Manuel hata azaltma
```

---

## 2. Erişim Yöntemlerinin Karşılaştırması

| Özellik | Console | CLI | SDK | IaC |
|---------|---------|-----|-----|-----|
| **Öğrenme Eğrisi** | Düşük | Orta | Yüksek | Yüksek |
| **Otomasyon** | ❌ | ✅ | ✅ | ✅ |
| **Tekrarlanabilirlik** | ❌ | ⚠️ | ⚠️ | ✅ |
| **Versiyon Kontrolü** | ❌ | ⚠️ | ✅ | ✅ |
| **CI/CD Uyumu** | ❌ | ✅ | ✅ | ✅ |
| **Hata Olasılığı** | Yüksek | Orta | Düşük | Düşük |
| **Hız (tek işlem)** | Hızlı | Hızlı | Orta | Yavaş |
| **Hız (çoklu işlem)** | Yavaş | Hızlı | Hızlı | Çok Hızlı |

---

## 3. Cloud Deployment Modelleri

### 3.1 Model Karşılaştırması

```
┌─────────────────────────────────────────────────────────────────┐
│                   DEPLOYMENT MODELLERİ                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   PUBLIC CLOUD          PRIVATE CLOUD         ON-PREMISES       │
│   ┌───────────┐         ┌───────────┐         ┌───────────┐    │
│   │           │         │           │         │           │    │
│   │   AWS     │         │ Outposts  │         │  Şirket   │    │
│   │   Azure   │         │ VMware    │         │   Veri    │    │
│   │   GCP     │         │ OpenStack │         │  Merkezi  │    │
│   │           │         │           │         │           │    │
│   └───────────┘         └───────────┘         └───────────┘    │
│        │                      │                     │          │
│        └──────────────────────┼─────────────────────┘          │
│                               │                                 │
│                        HYBRID CLOUD                             │
│                    (Public + Private)                           │
│                                                                 │
│                        HYBRID ENVIRONMENT                       │
│                     (AWS + On-Premises)                         │
│                                                                 │
│                         MULTI-CLOUD                             │
│                    (AWS + Azure + GCP)                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 3.2 Public Cloud

**Tanım:** Herkese açık, paylaşımlı altyapı üzerinde çalışan cloud ortamı.

**Örnekler:** AWS, Microsoft Azure, Google Cloud Platform

**Özellikler:**
- Kullandıkça öde modeli
- Sınırsız ölçeklenebilirlik
- Yönetim yükü AWS'de
- Global erişilebilirlik

**Avantajlar:**
| Avantaj | Açıklama |
|---------|----------|
| Maliyet | CapEx yok, sadece OpEx |
| Esneklik | Dakikalar içinde ölçekleme |
| Güvenilirlik | AWS'nin %99.99+ SLA'ları |
| Yenilik | Sürekli yeni servisler |

---

### 3.3 Private Cloud

**Tanım:** Tek bir organizasyona ayrılmış, cloud computing kriterlerini karşılayan altyapı.

**AWS Çözümü:** AWS Outposts

**Özellikler:**
- Dedicated donanım
- On-premises konumda
- Cloud computing kriterleri karşılanır:
  - On-demand self-service
  - Broad network access
  - Resource pooling
  - Rapid elasticity
  - Measured service

**Ne Zaman Kullanılır:**
- Veri yerelliği gereksinimleri
- Düzenleyici uyumluluk
- Düşük latency ihtiyacı
- Mevcut yatırımları koruma

---

### 3.4 Hybrid Cloud

**Tanım:** Public cloud + Private cloud birlikte kullanımı.

```
┌─────────────────────────────────────────────────────────────┐
│                      HYBRID CLOUD                            │
│                                                             │
│   ┌─────────────────┐         ┌─────────────────┐          │
│   │  PUBLIC CLOUD   │◄───────►│  PRIVATE CLOUD  │          │
│   │     (AWS)       │ Bağlantı │ (AWS Outposts)  │          │
│   │                 │         │                 │          │
│   │ Cloud kriterleri│         │ Cloud kriterleri│          │
│   │ karşılanır ✓    │         │ karşılanır ✓    │          │
│   └─────────────────┘         └─────────────────┘          │
│                                                             │
│   Örnek: AWS + AWS Outposts                                │
└─────────────────────────────────────────────────────────────┘
```

---

### 3.5 Hybrid Environment (Dikkat!)

**Tanım:** AWS + Geleneksel On-Premises Data Center kullanımı.

```
┌─────────────────────────────────────────────────────────────┐
│                   HYBRID ENVIRONMENT                         │
│                 (Hybrid Cloud DEĞİL!)                        │
│                                                             │
│   ┌─────────────────┐         ┌─────────────────┐          │
│   │  PUBLIC CLOUD   │◄───────►│  ON-PREMISES    │          │
│   │     (AWS)       │ VPN/DX  │  DATA CENTER    │          │
│   │                 │         │                 │          │
│   │ Cloud kriterleri│         │ Geleneksel      │          │
│   │ karşılanır ✓    │         │ sunucular ✗     │          │
│   └─────────────────┘         └─────────────────┘          │
│                                                             │
│   Örnek: AWS + Şirket sunucuları (VPN ile bağlı)           │
└─────────────────────────────────────────────────────────────┘
```

**⚠️ SINAV İÇİN KRİTİK:**
```
Soru: "Şirket kendi data center'ını AWS ile kullanıyor"
Cevap: Hybrid ENVIRONMENT (Hybrid Cloud DEĞİL!)

Soru: "AWS Outposts ile AWS'i birlikte kullanıyor"
Cevap: Hybrid CLOUD
```

---

### 3.6 Multi-Cloud

**Tanım:** Birden fazla public cloud provider kullanımı.

```
┌─────────────────────────────────────────────────────────────┐
│                      MULTI-CLOUD                             │
│                                                             │
│   ┌──────────┐    ┌──────────┐    ┌──────────┐            │
│   │   AWS    │    │  Azure   │    │   GCP    │            │
│   │          │    │          │    │          │            │
│   └────┬─────┘    └────┬─────┘    └────┬─────┘            │
│        │               │               │                   │
│        └───────────────┼───────────────┘                   │
│                        │                                    │
│                   Uygulama                                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Avantajlar:**
- Vendor lock-in'den kaçınma
- Best-of-breed servis seçimi
- Coğrafi dağılım

**Dezavantajlar:**
- Karmaşık yönetim
- Farklı API'ler
- Maliyet takibi zor

---

### 3.7 On-Premises

**Tanım:** Tüm altyapının şirketin kendi veri merkezinde olması.

**Özellikler:**
- Tam kontrol
- CapEx yatırımı
- Fiziksel güvenlik sorumluluğu
- Ölçekleme zor ve yavaş

---

## 4. AWS Public vs Private Services

AWS servisleri ağ yapısına göre ikiye ayrılır:

```
┌─────────────────────────────────────────────────────────────┐
│                          AWS                                 │
│                                                             │
│  ┌────────────────────────┐  ┌────────────────────────┐    │
│  │     PUBLIC ZONE        │  │     PRIVATE ZONE       │    │
│  │                        │  │                        │    │
│  │  • S3                  │  │  • VPC                 │    │
│  │  • DynamoDB            │  │  • EC2 (default)       │    │
│  │  • Lambda (endpoints)  │  │  • RDS                 │    │
│  │  • CloudFront          │  │  • EBS                 │    │
│  │  • Route 53            │  │  • EFS                 │    │
│  │  • IAM                 │  │  • ElastiCache         │    │
│  │                        │  │                        │    │
│  │  İnternete doğrudan    │  │  Varsayılan izole      │    │
│  │  bağlı ✓               │  │  İnternet yok ✗        │    │
│  └────────────────────────┘  └────────────────────────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

| Servis Tipi | Ağ Bağlantısı | Erişim |
|-------------|---------------|--------|
| **Public Service** | AWS Public Zone | İnternet üzerinden |
| **Private Service** | AWS Private Zone (VPC) | Yapılandırma gerekli |

---

## 5. Sınav Senaryoları

| Senaryo | Doğru Seçim |
|---------|-------------|
| "Hızlıca bir EC2 başlatmak istiyorum" | Console |
| "Her gece S3'e yedek alacak script" | CLI |
| "Lambda fonksiyonundan DynamoDB'ye yazma" | SDK |
| "100 sunuculuk ortam kurulumu" | CloudFormation (IaC) |
| "Dev/Staging/Prod aynı altyapı" | CloudFormation (IaC) |
| "Şirket DC + AWS bağlantısı" | Hybrid Environment |
| "AWS Outposts + AWS" | Hybrid Cloud |
| "Veri yerelliği zorunluluğu" | Private Cloud / On-Prem |
| "AWS + Azure birlikte" | Multi-Cloud |

---

## 6. Anahtar Terimler Sözlüğü

| Terim | Tanım |
|-------|-------|
| **API** | Application Programming Interface - Servisler arası iletişim |
| **SDK** | Software Development Kit - Programlama kütüphaneleri |
| **CLI** | Command Line Interface - Komut satırı aracı |
| **IaC** | Infrastructure as Code - Kod olarak altyapı |
| **CapEx** | Capital Expenditure - Sermaye harcaması |
| **OpEx** | Operational Expenditure - İşletme harcaması |
| **Provisioning** | Kaynak oluşturma ve yapılandırma |

---

## 7. Sınav İpuçları

```
✓ Console = Öğrenme, tek seferlik işlemler
✓ CLI = Otomasyon, scripting
✓ SDK = Uygulama geliştirme
✓ IaC = Tekrarlanabilir altyapı, versiyon kontrolü

✓ Hybrid Cloud = Public + Private Cloud
✓ Hybrid Environment = AWS + On-Premises (geleneksel)
✓ Multi-Cloud = AWS + Azure + GCP

✓ Public Service = İnternetten erişilebilir (S3)
✓ Private Service = VPC içinde izole (EC2)
```

---

## Referanslar

- [AWS CLI Documentation](https://docs.aws.amazon.com/cli/)
- [AWS SDK Documentation](https://aws.amazon.com/tools/)
- [AWS CloudFormation User Guide](https://docs.aws.amazon.com/cloudformation/)
- [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/)
- [AWS Outposts](https://aws.amazon.com/outposts/)
- [AWS Cloud Adoption Framework](https://aws.amazon.com/professional-services/CAF/)


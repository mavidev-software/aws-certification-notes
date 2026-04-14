# Domain 1: Design Secure Architectures (%30)

Bu domain, AWS kaynaklarına güvenli erişim tasarımını kapsar. Sınavın en büyük bölümüdür.

---

## Task Statement 1.1: AWS Kaynaklarına Güvenli Erişim Tasarımı

### Temel Kavramlar (Fundamentals)

Mimari tasarıma başlamadan önce şu üç temel kavramı anlamalısın:

#### 1. Shared Responsibility Model (Paylaşılan Sorumluluk Modeli)

```
┌─────────────────────────────────────────────────────────────────┐
│                    MÜŞTERİ SORUMLULUĞU                          │
│         "Security IN the Cloud" (Buluttaki Güvenlik)            │
├─────────────────────────────────────────────────────────────────┤
│  - Müşteri verileri                                             │
│  - Platform, uygulamalar, IAM                                   │
│  - İşletim sistemi, ağ ve firewall yapılandırması               │
│  - Client-side encryption                                       │
│  - Server-side encryption                                       │
│  - Network traffic protection                                   │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                      AWS SORUMLULUĞU                            │
│         "Security OF the Cloud" (Bulutun Güvenliği)             │
├─────────────────────────────────────────────────────────────────┤
│  - Hardware / AWS Global Infrastructure                         │
│  - Regions, Availability Zones, Edge Locations                  │
│  - Compute, Storage, Database, Networking                       │
│  - Software (Hypervisor, vb.)                                   │
└─────────────────────────────────────────────────────────────────┘
```

#### 2. AWS Global Infrastructure

| Bileşen | Açıklama |
|---------|----------|
| **Region** | Coğrafi bölge, 2+ AZ içerir |
| **Availability Zone (AZ)** | Bir veya daha fazla veri merkezi |
| **Edge Location** | CloudFront cache noktaları |
| **Local Zone** | Düşük latency için büyük şehirlere yakın |

#### 3. AWS Service Resilience (Servis Dayanıklılık Seviyeleri)

| Seviye | Açıklama | Örnek Servisler |
|--------|----------|-----------------|
| **Global** | Tüm region'larda tek bir instance | IAM, Route 53, CloudFront |
| **Regional** | Region içinde otomatik replikasyon | S3, DynamoDB, SQS |
| **Zonal** | Tek AZ'de çalışır | EC2, EBS, RDS (single-AZ) |

---

## Cloud Ortam Türleri

Sınavda farklı ortamlar için güvenli erişim tasarımı sorulabilir:

| Ortam | Açıklama | Güvenlik Dikkat Noktaları |
|-------|----------|---------------------------|
| **Public Cloud** | Tamamen AWS üzerinde | IAM, Security Groups, NACLs |
| **Private Cloud** | On-premises veya dedicated | VPN, Direct Connect |
| **Hybrid Cloud** | AWS + On-premises | Site-to-Site VPN, Direct Connect, Transit Gateway |
| **Multi-Cloud** | AWS + Diğer cloud providers | Federation, cross-cloud IAM |

---

## AWS Accounts (AWS Hesapları)

### Neden Hesapları Anlamalısın?

AWS hesapları basit görünür ama **güvenlik sınırı (security boundary)** olarak kritik öneme sahiptir. Her hesap:

- Kendi IAM kullanıcıları, grupları ve rolleri
- Kendi kaynakları
- Kendi faturalandırması
- Kendi güvenlik izolasyonu

### Account Root User

Her AWS hesabı bir **root user** ile başlar.

```
┌─────────────────────────────────────────────────────────────────┐
│                     ROOT USER ÖZELLİKLERİ                       │
├─────────────────────────────────────────────────────────────────┤
│  ✓ Hesap oluşturulduğunda otomatik oluşur                       │
│  ✓ Email + password ile giriş                                   │
│  ✓ TAM ve SINIRLANDIRILAMAZ yetkiye sahip                       │
│  ✓ Billing bilgilerine erişim                                   │
│  ✓ Hesabı kapatma yetkisi                                       │
│  ✗ IAM policy ile kısıtlanamaz                                  │
│  ✗ SCP bile tam olarak kısıtlayamaz (bazı istisnalar var)       │
└─────────────────────────────────────────────────────────────────┘
```

### Root User Güvenlik Riskleri

| Risk | Sonuç |
|------|-------|
| Root user ele geçirilirse | Tüm AWS ortamı tehlikede |
| Yetkileri kısıtlanamaz | Saldırgan her şeyi yapabilir |
| Tek kimlik bilgisi | Single point of failure |

### Root User Güvenlik Best Practices

```
ROOT USER GÜVENLİK CHECKLIST:

[ ] 1. MFA (Multi-Factor Authentication) MUTLAKA etkinleştir
    - Hardware MFA (YubiKey) tercih et
    - Virtual MFA (Google Authenticator, Authy) minimum

[ ] 2. Root user access key OLUŞTURMA
    - Varsa hemen sil
    - CLI/SDK için asla root kullanma

[ ] 3. Root user'ı günlük işler için KULLANMA
    - Sadece hesap seviyesi görevler için:
      * Billing ayarları
      * İlk IAM admin kullanıcı oluşturma
      * Hesap kapatma

[ ] 4. Güçlü ve benzersiz password kullan

[ ] 5. Root user credentials'ı güvenli sakla
    - Password manager
    - Fiziksel kasa

[ ] 6. Root user aktivitelerini izle
    - CloudTrail ile logla
    - Alert oluştur
```

### Root User'ın Gerekli Olduğu Görevler

Bazı görevler SADECE root user ile yapılabilir:

- Hesap ayarlarını değiştirme (hesap adı, email, password)
- IAM user izinlerini geri yükleme
- Billing ve Cost Management konsoluna erişim
- GovCloud kaydı
- Hesabı kapatma
- Belirli S3 bucket policy'lerini değiştirme
- Deep inspection için AWS Support case açma

---

## AWS IAM (Identity and Access Management)

### IAM Servis Özellikleri

| Özellik | Değer |
|---------|-------|
| **Servis Seviyesi** | **GLOBAL** (tüm region'larda geçerli) |
| **Ücret** | Ücretsiz |
| **Data Replikasyonu** | Tüm region'lara otomatik |
| **Eventual Consistency** | Değişiklikler kısa sürede yayılır |

```
┌─────────────────────────────────────────────────────────────────┐
│                      IAM = GLOBAL SERVİS                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│    us-east-1 ◄──────┐                                          │
│                     │                                          │
│    us-west-2 ◄──────┼─────── IAM Database ────┐                │
│                     │         (Global)        │                │
│    eu-west-1 ◄──────┤                         │                │
│                     │                         │                │
│    ap-south-1 ◄─────┘                         │                │
│                                               │                │
│    Tüm region'larda aynı IAM kullanıcıları,   │                │
│    rolleri ve policy'leri geçerli             │                │
└─────────────────────────────────────────────────────────────────┘
```

### IAM Identities (Kimlikler)

#### 1. IAM Users

**Tanım:** AWS ile etkileşime giren bireysel kişiler veya uygulamalar

```
IAM USER ÖZELLİKLERİ:

┌─────────────────────────────────────────────────────────────────┐
│  - Benzersiz isim (hesap içinde)                                │
│  - Kalıcı kimlik bilgileri (long-term credentials)              │
│  - Console password (AWS Console erişimi)                       │
│  - Access keys (CLI/SDK erişimi - max 2 adet)                   │
│  - MFA eklenebilir                                              │
│  - Direkt policy eklenebilir veya grup üzerinden                │
└─────────────────────────────────────────────────────────────────┘

YENİ USER = 0 YETKİ (No permissions by default)

Yeni oluşturulan IAM user hiçbir şey yapamaz!
Explicit olarak izin verilmeli.
```

**User Ne Zaman Kullanılır:**
- Belirli bir kişi için uzun süreli erişim
- Programmatic erişim gereken uygulamalar (ancak role tercih et)
- Üçüncü parti entegrasyonlar

#### 2. IAM Groups

**Tanım:** IAM user'ların mantıksal koleksiyonu

```
IAM GROUP ÖZELLİKLERİ:

┌─────────────────────────────────────────────────────────────────┐
│  - User koleksiyonu (identity DEĞİL)                            │
│  - Grup'a login YAPILAMAZ                                       │
│  - Policy gruba eklenir → tüm üyeler miras alır                 │
│  - Bir user birden fazla gruba üye olabilir                     │
│  - Gruplar iç içe YAPILAMAZ (nested groups yok)                 │
│  - Bir hesapta max 300 grup                                     │
│  - Bir user max 10 gruba üye olabilir                           │
└─────────────────────────────────────────────────────────────────┘
```

**Örnek Grup Yapısı:**

```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS ACCOUNT                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │  Developers │  │   Admins    │  │   Auditors  │             │
│  │    Group    │  │    Group    │  │    Group    │             │
│  ├─────────────┤  ├─────────────┤  ├─────────────┤             │
│  │ EC2 Full    │  │ Admin       │  │ ReadOnly    │             │
│  │ S3 Full     │  │ Access      │  │ Access      │             │
│  │ Lambda Full │  │             │  │             │             │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘             │
│         │                │                │                     │
│    ┌────┴────┐      ┌────┴────┐      ┌────┴────┐               │
│    │ User A  │      │ User C  │      │ User E  │               │
│    │ User B  │      │ User D  │      │         │               │
│    └─────────┘      └─────────┘      └─────────┘               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 3. IAM Roles

**Tanım:** Geçici güvenlik kimlik bilgileri sağlayan IAM kimliği

```
IAM ROLE ÖZELLİKLERİ:

┌─────────────────────────────────────────────────────────────────┐
│  - Kalıcı credentials YOK                                       │
│  - Geçici credentials (STS ile)                                 │
│  - Users, services, veya external identities assume edebilir    │
│  - Cross-account erişim sağlar                                  │
│  - Federation için kullanılır                                   │
│  - AWS servisleri için tercih edilen yöntem                     │
└─────────────────────────────────────────────────────────────────┘
```

**Role Bileşenleri:**

```
┌─────────────────────────────────────────────────────────────────┐
│                         IAM ROLE                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. TRUST POLICY (Kim assume edebilir?)                         │
│     ┌─────────────────────────────────────────────────────────┐ │
│     │ {                                                       │ │
│     │   "Effect": "Allow",                                    │ │
│     │   "Principal": {                                        │ │
│     │     "Service": "ec2.amazonaws.com"                      │ │
│     │   },                                                    │ │
│     │   "Action": "sts:AssumeRole"                            │ │
│     │ }                                                       │ │
│     └─────────────────────────────────────────────────────────┘ │
│                                                                 │
│  2. PERMISSIONS POLICY (Ne yapabilir?)                          │
│     ┌─────────────────────────────────────────────────────────┐ │
│     │ {                                                       │ │
│     │   "Effect": "Allow",                                    │ │
│     │   "Action": "s3:GetObject",                             │ │
│     │   "Resource": "arn:aws:s3:::my-bucket/*"                │ │
│     │ }                                                       │ │
│     └─────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Role Kullanım Senaryoları:**

| Senaryo | Açıklama |
|---------|----------|
| **EC2 Instance Role** | EC2'nin S3, DynamoDB vb. erişimi |
| **Lambda Execution Role** | Lambda'nın AWS servislerine erişimi |
| **Cross-Account Access** | Hesap A'dan Hesap B kaynaklarına erişim |
| **Federation** | External identity provider kullanıcıları |
| **AWS Service Role** | Bir servisin diğer servislere erişimi |

### Users vs Roles - Ne Zaman Hangisi?

| Durum | Tercih | Neden |
|-------|--------|-------|
| Bireysel kişi, uzun süreli | User | Kalıcı kimlik gerekli |
| AWS servisi (EC2, Lambda) | **Role** | Credentials otomatik rotate |
| Cross-account erişim | **Role** | Geçici, güvenli |
| External users (federation) | **Role** | Identity provider ile entegre |
| Uygulamadan AWS API | **Role** (tercihen) | Hard-coded credentials yok |
| CI/CD pipeline | **Role** | Geçici credentials |

---

## Principle of Least Privilege (En Az Yetki Prensibi)

### Tanım

> Bir kimliğe sadece görevini yerine getirmek için **gereken minimum yetkileri** ver.

### Neden Önemli?

```
BLAST RADIUS (Patlama Yarıçapı) Kavramı:

┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  FAZLA YETKİ = BÜYÜK BLAST RADIUS                               │
│                                                                 │
│         Kimlik ele geçirilirse:                                 │
│         ┌─────────────────────────────────┐                     │
│         │  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │                     │
│         │  ░░░░░ BÜYÜK HASAR ALANI ░░░░░  │                     │
│         │  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │                     │
│         └─────────────────────────────────┘                     │
│                                                                 │
│  MINIMUM YETKİ = KÜÇÜK BLAST RADIUS                             │
│                                                                 │
│         Kimlik ele geçirilirse:                                 │
│         ┌──────────┐                                            │
│         │ ░░ küçük │                                            │
│         └──────────┘                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Uygulama Adımları

```
LEAST PRIVILEGE CHECKLIST:

[ ] 1. Varsayılan olarak tüm erişimi reddet (deny by default)

[ ] 2. Sadece gerekli aksiyonları izin ver
    ✗ "s3:*" yerine
    ✓ "s3:GetObject", "s3:PutObject"

[ ] 3. Sadece gerekli kaynakları belirt
    ✗ "*" yerine
    ✓ "arn:aws:s3:::specific-bucket/*"

[ ] 4. Koşullar ekle (conditions)
    - IP adresi kısıtlaması
    - MFA zorunluluğu
    - Zaman kısıtlaması

[ ] 5. Düzenli olarak yetkileri gözden geçir
    - IAM Access Analyzer kullan
    - Kullanılmayan yetkileri kaldır

[ ] 6. Geçici yetkiler tercih et
    - Long-term credentials yerine STS
```

---

## IAM Policies (Politikalar)

### Policy Nedir?

**Policy:** Bir kimlik veya kaynakla ilişkilendirildiğinde izinlerini tanımlayan JSON belgesi.

### Policy Türleri

```
┌─────────────────────────────────────────────────────────────────┐
│                      POLICY TÜRLERİ                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. IDENTITY-BASED POLICIES                                     │
│     ├── AWS Managed Policies (AWS tarafından oluşturulan)       │
│     ├── Customer Managed Policies (Müşteri tarafından)          │
│     └── Inline Policies (Direkt kimliğe gömülü)                 │
│                                                                 │
│  2. RESOURCE-BASED POLICIES                                     │
│     ├── S3 Bucket Policies                                      │
│     ├── SQS Queue Policies                                      │
│     ├── KMS Key Policies                                        │
│     └── VPC Endpoint Policies                                   │
│                                                                 │
│  3. PERMISSIONS BOUNDARIES                                      │
│     └── IAM kullanıcı/rol için maksimum izin sınırı             │
│                                                                 │
│  4. SERVICE CONTROL POLICIES (SCPs)                             │
│     └── AWS Organizations ile hesap seviyesi sınırlar           │
│                                                                 │
│  5. SESSION POLICIES                                            │
│     └── Role assume ederken geçici kısıtlamalar                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Identity-Based vs Resource-Based Policies

| Özellik | Identity-Based | Resource-Based |
|---------|----------------|----------------|
| **Bağlandığı yer** | User, Group, Role | S3, SQS, KMS, vb. |
| **Principal** | Policy'de YOK (bağlı kimlik) | Policy'de VAR |
| **Kontrol eder** | Kimlik ne yapabilir | Kaynağa kim erişebilir |
| **Cross-account** | Role assume gerekli | Direkt izin verilebilir |

### Policy Anatomy (Policy Yapısı)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/example"
      },
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-bucket",
        "arn:aws:s3:::my-bucket/*"
      ],
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "192.168.1.0/24"
        }
      }
    }
  ]
}
```

**Policy Elementleri:**

| Element | Zorunlu | Açıklama |
|---------|---------|----------|
| **Version** | Evet | Policy dil versiyonu ("2012-10-17" kullan) |
| **Statement** | Evet | İzin kuralları dizisi |
| **Sid** | Hayır | Statement tanımlayıcı (opsiyonel) |
| **Effect** | Evet | "Allow" veya "Deny" |
| **Principal** | Resource-based için | Kimin etkilendiği |
| **Action** | Evet | İzin verilen/reddedilen aksiyonlar |
| **Resource** | Evet* | Hangi kaynaklar için geçerli |
| **Condition** | Hayır | Koşullu izinler |

### Policy Evaluation Logic (Değerlendirme Mantığı)

```
┌─────────────────────────────────────────────────────────────────┐
│                  POLICY DEĞERLENDİRME SIRASI                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Varsayılan: TÜM İSTEKLER REDDEDİLİR (Implicit Deny)         │
│                         │                                       │
│                         ▼                                       │
│  2. Explicit DENY var mı? ──── EVET ───▶ RED (Deny wins)        │
│                         │                                       │
│                        HAYIR                                    │
│                         │                                       │
│                         ▼                                       │
│  3. SCP izin veriyor mu? ──── HAYIR ───▶ RED                    │
│     (Organizations)     │                                       │
│                        EVET                                     │
│                         │                                       │
│                         ▼                                       │
│  4. Resource-based policy ─── EVET ───▶ İZİN VER*               │
│     Allow var mı?       │                                       │
│                        HAYIR                                    │
│                         │                                       │
│                         ▼                                       │
│  5. Identity-based policy ─── EVET ───▶ İZİN VER                │
│     Allow var mı?       │                                       │
│                        HAYIR                                    │
│                         │                                       │
│                         ▼                                       │
│  6. Permissions boundary ──── HAYIR ──▶ RED                     │
│     izin veriyor mu?    │                                       │
│                        EVET                                     │
│                         │                                       │
│                         ▼                                       │
│                      İZİN VER                                   │
│                                                                 │
│  * Cross-account için her iki tarafta da izin gerekli           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

ÖZET: Explicit Deny > Allow > Implicit Deny
```

### Birden Fazla Policy Değerlendirmesi

Bir kimliğe birden fazla policy bağlı olabilir:

```
USER: Alice
├── Inline Policy 1: S3 Allow
├── Attached Policy: EC2 Allow
└── Group Policy: RDS Deny

Sonuç:
- S3: Allow (Inline'dan)
- EC2: Allow (Attached'dan)
- RDS: DENY (Group'tan - Deny kazanır)
- Lambda: Implicit Deny (hiçbir policy'de yok)
```

---

## Credentials Management (Kimlik Bilgisi Yönetimi)

### Credential Türleri

| Tür | Kullanım | Ömür |
|-----|----------|------|
| **Console Password** | AWS Console login | Long-term |
| **Access Keys** | CLI, SDK, API | Long-term |
| **Temporary Credentials** | STS ile | Short-term (15dk - 36 saat) |
| **SSH Keys** | CodeCommit | Long-term |
| **Server Certificates** | HTTPS | Long-term |

### Credentials Best Practices

```
CREDENTIALS BEST PRACTICES:

┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  ❌ ASLA YAPMA                                                  │
│  ─────────────                                                  │
│  • Credentials'ı koda HARD-CODE etme                            │
│  • Credentials'ı Git'e COMMIT etme                              │
│  • Root user access key OLUŞTURMA                               │
│  • Access key'leri PAYLAŞMA                                     │
│  • Credentials'ı email ile GÖNDERME                             │
│                                                                 │
│  ✅ HER ZAMAN YAP                                               │
│  ────────────────                                               │
│  • IAM Roles kullan (özellikle AWS servisleri için)             │
│  • Environment variables kullan                                 │
│  • AWS Secrets Manager veya Parameter Store kullan              │
│  • Access key'leri düzenli rotate et (90 gün)                   │
│  • MFA zorunlu tut                                              │
│  • Kullanılmayan credentials'ı devre dışı bırak/sil             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Uygulamalarda Credentials Hiyerarşisi

AWS SDK credentials'ı şu sırayla arar:

```
1. Environment Variables
   └── AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY

2. Shared Credentials File
   └── ~/.aws/credentials

3. AWS Config File
   └── ~/.aws/config

4. Container Credentials (ECS)
   └── AWS_CONTAINER_CREDENTIALS_RELATIVE_URI

5. Instance Profile Credentials (EC2)
   └── Instance Metadata Service (IMDS)

6. Web Identity Token (EKS)
   └── AWS_WEB_IDENTITY_TOKEN_FILE

ÖNERİ: EC2 için Instance Profile (Role),
       Lambda için Execution Role kullan
```

---

## AWS Security Token Service (STS)

### STS Nedir?

**STS:** Geçici, sınırlı yetkili credentials sağlayan web servisi.

```
STS ÖZELLİKLERİ:

┌─────────────────────────────────────────────────────────────────┐
│  - Global servis (sts.amazonaws.com)                            │
│  - Regional endpoint'ler de var (latency için)                  │
│  - Geçici credentials üretir:                                   │
│    • Access Key ID                                              │
│    • Secret Access Key                                          │
│    • Session Token                                              │
│    • Expiration                                                 │
│  - Süre: 15 dakika - 36 saat (varsayılan 1 saat)               │
└─────────────────────────────────────────────────────────────────┘
```

### STS API Çağrıları

| API | Kullanım |
|-----|----------|
| **AssumeRole** | Bir IAM role'ü assume et |
| **AssumeRoleWithSAML** | SAML federation ile role assume |
| **AssumeRoleWithWebIdentity** | Web identity (Cognito, OIDC) ile role assume |
| **GetSessionToken** | MFA ile geçici credentials al |
| **GetFederationToken** | Federated user için credentials |
| **GetCallerIdentity** | Kim olduğunu öğren |
| **DecodeAuthorizationMessage** | Encoded hata mesajını çöz |

### Cross-Account Access Örneği

```
┌─────────────────────────────────────────────────────────────────┐
│                    CROSS-ACCOUNT ACCESS                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ACCOUNT A (111111111111)         ACCOUNT B (222222222222)      │
│  ┌─────────────────────┐          ┌─────────────────────┐       │
│  │                     │          │                     │       │
│  │  IAM User: Alice    │          │  IAM Role:          │       │
│  │                     │          │  CrossAccountRole   │       │
│  │  Policy:            │          │                     │       │
│  │  sts:AssumeRole     │──────────│  Trust Policy:      │       │
│  │  on Role ARN        │  assume  │  Account A allowed  │       │
│  │                     │          │                     │       │
│  │                     │          │  Permissions:       │       │
│  │                     │          │  S3 Full Access     │       │
│  └─────────────────────┘          └─────────────────────┘       │
│                                             │                   │
│                                             ▼                   │
│                                   ┌─────────────────────┐       │
│                                   │  S3 Bucket          │       │
│                                   │  (Account B)        │       │
│                                   └─────────────────────┘       │
│                                                                 │
│  Adımlar:                                                       │
│  1. Alice, STS AssumeRole çağırır                               │
│  2. STS geçici credentials döner                                │
│  3. Alice, geçici credentials ile Account B S3'e erişir         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Identity Federation

### Federation Nedir?

Kullanıcıların mevcut kimliklerini (şirket dizini, sosyal login) kullanarak AWS kaynaklarına erişmesi.

### Federation Türleri

```
┌─────────────────────────────────────────────────────────────────┐
│                    FEDERATION TÜRLERİ                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. SAML 2.0 Federation                                         │
│     └── Enterprise identity providers (Active Directory, Okta)  │
│                                                                 │
│  2. Web Identity Federation                                     │
│     └── Social logins (Google, Facebook, Amazon)                │
│     └── OpenID Connect (OIDC) providers                         │
│                                                                 │
│  3. Amazon Cognito                                              │
│     └── Mobile/web apps için identity management                │
│     └── User Pools (authentication)                             │
│     └── Identity Pools (authorization - AWS credentials)        │
│                                                                 │
│  4. AWS IAM Identity Center (eski adı: AWS SSO)                 │
│     └── Merkezi erişim yönetimi                                 │
│     └── Multiple AWS accounts                                   │
│     └── SAML-enabled applications                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### SAML 2.0 Federation Akışı

```
┌─────────────────────────────────────────────────────────────────┐
│                  SAML 2.0 FEDERATION AKIŞI                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Kullanıcı              IdP                 AWS                 │
│     │                    │                   │                  │
│     │  1. Login isteği   │                   │                  │
│     │───────────────────▶│                   │                  │
│     │                    │                   │                  │
│     │  2. Authenticate   │                   │                  │
│     │◀──────────────────▶│                   │                  │
│     │                    │                   │                  │
│     │  3. SAML Assertion │                   │                  │
│     │◀───────────────────│                   │                  │
│     │                    │                   │                  │
│     │  4. AssumeRoleWithSAML                 │                  │
│     │────────────────────────────────────────▶                  │
│     │                                        │                  │
│     │  5. Temporary Credentials              │                  │
│     │◀────────────────────────────────────────                  │
│     │                                        │                  │
│     │  6. Access AWS Resources               │                  │
│     │────────────────────────────────────────▶                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Multi-Account Strategy

### AWS Organizations

```
┌─────────────────────────────────────────────────────────────────┐
│                     AWS ORGANIZATIONS                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                    ┌─────────────────┐                          │
│                    │  Management     │                          │
│                    │    Account      │                          │
│                    │  (Root)         │                          │
│                    └────────┬────────┘                          │
│                             │                                   │
│              ┌──────────────┼──────────────┐                    │
│              │              │              │                    │
│        ┌─────▼─────┐  ┌─────▼─────┐  ┌─────▼─────┐             │
│        │    OU:    │  │    OU:    │  │    OU:    │             │
│        │   Prod    │  │    Dev    │  │  Security │             │
│        └─────┬─────┘  └─────┬─────┘  └─────┬─────┘             │
│              │              │              │                    │
│        ┌─────▼─────┐  ┌─────▼─────┐  ┌─────▼─────┐             │
│        │ Account A │  │ Account C │  │ Account E │             │
│        │ Account B │  │ Account D │  │ (Logging) │             │
│        └───────────┘  └───────────┘  └───────────┘             │
│                                                                 │
│  Özellikler:                                                    │
│  • Consolidated billing (tek fatura)                            │
│  • Service Control Policies (SCP)                               │
│  • Organizational Units (OU)                                    │
│  • Account oluşturma/davet etme                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Service Control Policies (SCPs)

**SCP:** Organization içindeki hesaplara uygulanan izin sınırları.

```
SCP ÖZELLİKLERİ:

┌─────────────────────────────────────────────────────────────────┐
│  • Root, OU veya Account seviyesinde uygulanabilir              │
│  • İzin VERMEZ, sadece SINIRLAR                                 │
│  • Management account'a uygulanmaz                              │
│  • IAM policy'leri ile kesişim şeklinde çalışır                 │
│  • Varsayılan: FullAWSAccess (tüm aksiyonlara izin)             │
│                                                                 │
│  SCP + IAM Policy = Efektif İzinler                             │
│                                                                 │
│        ┌───────────────┐                                        │
│        │      SCP      │                                        │
│        │   (Boundary)  │                                        │
│        │  ┌─────────┐  │                                        │
│        │  │   IAM   │  │                                        │
│        │  │ Policy  │  │ ◀── Efektif izinler                    │
│        │  │(Allow)  │  │     (kesişim alanı)                    │
│        │  └─────────┘  │                                        │
│        └───────────────┘                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**SCP Örneği - Belirli Region'ları Kısıtla:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyAllOutsideEU",
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": [
            "eu-west-1",
            "eu-central-1"
          ]
        }
      }
    }
  ]
}
```

### AWS Control Tower

```
AWS CONTROL TOWER:

┌─────────────────────────────────────────────────────────────────┐
│  Multi-account AWS ortamı kurmak için best-practice servisi     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Özellikler:                                                    │
│  • Landing Zone: Güvenli multi-account ortam                    │
│  • Account Factory: Yeni hesap oluşturma otomasyonu             │
│  • Guardrails: Preventive ve detective kontroller               │
│  • Dashboard: Compliance durumu görünümü                        │
│                                                                 │
│  Guardrail Türleri:                                             │
│  ┌─────────────────┬──────────────────────────────────────────┐ │
│  │ Preventive      │ SCP ile engelleyen (ör: root user MFA)   │ │
│  │ Detective       │ Config Rules ile tespit eden             │ │
│  │ Proactive       │ CloudFormation hooks ile önleyen         │ │
│  └─────────────────┴──────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Traceability & Monitoring

### Güvenlik İzlenebilirliği

Kimin neye, ne zaman eriştiğini izlemek için:

| Servis | İşlev |
|--------|-------|
| **AWS CloudTrail** | API çağrılarını logla |
| **Amazon CloudWatch** | Metrikler ve alarmlar |
| **AWS Config** | Kaynak yapılandırma değişiklikleri |
| **Amazon GuardDuty** | Tehdit algılama |
| **AWS Security Hub** | Merkezi güvenlik görünümü |
| **IAM Access Analyzer** | External erişim analizi |

### CloudTrail

```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS CLOUDTRAIL                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  • Tüm API çağrılarını kaydeder                                 │
│  • Kim, ne, ne zaman, nereden                                   │
│  • Management events (varsayılan açık)                          │
│  • Data events (opsiyonel - S3 object level)                    │
│  • 90 gün geçmiş (ücretsiz)                                     │
│  • S3'e export (uzun süreli saklama)                            │
│  • CloudWatch Logs entegrasyonu                                 │
│                                                                 │
│  Örnek Log Entry:                                               │
│  {                                                              │
│    "eventTime": "2024-01-15T10:30:00Z",                         │
│    "eventSource": "s3.amazonaws.com",                           │
│    "eventName": "DeleteBucket",                                 │
│    "userIdentity": {                                            │
│      "type": "IAMUser",                                         │
│      "userName": "alice"                                        │
│    },                                                           │
│    "sourceIPAddress": "192.168.1.100"                           │
│  }                                                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Sınav İçin Özet

```
┌─────────────────────────────────────────────────────────────────┐
│              DOMAIN 1 - SINAV İÇİN HATIRLA                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  IAM TEMELLER:                                                  │
│  ✓ IAM = Global servis                                          │
│  ✓ Yeni IAM user = 0 yetki                                      │
│  ✓ Root user = Kısıtlanamaz, MFA şart                           │
│  ✓ Groups = Nested olamaz, identity değil                       │
│  ✓ Roles = Geçici credentials, servisler için tercih et         │
│                                                                 │
│  POLICIES:                                                      │
│  ✓ Explicit Deny > Allow > Implicit Deny                        │
│  ✓ Identity-based = Ne yapabilir                                │
│  ✓ Resource-based = Kim erişebilir (Principal içerir)           │
│  ✓ SCP = Organization seviyesinde sınır                         │
│                                                                 │
│  CREDENTIALS:                                                   │
│  ✓ ASLA hard-code etme                                          │
│  ✓ EC2 = Instance Profile (Role)                                │
│  ✓ Lambda = Execution Role                                      │
│  ✓ STS = Geçici credentials                                     │
│                                                                 │
│  MULTI-ACCOUNT:                                                 │
│  ✓ Organizations = Hesap yönetimi                               │
│  ✓ SCP = İzin sınırı (izin vermez)                              │
│  ✓ Control Tower = Best practice setup                          │
│                                                                 │
│  MONITORING:                                                    │
│  ✓ CloudTrail = API logları                                     │
│  ✓ Config = Yapılandırma değişiklikleri                         │
│  ✓ GuardDuty = Tehdit algılama                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [IAM Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [AWS Organizations Documentation](https://docs.aws.amazon.com/organizations/latest/userguide/)
- [AWS Control Tower Documentation](https://docs.aws.amazon.com/controltower/latest/userguide/)

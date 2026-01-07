# Domain 2: Security and Compliance (Güvenlik ve Uyumluluk)

> **Sınav Ağırlığı: %30** (En yüksek ağırlıklı domain!)
>
> **Kaynak:** [AWS Official Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain2.html)

---

## Task Statement 2.1: AWS Shared Responsibility Model

### Model Özeti

```
┌─────────────────────────────────────────────────────────────┐
│              SHARED RESPONSIBILITY MODEL                     │
│                                                             │
│  AWS: Security "OF" the Cloud (Bulutun güvenliği)          │
│  Customer: Security "IN" the Cloud (Buluttaki güvenlik)    │
└─────────────────────────────────────────────────────────────┘
```

### Detaylı Sorumluluk Dağılımı

```
╔═════════════════════════════════════════════════════════════╗
║                                                             ║
║  MÜŞTERİ SORUMLULUĞU (IN the Cloud)                        ║
║  ══════════════════════════════════                         ║
║  • Customer Data (Müşteri verisi)                          ║
║  • Platform, Applications, IAM                              ║
║  • Operating System, Network & Firewall Configuration      ║
║  • Client-side Encryption                                   ║
║  • Server-side Encryption (File System/Data)               ║
║  • Network Traffic Protection                               ║
║                                                             ║
╠═════════════════════════════════════════════════════════════╣
║                                                             ║
║  AWS SORUMLULUĞU (OF the Cloud)                            ║
║  ══════════════════════════════                             ║
║  • Software: Compute, Storage, Database, Networking        ║
║  • Hardware / AWS Global Infrastructure                     ║
║  • Regions, Availability Zones, Edge Locations             ║
║  • Physical Security of Data Centers                        ║
║                                                             ║
╚═════════════════════════════════════════════════════════════╝
```

### Servise Göre Sorumluluk Değişimi - KRİTİK!

AWS resmi dokümanına göre: *"Responsibilities can shift, depending on the service used"*

| Servis | Müşteri Sorumluluğu | AWS Sorumluluğu |
|--------|---------------------|-----------------|
| **EC2 (IaaS)** | OS patching, firewall config, data | Hardware, network, DC |
| **RDS (PaaS)** | Data, user access, backups | OS patching, DB engine updates |
| **Lambda (Serverless)** | Code, IAM permissions | Runtime, scaling, infrastructure |
| **S3 (Storage)** | Bucket policies, encryption config, data | Infrastructure, durability |

### Sınav Sorusu Örnekleri

| Soru | Cevap | Neden |
|------|-------|-------|
| "Who patches EC2 OS?" | **Customer** | IaaS = Müşteri OS yönetir |
| "Who patches RDS database engine?" | **AWS** | PaaS = AWS yönetir |
| "Who configures Security Groups?" | **Customer** | Firewall config müşterinin |
| "Who secures the data center?" | **AWS** | Physical security AWS'in |
| "Who manages Lambda infrastructure?" | **AWS** | Serverless = AWS yönetir |
| "Who writes Lambda function code?" | **Customer** | Application code müşterinin |

---

## Task Statement 2.2: Security, Governance, and Compliance Concepts

### AWS Artifact - Compliance Bilgisi

```
┌─────────────────────────────────────────────────────────────┐
│  SORU: "Where do I find AWS compliance reports?"           │
│  CEVAP: AWS ARTIFACT                                        │
└─────────────────────────────────────────────────────────────┘
```

**AWS Artifact İçeriği:**
- SOC 1, 2, 3 raporları
- PCI DSS uyumluluk raporu
- ISO sertifikaları (27001, 27017, 27018)
- HIPAA belgeleri
- GDPR (General Data Protection Regulation) belgeleri
- AWS ile imzalanacak anlaşmalar (BAA, NDA)

**Önemli:** Her AWS servisi için uyumluluk gereksinimleri **farklı** olabilir!

### Coğrafi/Endüstriyel Uyumluluk Gereksinimleri

| Uyumluluk | Bölge/Endüstri | Açıklama |
|-----------|----------------|----------|
| **GDPR** | Avrupa Birliği | Kişisel veri koruma |
| **HIPAA** | ABD Sağlık | Sağlık bilgisi güvenliği |
| **PCI DSS** | Global Finans | Kredi kartı verisi |
| **SOC** | Global | Güvenlik denetimleri |
| **FedRAMP** | ABD Kamu | Federal hükümet gereksinimleri |

---

### Şifreleme (Encryption) - KRİTİK!

#### Data at Rest vs Data in Transit

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  DATA AT REST              DATA IN TRANSIT                  │
│  (Depolanmış veri)         (Hareket halindeki veri)         │
│  ═══════════════           ════════════════════             │
│                                                             │
│  • S3 bucket içinde        • HTTPS üzerinden transfer       │
│  • EBS volume içinde       • TLS/SSL ile şifreleme          │
│  • RDS veritabanında       • VPN tüneli içinde              │
│  • Glacier arşivinde       • API çağrıları                  │
│                                                             │
│  Çözüm:                    Çözüm:                           │
│  • KMS                     • TLS 1.2+                       │
│  • SSE-S3                  • HTTPS                          │
│  • SSE-KMS                 • VPN                            │
│  • SSE-C                   • Direct Connect + MACsec        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### S3 Şifreleme Seçenekleri

| Tür | Anahtar Yönetimi | Kullanım |
|-----|------------------|----------|
| **SSE-S3** | AWS yönetir | Varsayılan, basit |
| **SSE-KMS** | KMS ile yönetim | Denetim gerektiğinde |
| **SSE-C** | Müşteri sağlar | Tam kontrol |
| **Client-side** | Müşteri şifreler | Veriden önce şifreleme |

---

### Loglama ve Denetim Servisleri - ÇOK KRİTİK!

AWS resmi dokümanından: *"Recognizing services that aid in governance and compliance (CloudWatch for monitoring; CloudTrail, Audit Manager, Config for auditing)"*

```
┌─────────────────────────────────────────────────────────────┐
│  "Kim ne yaptı?"           → CLOUDTRAIL                     │
│  "Sistem nasıl çalışıyor?" → CLOUDWATCH                     │
│  "Konfigürasyon doğru mu?" → CONFIG                         │
│  "Denetim kanıtı topla"    → AUDIT MANAGER                  │
└─────────────────────────────────────────────────────────────┘
```

| Servis | Amaç | Örnek Soru |
|--------|------|------------|
| **CloudTrail** | API aktivite logu (WHO did WHAT) | "Kim bu S3 bucket'ı sildi?" |
| **CloudWatch** | Monitoring, metrics, alarms | "CPU %90 olunca uyar" |
| **AWS Config** | Konfigürasyon uyumluluk denetimi | "Tüm EBS'ler şifreli mi?" |
| **AWS Audit Manager** | Sürekli denetim kanıt toplama | "SOC 2 için kanıt topla" |

### CloudTrail Detayları

- **Tüm AWS API çağrılarını** kaydeder
- 90 gün ücretsiz event history
- S3'e uzun süreli arşivleme
- Multi-region trail oluşturulabilir
- Yönetim olayları (Management events) ve veri olayları (Data events)

### CloudWatch Bileşenleri

| Bileşen | Açıklama |
|---------|----------|
| **Metrics** | CPU, Network, Disk, Memory (custom) |
| **Alarms** | Eşik aşımında bildirim/aksiyon |
| **Logs** | Uygulama ve sistem logları |
| **Events/EventBridge** | Kaynak değişikliği olayları |
| **Dashboards** | Görsel panolar |

### AWS Config

- Kaynak konfigürasyonlarını **sürekli** izler
- **Config Rules** ile uyumluluk kontrolü
- Değişiklik geçmişi (timeline) tutar
- **Remediation** ile otomatik düzeltme
- Örnek kurallar:
  - "S3 bucket'lar public olmamalı"
  - "EBS volume'lar şifrelenmiş olmalı"
  - "Security Group'larda SSH 0.0.0.0/0 olmamalı"

---

## Task Statement 2.3: AWS Access Management Capabilities

### IAM (Identity and Access Management)

#### IAM Bileşenleri

| Bileşen | Açıklama | Örnek |
|---------|----------|-------|
| **User** | Bireysel kimlik | john@company.com |
| **Group** | Kullanıcı koleksiyonu | Developers, Admins |
| **Role** | Geçici kimlik (servisler/federation için) | EC2-S3-Access-Role |
| **Policy** | İzin belgesi (JSON formatında) | S3ReadOnlyAccess |

#### Policy Türleri

| Tür | Açıklama | Yönetim |
|-----|----------|---------|
| **AWS Managed** | AWS tarafından oluşturulan | AWS günceller |
| **Customer Managed** | Sizin oluşturduğunuz | Siz yönetirsiniz |
| **Inline** | Tek bir user/group/role'e bağlı | Taşınamaz |

### Root User - ÇOK KRİTİK!

```
┌─────────────────────────────────────────────────────────────┐
│                    ROOT USER KURALLARI                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ❌ Günlük işler için ROOT KULLANMA!                        │
│  ✅ MFA mutlaka etkinleştir                                 │
│  ✅ Access key oluşturma (root için)                        │
│  ✅ IAM user oluştur ve onu kullan                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Sadece Root User Yapabileceği İşlemler

| İşlem | Açıklama |
|-------|----------|
| AWS hesap ayarlarını değiştirme | İsim, email, şifre |
| IAM user permissions boundary | Boundary limitleme |
| Hesabı kapatma | Account closure |
| GovCloud kaydı | US GovCloud |
| Reserved Instance Marketplace satış | RI satışı |
| S3 bucket policy restore | MFA delete sonrası |
| Support plan değişikliği | Support tier change |

### Least Privilege Principle (En Az Yetki İlkesi)

```
┌─────────────────────────────────────────────────────────────┐
│  "Sadece ihtiyaç duyulan MİNİMUM yetkiyi ver"               │
│                                                             │
│  YANLIŞ: AdministratorAccess herkese ver                    │
│  DOĞRU:  S3ReadOnlyAccess (sadece okuma gerekiyorsa)       │
│                                                             │
│  YANLIŞ: Geniş "*" permissions                              │
│  DOĞRU:  Spesifik resource ARN'leri                        │
└─────────────────────────────────────────────────────────────┘
```

### Authentication Methods (Kimlik Doğrulama)

| Yöntem | Açıklama | Kullanım |
|--------|----------|----------|
| **Password** | Kullanıcı şifresi | Console erişimi |
| **Access Keys** | Programatik erişim | CLI, SDK |
| **MFA** | Çok faktörlü doğrulama | Ekstra güvenlik |
| **IAM Roles** | Geçici credentials | EC2, Lambda, Federation |
| **Federation** | Harici identity provider | SAML, OIDC |

### MFA (Multi-Factor Authentication)

**Desteklenen MFA türleri:**
- Virtual MFA device (Google Authenticator, Authy)
- Hardware TOTP token
- FIDO security key (YubiKey)

### IAM Identity Center (eski adı: AWS SSO)

- Merkezi kimlik yönetimi
- **Çoklu AWS hesabına** tek giriş
- Active Directory entegrasyonu
- SAML 2.0 uyumlu uygulamalar
- Permission sets ile yetkilendirme

### Credential Storage

| Servis | Amaç | Özellik |
|--------|------|---------|
| **AWS Secrets Manager** | DB passwords, API keys | **Auto rotation** |
| **AWS Systems Manager Parameter Store** | Config değerleri, secrets | Ücretsiz tier var |

---

## Task Statement 2.4: Security Components and Resources

### Ağ Güvenliği - Security Groups vs NACLs

```
┌─────────────────────────────────────────────────────────────┐
│                    VPC GÜVENLİK KATMANLARI                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Internet                                                   │
│      │                                                      │
│      ▼                                                      │
│  ┌─────────────┐                                           │
│  │ Network ACL │ ◄── Subnet seviyesi (STATELESS)           │
│  └─────────────┘     Allow + DENY kuralları                │
│      │                                                      │
│      ▼                                                      │
│  ┌─────────────┐                                           │
│  │  Security   │ ◄── Instance seviyesi (STATEFUL)          │
│  │   Group     │     Sadece ALLOW kuralları                │
│  └─────────────┘                                           │
│      │                                                      │
│      ▼                                                      │
│  ┌─────────────┐                                           │
│  │    EC2      │                                           │
│  └─────────────┘                                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Karşılaştırma Tablosu - EZBERLE!

| Özellik | Security Group | Network ACL |
|---------|----------------|-------------|
| **Seviye** | Instance (ENI) | Subnet |
| **Durum** | **STATEFUL** | **STATELESS** |
| **Kural tipi** | Sadece **ALLOW** | Allow + **DENY** |
| **Varsayılan (inbound)** | Tüm trafik **engelli** | Tüm trafik **açık** |
| **Değerlendirme** | Tüm kurallar birlikte | Sıra numarasına göre |
| **Uygulama** | Instance'a atanır | Subnet'e otomatik |

### Stateful vs Stateless Açıklaması

```
STATEFUL (Security Group):
══════════════════════════
Request (IN)  ──────► İzin verildi
Response (OUT) ◄───── Otomatik izin (aynı bağlantı)

Tek kural yeterli!


STATELESS (NACL):
═════════════════
Request (IN)  ──────► Inbound rule gerekli
Response (OUT) ◄───── Outbound rule AYRICA gerekli!

İki ayrı kural yazmalısın!
```

### AWS Güvenlik Servisleri

AWS resmi dokümanından: *"AWS WAF, AWS Firewall Manager, AWS Shield, Amazon GuardDuty"*

| Servis | Ne Yapar | Ne Zaman Kullan |
|--------|----------|-----------------|
| **AWS WAF** | Web saldırılarını engeller | SQL injection, XSS koruması |
| **AWS Shield** | DDoS koruması | Her zaman (Standard ücretsiz) |
| **Amazon GuardDuty** | Tehdit algılama | Sürekli güvenlik izleme |
| **Amazon Inspector** | Güvenlik açığı tarama | EC2, Container, Lambda |
| **AWS Firewall Manager** | Merkezi firewall yönetimi | Multi-account WAF/Shield |
| **Amazon Macie** | S3'te hassas veri keşfi | PII, GDPR uyumluluğu |
| **Amazon Detective** | Güvenlik olayı araştırma | Olay sonrası analiz |
| **AWS Security Hub** | Merkezi güvenlik görünümü | Tüm bulguları topla |

### AWS WAF (Web Application Firewall)

**Korunan Saldırı Türleri:**
- SQL Injection
- Cross-Site Scripting (XSS)
- IP tabanlı engelleme
- Rate limiting (DDoS benzeri)
- Geo-blocking

**Entegre Edilebilir Servisler:**
- Amazon CloudFront
- Application Load Balancer (ALB)
- Amazon API Gateway
- AWS AppSync

### AWS Shield

| Versiyon | Özellik | Maliyet |
|----------|---------|---------|
| **Shield Standard** | L3/L4 DDoS koruması | **Ücretsiz** (otomatik) |
| **Shield Advanced** | L7 koruması, 24/7 DRT, maliyet koruması | $3,000/ay |

### Amazon GuardDuty

**Analiz Kaynakları:**
- VPC Flow Logs
- CloudTrail Logs
- DNS Logs
- EKS Audit Logs
- S3 Data Events

**Algılanan Tehditler:**
- Cryptocurrency mining
- Yetkisiz erişim denemeleri
- Unusual API calls
- Malicious IP communication

### AWS Trusted Advisor

AWS resmi dokümanından: *"Understanding the use of AWS services for identifying security issues (e.g., AWS Trusted Advisor)"*

**5 Kategori:**
| Kategori | Örnek Kontrol |
|----------|---------------|
| **Cost Optimization** | Kullanılmayan EC2, EBS |
| **Performance** | Over-utilized instances |
| **Security** | MFA, açık security group'lar |
| **Fault Tolerance** | Multi-AZ olmayan RDS |
| **Service Limits** | Limit yaklaşanlar |

**Support Plan Gereksinimleri:**
- Basic/Developer: 7 temel güvenlik kontrolü
- Business/Enterprise: **Tüm kontroller**

### Güvenlik Bilgisi Kaynakları

AWS resmi dokümanından: *"AWS Knowledge Center, AWS Security Center, AWS Security Blog"*

| Kaynak | İçerik |
|--------|--------|
| **AWS Knowledge Center** | SSS ve çözümler |
| **AWS Security Center** | Güvenlik best practices |
| **AWS Security Blog** | Güncel güvenlik haberleri |
| **AWS Well-Architected Tool** | Mimari değerlendirme |
| **AWS Marketplace** | 3. parti güvenlik ürünleri |

### AWS Marketplace - 3rd Party Security

AWS resmi dokümanından: *"Understanding that third-party security products are available from AWS Marketplace"*

- Firewall çözümleri
- Antivirus/Antimalware
- SIEM ürünleri
- Vulnerability scanners
- Identity management

---

## Domain 2 - Sınav İçin Kritik Noktalar

### Kesinlikle Ezberle!

```
┌─────────────────────────────────────────────────────────────┐
│                   KRİTİK NOKTALAR                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  SHARED RESPONSIBILITY:                                     │
│  • AWS = OF the cloud (altyapı)                            │
│  • Customer = IN the cloud (veri, config)                  │
│  • EC2 OS patching = Customer                              │
│  • RDS patching = AWS                                      │
│                                                             │
│  NETWORK SECURITY:                                          │
│  • Security Group = STATEFUL, ALLOW only                   │
│  • NACL = STATELESS, ALLOW + DENY                          │
│  • "Explicitly deny" = NACL kullan                         │
│                                                             │
│  LOGGING:                                                   │
│  • CloudTrail = WHO did WHAT? (API)                        │
│  • CloudWatch = HOW? (metrics, monitoring)                 │
│  • Config = IS IT COMPLIANT? (config denetimi)             │
│                                                             │
│  SECURITY SERVICES:                                         │
│  • WAF = Web attacks (SQL injection, XSS)                  │
│  • Shield = DDoS (Standard = FREE)                         │
│  • GuardDuty = Threat detection                            │
│  • Inspector = Vulnerability scanning                      │
│  • Macie = S3 PII discovery                                │
│                                                             │
│  COMPLIANCE:                                                │
│  • Artifact = Compliance reports                           │
│                                                             │
│  IAM:                                                       │
│  • Least privilege                                         │
│  • MFA everywhere (especially root)                        │
│  • Roles for services (EC2, Lambda)                        │
│  • Don't use root for daily tasks                          │
│                                                             │
│  ENCRYPTION:                                                │
│  • KMS = Key management                                    │
│  • At rest vs In transit                                   │
│  • Secrets Manager = Auto rotation                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Sık Çıkan Soru Kalıpları

| Soru Kalıbı | Cevap |
|-------------|-------|
| "Block specific IP address" | **NACL** (explicit deny) |
| "Stateful firewall at instance level" | **Security Group** |
| "API activity log / who deleted resource" | **CloudTrail** |
| "CPU utilization alarm" | **CloudWatch** |
| "Check if S3 buckets are encrypted" | **AWS Config** |
| "Download SOC/PCI report" | **AWS Artifact** |
| "Protect from SQL injection" | **AWS WAF** |
| "DDoS protection" | **AWS Shield** |
| "Detect cryptocurrency mining" | **GuardDuty** |
| "Find credit card numbers in S3" | **Amazon Macie** |
| "Scan EC2 for vulnerabilities" | **Amazon Inspector** |
| "Manage encryption keys" | **AWS KMS** |
| "Auto-rotate database password" | **Secrets Manager** |
| "Temporary credentials for EC2" | **IAM Role** |
| "Single sign-on to multiple accounts" | **IAM Identity Center** |
| "Security best practice recommendations" | **Trusted Advisor** |

---

## Hızlı Referans Kartı

```
┌─────────────────────────────────────────────────────────────┐
│                   DOMAIN 2 ÖZET                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  SHARED RESPONSIBILITY MODEL:                               │
│  ┌─────────────────────────────────────┐                   │
│  │ Customer: IN the cloud             │                   │
│  │ (data, config, access, encryption) │                   │
│  ├─────────────────────────────────────┤                   │
│  │ AWS: OF the cloud                  │                   │
│  │ (hardware, network, facilities)    │                   │
│  └─────────────────────────────────────┘                   │
│                                                             │
│  SECURITY GROUP vs NACL:                                    │
│  ┌─────────────────┬─────────────────┐                     │
│  │ Security Group  │ NACL            │                     │
│  ├─────────────────┼─────────────────┤                     │
│  │ Instance level  │ Subnet level    │                     │
│  │ STATEFUL        │ STATELESS       │                     │
│  │ Allow only      │ Allow + Deny    │                     │
│  └─────────────────┴─────────────────┘                     │
│                                                             │
│  LOGGING SERVICES:                                          │
│  • CloudTrail  → WHO did WHAT (API audit)                  │
│  • CloudWatch  → HOW is it running (metrics)               │
│  • Config      → IS IT compliant (config audit)            │
│                                                             │
│  PROTECTION SERVICES:                                       │
│  • WAF         → Web attacks                               │
│  • Shield      → DDoS                                      │
│  • GuardDuty   → Threat detection                          │
│  • Inspector   → Vulnerabilities                           │
│  • Macie       → S3 sensitive data                         │
│                                                             │
│  IAM BEST PRACTICES:                                        │
│  • Don't use root account                                  │
│  • Enable MFA everywhere                                   │
│  • Use least privilege                                     │
│  • Use roles for services                                  │
│  • Rotate credentials regularly                            │
│                                                             │
│  KEY SERVICES:                                              │
│  • Artifact       → Compliance docs                        │
│  • KMS            → Encryption keys                        │
│  • Secrets Manager→ Auto-rotate secrets                    │
│  • Identity Center→ SSO                                    │
│  • Trusted Advisor→ Security checks                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

- [AWS CLF-C02 Domain 2 Official Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02-domain2.html)
- [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [AWS Security Best Practices](https://aws.amazon.com/architecture/security-identity-compliance/)
- [AWS Artifact](https://aws.amazon.com/artifact/)

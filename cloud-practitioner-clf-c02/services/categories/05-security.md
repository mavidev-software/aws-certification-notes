# Security, Identity & Compliance Services (In-Scope)

> **Kategori:** Guvenlik, Kimlik ve Uyumluluk Servisleri

## AWS IAM (Identity and Access Management)

**Ne Ise Yarar:** AWS kaynaklarina erisimi guvenli bir sekilde yonetir.

**Temel Kavramlar:**
| Kavram | Aciklama |
|--------|----------|
| User | Bireysel kullanici |
| Group | Kullanici grubu |
| Role | Gecici kimlik (EC2, Lambda icin) |
| Policy | Izin belgeleri (JSON) |

**Policy Turleri:**
- **AWS Managed:** AWS tarafindan olusturulan
- **Customer Managed:** Musteri tarafindan olusturulan
- **Inline:** Tek kaynaga bagli

**Best Practices:**
- Root hesabi kullanmayin
- MFA etkinlestirin
- En az yetki ilkesi (Least Privilege)
- Access Key'leri duzenli dondurun

---

## AWS IAM Identity Center (eski adi: AWS SSO)

**Ne Ise Yarar:** Tek oturum acma (SSO) ve merkezi erisim yonetimi.

**Temel Ozellikler:**
- Birden fazla AWS hesabina tek giris
- Active Directory entegrasyonu
- SAML 2.0 uyumlu uygulamalar

---

## Amazon Cognito

**Ne Ise Yarar:** Web ve mobil uygulamalar icin kullanici kimlik dogrulama.

**Bilesenleri:**
- **User Pools:** Kullanici dizini, kayit/giris
- **Identity Pools:** AWS servislerine gecici erisim

---

## AWS KMS (Key Management Service)

**Ne Ise Yarar:** Sifreleme anahtarlarini olusturur ve yonetir.

**Anahtar Turleri:**
| Tur | Yonetim |
|-----|---------|
| AWS Managed Keys | AWS yonetir |
| Customer Managed Keys | Siz yonetirsiniz |
| AWS Owned Keys | AWS servisleri icin |

---

## AWS Secrets Manager

**Ne Ise Yarar:** Gizli bilgileri (sifreler, API anahtarlari) guvenle saklar.

**Temel Ozellikler:**
- Otomatik sifre dondurme
- RDS, Redshift, DocumentDB entegrasyonu
- Sifrelenmis depolama

**Secrets Manager vs Parameter Store:**
| Ozellik | Secrets Manager | Parameter Store |
|---------|-----------------|-----------------|
| Otomatik rotation | Var | Yok |
| Maliyet | Ucretli | Ucretsiz tier var |
| Kullanim | Sifreler, credentials | Konfigurasyon degerleri |

---

## AWS WAF (Web Application Firewall)

**Ne Ise Yarar:** Web uygulamalarini yaygin saldirilardan korur.

**Korunan Saldiri Turleri:**
- SQL Injection
- Cross-Site Scripting (XSS)
- IP tabanli engelleme
- Rate limiting

**Entegrasyonlar:**
- CloudFront
- Application Load Balancer
- API Gateway

---

## AWS Shield

**Ne Ise Yarar:** DDoS saldirilarina karsi koruma saglar.

**Versiyonlari:**
| Versiyon | Ozellik | Maliyet |
|----------|---------|---------|
| Shield Standard | L3/L4 DDoS korumasi | Ucretsiz |
| Shield Advanced | L7 korumasi, 24/7 destek, maliyet korumasi | $3,000/ay |

---

## Amazon GuardDuty

**Ne Ise Yarar:** Akilli tehdit algilama hizmeti.

**Analiz Kaynaklari:**
- VPC Flow Logs
- CloudTrail Logs
- DNS Logs

---

## Amazon Inspector

**Ne Ise Yarar:** Otomatik guvenlik acigi taramasi.

**Tarama Turleri:**
- EC2 instance guvenlik aciklari
- Container image taramasi
- Lambda fonksiyon taramasi
- Network erisilebilirlik

---

## AWS Artifact

**Ne Ise Yarar:** AWS guvenlik ve uyumluluk belgelerine erisim saglar.

**Icerik:**
- SOC raporlari
- PCI DSS raporlari
- ISO sertifikalari
- GDPR belgeleri

---

## Amazon Macie

**Ne Ise Yarar:** S3'teki hassas verileri (PII) otomatik kesfeder ve korur.

---

## AWS Security Hub

**Ne Ise Yarar:** Merkezi guvenlik gorunumu ve uyumluluk kontrolu.

**Entegrasyonlar:**
- GuardDuty
- Inspector
- Macie
- IAM Access Analyzer
- Firewall Manager

---

## AWS Certificate Manager (ACM)

**Ne Ise Yarar:** SSL/TLS sertifikalarini yonetir.

**Temel Ozellikler:**
- Ucretsiz public sertifikalar
- Otomatik yenileme
- CloudFront, ALB, API Gateway entegrasyonu

---

## AWS CloudHSM

**Ne Ise Yarar:** Donanim guvenlik modulu (HSM) hizmeti.

**KMS vs CloudHSM:**
| Ozellik | KMS | CloudHSM |
|---------|-----|----------|
| Yonetim | AWS shared | Dedicated |
| FIPS 140-2 | Level 2 | Level 3 |
| Anahtar kontrolu | AWS | Musteri |

---

## Amazon Detective

**Ne Ise Yarar:** Guvenlik olaylarinin kok nedenini arastirir.

---

## AWS Directory Service

**Ne Ise Yarar:** Microsoft Active Directory hizmeti sunar.

**Turleri:**
- AWS Managed Microsoft AD
- AD Connector
- Simple AD

---

## AWS Firewall Manager

**Ne Ise Yarar:** Merkezi guvenlik duvari kurallari yonetimi.

**Yonetilen Servisler:**
- WAF kurallari
- Shield Advanced korumalari
- Security Group'lar
- Network Firewall

---

## AWS RAM (Resource Access Manager)

**Ne Ise Yarar:** AWS kaynaklarini hesaplar arasinda paylasir.

**Paylasilabilir Kaynaklar:**
- Subnets
- Transit Gateway
- License Manager
- Route 53 Resolver

---

## AWS Audit Manager

**Ne Ise Yarar:** Surekli denetim ve uyumluluk kaniti toplama.


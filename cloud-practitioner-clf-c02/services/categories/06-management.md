# Management, Governance & Cloud Financial Management (In-Scope)

> **Kategori:** Yonetim, Yonetisim ve Bulut Maliyet Yonetimi

## AWS CloudFormation

**Ne Ise Yarar:** Infrastructure as Code (IaC) - Altyapiyi kod olarak tanimlar.

**Temel Kavramlar:**
| Kavram | Aciklama |
|--------|----------|
| Template | JSON/YAML altyapi tanimi |
| Stack | Bir template'ten olusturulan kaynaklar |
| Change Set | Degisiklikleri onizleme |
| Drift Detection | Konfigurasyon degisikliklerini algilama |

---

## AWS CloudTrail

**Ne Ise Yarar:** AWS API cagrilarini kaydeder ve izler.

**Log Icerigi:**
- Kim (hangi kullanici/rol)
- Ne zaman
- Hangi kaynak
- Hangi islem
- Nereden (IP adresi)

**Onemli:** "Kim bu EC2'yi sildi?" sorularina cevap verir.

---

## Amazon CloudWatch

**Ne Ise Yarar:** AWS kaynaklarini izler ve log toplar.

**Bilesenleri:**
| Bilesen | Aciklama |
|---------|----------|
| Metrics | CPU, Network, Disk vb. metrikleri |
| Alarms | Esik asiminda uyari |
| Logs | Uygulama ve sistem loglari |
| Events | Kaynak degisikligi olaylari |
| Dashboards | Gorsel panolar |

**CloudTrail vs CloudWatch:**
| Ozellik | CloudTrail | CloudWatch |
|---------|------------|------------|
| Amac | API aktivite logu | Performans izleme |
| Soru | Kim ne yapti? | Sistem nasil calisiyor? |

---

## AWS Config

**Ne Ise Yarar:** Kaynak konfigurasyonlarini izler ve denetler.

**Temel Ozellikler:**
- Konfigurasyon gecmisi
- Compliance rules ile uyumluluk kontrolu
- Remediation (otomatik duzeltme)

**Ornek Config Rules:**
- "Tum S3 bucket'lari sifrelenmi olmali"
- "Security Group'larda 0.0.0.0/0 SSH olmamali"

---

## AWS Organizations

**Ne Ise Yarar:** Birden fazla AWS hesabini merkezi yonetir.

**Temel Ozellikler:**
- Consolidated billing (birlesik faturalandirma)
- Service Control Policies (SCP)
- Organizational Units (OU)

---

## AWS Control Tower

**Ne Ise Yarar:** Multi-account ortami icin best-practice landing zone kurar.

**Temel Ozellikler:**
- Guardrails (korukluklar)
- Account Factory (otomatik hesap olusturma)
- Dashboard

---

## AWS Trusted Advisor

**Ne Ise Yarar:** AWS best practice onerilerini sunar.

**Kontrol Kategorileri:**
| Kategori | Ornek |
|----------|-------|
| Cost Optimization | Kullanilmayan EC2, EBS |
| Performance | High utilization instances |
| Security | MFA, acik security group'lar |
| Fault Tolerance | Multi-AZ olmayan RDS |
| Service Limits | Limit yaklasanlar |

**Support Plan Gereksinimleri:**
- Basic/Developer: 7 temel kontrol
- Business/Enterprise: Tum kontroller

---

## AWS Systems Manager

**Ne Ise Yarar:** AWS ve on-premises kaynaklari merkezi yonetir.

**Onemli Ozellikleri:**
- **Session Manager:** Bastion host olmadan EC2 erisimi
- **Parameter Store:** Konfigurasyon ve secret saklama
- **Patch Manager:** Otomatik yamalama
- **Run Command:** Uzaktan komut calistirma

---

## AWS Auto Scaling

**Ne Ise Yarar:** Kaynaklari otomatik olarak olceklendirir.

**Olcekleme Turleri:**
| Tur | Tetikleyici |
|-----|-------------|
| Target Tracking | CPU %70'e ulasinca |
| Step Scaling | Asamali olcekleme |
| Scheduled | Zamanlanmis |

---

## AWS Well-Architected Tool

**Ne Ise Yarar:** Mimarinizi Well-Architected Framework'e gore degerlendirir.

**6 Sutun:**
1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability

---

## AWS Health Dashboard

**Ne Ise Yarar:** AWS servis durumunu ve hesabinizi etkileyen olaylari gosterir.

**Iki Gorunum:**
- **Service Health:** Genel AWS servis durumu
- **Your Account Health:** Sizin kaynaklarinizi etkileyen olaylar

---

## Service Quotas

**Ne Ise Yarar:** AWS servis limitlerini goruntuler ve artis talep eder.

---

## AWS Compute Optimizer

**Ne Ise Yarar:** Kaynak boyutlandirma onerileri sunar.

**Analiz Edilen Kaynaklar:**
- EC2 instances
- EBS volumes
- Lambda functions
- ECS on Fargate

---

## AWS License Manager

**Ne Ise Yarar:** Yazilim lisanslarini yonetir.

---

## AWS Service Catalog

**Ne Ise Yarar:** Onayli IT hizmetlerinin katalogunuolusturur.

---

# Cloud Financial Management (Maliyet Yonetimi)

## AWS Cost Explorer

**Ne Ise Yarar:** Maliyet verilerini gorsellestir ve analiz eder.

**Ozellikleri:**
- 13 aylik gecmis veri
- Gelecek 12 ay tahmin
- Filtreleme ve gruplama
- Savings Plans onerileri

---

## AWS Budgets

**Ne Ise Yarar:** Butce olusturur ve uyarilar gonderir.

**Butce Turleri:**
- Cost Budget
- Usage Budget
- Reservation Budget
- Savings Plans Budget

**Ornek:** "Aylik EC2 harcamasi $1000'i gecerse uyar"

---

## AWS Cost and Usage Reports

**Ne Ise Yarar:** En detayli maliyet verilerini CSV/Parquet olarak sunar.

---

## AWS Marketplace

**Ne Ise Yarar:** Ucuncu parti yazilim ve hizmetlerin magazasi.

**Kategoriler:**
- Security
- Networking
- Machine Learning
- DevOps
- Database


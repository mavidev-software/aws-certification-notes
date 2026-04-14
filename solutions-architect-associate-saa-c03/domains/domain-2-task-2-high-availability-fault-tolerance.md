# Domain 2 — Task Statement 2: Design Highly Available and/or Fault Tolerant Architectures

---

## Önemli Keywords (Sınav Odaklı)

### Temel Kavramlar — HA vs FT vs DR

| Kavram | Tanım | Kesinti? | Maliyet |
|--------|--------|----------|---------|
| **High Availability (HA)** | Arıza sonrası sistemi mümkün olan en kısa sürede geri getirme | Kısa kesinti olabilir | Daha düşük |
| **Fault Tolerance (FT)** | Arıza sırasında bile sistemin kesintisiz çalışmaya devam etmesi | Kesinti yok | Daha yüksek |
| **Disaster Recovery (DR)** | Felaket için ön planlama ve kurtarma süreci | Stratejiye bağlı | Stratejiye bağlı |

> **Sınav Tuzağı:** HA ≠ sıfır kesinti. FT = sıfır kesinti. Bu ayrımı karıştırmayın.

### RTO ve RPO

| Metrik | Tanım | Soru Biçimi |
|--------|--------|-------------|
| **RPO (Recovery Point Objective)** | Son veri kurtarma noktasından bu yana kabul edilebilir maksimum süre — veri kaybı toleransı | "Ne sıklıkla yedeklenmeli?" |
| **RTO (Recovery Time Objective)** | Hizmet kesilmesi ile geri yüklenmesi arasındaki maksimum kabul edilebilir gecikme | "Uygulama ne kadar süre kapalı kalabilir?" |

### Felaket Kurtarma Stratejileri

| Strateji | Tür | RTO/RPO | Maliyet | Açıklama |
|----------|------|---------|---------|----------|
| **Backup & Restore** | Aktif/Pasif | En yüksek RTO/RPO | En düşük | Yedekten geri yükleme |
| **Pilot Light** | Aktif/Pasif | Orta-yüksek | Düşük | Minimum çekirdek bileşenler çalışır |
| **Warm Standby** | Aktif/Pasif | Orta-düşük | Orta | Küçültülmüş tam kopya çalışır |
| **Active/Active (Multi-site)** | Aktif/Aktif | En düşük RTO/RPO | En yüksek | Tam kapasite birden fazla sitede |

### Veritabanı Dayanıklılığı

| Servis / Özellik | Sınav Notu |
|-------------------|-----------|
| **RDS Multi-AZ** | Otomatik failover, birincil çökerse standby devralır — failover süresi önemli |
| **Aurora Global Database** | Bölgeler arası failover, failover süresini bilin |
| **DynamoDB Global Tables** | Çok bölgeli aktif/aktif replikasyon |
| **RDS Proxy** | Failover süresini %66 azaltır, bağlantı havuzlama, Secrets Manager + IAM entegrasyonu |

### Depolama ve Yedekleme

| Servis | Kullanım |
|--------|----------|
| **Amazon S3** | Düşük maliyetli, yüksek dayanıklılıklı nesne depolama, yedekleme hedefi |
| **EFS** | Yönetilen dosya depolama |
| **Amazon FSx** | Windows / Lustre dosya sistemleri |
| **AMI + EC2 Image Builder** | EC2 için DR stratejisi, önceden hazırlanmış imajlar |
| **AWS Elastic Disaster Recovery** | Hem on-premises hem cloud uygulamaları için DR |
| **CloudFormation** | DR sonrası ortamı hızlıca yeniden sağlama (şablon bazlı) |

### Ağ ve Yönlendirme

| Servis | Neden Önemli |
|--------|-------------|
| **Route 53** | DNS failover routing, küresel mimari desteği |
| **Global Accelerator** | Uygulama kullanılabilirliği ve performans iyileştirmesi |
| **VPC Peering** | VPC'ler arası bağlantı |
| **Transit Gateway** | Merkezi ağ hub'ı |
| **Site-to-Site VPN** | Şirket içi ↔ AWS bağlantısı |
| **Direct Connect** | Dedike ağ bağlantısı |
| **Route 53 Resolver** | Hibrit DNS çözümleme |

### Dağıtım ve İzleme

| Servis | Rolü |
|--------|------|
| **Elastic Beanstalk** | Otomatik dağıtım ve yönetim |
| **CloudFormation** | Altyapı kodla tanımlama (IaC) |
| **OpsWorks** | Yapılandırma yönetimi |
| **ECS / EKS** | Konteyner dağıtımı |
| **CloudWatch** | Metrik izleme + alarmlar → otomatik eylemler |
| **X-Ray** | Dağıtık izleme (distributed tracing) |
| **EventBridge** | Neredeyse gerçek zamanlı olay yanıtı |
| **Amazon Inspector** | Altyapı güvenlik açığı taraması |
| **Amazon CodeGuru** | Kod kalitesi ve güvenlik incelemesi |

### AI/ML Servisleri (Sınavda Çıkıyor!)

| Servis | Kullanım Senaryosu |
|--------|-------------------|
| **Amazon Polly** | Metinden konuşmaya (TTS) — Connect self servis, sesli bildirimler |
| **Amazon Comprehend** | Doğal dil işleme — BT taleplerini otomatik sınıflandırma |
| **Amazon Connect** | Bulut tabanlı iletişim merkezi |

### Tasarım Prensipleri

| Prensip | Açıklama |
|---------|----------|
| **Single Point of Failure (SPOF)** | Her bileşeni tek tek değerlendirin — "Bu çökerse ne olur?" |
| **Arızadan geriye doğru düşünme** | Her bileşenin arızasını simüle ederek zayıf noktaları bulma |
| **Yedekleri tesis dışında saklama** | Aynı binada yedek = felaket riski |
| **DR tatbikatları yapma** | Gerçek felaketten önce süreci test etme |
| **Sürekli iyileştirme** | Well-Architected Framework prensibi |

---

## Genel Özet ve Anlatım

Bu video, SAA-C03 sınavının Domain 2'sindeki ikinci görev ifadesini ele alıyor: **yüksek kullanılabilirlik ve/veya hataya dayanıklı mimari tasarımı**.

### Üç Temel Kavram

Video, sınavın bel kemiğini oluşturan üç kavramı net biçimde ayırıyor:

- **High Availability (HA):** Arıza olduğunda sistemi mümkün olan en kısa sürede geri getirme. Kısa bir kesinti kabul edilebilir. Örnek: aktif/bekleme (active/standby) sunucu çifti.
- **Fault Tolerance (FT):** Arıza sırasında bile sıfır kesinti ile çalışmaya devam etme. Daha pahalı. Örnek: her ikisi de aktif olan iki sunucu.
- **Disaster Recovery (DR):** Felakete karşı önceden hazırlanmış plan. Yedekler tesis dışında, tercihen bulutta saklanmalı. Tatbikatlar yapılmalı.

### RTO/RPO ve DR Stratejileri

Felaket kurtarma stratejisi seçimi tamamen **RTO** (ne kadar süre kapalı kalabilir) ve **RPO** (ne kadar veri kaybı kabul edilebilir) değerlerine bağlıdır. Stratejiler maliyetten en düşükten en yükseğe: Backup & Restore → Pilot Light → Warm Standby → Active/Active. Bunlar birlikte de kullanılabilir.

### Veritabanı Dayanıklılığı

- **RDS Multi-AZ** otomatik failover sağlar ancak failover süresi vardır
- **Aurora Global Database** bölgeler arası failover yapar
- **DynamoDB Global Tables** çok bölgeli aktif/aktif replikasyon sunar
- **RDS Proxy** failover süresini %66 azaltır ve bağlantı havuzlama ile sunucusuz uygulamaların veritabanı kaynaklarını tüketmesini önler

### Tek Arıza Noktası (SPOF) Analizi

Sınavda en sık sorulan kalıp: "Bu mimarideki tek arıza noktası nedir ve nasıl giderilir?" Yaklaşım olarak **arızadan geriye doğru düşünün** — her bileşenin çökmesini simüle edin. Tipik çözüm: ELB + Multi-AZ Auto Scaling. Bölgeler arası failover için Route 53 failover routing kullanın.

### İzleme ve Sürekli İyileştirme

CloudWatch (metrikler + alarmlar), X-Ray (dağıtık izleme) ve EventBridge (gerçek zamanlı olay yanıtı) ile sürekli izleme ve otomatik aksiyon alma.

### Sürpriz Konu: Amazon Polly

Video, Amazon Polly'nin sınavda tekrar çıktığını vurguluyor. Kullanım senaryosu: Amazon Comprehend ile BT taleplerini otomatik sınıflandırma + Polly ile sesli çıktı / Amazon Connect self servis.

> **Sınav İpucu:** HA vs FT farkını, RTO/RPO kavramlarını ve 4 DR stratejisinin sırasını (maliyet ve kurtarma süresi açısından) kesinlikle ezberleyin. Ayrıca "bu bileşen çökerse ne olur?" sorusunu her mimaride kendinize sorun.

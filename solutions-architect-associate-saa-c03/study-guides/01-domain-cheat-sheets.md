# AWS SAA-C03 Sinav Hizli Basvuru Kartlari

---

## Domain 1: Guvenli Mimariler Tasarlama (Sinav Agirligi: %30)

### Mutlaka Bilinmesi Gereken 10 Kavram

1. **Shared Responsibility Model**: AWS bulutun guvenligini saglar (fiziksel altyapi, hypervisor), musteri bulut icindeki guvenligi saglar (veri, IAM, isletim sistemi yamalari)
2. **IAM**: Kullanicilar, gruplar, roller, politikalar — her zaman **en az yetki ilkesi (least privilege)** uygulanir
3. **IAM Role**: EC2, Lambda gibi servisler icin tercih edilen yontem — uzun omurlu kimlik bilgileri (access key) yerine gecici kimlik bilgileri kullanir
4. **AWS Organizations + SCP**: Coklu hesap yonetimi, Service Control Policy tum hesaplara ust sinir koyar (izin vermez, sadece kisitlar)
5. **VPC**: Ozel ag alani — public/private subnet, Internet Gateway (public erisim), NAT Gateway (private subnetlerin disa cikisi)
6. **Security Groups**: **Stateful** — gelen izin verilirse donus otomatik acilir, yalnizca ALLOW kurali vardir
7. **NACLs**: **Stateless** — gelen ve giden kurallar ayri tanimlanir, ALLOW ve DENY kurallari vardir, subnet seviyesinde calisir
8. **KMS**: AWS tarafli yonetilen sifreleme anahtarlari, at-rest sifreleme icin varsayilan cozum
9. **CloudHSM**: Musteri tarafli anahtar yonetimi, FIPS 140-2 Level 3, PII uyumlulugu gerektiren senaryolar
10. **Shield + WAF + GuardDuty**: Shield=DDoS korumasi, WAF=Layer 7 filtreleme (SQL injection, XSS), GuardDuty=tehdit tespiti (makine ogrenmesi tabanli)

### Guvenlik Servisleri Karsilastirma Tablosu

| Servis | Katman | Koruma Alani | Not |
|---|---|---|---|
| Security Group | L4-L7 | ENI (instance) | Stateful, sadece ALLOW |
| NACL | L4 | Subnet | Stateless, ALLOW + DENY |
| WAF | L7 | CloudFront, ALB, API GW | SQL injection, XSS, IP engelleme |
| Shield Standard | L3/L4 | Tum AWS | Ucretsiz, otomatik DDoS korumasi |
| Shield Advanced | L3/L4/L7 | Belirli kaynaklar | Ucretli, DRT ekip destegi, maliyet korumasi |
| GuardDuty | — | Hesap geneli | VPC Flow Logs, DNS Logs, CloudTrail analizi |

### Sifreleme Karsilastirma Tablosu

| Ozellik | KMS | CloudHSM |
|---|---|---|
| Yonetim | AWS yonetimli | Musteri yonetimli |
| Paylasim | Coklu hesap | Tek VPC (peering ile genisletilebilir) |
| Uyumluluk | FIPS 140-2 Level 2 | FIPS 140-2 Level 3 |
| Kullanim | Genel sifreleme | PII, regulasyon gerektiren durumlar |
| Entegrasyon | Cogu AWS servisi ile dogrudan | Ozel uygulama entegrasyonu gerekir |

### Sinav Tuzaklari

- **SCP izin vermez, sadece kisitlar.** Root hesaba bile uygulanabilir ama management account harictir.
- **Security Group varsayilan olarak tum gideni acar**, tum geleni kapatir. NACL ise varsayilan olarak her seyi acar.
- **IAM Policy "Deny" her zaman kazanir** — acik bir Deny varsa hicbir Allow onu gecemez.
- **NAT Gateway** public subnette olmalidir, private subnetlerdeki instance'larin internete cikisi icindir (gelen baglanti yok).
- **GuardDuty buldugu tehditleri engellemez** — sadece bildirir. Engellemek icin Lambda + EventBridge entegrasyonu gerekir.

---

## Domain 2: Dayanikli Mimariler Tasarlama (Sinav Agirligi: %26)

### Mutlaka Bilinmesi Gereken 10 Kavram

1. **Vertical Scaling (Scale Up/Down)**: Instance tipini buyutme/kucultme — kesinti gerektirir
2. **Horizontal Scaling (Scale Out/In)**: Instance sayisini artirma/azaltma — kesintisiz
3. **Elasticity = Otomasyon + Horizontal Scaling**: Talebe gore otomatik genisleme/daralma
4. **HA (High Availability)**: Kisa kesinti kabul edilir, hizli kurtarma hedeflenir (ornek: Multi-AZ)
5. **FT (Fault Tolerance)**: Sifir kesinti, ariza aninda bile calisma devam eder (ornek: Active/Active)
6. **DR (Disaster Recovery)**: Felaket oncesi planlama, RTO ve RPO hedefleri belirlenir
7. **ELB + Auto Scaling = Self-Healing**: Sagliksiz instance otomatik kaldirilir ve yenisi eklenir
8. **SQS ile Decoupling**: Bilesenleri birbirinden ayirir, bagimsiz olceklendirme saglar
9. **Serverless (Lambda, Fargate, API Gateway)**: Yerlesik HA/FT, altyapi yonetimi yok
10. **Stateless Tasarim**: Oturum verisini instance disinda tut (ElastiCache, DynamoDB, EFS)

### RTO ve RPO Karsilastirmasi

| Kavram | Tanim | Ornek |
|---|---|---|
| **RTO** (Recovery Time Objective) | Maksimum kabul edilebilir kesinti suresi | "4 saat icinde ayaga kalkmali" |
| **RPO** (Recovery Point Objective) | Maksimum kabul edilebilir veri kaybi | "Son 1 saatlik veri kaybedilebilir" |

### DR Strateji Tablosu (Maliyete gore artan sirada)

| Strateji | Maliyet | RTO | RPO | Aciklama |
|---|---|---|---|---|
| Backup & Restore | En dusuk | Saatler | Saatler | Yedekten geri yukleme |
| Pilot Light | Dusuk | Dakikalar-saatler | Dakikalar | Cekirdek sistem hazir, gerisi tetiklenir |
| Warm Standby | Orta | Dakikalar | Saniyeler-dakikalar | Kucultulmus kopya calisir durumda |
| Active/Active | En yuksek | Yaklasik sifir | Yaklasik sifir | Tam kapasite iki bolge ayni anda |

### RDS: Read Replica vs Multi-AZ

| Ozellik | Read Replica | Multi-AZ |
|---|---|---|
| Amac | Okuma performansi + okumaDA HA | Yazma HA (failover) |
| Okuma olceklendirme | Evet | Hayir |
| Asenkron/Senkron | Asenkron replikasyon | Senkron replikasyon |
| Farkli Region | Evet (Cross-Region) | Hayir (ayni Region) |
| Failover | Manuel promote gerekir | Otomatik failover |
| **RDS Proxy** | — | %66 daha hizli failover, connection pooling |

### Sinav Tuzaklari

- **Multi-AZ okuma performansini artirmaz** — sadece HA saglar. Okuma olceklendirme icin Read Replica kullanilir.
- **RDS Proxy** Lambda ile RDS kullaniyorsan neredeyse zorunludur — baglanti havuzu tasmasini onler.
- **Elasticity != Scalability**. Elasticity otomatik olceklendirmeyi icerir, scalability manual de olabilir.
- **SQS standart kuyruk** en az bir kez teslimat ve sirasiz olabilir. Siralama gerekiyorsa **FIFO** kullanilir.
- **Stateful uygulama** soran sorularda: disk icin EFS/FSx, oturum icin sticky sessions veya ElastiCache, container icin ECS+EFS.
- **Fargate** secenekler arasindaysa ve "sunucu yonetimi istemiyorum" deniyorsa dogru cevap buyuk olasilikla Fargate'dir.

---

## Domain 3: Yuksek Performansli Mimariler Tasarlama (Sinav Agirligi: %24)

### Mutlaka Bilinmesi Gereken 10 Kavram

1. **S3**: Sinirsiz depolama, global dayaniklilik, multi-part upload (>100MB), Transfer Accelerator (uzak bolge yuklemeleri)
2. **EBS Volume Tipleri**: gp3 (genel amac, IOPS ayri ayarlanir), io2 (yuksek IOPS), st1 (throughput), sc1 (soguk arsiv)
3. **EFS vs FSx**: EFS=Linux NFS, otomatik olceklenir | FSx for Windows=SMB+NTFS+AD | FSx for Lustre=HPC+Linux
4. **Lambda**: Dogasi geregi olceklenebilir, 15 dakika limit, sunucusuz, olay tabanli
5. **Aurora**: 6 kopya / 3 AZ, paylasimli cluster volume, replikalar hem okuma hem HA saglar
6. **DynamoDB**: Tek haneli milisaniye gecikme, otomatik olceklendirme, on-demand veya provisioned kapasite
7. **ElastiCache**: Redis (gelismis veri yapilari, replikasyon) vs Memcached (basit, coklu cekirdek)
8. **DAX**: Yalnizca DynamoDB icin in-memory cache, mikrosaniye gecikme
9. **Route 53**: 7 yonlendirme politikasi — latency-based, weighted, failover en sik sorulan uclu
10. **VPC Endpoint**: Gateway (S3 ve DynamoDB icin, ucretsiz) vs Interface (diger servisler, ucretli)

### Depolama Karsilastirma Tablosu

| Ozellik | S3 (Object) | EBS (Block) | EFS (File) | FSx |
|---|---|---|---|---|
| Olceklendirme | Sinirsiz | Manuel | Otomatik | Otomatik |
| Erisim | HTTP/HTTPS | Tek EC2 (Multi-Attach io2 haric) | Coklu EC2 (NFS) | SMB veya Lustre |
| Kullanim | Statik icerik, yedek, data lake | Boot volume, DB | Paylasimli dosya sistemi | Windows / HPC |
| Dayaniklilik | 11x9 | AZ icinde | Coklu AZ | Coklu AZ |

### EBS Volume Tipleri

| Tip | Kullanim | Maks IOPS | Not |
|---|---|---|---|
| gp3 | Genel amac | 16.000 | IOPS ve throughput bagimsiz ayarlanir |
| gp2 | Genel amac (eski) | 16.000 | IOPS boyuta bagli (3 IOPS/GB) |
| io2/io2 Block Express | Yuksek performans DB | 256.000 | Multi-Attach destegi |
| st1 | Buyuk veri, log isleme | 500 | Throughput odakli, boot volume olamaz |
| sc1 | Soguk arsiv | 250 | En ucuz EBS, boot volume olamaz |

### Veritabani Secim Tablosu

| Ihtiyac | Servis | Neden |
|---|---|---|
| Iliskisel, genel amac | RDS (MySQL, PostgreSQL...) | Yonetimli, Multi-AZ |
| Yuksek performansli iliskisel | Aurora | 5x MySQL, 3x PostgreSQL performans |
| Sporadik / degisken yuk | Aurora Serverless | ACU tabanli, sifira inebilir |
| Anahtar-deger, dusuk gecikme | DynamoDB | Tek haneli ms, sinirsiz olceklendirme |
| In-memory cache (genel) | ElastiCache Redis/Memcached | Mikrosaniye erisim |
| In-memory cache (DynamoDB) | DAX | DynamoDB onunde otomatik cache |
| Grafik veritabani | Neptune | Sosyal ag, oneri motorlari |
| Zaman serisi | Timestream | IoT, metrik verileri |

### Yukle Dengeleyici Karsilastirmasi

| Tip | Katman | Kullanim | Not |
|---|---|---|---|
| ALB | L7 (HTTP/HTTPS) | Web uygulamalari, mikroservisler | Path/host-based routing, tek Region |
| NLB | L4 (TCP/UDP) | Yuksek performans, statik IP | Ultra dusuk gecikme, milyon RPS |
| GLB | L3 (IP) | Guvenlik cihazlari (firewall, IDS) | GENEVE protokolu |

### Sinav Tuzaklari

- **CloudWatch varsayilan olarak bellek metrigi toplamaz.** Bellek izleme icin custom metric + CloudWatch Agent gerekir.
- **EBS snapshot'lari S3'te saklanir** ama S3 konsolunda gorunmez.
- **S3 Transfer Accelerator** "uzak bolgeden yuklemeler yavasdir" senaryolarinda cevaptir.
- **Aurora Read Replica** RDS Read Replica'dan farklidir — Aurora'da replikalar otomatik failover hedefi olabilir.
- **Lambda@Edge** CloudFront dagitim noktalarinda calisir — viewer/origin request/response degistirme senaryolarinda sorarlar.
- **Gateway VPC Endpoint** sadece S3 ve DynamoDB icindir ve **ucretsizdir**. Diger servisler Interface Endpoint kullanir.
- **Lazy Loading** cache miss olunca DB'den okur ve cache'e yazar (eski veri riski). **Write-Through** her yazmada cache gunceller (yazma yuku artar).
- **Kinesis Streams** gercek zamanli isleme, **Firehose** en basit teslim yolu (S3, Redshift, OpenSearch'e), **Analytics** SQL ile donustururme.

---

## Domain 4: Maliyet Optimize Edilmis Mimariler Tasarlama (Sinav Agirligi: %20)

### Mutlaka Bilinmesi Gereken 8 Kavram

1. **5 Maliyet Ilkesi**: Dogru boyutlandirma, esneklik, fiyat modeli, depolama eslestirme, surekli iyilestirme
2. **EC2 Fiyatlandirma**: On-Demand > Savings Plans > Reserved > Spot (en ucuz ama kesintiye ugramaya hazir ol)
3. **S3 Lifecycle**: Zamana gore sinif degistirme | Intelligent-Tiering: erisim desenine gore otomatik
4. **Aurora Serverless**: Duzenli olmayan is yukleri icin ideal, ACU tabanli, sifira inebilir
5. **Data Transfer Kurallari**: Ayni AZ ucretsiz, Cross-AZ ucretli, Cross-Region pahali
6. **NAT Gateway**: Dev ortaminda paylasimli, prod ortaminda AZ basina bir tane
7. **Session Manager**: Ucretsiz, bastion host gereksiz, port acmaya gerek yok
8. **Maliyet Araclari**: Cost Explorer (analiz), Budgets (uyari), Trusted Advisor (optimizasyon onerileri)

### EC2 Fiyatlandirma Karsilastirmasi

| Model | Maliyet | Taahhut | Kullanim Alani |
|---|---|---|---|
| On-Demand | En yuksek | Yok | Kisa sureli, tahmin edilemeyen yukler |
| Savings Plans | ~%72'ye kadar indirim | 1 veya 3 yil | Esnek (instance ailesi, Region degisebilir) |
| Reserved Instance | ~%75'e kadar indirim | 1 veya 3 yil | Sabit is yuku, belirli instance tipi |
| Spot Instance | ~%90'a kadar indirim | Yok | Kesilmeye dayanikli isler (batch, CI/CD) |
| Dedicated Host | Degisken | Opsiyonel | Lisanslama gereksinimleri (BYOL) |

### Veri Transferi Maliyet Tablosu

| Senaryo | Maliyet |
|---|---|
| Ayni AZ icinde (ozel IP ile) | Ucretsiz |
| Cross-AZ | Ucretli (dusuk) |
| Cross-Region | Ucretli (yuksek) |
| Internet'ten AWS'ye (ingress) | Ucretsiz |
| AWS'den Internet'e (egress) | Ucretli |
| Origin'den CloudFront'a | Ucretsiz |
| Gateway VPC Endpoint (S3/DynamoDB) | Ucretsiz |
| NAT Gateway uzerinden | Islem + veri transfer ucreti |

### Ag Cozumleri Maliyet Karsilastirmasi

| Cozum | Maliyet | Kullanim |
|---|---|---|
| VPN | Dusuk | Sifrelenmis internet uzerinden baglanti |
| Direct Connect | Yuksek | Adanmis hat, tutarli performans |
| VPC Peering | Dusuk | Iki VPC arasi dogrudan baglanti |
| Transit Gateway | Yuksek | Cok sayida VPC/VPN merkezi yonetim |

### Sinav Tuzaklari

- **"En uygun maliyetli" sorusunda Spot Instance cazip gorunur** ama is yuku kesilmeye dayanikli degilse yanlis cevaptir. Soru baglami kritiktir.
- **Savings Plans, Reserved Instance'tan daha esnektir** — instance ailesi ve Region degisimine izin verir. Soru "esneklik" diyorsa Savings Plans.
- **S3 Intelligent-Tiering** erisim deseni bilinmiyorsa dogru cevap. Bilinen ve duzenli gecis icin Lifecycle Policy.
- **Trusted Advisor** kullanilmayan EBS volumlerini, bos Elastic IP'leri ve idle Load Balancer'lari tespit eder.
- **Read Replica veya ElastiCache** ile okuma yukunu azaltmak, RDS instance tipini buyutmekten (scale up) her zaman daha maliyet etkilidir.
- **"Bastion host yerine ne kullaniriz?"** sorusunun cevabi **Session Manager** — ucretsiz, port acma yok, audit log otomatik.
- **Requester Pays** baska hesaplarin sizin S3 verilerinize eristigi senaryolarda maliyeti onlara yansitir.
- **Aurora Serverless** "gece kullanilmiyor, gun icinde ani yukler" senaryolarinda en uygun maliyetli RDS secenegidir.

---

## Hizli Referans: Sik Karistirilan Kavramlar

| Kavram A | Kavram B | Fark |
|---|---|---|
| Security Group | NACL | Stateful vs Stateless |
| HA | FT | Kisa kesinti kabul vs sifir kesinti |
| RTO | RPO | Kesinti suresi vs veri kaybi suresi |
| Read Replica | Multi-AZ | Performans+okuma HA vs yazma HA |
| Lifecycle | Intelligent-Tiering | Zamana dayali vs erisim desenine dayali |
| Savings Plans | Reserved | Esnek taahhut vs sabit taahhut |
| Gateway Endpoint | Interface Endpoint | S3/DynamoDB icin ucretsiz vs diger servisler ucretli |
| Vertical Scaling | Horizontal Scaling | Buyut (kesinti) vs cogalt (kesintisiz) |
| KMS | CloudHSM | AWS yonetimli vs musteri yonetimli |
| ALB | NLB | L7 HTTP/path routing vs L4 TCP/UDP yuksek performans |
| Kinesis Streams | Kinesis Firehose | Gercek zamanli isleme vs en basit teslim |
| Lazy Loading | Write-Through | Okumada cache vs yazmada cache |

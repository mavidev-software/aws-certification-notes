# AWS SAA-C03 Sinav Tuzaklari, Karistirilan Kavramlar ve Strateji Rehberi

---

## Bolum 1: En Sik Dusulen Tuzaklar (Top 20)

| # | Tuzak | Dogru Bilgi | Neden Onemli |
|---|-------|-------------|--------------|
| 1 | EFS Windows'ta calisir | **YANLIS.** EFS sadece Linux tabanli sistemlerde calisir (NFS protokolu). Windows icin **Amazon FSx** (FSx for Windows File Server) kullanilmalidir. | Sinavda "Windows dosya paylasimi" sorularinda EFS secenegi dikkat dagitici olarak konur. Isletim sistemi uyumlulugunu mutlaka kontrol edin. |
| 2 | RDS Multi-AZ okuma olceklendirir | **YANLIS.** Multi-AZ yalnizca **High Availability (HA)** saglar; standby replika okuma trafigi almaz. Okuma olceklendirmesi icin **Read Replica** kullanilmalidir. | "Okuma performansi artirmak" ile "kesintisizlik saglamak" farkli gereksinimlerdir. Multi-AZ = HA, Read Replica = okuma olceklendirme. |
| 3 | Read Replica = Cache | **YANLIS.** Read Replica hala bir veritabani motorudur ve sorgu isleme yukunu tasir. Gercek cache icin **ElastiCache** (Redis/Memcached) veya **DAX** (DynamoDB icin) kullanilmalidir. | Mikrosaniye seviyesinde yanit suresi isteniyorsa cache, milisaniye seviyesinde okuma dagitimi isteniyorsa Read Replica uygundur. |
| 4 | Lambda sinirsiz calisir | **YANLIS.** Lambda fonksiyonlari **maksimum 15 dakika** calisabilir. Daha uzun sureli islemler icin **Step Functions**, **ECS/Fargate** veya **EC2** tercih edilmelidir. | "Uzun sureli batch islemi" iceren sorularda Lambda secenegi her zaman yanlistir. Step Functions ile orkestrasyon veya Fargate ile container kullanin. |
| 5 | S3 EC2'ye mount edilebilir | **YANLIS.** S3 bir **nesne depolama** (object storage) servisidir ve dosya sistemi olarak mount edilemez. Blok depolama icin **EBS**, ag dosya sistemi icin **EFS** veya **FSx** kullanilmalidir. | S3 REST API uzerinden erisim saglar. "Mount", "dosya sistemi", "POSIX uyumluluk" anahtar kelimeleri gordugunuzde S3'u eleyin. |
| 6 | ALB cross-Region hedef alabilir | **YANLIS.** ALB yalnizca **tek bir Region** icerisinde calisir. Cross-Region yuk dagitimi icin **Route 53** (DNS tabanli) veya **Global Accelerator** (anycast IP tabanli) kullanilmalidir. | Multi-Region mimarilerde ALB degil, Global Accelerator veya Route 53 tercih edilir. ALB sadece ayni Region'daki hedeflere trafik yonlendirir. |
| 7 | CloudWatch memory metrigi varsayilan | **YANLIS.** EC2 icin bellek (memory) kullanimi varsayilan olarak **izlenmez**. CloudWatch Agent kurularak **custom metric** olarak eklenmelidir. | Varsayilan metrikler: CPU, disk I/O, ag trafigi. Bellek ve disk alani kullanimi icin CloudWatch Agent sart. Bu cok sik sorulan bir konudur. |
| 8 | NAT Gateway uzerinden inbound baglanti | **YANLIS.** NAT Gateway yalnizca **outbound (egress)** trafik icin kullanilir. Internet'ten private subnet'e erisim saglamaz. Inbound icin **ALB/NLB**, **Bastion Host** veya **VPN** kullanilir. | NAT = "Network Address Translation" ve tek yonlu calismasi sinavin temel konularidir. Private subnet'ten disariya cikmak icin NAT, disaridan iceriye girmek icin load balancer veya bastion kullanilir. |
| 9 | Spot Instance olceklendirme sorunu cozer | **YANLIS.** Spot Instance'lar ucuzdur (%90'a kadar indirim) ancak **herhangi bir anda geri alinabilir** ve boot suresi aynidir. Kullanilabilirlik garantisi yoktur. | Spot, maliyet optimizasyonu icin mukemmeldir ancak "garantili kapasite" veya "kesintisiz calisma" gerektiren senaryolarda kullanilamaz. On-Demand veya Reserved tercih edin. |
| 10 | VPC Peering sinirsiz | **YANLIS.** Bir VPC'nin **maksimum 125 peering baglantisi** olabilir. Cok sayida VPC baglantisi gerekiyorsa **Transit Gateway** kullanilmalidir. | 10+ VPC baglanmasi gereken senaryolarda Transit Gateway hem yonetim kolayligi hem olceklenebilirlik saglar. Peering mesh topolojisi karmasiklasir. |
| 11 | DynamoDB = document database | **YANLIS.** DynamoDB bir **key-value** veritabanidir. Document database ihtiyaci icin **Amazon DocumentDB** (MongoDB uyumlu) kullanilmalidir. | DynamoDB partition key + sort key ile calisir. JSON belge sorgulama, ic ice gecmis yapilar ve esnek sema icin DocumentDB daha uygundur. |
| 12 | Kinesis duplicate uretmez | **YANLIS.** Producer veya consumer retry mekanizmalari nedeniyle Kinesis **duplicate kayitlar uretebilir**. Exactly-once isleme icin **SQS FIFO** tercih edilmelidir. | Kinesis at-least-once teslimat saglar. Idempotent islem tasarimi veya SQS FIFO ile exactly-once semantigi elde edilebilir. |
| 13 | Aurora = RDS ile ayni mimari | **YANLIS.** Aurora, **shared cluster volume** mimarisi kullanir: veriler **3 AZ'da 6 kopya** olarak saklanir. Replikalar hem okuma hem HA icin kullanilir. Standart RDS'ten cok farkli bir depolama katmani vardir. | Aurora, RDS'ten 5 kata kadar hizli (MySQL) ve 3 kata kadar hizli (PostgreSQL) olabilir. Failover suresi de cok daha kisadir. |
| 14 | HA = sifir kesinti | **YANLIS.** High Availability (HA) **kisa sureli kesintiye** izin verir (ornegin RDS failover ~60sn). Sifir kesinti = **Fault Tolerance (FT)**. | Sinavda "sifir kesinti" isteniyorsa FT mimarisi (aktif-aktif, multi-site) gerekir. "Minimum kesinti" isteniyorsa HA yeterlidir. |
| 15 | CAA Record = ag yonlendirme | **YANLIS.** CAA (Certificate Authority Authorization) kaydi, bir alan adi icin **hangi sertifika otoritelerinin sertifika verebilecegini** belirler. Ag yonlendirme icin A, AAAA, CNAME, Alias kayitlari kullanilir. | Route 53 soru turleri icinde CAA yaniltici olabilir. CAA = guvenlik/sertifika, diger kayitlar = trafik yonlendirme. |
| 16 | Bastion host ucretsiz | **YANLIS.** Bastion host ek bir **EC2 instance maliyeti** gerektirir. Ucretsiz alternatif olarak **AWS Systems Manager Session Manager** kullanilabilir. | Session Manager: port acmaya gerek yok, SSH key yonetimi yok, ek EC2 maliyeti yok, denetim (audit) icin CloudTrail entegrasyonu var. |
| 17 | Direct Connect ucuz | **YANLIS.** Direct Connect, **VPN'den cok daha pahalidir** (fiziksel hat, port ucreti, veri aktarim ucreti). Direct Connect; yuksek throughput, tutarli gecikme veya uyumluluk gerektiren senaryolarda tercih edilir. | "En uygun maliyetli" sorularinda VPN, "en yuksek performansli/en guvenli" sorularinda Direct Connect dogru cevaptir. |
| 18 | Gateway Endpoint tum servisler icin | **YANLIS.** Gateway Endpoint yalnizca **S3 ve DynamoDB** icin kullanilabilir. Diger AWS servisleri icin **Interface Endpoint (PrivateLink)** gereklidir. | Gateway Endpoint ucretsizdir (ayni Region icinde). Interface Endpoint saatlik + veri aktarim ucreti vardir. Maliyet sorularinda bu ayrimi bilin. |
| 19 | PrivateLink ALB ile kurulur | **YANLIS.** AWS PrivateLink, **Network Load Balancer (NLB)** gerektirir. ALB ile PrivateLink kurulamaz. | Servis saglayici tarafinda NLB, tuketici tarafinda Interface Endpoint olustururlur. Bu mimari soru kaliplari sinavda sikca cikar. |
| 20 | Aurora Serverless her zaman calisir | **YANLIS.** Aurora Serverless v2'de ACU (Aurora Capacity Unit) **sifira inebilir** ve veritabani **duraklatilabilir**. Yeniden baslatma birkacsaniye surabilir. | Gelistirme/test ortamlari veya dusuk/ongorulemez trafik icin idealdir. Uretim ortaminda minimum ACU ayarlamayi unutmayin. |

---

## Bolum 2: Sinavda Karistirilan Kavram Ciftleri

### 2.1 HA vs FT (High Availability vs Fault Tolerance)

| Ozellik | High Availability (HA) | Fault Tolerance (FT) |
|---------|----------------------|---------------------|
| **Tanim** | Kisa sureli kesinti kabul edilebilir | Sifir kesinti, surekli calisma |
| **Mekanizma** | Failover (yedege gecis) | Aktif-aktif, esanli calisma |
| **Maliyet** | Daha dusuk | Daha yuksek (cift kaynak) |
| **Ornek** | RDS Multi-AZ (failover ~60sn) | Multi-site aktif-aktif mimari |
| **Ne zaman** | Cogu uretim ortami icin yeterli | Kritik sistemler (finans, saglik) |

### 2.2 RTO vs RPO

| Ozellik | RTO (Recovery Time Objective) | RPO (Recovery Point Objective) |
|---------|------------------------------|-------------------------------|
| **Tanim** | Maksimum kabul edilebilir **kesinti suresi** | Maksimum kabul edilebilir **veri kaybi suresi** |
| **Soru** | "Sistem ne kadar surede ayaga kalkmali?" | "Ne kadarlik veri kaybedebiliriz?" |
| **Dusuk RTO** | Pilot Light, Warm Standby, Multi-Site | - |
| **Dusuk RPO** | - | Sik yedekleme, replikasyon, Multi-Site |
| **En dusuk her ikisi** | Multi-Site Active-Active (en pahali) | Multi-Site Active-Active (en pahali) |

### 2.3 Security Group vs NACL

| Ozellik | Security Group (SG) | Network ACL (NACL) |
|---------|---------------------|---------------------|
| **Seviye** | Instance (ENI) seviyesi | Subnet seviyesi |
| **Durum** | **Stateful** (donus trafigi otomatik) | **Stateless** (donus trafigi icin ayri kural) |
| **Varsayilan** | Tum gelen trafik engelli, tum giden acik | Tum trafik acik |
| **Kurallar** | Sadece ALLOW kurallari | ALLOW ve DENY kurallari |
| **Degerlendirme** | Tum kurallar birlikte degerlendirilir | Kural numarasina gore sirayla |
| **Ne zaman** | Instance bazli erisim kontrolu | Subnet bazli kara liste (IP engelleme) |

### 2.4 Scaling Up vs Scaling Out

| Ozellik | Scaling Up (Dikey) | Scaling Out (Yatay) |
|---------|-------------------|---------------------|
| **Tanim** | Daha buyuk instance kullan | Daha fazla instance ekle |
| **Ornek** | t3.micro -> t3.xlarge | 1 instance -> 5 instance |
| **Sinir** | Donanim siniri var | Neredeyse sinirsiz |
| **Kesinti** | Genellikle yeniden baslatma gerekir | Kesintisiz eklenebilir |
| **Ne zaman** | Veritabanlari (dikey olceklenmesi kolay) | Web sunuculari, stateless uygulamalar |

### 2.5 S3 Lifecycle vs Intelligent-Tiering

| Ozellik | S3 Lifecycle Policies | S3 Intelligent-Tiering |
|---------|----------------------|------------------------|
| **Mekanizma** | **Zamana dayali**, tum nesneler icin gecerli | **Erisim oruntusune dayali**, nesne bazinda |
| **Kontrol** | Siz kurallari tanimlarsiniz | AWS otomatik tasir |
| **Maliyet** | Tasima ucreti yok, depolama ucretleri degisir | Aylik izleme ucreti (nesne basina) |
| **Ne zaman** | Erisim kaliplari onceden biliniyorsa | Erisim kaliplari bilinmiyorsa veya degiskenese |
| **Ornek** | "30 gun sonra IA'ya, 90 gun sonra Glacier'a tasi" | "Hangi nesnelerin ne zaman erisilecegi belirsiz" |

### 2.6 EC2 Auto Scaling vs AWS Auto Scaling

| Ozellik | EC2 Auto Scaling | AWS Auto Scaling |
|---------|-----------------|------------------|
| **Kapsam** | Sadece **EC2 instance'lari** | **Birden fazla servis** (EC2, ECS, DynamoDB, Aurora) |
| **Ozellikler** | Launch config/template, scaling policies | Scaling plans, predictive scaling |
| **Ne zaman** | Sadece EC2 olceklendiriyorsaniz | Birden fazla servisi birlikte olceklendiriyorsaniz |

### 2.7 SQS Standard vs SQS FIFO

| Ozellik | SQS Standard | SQS FIFO |
|---------|-------------|----------|
| **Siralama** | En iyi cabayla siralama (garanti yok) | **Kesin FIFO siralama** |
| **Teslimat** | At-least-once (duplicate olabilir) | **Exactly-once** isleme |
| **Throughput** | Neredeyse sinirsiz | 300 msg/sn (batch ile 3000) |
| **Ne zaman** | Yuksek throughput, siralama onemsiz | Finansal islemler, siralama kritik |

### 2.8 DynamoDB On-Demand vs Provisioned

| Ozellik | On-Demand | Provisioned |
|---------|-----------|-------------|
| **Kapasite** | Otomatik olceklenir | RCU/WCU onceden belirlenir |
| **Maliyet** | Istek basina ucret (daha pahali) | Onceden ayrilmis kapasite (daha ucuz) |
| **Ne zaman** | Trafik kaliplari **bilinmiyorsa** veya degiskense | Trafik kaliplari **onceden tahmin edilebiliyorsa** |
| **Avantaj** | Sifir kapasite planlamasi | Maliyet optimizasyonu, Auto Scaling ile |

### 2.9 Lazy Loading vs Write-Through (Cache Stratejileri)

| Ozellik | Lazy Loading | Write-Through |
|---------|-------------|---------------|
| **Mekanizma** | Cache miss olursa DB'den oku, sonra cache'e yaz | Her yazmada hem DB'ye hem cache'e yaz |
| **Avantaj** | Sadece istenen veri cache'lenir, az bellek | Cache her zaman guncel |
| **Dezavantaj** | Ilk erisimde gecikme (cache miss), eski veri riski | Yazma gecikmesi, kullanilmayan veri de cache'lenir |
| **Ne zaman** | Okuma agirlikli, tum verinin cache'lenmesi gereksiz | Veri guncelliginin kritik oldugu durumlar |

---

## Bolum 3: Soru Okuma Stratejisi

### Adim 1: Anahtar Kelimeleri Isaretleyin

Sorunun kokunde (stem) su kelimeleri arayin ve altini cizip ona gore eleyip secin:

| Anahtar Kelime | Anlami | Yonlendirdigi Cevap |
|---------------|--------|---------------------|
| **"most cost-effective"** | En uygun maliyetli | Calisan ama pahali sesenekleri eleyin |
| **"least operational overhead"** | En az operasyonel yuk | Managed/serverless servisleri tercih edin |
| **"highest availability"** | En yuksek kullanilabilirlik | Multi-AZ, Multi-Region cozumleri |
| **"minimum latency"** | En dusuk gecikme | CloudFront, Global Accelerator, ElastiCache |
| **"most secure"** | En guvenli | Sifreleme, IAM, VPC endpoint, PrivateLink |
| **"least amount of changes"** | En az degisiklik | Mevcut mimariyi bozmayan cozum |
| **"real-time"** | Gercek zamanli | Kinesis, DynamoDB Streams, WebSocket |
| **"decouple"** | Ayristirma | SQS, SNS, EventBridge |
| **"durable"** | Dayaniikli | S3 (11 nines), cross-region replikasyon |

### Adim 2: Ne Soruldugunu Belirleyin

Her sorunun bir **ana odagi** vardir. Bunu belirlemek yanlis secenekleri hizla elemenizi saglar:

- **Maliyet mi?** En ucuz calisan cozumu secin (Spot, Reserved, S3 IA, Lambda)
- **Performans mi?** En hizli, en dusuk gecikmeli cozumu secin (ElastiCache, CloudFront, DAX)
- **Guvenlik mi?** En kisitlayici, en guvenli cozumu secin (sifreleme, IAM, VPC endpoint)
- **HA/FT mi?** En fazla yedeklilik saglayan cozumu secin (Multi-AZ, Multi-Region)
- **Olceklenebilirlik mi?** Otomatik buyuyen cozumu secin (Auto Scaling, serverless)

### Adim 3: Hemen Eleyin

Su secenekleri dogrudan eleyin:

- **Teknik olarak imkansiz olanlar** (ornegin: "S3'u EC2'ye mount et", "EFS'i Windows'ta kullan")
- **Var olmayan servis veya ozellikler** (ornegin: uydurma servis isimleri)
- **Senaryo ile uyumsuz olanlar** (ornegin: "serverless" istenen soruda EC2 bazli cozum)

### Adim 4: Kalan Secenekleri Karsilastirin

Eleme sonrasi genellikle 2 secenek kalir. Bunlari sorunun **ana odagina** gore karsilastirin:

- Her iki secenek de teknik olarak dogru olabilir
- Sorunun vurguladigi kriteri (maliyet, performans, guvenlik) one cikaran secenegi secin
- "En iyi" demek "tek dogru" demek degildir; **sorunun baglaminda en uygun** olan dogrudur

### Ozel Soru Tipleri Icin Stratejiler

**"Iki secenek secin" sorulari:**
- Her iki cevabin **birlikte tam bir cozum** olusturdugunu dogrulayin
- Birbirine alternatif olan iki secenegi isaretlemeyin; birbirini **tamamlayan** iki secenegi isaretleyin
- Ornek: "Yuksek kullanilabilirlik icin ne yapilmali?" -> Multi-AZ + Read Replica (birbirini tamamlar)

**"Hepsini secin" sorulari:**
- Her secenegi bagimsiz olarak degerlendirin
- Bir secenegin dogrulugu diger secenekleri etkilemez

**Senaryo bazli uzun sorular:**
- Son cumleyi (asil soruyu) once okuyun
- Sonra senaryoyu okuyarak gereksinimleri cikartin
- Zaman kazandirir ve odagi kaybetmezsiniz

---

## Bolum 4: Son Dakika Hatirlatalari (Quick Facts)

### Depolama

| Bilgi | Deger |
|-------|-------|
| S3 dayaniklilik (durability) | **11 nines** (99.999999999%) |
| S3 kapsam | **Globally resilient** (Region bazli ama dahili olarak 3+ AZ) |
| EBS kapsam | **Tek AZ** |
| EFS kapsam | **Multi-AZ** (Regional) |
| Gateway Endpoint destegi | Sadece **S3 + DynamoDB** |
| Gateway Endpoint ucreti | **Ucretsiz** (ayni Region icinde) |

### Veritabani

| Bilgi | Deger |
|-------|-------|
| Aurora replikasyon | **3 AZ, 6 kopya** |
| RDS Proxy failover | **%66 daha hizli** |
| DynamoDB gecikme | **Tek haneli milisaniye** (consistent) |
| DynamoDB DAX gecikme | **Mikrosaniye** |
| Aurora maks Read Replica | **15** |
| RDS maks Read Replica | **5** (Aurora haric) |

### Islem ve Ag

| Bilgi | Deger |
|-------|-------|
| Lambda maks calisma suresi | **15 dakika** |
| Lambda maks bellek | **10 GB** |
| VPC Peering maks | **125 peering/VPC** |
| ALB katmani | **Layer 7** (HTTP/HTTPS) |
| NLB katmani | **Layer 4** (TCP/UDP/TLS) |
| GLB katmani | **Layer 3** (IP) |
| NAT Gateway yonu | Sadece **egress** (outbound) |
| CloudFront Origin -> Edge aktarimi | **Ucretsiz** |

### Guvenlik ve Yonetim

| Bilgi | Deger |
|-------|-------|
| EC2 memory metrigi | **Custom** (varsayilan degil) |
| Session Manager | **Ucretsiz**, port acma yok, bastion yok |
| PrivateLink gereksinimi | **NLB** (ALB degil) |
| Spot Instance tasarruf | **%90'a kadar** (ancak kesintiye ugrayabilir) |
| Direct Connect vs VPN | DC = yuksek performans/maliyet, VPN = dusuk maliyet |

### DR (Disaster Recovery) Stratejileri (Ucuzdan Pahaliya)

| Strateji | RTO | RPO | Maliyet |
|----------|-----|-----|---------|
| Backup & Restore | Saatler | Saatler | En dusuk |
| Pilot Light | On dakikalar | Dakikalar | Dusuk |
| Warm Standby | Dakikalar | Saniyeler | Orta |
| Multi-Site Active-Active | Gercek zamanli | Sifira yakin | En yuksek |

---

## Son Soz

Sinav sirasinda su uc kurali aklinizan cikarmayin:

1. **Anahtar kelimeyi yakalayin** — Sorunun ne istedigini anlamadan cevap vermeyin
2. **Once eleyin** — 4 secenekten 2'sini hemen elemek basari oraninizi %50'den %100'e cikarir
3. **AWS'nin dusunce yapisini benimseyin** — AWS her zaman managed, serverless ve olceklenebilir cozumleri tercih eder

Basarilar!

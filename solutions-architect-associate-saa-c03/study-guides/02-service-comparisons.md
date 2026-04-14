# AWS SAA-C03 - Servis Karsilastirmalari

Bu rehber, sinavda sik karistirilan AWS servislerini karsilastirmali tablolarla ozetler.

---

## 1. Load Balancer Karsilastirmasi: ALB vs NLB vs GLB vs CLB

| Ozellik | ALB | NLB | GLB | CLB |
|---|---|---|---|---|
| **OSI Katmani** | Layer 7 (Application) | Layer 4 (Transport) | Layer 3 (Network) | Layer 4/7 (eski nesil) |
| **Protokol** | HTTP, HTTPS, gRPC | TCP, UDP, TLS | IP (GENEVE kapsulleme) | TCP, SSL, HTTP, HTTPS |
| **Performans** | Yuksek | Ultra dusuk gecikme, saniyede milyonlarca istek | Tum trafigi tek bir giris noktasindan yonetir | Dusuk-orta |
| **Hedef Tipi** | Instance, IP, Lambda | Instance, IP, ALB | Instance, IP | Instance |
| **Kullanim Alani** | Mikroservisler, container tabanli uygulamalar, HTTP yonlendirme | Oyun, IoT, finansal islemler, yuksek performans gerektiren durumlar | 3. parti guvenlik cihazlari (firewall, IDS/IPS) | Eski uygulamalar (kullanilmasi onerilmez) |
| **Cross-Region** | Hayir (hedefler tek bir region'da olmali) | Hayir (ancak Global Accelerator ile entegre olabilir) | Hayir | Hayir |
| **Sticky Sessions** | Evet | Evet | Hayir | Evet |
| **Maliyet** | Orta | Orta-yuksek | Orta | Dusuk |

**Sinav Ipuclari:**
- "HTTP path-based routing" veya "host-based routing" gorursen -> **ALB**
- "Ultra dusuk gecikme" veya "milyonlarca istek" gorursen -> **NLB**
- "3. parti guvenlik cihazi" veya "firewall" gorursen -> **GLB**
- "Eski uygulama" veya "Classic" gorursen -> **CLB**
- ALB hedefleri tek bir region'da olmak zorundadir

---

## 2. Storage Karsilastirmasi: EFS vs FSx for Windows vs FSx for Lustre vs S3 vs EBS

| Ozellik | EFS | FSx for Windows | FSx for Lustre | S3 | EBS |
|---|---|---|---|---|---|
| **Tip** | Paylasimli dosya sistemi | Paylasimli dosya sistemi | Yuksek performansli dosya sistemi | Nesne depolama | Blok depolama |
| **Protokol** | NFS (v4.0/4.1) | SMB + NTFS | Lustre | REST API (HTTP) | Block-level (EC2'ye bagli) |
| **OS Destegi** | Yalnizca Linux | Windows + Linux | Yalnizca Linux | OS bagimsiz (API uzerinden) | Linux + Windows |
| **Olcekleme** | Otomatik (petabyte'a kadar) | Yapilandirilabilir | Yapilandirilabilir | Sinirsiz | Manuel (volume buyutme) |
| **Coklu EC2 Erisimi** | Evet (binlerce instance) | Evet | Evet | Evet (API ile) | Hayir (tek AZ, tek instance*) |
| **Multi-AZ** | Evet (varsayilan) | Evet (Multi-AZ destegi) | Hayir (tek AZ) | Evet (otomatik 3+ AZ) | Hayir (tek AZ) |
| **Kullanim Alani** | Web sunucu icerigi, CMS, veri paylasimi | Windows tabanli uygulamalar, AD entegrasyonu, SharePoint | HPC, makine ogrenmesi, medya isleme | Yedekleme, statik web, data lake | Boot volume, veritabani, is yukleri |
| **Active Directory** | Hayir | Evet (yerel veya AWS Managed AD) | Hayir | Hayir | Hayir |

> *EBS io1/io2 Multi-Attach ozelligi ile ayni AZ'deki birden fazla instance'a baglanabilir ancak bu istisna bir durumdur.

**Sinav Ipuclari:**
- "Linux + NFS + paylasimli dosya" -> **EFS**
- "Windows + SMB + Active Directory" -> **FSx for Windows**
- "HPC + yuksek throughput + Linux" -> **FSx for Lustre**
- "S3, EC2'ye dosya sistemi olarak mount edilemez" (S3 Mountpoint harici genel kural)
- "Tek instance'a bagli blok depolama" -> **EBS**

---

## 3. Database Karsilastirmasi: RDS vs Aurora vs DynamoDB vs ElastiCache vs Redshift

| Ozellik | RDS | Aurora | DynamoDB | ElastiCache | Redshift |
|---|---|---|---|---|---|
| **Tip** | Iliskisel (SQL) | Iliskisel (SQL) | NoSQL (Key-Value / Document) | In-Memory cache | Veri ambari (Data Warehouse) |
| **Motor** | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server | MySQL, PostgreSQL uyumlu | Ozel (yonetilen) | Redis, Memcached | PostgreSQL tabanli |
| **Olcekleme** | Dikey (instance boyutu) + Read Replica | Otomatik storage (128 TB), 15 read replica | Otomatik yatay olcekleme (sinirsiz) | Dikey (node boyutu) + cluster | Node ekleme (resize) |
| **Replikasyon** | Multi-AZ (standby), Read Replica (5'e kadar) | 6 kopya / 3 AZ (otomatik), 15 read replica | Global Tables (multi-region), DAX cache | Cluster mode (Redis) | Cross-region snapshot |
| **Yuksek Erisilebilirlik** | Multi-AZ = yalnizca HA (okuma icin kullanilamaz) | Replikalar = hem okuma hem HA (otomatik failover) | Yerlesik (multi-AZ varsayilan) | Redis: Multi-AZ + otomatik failover | Multi-AZ destegi |
| **Gecikme** | Milisaniye | Milisaniye (RDS'den 5x hizli) | Tek haneli milisaniye | Mikro-saniye | Saniye (buyuk sorgular) |
| **Kullanim Alani** | Geleneksel uygulamalar, ERP, CRM | Yuksek performansli iliskisel is yukleri | Web/mobil, oyun, IoT, yuksek olcek | Oturum yonetimi, sorgu onbellekleme | OLAP, BI, raporlama, analitik |
| **Maliyet Modeli** | Instance saati + storage | Instance saati + I/O + storage | Istek basina (on-demand) veya kapasite (provisioned) | Node saati | Node saati + storage |

**Sinav Ipuclari:**
- RDS Multi-AZ = yalnizca HA, okuma icin kullanilamaz; Read Replica = performans + farkli region'da DR
- Aurora paylasilmis kumeleme depolamasi kullanir: 6 kopya, 3 AZ, otomatik onarim
- DynamoDB = tek haneli milisaniye, otomatik olcekleme, key-value -> sunucusuz NoSQL
- Redshift = "analitik", "veri ambari", "OLAP" anahtar kelimelerinde
- ElastiCache = "onbellekleme", "oturum yonetimi" anahtar kelimelerinde

---

## 4. Caching Karsilastirmasi: ElastiCache Redis vs Memcached vs DAX vs CloudFront

| Ozellik | ElastiCache Redis | ElastiCache Memcached | DAX | CloudFront |
|---|---|---|---|---|
| **Tip** | In-memory key-value store | In-memory key-value store | In-memory DynamoDB cache | CDN (Edge cache) |
| **Kalicilik (Persistence)** | Evet (snapshot, AOF) | Hayir | Hayir (yalnizca cache) | Hayir (TTL bazli) |
| **Kullanim Alani** | Genel amacli onbellekleme, leaderboard, pub/sub, oturum yonetimi | Basit onbellekleme, cok is parcacikli uygulamalar | Yalnizca DynamoDB okuma hizlandirma | Statik/dinamik icerik dagilimi, edge'de onbellekleme |
| **Replikasyon** | Evet (Multi-AZ, cluster mode) | Hayir | Multi-AZ (cluster) | Global edge network |
| **Protokol** | Redis protokolu | Memcached protokolu | DynamoDB API uyumlu | HTTP/HTTPS |
| **Veri Yapilari** | String, List, Set, Hash, Sorted Set | Yalnizca String (key-value) | DynamoDB item'lari | HTTP response'lari |
| **Yedekleme** | Evet | Hayir | Hayir | Hayir |

**Sinav Ipuclari:**
- "DynamoDB okuma performansi" veya "DynamoDB onbellekleme" -> **DAX** (baska servislerle calismaz)
- "Pub/sub", "leaderboard", "karmasik veri yapilari" -> **ElastiCache Redis**
- "Basit onbellekleme", "cok is parcacikli" -> **ElastiCache Memcached**
- "Global icerik dagilimi", "edge" -> **CloudFront**

---

## 5. Queue/Messaging Karsilastirmasi: SQS Standard vs SQS FIFO vs SNS vs EventBridge

| Ozellik | SQS Standard | SQS FIFO | SNS | EventBridge |
|---|---|---|---|---|
| **Tip** | Mesaj kuyrugu | Sirali mesaj kuyrugu | Bildirim servisi (pub/sub) | Olay yolu (event bus) |
| **Teslimat Garantisi** | En az bir kez (at-least-once) | Tam olarak bir kez (exactly-once) | En az bir kez | En az bir kez |
| **Siralama** | Garanti edilmez | FIFO (ilk giren ilk cikar) garantili | Garanti edilmez | Garanti edilmez |
| **Throughput** | Neredeyse sinirsiz | 300 msg/s (batch ile 3000) | Neredeyse sinirsiz | Neredeyse sinirsiz |
| **Mesaj Saklama** | 14 gune kadar | 14 gune kadar | Saklanmaz (anlik teslimat) | Saklanmaz (replay destegi ile sinirli) |
| **Tuketici Modeli** | Polling (pull) | Polling (pull) | Push (abonelere gonderm) | Push (kurallara gore yonlendirme) |
| **Kullanim Alani** | Is yuklerini ayristirma, tamponlama | Finansal islemler, siparis isleme, siralama gereken durumlar | Bildirim gonderme (e-posta, SMS, Lambda, SQS) | Karmasik olay yonlendirme, SaaS entegrasyonu, 3. parti event'ler |
| **Filtreleme** | Yok | Yok | Mesaj filtreleme (attribute bazli) | Gelismis kural tabanli filtreleme (100+ kaynak) |

**Sinav Ipuclari:**
- "Tam olarak bir kez isleme" veya "siralama garantisi" -> **SQS FIFO**
- "Birden fazla aboneye ayni anda gonder" -> **SNS** (fan-out pattern: SNS -> SQS)
- "3. parti SaaS olaylari" veya "karmasik olay yonlendirme" -> **EventBridge**
- SNS kuyruk degildir, mesajlari saklamaz; SQS bildirim servisi degildir
- Fan-out pattern: SNS topic -> birden fazla SQS kuyrugu

---

## 6. Kinesis Karsilastirmasi: Data Streams vs Firehose vs Analytics vs Video Streams

| Ozellik | Kinesis Data Streams | Kinesis Data Firehose | Kinesis Data Analytics | Kinesis Video Streams |
|---|---|---|---|---|
| **Amac** | Gercek zamanli veri akisi toplama | Veri akisini AWS hedeflerine teslim etme | Akan veri uzerinde SQL/Flink ile analiz | Video akisi toplama ve isleme |
| **Gercek Zamanlilik** | Gercek zamanli (~200ms) | Yakin gercek zamanli (60s tamponlama) | Gercek zamanli | Gercek zamanli |
| **Teslim Hedefi** | Ozel tuketici (Lambda, EC2, vb.) | S3, Redshift, OpenSearch, Splunk, HTTP | Kinesis Data Streams, Firehose | Ozel tuketici, Rekognition |
| **Veri Donusumu** | Yok (tuketici tarafinda) | Lambda ile basit donusum | SQL veya Apache Flink ile gelismis donusum | Yok |
| **Veri Saklama** | 24 saat - 365 gun | Saklamaz (dogrudan teslim) | Saklamaz | 24 saat - 7 gun |
| **Olcekleme** | Shard yonetimi (manuel veya on-demand) | Otomatik | Otomatik | Otomatik |
| **Duplikasyon Riski** | Var (en az bir kez teslimat) | Yok (en az bir kez ama hedefe bir kez) | Var | Dusuk |
| **Yonetim Yukunuz** | Orta (shard yonetimi) | Dusuk (tam yonetilen) | Dusuk | Dusuk |

**Sinav Ipuclari:**
- "Gercek zamanli veri toplama" + ozel islem gerekiyor -> **Kinesis Data Streams**
- "Veriyi S3/Redshift'e en kolay sekilde gonder" -> **Kinesis Data Firehose** (en basit yol)
- "Akan veri uzerinde SQL sorgusu" -> **Kinesis Data Analytics**
- Firehose = "near real-time", Data Streams = "real-time"

---

## 7. Baglanti Karsilastirmasi: VPN vs Direct Connect vs VPC Peering vs Transit Gateway vs PrivateLink

| Ozellik | Site-to-Site VPN | Direct Connect | VPC Peering | Transit Gateway | PrivateLink |
|---|---|---|---|---|---|
| **Baglanti Tipi** | Sifrelenmis tunel (internet uzerinden) | Ozel fiziksel baglanti | VPC'ler arasi dogrudan baglanti | Merkezi hub (hub-spoke) | Ozel endpoint (NLB uzerinden) |
| **Kurulum Suresi** | Dakikalar | Haftalar-aylar | Dakikalar | Dakikalar | Dakikalar |
| **Bant Genisligi** | Internet hizina bagli (~1.25 Gbps) | 1 Gbps, 10 Gbps, 100 Gbps | VPC limitleri dahilinde | 50 Gbps (VPN ekleme ile) | VPC limitleri dahilinde |
| **Sifreleme** | Evet (IPSec) | Hayir (varsayilan), VPN ile eklenebilir | Hayir | VPN attachment ile evet | Hayir (AWS ag ici) |
| **Maliyet** | Dusuk | Yuksek (port + veri cikisi) | Dusuk (veri transferi ucreti) | Orta (saat + veri) | Orta (saat + veri) |
| **Sinirlamalar** | Internet gecikme degiskenligine bagli | Fiziksel kurulum gerekli | Transitif degil, max ~125 peering | 5000 VPC baglantisi | Servise ozel, NLB gerektirir |
| **Kullanim Alani** | Hizli ve ucuz hibrit baglanti | Tutarli yuksek bant genisligi, uyumluluk gereksinimleri | Az sayida VPC arasi iletisim | Cok sayida VPC + on-premises merkezi yonetim | Belirli bir servisi ozel ag uzerinden acma |

**Sinav Ipuclari:**
- "Hizli kurulum + sifrelenmis baglanti" -> **VPN**
- "Tutarli gecikme + yuksek bant genisligi + ozel hat" -> **Direct Connect**
- "Cok sayida VPC yonetimi" veya "hub-spoke" -> **Transit Gateway**
- VPC Peering transitif degildir (A-B ve B-C varsa A-C otomatik bagli degil)
- PrivateLink = bir servisi NLB + Interface Endpoint uzerinden ozel ag'da sunma

---

## 8. Compute Karsilastirmasi: EC2 vs Lambda vs Fargate vs ECS+EC2

| Ozellik | EC2 | Lambda | Fargate | ECS + EC2 |
|---|---|---|---|---|
| **Tip** | Sanal makine | Sunucusuz fonksiyon | Sunucusuz container | Yonetilen container (EC2 uzerinde) |
| **Olcekleme** | Manuel/ASG (otomatik degil, yapilandirilmali) | Otomatik (erisken olcekleme, yerlesik) | Otomatik (gorev bazli) | ASG ile yapilandirilmali |
| **Maks. Calisma Suresi** | Sinirsiz | 15 dakika | Sinirsiz | Sinirsiz |
| **Yonetim Yuku** | Yuksek (OS, yama, guvenlik) | Cok dusuk (yalnizca kod) | Dusuk (container image) | Orta (EC2 fleet yonetimi) |
| **Maliyet Modeli** | Saat/saniye bazli (her zaman calisir) | Istek + sure bazli (bosta maliyet yok) | vCPU + bellek (gorev suresi) | EC2 maliyeti + ECS ucretsiz |
| **Kullanim Alani** | Geleneksel uygulamalar, ozel yapilandirma | Olay tabanli isler, API backend, kisa sureli islemler | Mikroservisler, container is yukleri (yonetim istemeyenler) | Buyuk olcekli container is yukleri (maliyet optimizasyonu) |
| **State** | Stateful | Stateless | Stateless/Stateful (EFS ile) | Stateful |

**Sinav Ipuclari:**
- Lambda max 15 dakika; daha uzun islemler icin -> **Step Functions** (Lambda orkestrasyon) veya **EC2/Fargate**
- "Sunucusuz" + "container" -> **Fargate**
- "Yama yonetimi istemiyorum" + "kisa sureli islem" -> **Lambda**
- EC2 olcekleme yerlesik degildir, ASG yapilandirilmalidir
- Lambda yerlesik olarak otomatik olceklenir (0'dan binlerce esamanli calismaya)

---

## 9. EC2 Fiyatlandirma Karsilastirmasi

| Ozellik | On-Demand | Reserved Instance | Savings Plans | Spot | Dedicated Host |
|---|---|---|---|---|---|
| **Taahhut** | Yok | 1 veya 3 yil | 1 veya 3 yil | Yok | On-Demand veya rezervasyon |
| **Indirim Orani** | %0 (referans fiyat) | %72'ye kadar | %72'ye kadar | %90'a kadar | Degisken |
| **Kesinti Riski** | Yok | Yok | Yok | Yuksek (2 dakika uyari ile geri alinabilir) | Yok |
| **Esneklik** | Tam esneklik | Sinirli (instance tipi, region, OS) | Yuksek (EC2, Fargate, Lambda arasi) | Instance tipi, AZ secimi | Fiziksel sunucu seviyesinde kontrol |
| **Kullanim Alani** | Kisa sureli, tahmin edilemeyen is yukleri | Kararli, surekli calisan is yukleri | Kararli is yukleri + servisler arasi esneklik | Toplu islem, CI/CD, esnek is yukleri | Lisans uyumlulugu (BYOL), mevzuat gereksinimleri |
| **Odeme Secenekleri** | Kullandikca ode | Hepsi pesinat / kismi / odeme yok | Saatlik taahhut | Piyasa fiyati (degisken) | Saat bazli veya rezervasyon |

**Sinav Ipuclari:**
- "Kesintiye toleransli is yuku" + "maliyet optimizasyonu" -> **Spot**
- "Kararli is yuku" + "1-3 yil taahhut" -> **Reserved Instance** veya **Savings Plans**
- "Lisans uyumlulugu" veya "fiziksel sunucu gereksinimi" -> **Dedicated Host**
- Savings Plans, Reserved'dan daha esnektir (EC2, Fargate, Lambda'da gecerli)
- Spot Instance'lar 2 dakika onceden bildirim ile geri alinabilir

---

## 10. S3 Depolama Siniflari Karsilastirmasi

| Sinif | Depolama Maliyeti | Erisim Maliyeti | Min. Saklama | Erisim Suresi | Kullanim Alani |
|---|---|---|---|---|---|
| **S3 Standard** | En yuksek | En dusuk | Yok | Milisaniye | Sik erisilenler, aktif veri |
| **S3 Standard-IA** | Dusuk | Orta | 30 gun | Milisaniye | Seyrek erisilenler ama hizli erisim gerekli |
| **S3 One Zone-IA** | Daha dusuk | Orta | 30 gun | Milisaniye | Yeniden uretilebilir, tek AZ yeterli |
| **S3 Intelligent-Tiering** | Degisken (otomatik) | Izleme ucreti (nesne basi) | Yok | Milisaniye | Erisim deseni tahmin edilemeyen veriler |
| **Glacier Instant Retrieval** | Cok dusuk | Yuksek | 90 gun | Milisaniye | Ceyrek bazinda erisilenler, aninda erisim |
| **Glacier Flexible Retrieval** | Cok dusuk | Yuksek | 90 gun | 1 dk - 12 saat | Arsiv, yilda birirkac kez erisim |
| **Glacier Deep Archive** | En dusuk | En yuksek | 180 gun | 12 - 48 saat | Uzun sureli arsiv, mevzuat uyumlulugu |

**Depolama Maliyeti vs Erisim Maliyeti Dengesi:**
```
Depolama Maliyeti:  Standard > Standard-IA > One Zone-IA > Intelligent-Tiering > Glacier Instant > Glacier Flexible > Deep Archive
Erisim Maliyeti:    Standard < Standard-IA < One Zone-IA < Glacier Instant < Glacier Flexible < Deep Archive
```

**Sinav Ipuclari:**
- **Lifecycle Rules** = zamana dayali kurallar (orn. 30 gun sonra IA'ya tasi, 90 gun sonra Glacier'a)
- **Intelligent-Tiering** = nesne bazinda otomatik tasima (erisim desenini izler, her nesne icin bagimsiz karar verir)
- "Aninda erisim + dusuk maliyet + seyrek erisim" -> **Glacier Instant Retrieval**
- One Zone-IA tek AZ'de saklanir; AZ kaybi durumunda veri kaybedilebilir

---

## 11. DR (Felaket Kurtarma) Strateji Karsilastirmasi

| Strateji | RTO | RPO | Maliyet | Aciklama |
|---|---|---|---|---|
| **Backup & Restore** | Saatler | Saatler | En dusuk | Verileri yedekle, felaket durumunda yeniden olustur. S3, snapshot, AMI kullanilir. |
| **Pilot Light** | On dakikalar | Dakikalar | Dusuk | Cekirdek altyapi her zaman calisir (orn. veritabani replikasyonu). Diger kaynaklar felaket durumunda baslatilir. |
| **Warm Standby** | Dakikalar | Dakikalar | Orta | Tam calisir ortamin kucultulmus bir kopyasi her zaman aktif. Felaket durumunda olceklenir. |
| **Active/Active (Multi-Site)** | Saniyeler-dakikalar | Neredeyse sifir | En yuksek | Her iki region tam kapasitede calisir. Route 53 ile trafik yonlendirilir. |

**Gorsel Karsilastirma:**
```
Maliyet:    Backup & Restore < Pilot Light < Warm Standby < Active/Active
                   ^                                              ^
               En dusuk maliyet                           En yuksek maliyet

RTO/RPO:    Active/Active < Warm Standby < Pilot Light < Backup & Restore
                   ^                                              ^
               En dusuk RTO/RPO                           En yuksek RTO/RPO
```

**Sinav Ipuclari:**
- Soldan saga: maliyet artar, RTO/RPO azalir
- "En dusuk maliyet DR" -> **Backup & Restore**
- "Neredeyse sifir kesinti" -> **Active/Active**
- Pilot Light ve Warm Standby arasindaki fark: Pilot Light yalnizca cekirdek (veritabani), Warm Standby ise kucultulmus tam ortam

---

## 12. VPC Endpoint Karsilastirmasi: Gateway vs Interface

| Ozellik | Gateway Endpoint | Interface Endpoint |
|---|---|---|
| **Desteklenen Servisler** | Yalnizca S3 ve DynamoDB | Diger tum AWS servisleri (100+) |
| **Maliyet** | Ucretsiz (ayni region icinde) | Saat bazli + veri isleme ucreti |
| **Calisma Sekli** | Route table'a eklenir | ENI (Elastic Network Interface) olusturur |
| **Teknoloji** | Route table girisi | AWS PrivateLink |
| **DNS** | Varsayilan DNS kullanir | Ozel DNS adi olusturur |
| **Guvenlik** | VPC Endpoint Policy | VPC Endpoint Policy + Security Group |
| **Cross-Region** | Hayir | Hayir |

**Sinav Ipuclari:**
- "S3'e veya DynamoDB'ye ozel ag uzerinden erisim" + "ucretsiz" -> **Gateway Endpoint**
- Diger tum AWS servislerine ozel ag erisimi -> **Interface Endpoint (PrivateLink)**
- Gateway Endpoint yalnizca ayni region'daki S3/DynamoDB icin gecerlidir
- Interface Endpoint bir ENI olusturur, bu nedenle Security Group uygulanabilir

---

## 13. Route 53 Yonlendirme Politikalari

| Politika | Aciklama | Kullanim Alani |
|---|---|---|
| **Simple** | Tek bir kaynak dondurur (birden fazla IP verebilir, rastgele secilir) | Tek sunuculu basit web siteleri |
| **Weighted** | Trafigi belirlenen agirliklara gore dagitir (orn. %70-%30) | A/B testi, kademeli gecis (blue/green deployment) |
| **Latency-based** | En dusuk gecikmeye sahip region'a yonlendirir | Global uygulamalarda en iyi performans |
| **Failover** | Birincil saglikli degilse ikincile yonlendirir (active-passive) | Felaket kurtarma, yuksek erisilebilirlik |
| **Geolocation** | Kullanicinin cografi konumuna gore yonlendirir (ulke/kitabasinda) | Icerik yerellistirme, yasal kisitlamalar |
| **Geoproximity** | Kaynaklarin cografi konumuna gore yonlendirir, bias ile ayarlanabilir | Trafigi belirli region'lara kaydirma (bias artir/azalt) |
| **Multivalue Answer** | Birden fazla sagliklari deger dondurur (8'e kadar) | Basit DNS seviyesinde yuk dengeleme (LB yerine gecmez) |

**Sinav Ipuclari:**
- "A/B testi" veya "kademeli gecis" -> **Weighted**
- "En yakin region" veya "en dusuk gecikme" -> **Latency-based**
- "Kullanici ulkesine gore" -> **Geolocation**
- "Failover" veya "DR" -> **Failover** (health check ile birlikte)
- Geolocation vs Geoproximity: Geolocation = kullanici konumu, Geoproximity = kaynak konumu + bias
- Multivalue = health check'li Simple; ancak gercek bir load balancer degildir

---

## Hizli Referans Tablosu - Sinav Anahtar Kelimeleri

| Anahtar Kelime | Servis |
|---|---|
| "Sunucusuz / Serverless" | Lambda, Fargate, DynamoDB, S3, Aurora Serverless |
| "Gercek zamanli / Real-time" | Kinesis Data Streams |
| "Yakin gercek zamanli / Near real-time" | Kinesis Data Firehose |
| "Tek haneli milisaniye / Single-digit ms" | DynamoDB |
| "Mikro-saniye / Microsecond" | ElastiCache, DAX |
| "Veri ambari / Data warehouse" | Redshift |
| "ETL" | Glue |
| "Tam yonetilen / Fully managed" | Genellikle sunucusuz secenek |
| "Ayristirma / Decoupling" | SQS, SNS, EventBridge |
| "Fan-out" | SNS -> SQS |
| "FIFO + exactly-once" | SQS FIFO |
| "Hub-spoke ag" | Transit Gateway |
| "Ozel hat / Dedicated line" | Direct Connect |
| "Lisans uyumlulugu / BYOL" | Dedicated Host |
| "Kesintiye toleransli / Fault-tolerant" | Spot Instance |
| "HPC" | FSx for Lustre, Placement Group (Cluster) |
| "Windows dosya paylasimi / SMB" | FSx for Windows |
| "Linux dosya paylasimi / NFS" | EFS |

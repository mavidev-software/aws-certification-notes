# AWS SAA-C03 Senaryo-Cevap Eslestirme Rehberi

> **"Soruda X goruyorsan, Y dusun"** formatinda hizli basvuru rehberi.
> Sinavda zaman kazanmak icin bu kaliplari ezberlemek kritik oneme sahiptir.

---

## 1. Depolama (Storage) Senaryolari

| Soruda Gordugunuz Ifade | Dusunulecek Cozum | Neden |
|---|---|---|
| Windows + paylasimli dosya sistemi + Active Directory | **FSx for Windows File Server** | SMB protokolu + AD entegrasyonu sadece FSx for Windows'ta var |
| Linux + paylasimli dosya sistemi + NFS | **EFS (Elastic File System)** | EFS, NFS v4 protokolunu destekler ve birden fazla EC2'ye ayni anda baglanir |
| HPC + Linux + Lustre | **FSx for Lustre** | Yuksek performansli hesaplama icin optimize edilmis paralel dosya sistemi |
| Nesne depolama + sinirsiz kapasite | **S3** | S3 nesne bazli depolama sunar, kapasite limiti yoktur |
| Dusuk gecikme + blok depolama + yuksek IOPS | **EBS (Elastic Block Store)** | EBS, EC2'ye bagli blok seviyesinde depolama saglar; io2 Block Express ile 256.000 IOPS'a kadar cikar |
| Hibrit depolama (on-prem + bulut) | **Storage Gateway** | On-prem uygulamalarin S3/EBS/Glacier'a erisimini saglayan kopru gorevi gorur |
| Otomatik olceklenen dosya sistemi | **EFS** (EBS degil!) | EFS otomatik buyur/kuculur; EBS icin manuel olarak boyut degistirmek gerekir |
| EFS + Windows | **YANLIS SECENEK** | EFS sadece Linux (NFS) destekler. Windows icin FSx for Windows kullanilmali |
| S3'u EC2'ye mount etme | **YANLIS SECENEK** | S3 bir nesne deposudur, blok depolama gibi mount edilemez. S3 Mountpoint sinirli erisim saglar ama sinav baglaminda bu secenegi isaretlemeyin |

---

## 2. Islem Gucu (Compute) Senaryolari

| Soruda Gordugunuz Ifade | Dusunulecek Cozum | Neden |
|---|---|---|
| Altyapi yonetimi yok + mikroservisler | **Lambda + API Gateway** | Sunucusuz mimari: altyapi yonetimi AWS'e kalir, sadece kodu yazarsiniz |
| Islem suresi 15 dakikayi asiyor | **Step Functions** | Lambda max 15 dk calisir; Step Functions ile is akislarini parcalayip zincirleyebilirsiniz |
| Sunucusuz konteynerler | **Fargate** | EC2 yonetimi olmadan konteyner calistirmanin yolu Fargate'tir |
| Konteyner + tam kontrol | **ECS + EC2** | EC2 launch type ile isletim sistemi, GPU, ozel yapilandirma uzerinde tam kontrol sahibi olursunuz |
| Global dusuk gecikme + Lambda | **Lambda@Edge** | CloudFront kenar noktalarinda kod calistirir, kullaniciya en yakin lokasyonda islem yapar |
| Toplu isleme + kesinti tolere edilebilir | **Spot Instances** | Normal fiyatin %90'a kadar tasarruf saglar; AWS istediginde geri alabilir |
| Esnek tasarruf + EC2/Fargate/Lambda | **Savings Plans** | Compute Savings Plans tum islem turleri icin gecerlidir; Reserved Instance'tan daha esnektir |
| Lisans gereksinimleri + fiziksel sunucu | **Dedicated Hosts** | Fiziksel sunucu tahsisi saglar; soket/cekirdek bazli lisanslar (Oracle, SQL Server) icin gereklidir |
| Degisken trafik + yuksek erisilebilirlik + web uygulamasi | **EC2 + ELB + Auto Scaling** | ELB trafigi dagitir, Auto Scaling talebe gore olceklenir, coklu AZ ile HA saglanir |

---

## 3. Veritabani (Database) Senaryolari

| Soruda Gordugunuz Ifade | Dusunulecek Cozum | Neden |
|---|---|---|
| Tek haneli milisaniye gecikme + yuksek hacim + NoSQL | **DynamoDB** | Tam yonetilen NoSQL; herhangi bir olcekte tutarli tek haneli ms yanit suresi |
| Anahtar-deger veritabani | **DynamoDB** | DynamoDB'nin temel veri modeli anahtar-deger ciftidir |
| Bolgesel yedekleme + iliskisel veritabani | **Aurora Global Database** | 5 bolgeye kadar 1 saniyeden kisa gecikmeyle replikasyon yapar; bolgesel felaket kurtarma saglar |
| Lambda + RDS baglanti sorunu | **RDS Proxy** | Baglanti havuzlama yapar; Lambda'nin her cagrilisinda yeni baglanti acma sorununu cozer |
| Duzensize/tahmin edilemez is yuku + iliskisel + maliyet | **Aurora Serverless** | Kullanim olmadigi zaman kapanabilir; otomatik olceklenir, sadece kullandiginiz kadar odersiniz |
| Okuma agirlikli trafik + maliyet optimizasyonu | **Read Replica veya ElastiCache** | Olcek buyutme (scale-up) yerine yatay olcekleme; Read Replica okuma yukunu dagitir, ElastiCache onbellekler |
| RDS Multi-AZ | **Sadece HA (Yuksek Erisilebilirlik)** | Multi-AZ okuma performansini artirmaz! Standby kopya sadece yedekleme icindir |
| DynamoDB onbellekleme | **DAX** (ElastiCache degil!) | DAX, DynamoDB icin ozel olarak tasarlanmis bir onbellek servisidir; kod degisikligi minimumda |
| Erisim deseni bilinmiyor + DynamoDB | **On-Demand kapasite modu** | Provizyone kapasite gerektirmez; kullandiginiz kadar odersiniz, tahmin yapmaniz gerekmez |
| Veri ambari (data warehouse) | **Redshift** | Petabayt olceginde analitik sorgular icin optimize edilmis sutunsal veritabani |
| Belge veritabani (document database) | **DocumentDB** | MongoDB uyumlu belge veritabani; anahtar-deger (DynamoDB) ile karistirmayin |

---

## 4. Ag (Network) Senaryolari

| Soruda Gordugunuz Ifade | Dusunulecek Cozum | Neden |
|---|---|---|
| Ozel alt ag (private subnet) → S3 erisimi + maliyet | **Gateway VPC Endpoint** (ucretsiz) | Internet cikisi gerektirmez, NAT Gateway maliyetini ortadan kaldirir, tamamen ucretsiz |
| VPC'ler arasi ozel erisim, internet yok | **PrivateLink** | AWS omurgasi uzerinden ozel baglanti; trafik internete cikmaz |
| On-prem + AWS, yuksek bant genisligi | **Direct Connect** | Adanmis fiziksel hat; tutarli performans ve yuksek bant genisligi saglar |
| On-prem + AWS, maliyet oncelikli | **Site-to-Site VPN** | Internet uzerinden sifrelenmis tunel; Direct Connect'ten cok daha ucuz |
| Direct Connect yedegi + minimum maliyet | **Site-to-Site VPN** | VPN, Direct Connect'in en ucuz yedek cozumudur; ikinci bir DX hatti pahalidir |
| Birden fazla VPC'yi merkezi bir noktadan yonetme | **Transit Gateway** | Hub-spoke topolojisi; onlarca/yuzlerce VPC'yi tek noktadan birbirine baglar |
| 200+ VPC + paylasimli servis | **PrivateLink** | VPC Peering limiti 125'tir; PrivateLink bu limiti asar ve olceklenebilir |
| Kullaniciyi en yakin bolgeye yonlendirme | **Route 53 Geoproximity** | Kullanicinin cografi konumuna gore en yakin AWS bolgesine yonlendirir |
| En dusuk gecikme suresiyle yonlendirme | **Route 53 Latency-based** | Gercek ag gecikme olcumlerine dayanarak en hizli bolgeye yonlendirir |
| DDoS korumasi + API | **API Gateway** | Katman 3 + Katman 7 korumasi saglar; throttling, WAF entegrasyonu vardir |
| Ozel sunucuya guvenli erisim + maliyet | **Session Manager** (ucretsiz) | SSH anahtari ve bastion host gerektirmez; tamamen ucretsiz ve denetlenebilir |
| NAT Gateway uzerinden sunucuya baglanma | **IMKANSIZ** | NAT Gateway sadece dis yonlu (egress) trafik icindir; iceriye baglanti kabul etmez |

---

## 5. Olcekleme ve Erisilebilirlik (Scaling & Availability) Senaryolari

| Soruda Gordugunuz Ifade | Dusunulecek Cozum | Neden |
|---|---|---|
| Tekrarlanan / birden fazla kez islenen mesajlar | **SQS FIFO** | Tam olarak bir kez teslim (exactly-once) garantisi verir; standart SQS en az bir kez (at-least-once) teslim eder |
| Web katmani + isleme katmanini ayirma (decouple) | **SQS + ozel kuyruk metrigi** | SQS katmanlari birbirinden ayirir; ozel metrik ile kuyruk derinligine gore olcekleme yapilir |
| Kendini onaran mimari (self-healing) | **ELB + Auto Scaling** | ELB saglik kontrolu basarisiz olan instance'lari tespit eder, Auto Scaling yenisini baslatir |
| Bolgeler arasi yedekleme (cross-region failover) | **Route 53 Failover Routing** | Birincil bolge cokerse trafigi otomatik olarak yedek bolgeye yonlendirir |
| Tek hata noktasi (single point of failure) | **Hatadan geriye dogru dusun** | Sorudaki tek noktayi bulun ve onu yedekli/dagitik hale getirin (Multi-AZ, ELB, vb.) |
| Bellek tabanli olcekleme | **Ozel CloudWatch metrigi** | Bellek (memory) varsayilan CloudWatch metriklerinde yoktur! CloudWatch Agent ile ozel metrik olusturulmalidir |
| Durumlu uygulama + disk | **EFS veya FSx** | Paylasimli dosya sistemi sayesinde birden fazla instance ayni veriye erisir |
| Durumlu uygulama + bellekte oturum bilgisi | **ALB Sticky Sessions** | Ayni kullaniciyi ayni instance'a yonlendirir; oturum bilgisi kaybolmaz |

---

## 6. Maliyet Optimizasyonu Senaryolari

| Soruda Gordugunuz Ifade | Dusunulecek Cozum | Neden |
|---|---|---|
| En uygun depolama sinifi + erisim deseni bilinmiyor | **S3 Intelligent-Tiering** | Erisim desenini otomatik analiz eder ve verileri uygun katmana tasir |
| Zamana dayali arsivleme | **S3 Lifecycle Policies** | Belirli gun sonra verileri otomatik olarak ucuz katmanlara (Glacier, Deep Archive) tasir |
| Baglanmamis EBS hacimleri | **Trusted Advisor + sil** | Trusted Advisor kullanilmayan kaynaklari tespit eder; baglanmamis EBS gereksiz maliyettir |
| IOPS icin fazla odeme | **Hacim tipini degistir (io → gp)** | gp3, cogu is yuku icin yeterli IOPS saglar ve io1/io2'den cok daha ucuzdur |
| 250 TB on-prem veri → S3 aktarimi | **Snowball** | Internet uzerinden haftalar surer; Snowball fiziksel cihazla gunler icerisinde aktarir |
| Gelistirme ortami NAT maliyeti | **Tek paylasimli NAT Gateway** | Her ortam icin ayri NAT Gateway yerine tek bir NAT Gateway paylasimli kullanilir |
| Gece bosta kalan EC2 | **Auto Scaling zamanlanmis olcekleme** | Scheduled Action ile mesai saatleri disinda instance sayisini sifira indirir |
| Maliyet gorunurlugu | **Cost Allocation Tags + Cost Explorer** | Etiketlerle maliyetleri proje/takim/ortam bazinda ayirir; Cost Explorer gorsellestirir |

---

## 7. Veri ve Analitik (Data & Analytics) Senaryolari

| Soruda Gordugunuz Ifade | Dusunulecek Cozum | Neden |
|---|---|---|
| Gercek zamanli akim verisi toplama (streaming ingestion) | **Kinesis Data Streams** | Milisaniye gecikmeyle gercek zamanli veri akisi; tuketici uygulamalar veriyi aninda isler |
| Akim verisini AWS'e yuklemenin en basit yolu | **Kinesis Data Firehose** | Tamamen yonetilen servis; kod yazmadan S3, Redshift, OpenSearch'e veri aktarir |
| Akim verisi donusturme (stream transformation) | **Kinesis Data Analytics** | Akan veri uzerinde SQL veya Apache Flink ile gercek zamanli analiz yapar |
| S3 uzerinde SQL sorgusu | **Athena** | Sunucusuz; S3'teki verileri dogrudan SQL ile sorgular, altyapi yonetimi yoktur |
| ETL + veri kesfi (data discovery) | **Glue** | Sunucusuz ETL servisi; Glue Crawler verileri otomatik kesfeder ve kataloglar |
| Buyuk veri Hadoop/Spark | **EMR (Elastic MapReduce)** | Yonetilen Hadoop/Spark kumesi; buyuk olcekli veri isleme icin optimize |
| Veri golu (data lake) | **S3** | Ucuz, sinirsiz, dayanimli depolama; tum analitik servislerin merkezi veri kaynagi |
| Buyuk fiziksel veri aktarimi | **Snow Family** | Snowcone (8-14 TB), Snowball Edge (80 TB), Snowmobile (100 PB); ag yerine fiziksel tasima |
| PII uyumlulugu + donanim tabanli sifreleme | **CloudHSM** | FIPS 140-2 Level 3 sertifikali donanim; sifreleme anahtarlari tamamen sizin kontrolunuzde |
| Birden fazla veri kaynagi + GraphQL | **AppSync** | Yonetilen GraphQL servisi; DynamoDB, Lambda, RDS gibi kaynaklari tek bir API'de birlestirir |

---

## Hizli Hatirlatma: Sik Karistirilan Kavramlar

| Kavramsallik | Dogru | Yanlis |
|---|---|---|
| EFS isletim sistemi destegi | Sadece Linux | Windows destekler |
| RDS Multi-AZ amaci | Yuksek erisilebilirlik (HA) | Okuma performansi artirma |
| DynamoDB onbellegi | DAX | ElastiCache |
| S3'e EC2'den erisim | SDK/CLI ile API cagrisi | Mount ederek dosya sistemi gibi |
| NAT Gateway yonu | Sadece disari (egress) | Disaridan iceri baglanti |
| Bellek metrigi CloudWatch'ta | Ozel metrik gerekir | Varsayilan olarak mevcut |
| Gateway VPC Endpoint | S3 ve DynamoDB | Tum AWS servisleri |

---

> **Ipucu:** Sinavda bir soru gordugunuzde, once anahtar kelimeleri (keyword) belirleyin, sonra bu tablodaki eslesmeyi bulun. Cogu soru 2-3 anahtar kelime ile dogru cevaba yonlendirir.

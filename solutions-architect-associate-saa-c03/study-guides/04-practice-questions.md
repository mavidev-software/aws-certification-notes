# AWS SAA-C03 Pratik Sorular

> 20 senaryo bazli soru - her domainden 5 soru.
> Gercek sinav formatinda, detayli aciklamalarla.

---

## Domain 1: Guvenli Mimariler Tasarlama (Sorular 1-5)

---

### Soru 1 — Domain 1: Guvenlik

Bir sirket, EC2 uzerinde calisan bir uygulamanin S3 bucket'ina dosya yuklemesini istiyor. Gelistirici ekip, uygulamanin icine AWS access key ve secret key gommeyi oneriyor. Cozum mimariniz en guvenli erisim yontemini onermeli.

**En uygun cozum nedir?**

- **A)** EC2 instance'a bir IAM Role atayarak S3 erisimi saglamak
- **B)** Access key ve secret key'i environment variable olarak EC2'ye tanimlamak
- **C)** S3 bucket policy ile EC2'nin IP adresine izin vermek
- **D)** IAM User olusturup credentials dosyasini EC2'ye kopyalamak

**Dogru Cevap:** A

**Aciklama:** IAM Role, EC2 instance'lara gecici kimlik bilgileri (temporary credentials) saglar ve bu bilgiler otomatik olarak rotate edilir. Credential'lari instance uzerinde saklamak (B ve D) guvenlik riski olusturur — key'ler sizdirabilir veya loglanabilir. IP bazli bucket policy (C) kimlik dogrulama saglamaz ve IP degisirse erisim kesilir. AWS en iyi uygulamasi: **asla credential'lari instance uzerine gommeyin, her zaman IAM Role kullanin.**

**Anahtar Kelime:** EC2 + S3 erisim + guvenli = IAM Role

---

### Soru 2 — Domain 1: Ag Guvenligi

Bir sirketin private subnet'teki EC2 instance'lari isletim sistemi guncellemelerini indirmek icin internet erisimi gerektiriyor. Ancak bu instance'lara internet uzerinden dogrudan erisim saglanmamali.

**Bu gereksinimi karsilayan cozum nedir?**

- **A)** Private subnet'e bir Internet Gateway eklemek
- **B)** Public subnet'te bir NAT Gateway olusturmak ve private subnet'in route table'ini guncellemek
- **C)** Her EC2 instance'a Elastic IP atamak
- **D)** Private subnet'teki security group'a tum outbound trafige izin vermek

**Dogru Cevap:** B

**Aciklama:** NAT Gateway, private subnet'teki kaynaklarin internete cikis yapmasina izin verirken, internetin bu kaynaklara dogrudan erisimini engeller. Internet Gateway (A) dogrudan public erisim saglar, amaca aykiridir. Elastic IP (C) instance'lari public yapar, guvenlik ihlaline neden olur. Security group (D) zaten varsayilan olarak tum outbound trafige izin verir ve routing sorununu cozmez.

**Anahtar Kelime:** Private subnet + internet erisimi + gelen trafik yok = NAT Gateway

---

### Soru 3 — Domain 1: Veri Sifreleme

Bir finans sirketi, S3'teki hassas musteri verilerini sifreleme zorunlulugu olan reguleasyonlara tabi. Sirket, sifreleme anahtarlarinin yasam dongusunu kendisi yonetmek ve anahtar kullanimini denetlemek istiyor.

**En uygun sifreleme yontemi nedir?**

- **A)** SSE-S3 (Amazon S3 yonetimli anahtarlar)
- **B)** SSE-KMS ile musterinin olusturdugu CMK (Customer Managed Key)
- **C)** SSE-C (musteri tarafindan saglanan anahtarlar)
- **D)** Client-side encryption ile AWS Encryption SDK

**Dogru Cevap:** B

**Aciklama:** SSE-KMS ile CMK, musterinin anahtar yasam dongusunu yonetmesine (rotation, disable, delete) ve CloudTrail ile anahtar kullanimini denetlemesine olanak tanir. SSE-S3 (A) anahtarlari AWS yonetir, musteri kontrolu yoktur. SSE-C (C) her istekte anahtar gondermek gerektirir ve AWS tarafinda denetim kaydi tutulmaz. Client-side encryption (D) ek karmasiklik getirir ve sunucu tarafindan yonetilen sifreleme gereksinimini karsilamaz.

**Anahtar Kelime:** Anahtar yonetimi + denetim (audit) + S3 = SSE-KMS + CMK

---

### Soru 4 — Domain 1: Ag Erisim Kontrolu

Bir sirket, S3 bucket'ina yalnizca belirli bir VPC icerisindeki kaynaklardan erisim saglanmasini istiyor. Bucket'a internet uzerinden veya baska VPC'lerden erisim kesinlikle engellenmeli.

**Bu gereksinimi en iyi karsilayan yaklasim nedir?**

- **A)** S3 bucket'a NACL kurallari uygulamak
- **B)** VPC Gateway Endpoint olusturmak ve S3 bucket policy'de `aws:sourceVpce` kosulunu kullanmak
- **C)** S3 bucket'i public access block ile korumak
- **D)** IAM policy'de kaynak IP adresi kosulu ile sinirlamak

**Dogru Cevap:** B

**Aciklama:** VPC Gateway Endpoint, S3 erisimini AWS ozel agi uzerinden saglar (trafik internete cikmaz). Bucket policy'de `aws:sourceVpce` kosulu ile sadece belirli VPC endpoint'inden gelen isteklere izin verilir. NACL (A) subnet seviyesinde calisir, S3 servise uygulanamaz. Public access block (C) anonim erisimi engeller ama baska VPC veya IAM kullanicilarini engellemez. IP sinirlamasi (D) VPC icerisindeki private IP'ler icin calisir ancak VPC endpoint kadar guvenli ve olceklenebilir degildir.

**Anahtar Kelime:** S3 + yalnizca belirli VPC + ozel erisim = VPC Endpoint + Bucket Policy

---

### Soru 5 — Domain 1: Coklu Hesap Yonetimi

Bir sirket AWS Organizations kullanarak 20 farkli AWS hesabini yonetiyor. Guvenlik ekibi, tum hesaplarda belirli bolgelerin (regions) disinda kaynak olusturulmasini engellemek ve bir hesaptaki gelistiricilerin baska bir hesaptaki DynamoDB tablosuna erisimini saglamak istiyor.

**Bu iki gereksinimi karsilayan cozum kombinasyonu nedir?**

- **A)** Her hesapta IAM policy ile bolge sinirlamasi + cross-account IAM User paylasimi
- **B)** SCP ile bolge sinirlamasi + hedef hesapta IAM Role ve trust policy ile cross-account erisim
- **C)** AWS Config kurallari ile bolge sinirlamasi + VPC Peering ile cross-account erisim
- **D)** SCP ile bolge sinirlamasi + Resource-based policy ile DynamoDB erisimini dogrudan acmak

**Dogru Cevap:** B

**Aciklama:** SCP (Service Control Policy), Organizations seviyesinde tum hesaplara uygulanir ve belirli bolgeler disinda API cagrilarini engeller — merkezi ve zorunlu bir kontrol saglar. Cross-account erisim icin hedef hesapta bir IAM Role olusturulur ve kaynak hesabin buna "assume" etmesine izin veren trust policy eklenir. IAM User paylasimi (A) guvenlik riski tasir ve olceklenmez. AWS Config (C) izleme amacidir, engelleme yapmaz. DynamoDB resource-based policy desteklemez (D), bu nedenle dogrudan erisim acmak mumkun degildir.

**Anahtar Kelime:** Organizations + bolge sinirlamasi + cross-account = SCP + IAM Role trust policy

---

## Domain 2: Dayanikli Mimariler Tasarlama (Sorular 6-10)

---

### Soru 6 — Domain 2: Oturum Yonetimi

Bir e-ticaret uygulamasi Auto Scaling grubu icerisinde birden fazla EC2 instance uzerinde calisiyor. Kullanicilar, ALB'nin trafiklerini farkli instance'a yonlendirmesi durumunda oturum bilgilerini kaybediyor ve tekrar giris yapmak zorunda kaliyor.

**Oturum surekliligini saglayan en olceklenebilir cozum nedir?**

- **A)** ALB'de sticky sessions (session affinity) etkinlestirmek
- **B)** Oturum verilerini Amazon ElastiCache Redis'te depolamak
- **C)** Oturum verilerini her EC2 instance'in yerel diskinde tutmak
- **D)** Kullanicilari her zaman ayni instance'a yonlendirmek icin Route 53 weighted routing kullanmak

**Dogru Cevap:** B

**Aciklama:** ElastiCache Redis, oturum verilerini merkezi ve yuksek performansli bir depoda saklar. Herhangi bir instance, herhangi bir kullanicinin oturumuna erisebilir. Sticky sessions (A) calisir ancak instance terminate olursa oturum kaybolur ve yuk dagilimi dengesiz olur. Yerel disk (C) instance'a bagimlidir, olceklenmez. Route 53 (D) DNS bazli yonlendirmedir, instance seviyesinde oturum takibi icin uygun degildir.

**Anahtar Kelime:** Oturum kaybediyor + Auto Scaling + olceklenebilir = ElastiCache (veya DynamoDB)

---

### Soru 7 — Domain 2: Veritabani Yuksek Erisilebilirlik

Bir sirketin uretim veritabani Amazon RDS MySQL uzerinde calisiyor. Veritabani instance'inin arizalanmasi durumunda uygulamanin en fazla birkaç dakika icerisinde tekrar calismaya baslamasi gerekiyor. Sirket ayrica okuma performansini artirmak istiyor.

**Hem yuksek erisilebilirlik hem okuma performansi icin en uygun yapilandirma nedir?**

- **A)** RDS Multi-AZ deployment + Read Replica
- **B)** Yalnizca Read Replica olusturmak (farkli AZ'de)
- **C)** RDS'in snapshot'larini her 5 dakikada bir almak
- **D)** Iki ayri RDS instance'i manuel olarak senkronize etmek

**Dogru Cevap:** A

**Aciklama:** Multi-AZ, standby replica'ya senkron replikasyon yapar ve ariza durumunda otomatik failover saglar (genellikle 60-120 saniye). Read Replica ise asenkron replikasyon ile okuma islemlerini dagitir. Yalnizca Read Replica (B) otomatik failover saglamaz — Read Replica'yi manual olarak promote etmeniz gerekir. Snapshot (C) RPO dakikalar olabilir ve recovery suresi uzundur. Manuel senkronizasyon (D) hata egilimli ve operasyonel yuku yuksek bir yaklasimdir.

**Anahtar Kelime:** RDS + otomatik failover + okuma performansi = Multi-AZ + Read Replica

---

### Soru 8 — Domain 2: Otomatik Olceklendirme

Bir online perakende sirketi, yillik buyuk kampanya donemlerinde trafikte 10 kat artis yasiyor. Normal zamanlarda 2 instance yeterli olurken, kampanya donemlerinde 20 instance gerekiyor. Uygulama, ALB arkasindaki EC2 instance'larinda calisiyor.

**Kampanya doneminde performansi korurken maliyeti optimize eden cozum nedir?**

- **A)** Surekli 20 instance calistirmak
- **B)** Auto Scaling Group ile target tracking scaling policy kullanmak (ornegin CPU %60)
- **C)** Kampanya oncesinde manuel olarak instance sayisini artirmak
- **D)** Sadece ALB'nin capacity'sini artirmak

**Dogru Cevap:** B

**Aciklama:** Target tracking scaling policy, belirtilen metrik hedefine (ornegin CPU kullanimi %60) gore otomatik olarak instance ekler veya cikarir. Trafik arttikca olceklenir, dusunce kuculur. 20 instance surekli calistirmak (A) gereksiz maliyet olusturur. Manuel mudahale (C) tepki suresini uzatir ve insan hatasina aciktir. ALB (D) tek basina backend kapasitesini artirmaz, sadece trafik dagitir.

**Anahtar Kelime:** Trafik dalgalanmasi + otomatik + maliyet optimizasyonu = Auto Scaling + Target Tracking

---

### Soru 9 — Domain 2: Sifir Veri Kaybi

Bir finans uygulamasi icin regulator, RPO=0 (sifir veri kaybi) ve RTO'nun 1 dakikanin altinda olmasini zorunlu kiliyor. Veritabani PostgreSQL uyumlu olmali.

**Bu gereksinimleri karsilayan en uygun cozum nedir?**

- **A)** RDS PostgreSQL + gunluk otomatik snapshot
- **B)** Amazon Aurora PostgreSQL Multi-AZ deployment
- **C)** RDS PostgreSQL + cross-region Read Replica
- **D)** EC2 uzerinde PostgreSQL + EBS snapshot

**Dogru Cevap:** B

**Aciklama:** Aurora Multi-AZ, veriyi 3 AZ'de 6 kopya olarak senkron sekilde yazar. Bir AZ arizalandiginda otomatik failover genellikle 30 saniyenin altinda gerceklesir ve veri kaybi olmaz (RPO=0). Gunluk snapshot (A) RPO en fazla 24 saattir. Cross-region Read Replica (C) asenkron replikasyondur, RPO=0 garanti edemez. EC2 uzerinde PostgreSQL (D) yonetim yuku yuksektir ve otomatik failover saglamaz.

**Anahtar Kelime:** RPO=0 + otomatik failover + PostgreSQL = Aurora Multi-AZ

---

### Soru 10 — Domain 2: Servis Ayristirma

Bir fotografcilik platformu, kullanicilarin yukledigi resimleri farkli boyutlarda yeniden isliyor. Yukleme API'si ve resim isleme servisi ayni EC2 instance uzerinde calisiyor. Yogun yuklemelerde resim isleme kuyruklaniyor ve API yanitlari yavaslayarak zaman asimi hatasi aliyor.

**API performansini iyilestirmek icin en uygun mimari degisiklik nedir?**

- **A)** EC2 instance tipini daha buyuk bir tipe yukseltmek
- **B)** Yukleme API'si ile resim isleme arasina SQS kuyruğu ekleyerek isleri ayristirmak
- **C)** Resim isleme islemini API icinde asenkron thread'e tasimak
- **D)** CloudFront ile API yanit surelerini iyilestirmek

**Dogru Cevap:** B

**Aciklama:** SQS kuyruğu, upload API'sini resim isleme isleminden ayirir (decoupling). API, mesaji kuyruga atip hemen yanit doner; resim isleme servisi kuyruktan bagimsiz olarak calisir. Instance yukseltme (A) gecici cozumdur, olceklenmez. Asenkron thread (C) ayni instance uzerinde kaynak rekabeti devam eder. CloudFront (D) statik icerik icin faydalıdır, backend isleme performansini iyilestirmez.

**Anahtar Kelime:** API yavaslama + isleme kuyruklaniyor + ayristirma = SQS Queue (decoupling)

---

## Domain 3: Yuksek Performansli Mimariler Tasarlama (Sorular 11-15)

---

### Soru 11 — Domain 3: Paylasimli Depolama

Bir medya sirketi, birden fazla AZ'de calisan Linux tabanli EC2 instance'larinin ayni dosya sistemine ayni anda erismesini gerektiriyor. Dosyalar uzerinde POSIX uyumlu islemler (lock, append) yapiliyor.

**Bu gereksinimi karsilayan depolama servisi nedir?**

- **A)** Amazon EBS Multi-Attach
- **B)** Amazon EFS (Elastic File System)
- **C)** Amazon S3 ile s3fs-fuse mount
- **D)** Amazon FSx for Windows File Server

**Dogru Cevap:** B

**Aciklama:** EFS, birden fazla AZ'deki binlerce EC2 instance'in ayni anda eristigi, tamamen yonetilen, POSIX uyumlu bir NFS dosya sistemidir. EBS Multi-Attach (A) ayni AZ icinde sinirli instance sayisini destekler ve paylasilmis dosya sistemi semantigine sahip degildir. S3 + s3fs (C) POSIX uyumlu degildir, tutarlilik ve performans sorunlari yasar. FSx for Windows (D) SMB protokolu kullanir, Linux POSIX gereksinimleri icin uygun degildir.

**Anahtar Kelime:** Linux + birden fazla AZ + paylasilmis dosya sistemi + POSIX = EFS

---

### Soru 12 — Domain 3: DynamoDB Performans

Bir oyun sirketi, DynamoDB'de oyuncu profil verilerini tutuyor. Oyun baslangicinda milyonlarca oyuncu ayni anda profil verilerini okuyor ve okuma gecikmesi 15ms'nin uzerine cikiyor. Sirket, okuma gecikmesini mikrosaniye seviyesine indirmek istiyor.

**En uygun cozum nedir?**

- **A)** DynamoDB tablonun RCU (Read Capacity Units) degerini artirmak
- **B)** Onunde Amazon ElastiCache Redis kuyruğu kullanmak
- **C)** Amazon DynamoDB Accelerator (DAX) kullanmak
- **D)** DynamoDB Global Tables etkinlestirmek

**Dogru Cevap:** C

**Aciklama:** DAX, DynamoDB icin ozel olarak tasarlanmis bir in-memory cache'tir. Okuma gecikmesini milisaniyeden mikrosaniyeye dusurur ve DynamoDB API'si ile tamamen uyumludur — uygulama kodu minimum degisiklikle calisir. RCU artirmak (A) throughput'u artirır ama gecikmeyi mikrosaniyeye indirmez. ElastiCache (B) genel amacli bir cache'tir, ek uygulama mantigi gerektirir ve DynamoDB API uyumlulugu yoktur. Global Tables (D) coklu bolge erisimi icindir, tek bolgedeki okuma gecikmesini iyilestirmez.

**Anahtar Kelime:** DynamoDB + mikrosaniye gecikme + okuma cache = DAX

---

### Soru 13 — Domain 3: Buyuk Veri Tasima

Bir uretim sirketi, on-premises veri merkezindeki 500 TB veriyi AWS'ye tasimak istiyor. Sirketin internet baglantisi 1 Gbps. Verinin 2 hafta icinde AWS'ye aktarilmasi gerekiyor.

**En uygun veri tasima yontemi nedir?**

- **A)** AWS Direct Connect baglantisi kurmak ve veriyi transfer etmek
- **B)** Birden fazla AWS Snowball Edge cihazi siparis etmek
- **C)** S3 Transfer Acceleration ile internet uzerinden yuklemek
- **D)** AWS DataSync ile internet uzerinden transfer etmek

**Dogru Cevap:** B

**Aciklama:** 1 Gbps ile 500 TB transferi yaklasik 46 gun surer — 2 haftalik hedefe ulasilamaz. Snowball Edge cihazlari (her biri ~80 TB) fiziksel olarak gonderilir ve birden fazla cihaz paralel kullanilabilir; toplam sure yaklasik 1 hafta (kargo + yukleme). Direct Connect (A) kurulumu haftalar/aylar alir, zamana yetismez. S3 Transfer Acceleration (C) ve DataSync (D) bant genisligi siniriyla karsilasmaya devam eder.

**Anahtar Kelime:** Yuzlerce TB + sinirli bant genisligi + kisa sure = Snowball Edge

---

### Soru 14 — Domain 3: Global Icerik Dagitimi

Bir SaaS sirketi, statik web sitesini (HTML, CSS, JS, goruntuler) dunyaya hizmet veriyor. Kullanicilar farkli kitalardan erisim sagliyor ve bazi bolgelerde sayfa yukleme sureleri 5 saniyeyi aşiyor.

**Global olarak dusuk gecikme ve yuksek performans saglayan cozum nedir?**

- **A)** Her bolgede ayri EC2 instance'lari ile web sunucusu kurmak
- **B)** Amazon S3 static website hosting + Amazon CloudFront dagitimi
- **C)** Route 53 latency-based routing ile tek bir S3 bucket
- **D)** AWS Global Accelerator ile S3 bucket

**Dogru Cevap:** B

**Aciklama:** S3 static hosting maliyet etkin ve tamamen yonetilen bir barindirma saglarken, CloudFront 400'den fazla edge location ile icerigi kullaniciya en yakin noktadan sunar. Her bolgede EC2 (A) yuksek maliyet ve yonetim yuku getirir. Route 53 latency-based (C) DNS seviyesinde yonlendirir ama icerik cache'lemez, kaynak sunucuya her istek gider. Global Accelerator (D) TCP/UDP uygulamalari icindir, statik icerik dagitimi icin CloudFront cok daha uygundur.

**Anahtar Kelime:** Statik web sitesi + global dusuk gecikme = S3 + CloudFront

---

### Soru 15 — Domain 3: Gercek Zamanli Veri Isleme

Bir enerji sirketi, 100.000 IoT sensorunden saniyede yuz binlerce veri noktasi aliyor. Bu verilerin gercek zamanli olarak islenmesi ve bir dashboard'da gosterilmesi gerekiyor. Ayrica veriler uzerinde gercek zamanli anomali tespiti yapilmali.

**Bu gereksinimi karsilayan en uygun mimari nedir?**

- **A)** IoT sensörler → SQS → Lambda → DynamoDB → Dashboard
- **B)** IoT sensörler → Kinesis Data Streams → Kinesis Data Analytics → Dashboard
- **C)** IoT sensörler → SNS → Lambda → RDS → Dashboard
- **D)** IoT sensörler → S3 → Athena → QuickSight

**Dogru Cevap:** B

**Aciklama:** Kinesis Data Streams, saniyede milyonlarca kaydi alabilir ve gercek zamanli akis isleme icin tasarlanmistir. Kinesis Data Analytics, akis uzerinde SQL veya Apache Flink ile gercek zamanli anomali tespiti yapabilir. SQS + Lambda (A) kuyruk bazlidir, gercek zamanli akis isleme icin optimize degildir ve throughput sinirlamalari vardir. SNS + RDS (C) bildirim servisidir, buyuk hacimli akis verisi icin uygun degildir. S3 + Athena (D) batch analitik icindir, gercek zamanli degil.

**Anahtar Kelime:** IoT + gercek zamanli akis + anomali tespiti = Kinesis Data Streams + Analytics

---

## Domain 4: Maliyet Optimize Edilmis Mimariler Tasarlama (Sorular 16-20)

---

### Soru 16 — Domain 4: Zamanlanmis Olceklendirme

Bir sirketin dahili ERP uygulamasi yalnizca mesai saatlerinde (08:00-18:00, Pazartesi-Cuma) yogun kullaniliyor. Gece ve hafta sonlari trafik neredeyse sifir. Su anda 10 EC2 instance 7/24 calisiyor.

**Maliyeti en cok dusurecek cozum nedir?**

- **A)** Tum instance'lari Reserved Instance olarak satin almak
- **B)** Auto Scaling Group ile scheduled scaling kullanarak mesai disi saatlerde instance sayisini minimum'a indirmek
- **C)** Spot Instance'lar kullanmak
- **D)** Instance'lari her gece manuel olarak durdurup sabah baslatmak

**Dogru Cevap:** B

**Aciklama:** Scheduled scaling, bilinen trafik kaliplarina gore instance sayisini otomatik olarak ayarlar. Mesai saatlerinde 10, gece ve hafta sonu 1-2 instance calistirilabilir. Reserved Instance (A) %70 bos kaldiklari icin israf olur. Spot Instance (C) kesintiye ugramaya aciktir, is-kritik ERP icin risklidir. Manuel durdurma (D) operasyonel yuk olusturur ve insan hatasina aciktir.

**Anahtar Kelime:** Tahmin edilebilir kullanim kaliplari + bos zaman + maliyet = Scheduled Scaling

---

### Soru 17 — Domain 4: Gelistirme Ortami Veritabani

Bir gelistirme ekibi PostgreSQL veritabanini yalnizca is saatlerinde (gunluk ~8 saat) kullaniyor. Hafta sonlari ve gece veritabanina hic erisim yok. Su anki RDS db.r5.large instance'i aylik 200$ maliyete neden oluyor.

**Maliyeti en cok dusurecek cozum nedir?**

- **A)** RDS instance tipini db.t3.micro'ya dusirmek
- **B)** Amazon Aurora Serverless v2 kullanmak
- **C)** Her gece RDS instance'ini stop edip sabah start etmek (Lambda ile)
- **D)** DynamoDB on-demand moduna gecmek

**Dogru Cevap:** B

**Aciklama:** Aurora Serverless v2, kullanim olmayan donemlerde kapasiteyi minimum ACU'ya dusurur (neredeyse sifir maliyet). Kullanim basladiginda saniyeler icinde olceklenir. Kucuk instance (A) maliyet duser ama bos oldugu surece hala odeme yapilir. Lambda ile stop/start (C) calisir ancak her 7 gunde RDS otomatik baslar ve operasyonel karmasiklik yaratir. DynamoDB (D) iliskisel veritabani gereksinimlerini karsilamaz ve sema degisikligi gerektirir.

**Anahtar Kelime:** Gelistirme veritabani + dusuk kullanim + otomatik olceklenme = Aurora Serverless

---

### Soru 18 — Domain 4: Veri Transfer Maliyeti

Bir medya sirketi, S3'te depoladigi video dosyalarini global kullanicilara sunuyor. Aylik S3 data transfer maliyeti 50.000$'i asmis durumda. Videolar cok sik erisiliyor ve ortalama dosya boyutu 500 MB.

**Data transfer maliyetini dusurmek icin en etkili cozum nedir?**

- **A)** S3 Intelligent-Tiering etkinlestirmek
- **B)** Amazon CloudFront dagitimi kullanmak
- **C)** S3 bucket'i her bolgede coklamak (cross-region replication)
- **D)** S3 Requester Pays ozelligini etkinlestirmek

**Dogru Cevap:** B

**Aciklama:** CloudFront, S3'ten veriyi edge location'lara cache'ler. S3'ten CloudFront'a transfer ucretsizdir ve CloudFront'un data transfer fiyatlari S3'ten dogrudan internete transfer fiyatlarindan daha dusuktur. Ayrica cache sayesinde origin'e giden istek sayisi azalir. Intelligent-Tiering (A) depolama maliyetini optimize eder, transfer maliyetini degil. Cross-region replication (C) depolama maliyetini iki katina cikarir. Requester Pays (D) maliyeti kullaniciya yansitir ama teknik olarak maliyeti dusirmez, musteri deneyimini bozar.

**Anahtar Kelime:** S3 + yuksek data transfer maliyeti + sik erisim = CloudFront

---

### Soru 19 — Domain 4: Batch Isleme Maliyeti

Bir biyoinformatik sirketi, genomik veri analizi icin her gun 4-6 saat suren batch islemleri calistiriyor. Isler checkpoint mekanizmasi ile duraklatilip devam ettirilebilir. Is sonuclari S3'e yaziliyor.

**Bu is yukunu en dusuk maliyetle calistirmanin yolu nedir?**

- **A)** On-Demand EC2 instance'lari kullanmak
- **B)** Spot Instance'lari kullanmak
- **C)** Reserved Instance (1 yillik) satin almak
- **D)** AWS Lambda ile islemi parcalara bolmek

**Dogru Cevap:** B

**Aciklama:** Spot Instance'lar On-Demand fiyatinin %60-90 altinda fiyatlandirilir. Checkpoint mekanizmasi sayesinde Spot kesintisi durumunda is kaldigi yerden devam edebilir — bu da Spot icin ideal bir senaryo olusturur. On-Demand (A) gereksiz yere pahali. Reserved Instance (C) gunluk sadece 4-6 saat kullanim icin israf olur (kullanim orani dusuk). Lambda (D) 15 dakika calisma suresi siniri vardir, saatlerce suren batch islemleri icin uygun degildir.

**Anahtar Kelime:** Batch is + kesintiye dayanikli + maliyet minimizasyonu = Spot Instance

---

### Soru 20 — Domain 4: Guvenli Uzaktan Erisim

Bir sirket, private subnet'teki EC2 instance'larina yonetim erişimi saglamak istiyor. Su anda public subnet'te bir bastion host kullaniyorlar. Guvenlik ekibi, bastion host'un SSH port 22 acik olmasi ve ek instance maliyeti nedeniyle alternatif arıyor.

**Bastion host'u ortadan kaldiran ve maliyeti dusurebilecek en uygun cozum nedir?**

- **A)** EC2 Instance Connect ile dogrudan baglanmak
- **B)** AWS Systems Manager Session Manager kullanmak
- **C)** VPN baglantisi kurmak
- **D)** EC2 Serial Console kullanmak

**Dogru Cevap:** B

**Aciklama:** Systems Manager Session Manager, SSH portu acmadan ve bastion host kullanmadan private subnet'teki instance'lara guvenli erisim saglar. Erisim IAM ile yetkilendirilir ve tum oturumlar CloudTrail'de loglanir. Ek instance maliyeti yoktur. EC2 Instance Connect (A) instance'in public IP veya endpoint'e ihtiyaci vardir. VPN (C) ek altyapi ve maliyet gerektirir, bastion host'tan daha pahali olabilir. EC2 Serial Console (D) sorun giderme icindir, gunluk yonetim icin pratik degildir.

**Anahtar Kelime:** Bastion host yerine + SSH gereksiz + maliyet dusurme = Session Manager

---

## Ozet Tablosu

| # | Domain | Anahtar Konu | Dogru Cevap |
|---|--------|-------------|-------------|
| 1 | D1 - Guvenlik | EC2'den S3 erisimi | IAM Role |
| 2 | D1 - Ag | Private subnet internet | NAT Gateway |
| 3 | D1 - Sifreleme | S3 musteri anahtari | SSE-KMS + CMK |
| 4 | D1 - Erisim | S3 VPC sinirlamasi | VPC Endpoint + Bucket Policy |
| 5 | D1 - Coklu Hesap | Organizations yonetimi | SCP + IAM Role Trust Policy |
| 6 | D2 - Oturum | Session kaybı | ElastiCache Redis |
| 7 | D2 - Veritabani | RDS failover + okuma | Multi-AZ + Read Replica |
| 8 | D2 - Olceklendirme | Trafik dalgalanmasi | Auto Scaling + Target Tracking |
| 9 | D2 - RPO | Sifir veri kaybi | Aurora Multi-AZ |
| 10 | D2 - Ayristirma | API + resim isleme | SQS Queue |
| 11 | D3 - Depolama | Paylasilmis Linux FS | EFS |
| 12 | D3 - Performans | DynamoDB cache | DAX |
| 13 | D3 - Migrasyon | 500 TB tasima | Snowball Edge |
| 14 | D3 - CDN | Statik site global | S3 + CloudFront |
| 15 | D3 - Akis | IoT gercek zamanli | Kinesis Streams + Analytics |
| 16 | D4 - Olceklendirme | Mesai saati kaliplari | Scheduled Scaling |
| 17 | D4 - Veritabani | Gelistirme DB | Aurora Serverless |
| 18 | D4 - Transfer | S3 data transfer | CloudFront |
| 19 | D4 - Batch | Kesintiye dayanikli is | Spot Instance |
| 20 | D4 - Erisim | Bastion host alternatifi | Session Manager |

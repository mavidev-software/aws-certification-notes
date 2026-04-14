# Domain 3 — Task Statement 5: Determine High-Performing Data Ingestion and Transformation Solutions

---

## 2. Önemli Keywords (Sınav Odaklı)

### Veri Alım Kalıpları

| Kalıp | Açıklama | Odak |
|-------|----------|------|
| **Homojen** | Kaynak formatı = hedef formatı | Hız, bütünlük, otomasyon |
| **Heterojen** | Format dönüşümü gerekli | Veri türü değiştirme, ML ile yeni nitelik türetme |

### Kinesis Ailesi — Kritik Karşılaştırma

| Servis | Ne Yapar | Ne Zaman |
|--------|----------|----------|
| **Kinesis Data Streams** | Gerçek zamanlı veri akışı alımı | Gerçek zamanlı, yüksek ölçek, çoklu kaynak, saniyede GB'larca veri |
| **Kinesis Data Firehose** | Akış verilerini AWS veri depolarına yükleme | En basit yaklaşım — yakalama + dönüştürme + yükleme |
| **Kinesis Data Analytics** | Akış verileri üzerinde analiz + temel dönüştürme | Gerçek zamanlı analitik, veri dönüştürme |
| **Kinesis Video Streams** | Akış video alımı | Video işleme |

> **Sınav Tuzağı:** Data Streams = gerçek zamanlı alım, Firehose = AWS depolarına yükleme (en kolay yol), Analytics = dönüştürme + analiz

### Veri İşleme ve Dönüştürme Servisleri

| Servis | Kullanım |
|--------|----------|
| **AWS Glue** | ETL — veri keşfi, hazırlama, taşıma, entegrasyon |
| **Amazon EMR** | Büyük veri işleme (Hadoop/Spark), S3 ile yüksek ölçekli dağıtık işleme |
| **AWS Lake Formation** | Veri gölü oluşturma ve yönetim |
| **Amazon Athena** | S3 üzerinde SQL sorguları — sunucusuz, ETL |
| **Parquet formatı** | Sütun bazlı depolama — EMR/Glue ile dönüştürme, performans optimizasyonu |

### Veri Analitiği ve Görselleştirme

| Servis | Kullanım |
|--------|----------|
| **Amazon Athena** | S3 üzerinde etkileşimli SQL sorguları |
| **AWS Lake Formation** | Merkezi veri gölü yönetimi |
| **Amazon QuickSight** | BI görselleştirme ve dashboard |

### Veri Gölü (Data Lake) Kavramları

| Özellik | Açıklama |
|---------|----------|
| **Merkezi depo** | Yapılandırılmış + yapılandırılmamış veri |
| **Çoklu veri türleri** | Farklı kaynaklardan farklı formatlar |
| **Hızlı alım** | Veri hızla kullanılabilir |
| **Veri tekrarı eliminasyonu** | Merkezi yönetişim |
| **S3 üzerinde inşa** | En yaygın yaklaşım |

### Veri Gölüne Alım Servisleri

| Servis | Senaryo |
|--------|---------|
| **Kinesis Data Firehose** | Gerçek zamanlı akış → S3 |
| **Snow Family** | Büyük hacimli fiziksel veri transferi |
| **AWS Glue** | ETL işleri |
| **AWS DataSync** | Online veri aktarımı |
| **Transfer Family** | SFTP/FTPS dosya transferi |
| **Storage Gateway** | Hibrit depolama |
| **Direct Connect** | Dedike ağ bağlantısı |
| **DMS** | Veritabanı geçişi |

### Güvenlik — Veri Alımı İçin

| Servis / Özellik | Kullanım |
|-------------------|----------|
| **S3 Bucket Policies** | Hesaplar/kullanıcılar arası erişim kontrolü |
| **IAM User Policies** | Veri gölü erişim kontrolü |
| **S3 Encryption** | Birden fazla şifreleme seçeneği |
| **KMS** | Şifreleme anahtarı yönetimi |
| **CloudHSM** | PII uyumluluk gereksinimleri için donanım tabanlı anahtar yönetimi |
| **S3 Cross-Region Replication** | Bölgeler arası veri replikasyonu |
| **S3 Object Lock** | Değiştirilemez veri (WORM) |
| **S3 Versioning** | Sürümleme |
| **API Gateway + Cognito** | Erişim kontrolü ve kimlik doğrulama |

### On-Premises → AWS Veri Transferi Seçim Kılavuzu

| Senaryo | Servis |
|---------|--------|
| Çok büyük veri (TB/PB), çevrimdışı | **Snow Family** (Snowball, Snowmobile) |
| Sürekli gerçek zamanlı akış | **Kinesis** |
| Online dosya senkronizasyonu | **DataSync** |
| SFTP/FTP dosya transferi | **Transfer Family** |
| Veritabanı geçişi | **DMS** |
| Dedike yüksek bant genişliği | **Direct Connect** |
| Hibrit depolama | **Storage Gateway** |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 3'ün son görev ifadesini ele alıyor: **yüksek performanslı veri alımı ve dönüştürme çözümleri**.

### Veri Alım Kalıpları

İki temel kalıp var: **Homojen** (aynı format, odak hız ve otomasyon) ve **Heterojen** (format dönüşümü gerekli, ETL/ML ile dönüştürme). Sınavda senaryo verilip hangi kalıp ve servisin uygun olduğu sorulur.

### Kinesis Ailesi — En Önemli Ayrım

Bu videonun en kritik konusu Kinesis ailesinin 4 servisini ayırt etmek:
- **Data Streams** = gerçek zamanlı alım (producer → stream → consumer)
- **Firehose** = en basit yol — veriyi AWS depolarına yükle
- **Data Analytics** = temel veri dönüştürme + analiz
- **Video Streams** = video alımı

### ETL ve Veri İşleme

**Glue** = AWS'nin yönetilen ETL servisi (keşif, hazırlama, taşıma, entegrasyon). **EMR** = büyük veri işleme (Hadoop/Spark). İkisi de S3 ile dağıtık şekilde çalışır. Performans optimizasyonu için verileri **Parquet** formatına dönüştürmeyi düşünün.

### Data Lake (Veri Gölü)

S3 üzerinde inşa edilen merkezi depo. Lake Formation ile yönetilir. Yapılandırılmış + yapılandırılmamış veriyi tek yerde toplar. Alım için 8+ farklı servis kullanılabilir — veri miktarı, türü ve kaynağına göre seçin.

### Güvenlik

Veri gölü güvenliği katmanlıdır: S3 bucket policies + IAM user policies + KMS şifreleme + S3 Object Lock + Versioning. PII verileri için CloudHSM. Erişim kontrolü için API Gateway + Cognito.

> **Sınav İpucu:**
> - "Gerçek zamanlı akış verisi" → Kinesis Data Streams
> - "En basit yol, AWS depolarına yükle" → Kinesis Data Firehose
> - "Akış verisinde dönüştürme" → Kinesis Data Analytics
> - "S3 üzerinde SQL" → Athena
> - "ETL, veri keşfi ve entegrasyonu" → Glue
> - "Büyük veri, Hadoop/Spark" → EMR
> - "Büyük fiziksel veri transferi" → Snow Family
> - "PII uyumluluk, donanım tabanlı şifreleme" → CloudHSM

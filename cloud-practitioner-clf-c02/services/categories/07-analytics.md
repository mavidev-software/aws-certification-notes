# Analytics Services (In-Scope)

> **Kategori:** Analitik Servisleri

## Amazon Athena

**Ne Ise Yarar:** S3'teki verileri SQL ile sorgulama (Serverless).

**Ozellikler:**
- Sunucu yonetimi yok
- Sadece sorgulanan veri icin odeme
- Presto/Trino tabanli

**Ne Zaman Kullanilir:**
- S3'teki log analizi
- Ad-hoc sorgular
- Veri golu analizleri

---

## Amazon Redshift

**Ne Ise Yarar:** Veri ambari (Data Warehouse) hizmeti.

**Ozellikler:**
- Petabyte olceginde veri
- Columnar storage
- Massively Parallel Processing (MPP)
- Redshift Serverless secenegi

**Ne Zaman Kullanilir:**
- Is zekasi raporlari
- OLAP sorgulamalari
- Buyuk veri analizi

---

## Amazon EMR (Elastic MapReduce)

**Ne Ise Yarar:** Buyuk veri isleme (Hadoop, Spark, Hive).

**Ne Zaman Kullanilir:**
- Buyuk veri isleme
- Log analizi
- Machine learning on big data
- ETL islemleri

---

## AWS Glue

**Ne Ise Yarar:** ETL (Extract, Transform, Load) hizmeti.

**Bilesenleri:**
- **Glue Data Catalog:** Metadata deposu
- **Glue ETL Jobs:** Veri donusturme isleri
- **Glue Crawlers:** Otomatik sema kesfi

**Ne Zaman Kullanilir:**
- Veri entegrasyonu
- Veri hazirlama
- Veri katalogu olusturma

---

## Amazon Kinesis

**Ne Ise Yarar:** Gercek zamanli veri akisi isleme.

**Servisleri:**
| Servis | Kullanim |
|--------|----------|
| Kinesis Data Streams | Ham veri akisi |
| Kinesis Data Firehose | S3, Redshift'e yukleme |
| Kinesis Data Analytics | SQL ile analiz |
| Kinesis Video Streams | Video akisi |

**Ne Zaman Kullanilir:**
- Gercek zamanli analitik
- Log ve metric toplama
- IoT veri akisi
- Clickstream analizi

---

## Amazon OpenSearch Service

**Ne Ise Yarar:** Arama ve log analizi (Elasticsearch tabanli).

**Ne Zaman Kullanilir:**
- Log analizi
- Full-text arama
- Gorsellestirme (Kibana/OpenSearch Dashboards)

---

## Amazon QuickSight

**Ne Ise Yarar:** Is zekasi (BI) dashboard ve gorsellestirme.

**Ozellikler:**
- Serverless
- SPICE in-memory engine
- ML-powered insights
- Embedded analytics

**Ne Zaman Kullanilir:**
- BI raporlari
- Dashboard olusturma
- Veri gorsellestirme


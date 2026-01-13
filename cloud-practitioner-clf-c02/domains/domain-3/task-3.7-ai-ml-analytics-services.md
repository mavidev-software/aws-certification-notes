# Task 3.7: AWS AI/ML ve Analytics Servisleri

> **Resmi Tanım:** Identify AWS artificial intelligence and machine learning (AI/ML) services and analytics services.
>
> **Kaynak:** [AWS CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

## İçindekiler

1. [Genel Bakış](#genel-bakış)
2. [AWS AI/ML Servisleri](#aws-aiml-servisleri)
3. [Amazon SageMaker](#amazon-sagemaker)
4. [AI Servisleri (Pre-trained)](#ai-servisleri-pre-trained)
5. [AWS Analytics Servisleri](#aws-analytics-servisleri)
6. [Data Lake ve ETL](#data-lake-ve-etl)
7. [Karşılaştırma Tabloları](#karşılaştırma-tabloları)
8. [Sınav Senaryoları](#sınav-senaryoları)

---

## Genel Bakış

AWS, yapay zeka, makine öğrenimi ve veri analitiği için geniş bir servis yelpazesi sunar. Bu servisler, ML uzmanlarından veri analistlerine kadar farklı uzmanlık seviyelerine hitap eder.

### AI/ML ve Analytics Haritası

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    AWS AI/ML VE ANALİTİKS SERVİSLERİ                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │                    AI/ML SERVİS KATEGORİLERİ                        │    │
│  │                                                                     │    │
│  │   ML Experts                                                        │    │
│  │   ┌─────────────────────────────────────────────────────────────┐  │    │
│  │   │  SageMaker - Custom ML model training & deployment          │  │    │
│  │   └─────────────────────────────────────────────────────────────┘  │    │
│  │                               │                                     │    │
│  │   Developers                  │                                     │    │
│  │   ┌─────────────────────────────────────────────────────────────┐  │    │
│  │   │  Pre-trained AI Services (APIs)                             │  │    │
│  │   │  • Rekognition (görsel)    • Comprehend (NLP)              │  │    │
│  │   │  • Polly (text-to-speech)  • Transcribe (speech-to-text)   │  │    │
│  │   │  • Translate (çeviri)      • Lex (chatbot)                 │  │    │
│  │   │  • Textract (OCR)          • Kendra (search)               │  │    │
│  │   └─────────────────────────────────────────────────────────────┘  │    │
│  │                               │                                     │    │
│  │   Business Users             │                                     │    │
│  │   ┌─────────────────────────────────────────────────────────────┐  │    │
│  │   │  • QuickSight (BI dashboards)                               │  │    │
│  │   │  • Forecast (zaman serisi tahmin)                          │  │    │
│  │   │  • Personalize (öneriler)                                  │  │    │
│  │   └─────────────────────────────────────────────────────────────┘  │    │
│  │                                                                     │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │                      ANALİTİKS SERVİSLERİ                           │    │
│  │                                                                     │    │
│  │   Data Collection        Data Processing       Data Analysis        │    │
│  │   ┌─────────────┐       ┌─────────────┐       ┌─────────────┐      │    │
│  │   │  Kinesis    │ ───►  │   Glue      │ ───►  │  Athena     │      │    │
│  │   │  (streaming)│       │   (ETL)     │       │  (SQL)      │      │    │
│  │   └─────────────┘       └─────────────┘       └─────────────┘      │    │
│  │                                                      │              │    │
│  │                                                      ▼              │    │
│  │                               ┌─────────────────────────────────┐  │    │
│  │   Data Warehouse             │   QuickSight / Redshift         │  │    │
│  │   ┌─────────────┐            │   (Visualization/Warehousing)   │  │    │
│  │   │  Redshift   │◄───────────┘                                  │  │    │
│  │   └─────────────┘                                               │  │    │
│  │                                                                     │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## AWS AI/ML Servisleri

### AI/ML Servis Seçim Rehberi

```
┌─────────────────────────────────────────────────────────────────┐
│                  AI/ML SERVİS SEÇİM REHBERİ                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Kendi modelimi eğitmek istiyorum:                             │
│  ──────────────────────────────────                            │
│  ──────►  Amazon SageMaker                                      │
│                                                                 │
│  Hazır AI servisi kullanmak istiyorum:                         │
│  ─────────────────────────────────────                         │
│  • Görüntü/video analizi    ──────►  Rekognition               │
│  • Metin analizi (NLP)      ──────►  Comprehend                │
│  • Metin-sese dönüşüm       ──────►  Polly                     │
│  • Ses-metne dönüşüm        ──────►  Transcribe                │
│  • Dil çevirisi             ──────►  Translate                 │
│  • Chatbot/Sesli asistan    ──────►  Lex                       │
│  • Belge OCR                ──────►  Textract                  │
│  • Akıllı arama             ──────►  Kendra                    │
│  • Öneri sistemi            ──────►  Personalize               │
│  • Tahminleme               ──────►  Forecast                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Amazon SageMaker

> **Amazon SageMaker** - Tam yönetilen ML platformu.

### SageMaker Nedir?

```
┌─────────────────────────────────────────────────────────────────┐
│                      AMAZON SAGEMAKER                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "End-to-end makine öğrenimi platformu"                        │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                   ML WORKFLOW                              │ │
│  │                                                            │ │
│  │  1. PREPARE         2. BUILD          3. TRAIN & TUNE      │ │
│  │  ┌──────────┐      ┌──────────┐      ┌──────────┐         │ │
│  │  │ Ground   │ ───► │ Studio   │ ───► │ Training │         │ │
│  │  │ Truth    │      │ Notebooks│      │ Jobs     │         │ │
│  │  │ (Label)  │      │          │      │          │         │ │
│  │  └──────────┘      └──────────┘      └──────────┘         │ │
│  │       │                                    │               │ │
│  │       │                                    ▼               │ │
│  │       │            4. DEPLOY          5. MANAGE            │ │
│  │       │           ┌──────────┐      ┌──────────┐          │ │
│  │       └─────────► │ Endpoints│ ───► │ Monitor  │          │ │
│  │                   │ (Hosting)│      │ (MLOps)  │          │ │
│  │                   └──────────┘      └──────────┘          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Temel Özellikler:                                             │
│  • Jupyter Notebooks (SageMaker Studio)                       │
│  • Built-in algorithms                                         │
│  • Custom algorithms (TensorFlow, PyTorch, etc.)              │
│  • Automatic model tuning (hyperparameter)                    │
│  • One-click deployment                                        │
│  • A/B testing                                                 │
│  • Auto-scaling endpoints                                      │
│                                                                 │
│  SageMaker JumpStart:                                          │
│  • Pre-trained model hub                                      │
│  • Foundation models (LLMs)                                   │
│  • Hızlı başlangıç templates                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### SageMaker Bileşenleri

```
┌─────────────────────────────────────────────────────────────────┐
│                   SAGEMAKER BİLEŞENLERİ                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  SageMaker Ground Truth                                        │
│  ──────────────────────                                        │
│  • Veri etiketleme servisi                                    │
│  • İnsan + ML hibrit yaklaşımı                                │
│  • Görüntü, metin, video etiketleme                           │
│                                                                 │
│  SageMaker Studio                                              │
│  ─────────────────                                             │
│  • Web tabanlı IDE                                            │
│  • Jupyter notebooks                                           │
│  • Tüm ML workflow bir arada                                  │
│                                                                 │
│  SageMaker Autopilot                                           │
│  ────────────────────                                          │
│  • AutoML - Otomatik model seçimi                             │
│  • Otomatik hyperparameter tuning                             │
│  • Minimum ML bilgisi ile model oluşturma                     │
│                                                                 │
│  SageMaker Canvas                                              │
│  ────────────────                                              │
│  • No-code ML                                                  │
│  • Business analistler için                                   │
│  • Visual interface                                            │
│                                                                 │
│  SageMaker Model Monitor                                       │
│  ────────────────────────                                      │
│  • Model drift detection                                      │
│  • Data quality monitoring                                    │
│  • Otomatik alertler                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## AI Servisleri (Pre-trained)

### Amazon Rekognition

```
┌─────────────────────────────────────────────────────────────────┐
│                    AMAZON REKOGNITION                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Görüntü ve video analizi AI servisi"                         │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Input (Image/Video)                                      │ │
│  │   ┌────────────────────┐                                  │ │
│  │   │       📷 🎬        │                                  │ │
│  │   └─────────┬──────────┘                                  │ │
│  │             │                                              │ │
│  │             ▼                                              │ │
│  │   ┌────────────────────┐                                  │ │
│  │   │   Rekognition API  │                                  │ │
│  │   └─────────┬──────────┘                                  │ │
│  │             │                                              │ │
│  │   ┌─────────┴─────────────────────────────────────────┐   │ │
│  │   │                                                    │   │ │
│  │   ▼              ▼              ▼              ▼       │   │ │
│  │ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────────┐   │   │ │
│  │ │ Object  │ │  Face   │ │  Text   │ │Content      │   │   │ │
│  │ │Detection│ │Analysis │ │Detection│ │Moderation   │   │   │ │
│  │ │         │ │         │ │ (OCR)   │ │             │   │   │ │
│  │ └─────────┘ └─────────┘ └─────────┘ └─────────────┘   │   │ │
│  │                                                        │   │ │
│  │   ▼              ▼              ▼                      │   │ │
│  │ ┌─────────┐ ┌─────────┐ ┌─────────────────────┐       │   │ │
│  │ │Celebrity│ │ Custom  │ │ Video Analysis      │       │   │ │
│  │ │Recog.   │ │ Labels  │ │ (Person tracking,   │       │   │ │
│  │ │         │ │         │ │  Activity detection)│       │   │ │
│  │ └─────────┘ └─────────┘ └─────────────────────┘       │   │ │
│  │                                                        │   │ │
│  └────────────────────────────────────────────────────────┘   │ │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Yüz tanıma ve doğrulama                                    │
│  • İçerik moderasyonu (uygunsuz içerik tespiti)              │
│  • Ünlü tanıma                                                 │
│  • Nesne ve sahne tespiti                                     │
│  • Metin okuma (görüntüdeki yazılar)                          │
│  • PPE (Personal Protective Equipment) tespiti                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon Comprehend

```
┌─────────────────────────────────────────────────────────────────┐
│                    AMAZON COMPREHEND                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Doğal dil işleme (NLP) servisi"                              │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Input Text:                                              │ │
│  │   "AWS cloud hizmetlerinden çok memnunum. Harika servis!" │ │
│  │                                                            │ │
│  │   ┌─────────────────────────────────────────────────────┐ │ │
│  │   │               COMPREHEND ANALİZİ                     │ │ │
│  │   ├─────────────────────────────────────────────────────┤ │ │
│  │   │                                                      │ │ │
│  │   │  Sentiment Analysis:     POSITIVE (0.95 confidence) │ │ │
│  │   │                                                      │ │ │
│  │   │  Entities:               AWS (ORGANIZATION)         │ │ │
│  │   │                          cloud (SERVICE)            │ │ │
│  │   │                                                      │ │ │
│  │   │  Key Phrases:            "AWS cloud hizmetleri"     │ │ │
│  │   │                          "Harika servis"            │ │ │
│  │   │                                                      │ │ │
│  │   │  Language:               Turkish (tr)               │ │ │
│  │   │                                                      │ │ │
│  │   │  Syntax:                 Part-of-speech tagging     │ │ │
│  │   │                                                      │ │ │
│  │   └─────────────────────────────────────────────────────┘ │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Duygu analizi (Sentiment Analysis)                         │
│  • Varlık tanıma (Entity Recognition)                         │
│  • Anahtar kelime çıkarımı                                    │
│  • Dil tespiti                                                 │
│  • Konu modelleme                                             │
│  • Custom classification                                       │
│                                                                 │
│  Comprehend Medical:                                           │
│  • Tıbbi metin analizi                                        │
│  • İlaç, hastalık, tedavi tespiti                            │
│  • HIPAA uyumlu                                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon Polly ve Transcribe

```
┌─────────────────────────────────────────────────────────────────┐
│                AMAZON POLLY vs TRANSCRIBE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AMAZON POLLY (Text-to-Speech)                                 │
│  ─────────────────────────────                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Input:  "Merhaba, AWS'e hoş geldiniz."                  │ │
│  │              │                                             │ │
│  │              ▼                                             │ │
│  │         ┌─────────┐                                        │ │
│  │         │  Polly  │                                        │ │
│  │         └────┬────┘                                        │ │
│  │              │                                             │ │
│  │              ▼                                             │ │
│  │   Output: 🔊 MP3/OGG ses dosyası                          │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • 60+ dilde doğal ses                                        │
│  • Neural TTS (daha doğal)                                    │
│  • SSML desteği (konuşma kontrolü)                           │
│  • Lexicons (özel telaffuz)                                   │
│  • Real-time streaming                                        │
│                                                                 │
│  ─────────────────────────────────────────────────────────────│
│                                                                 │
│  AMAZON TRANSCRIBE (Speech-to-Text)                            │
│  ──────────────────────────────────                            │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Input: 🔊 Ses dosyası veya stream                       │ │
│  │              │                                             │ │
│  │              ▼                                             │ │
│  │         ┌───────────┐                                      │ │
│  │         │Transcribe │                                      │ │
│  │         └─────┬─────┘                                      │ │
│  │               │                                            │ │
│  │               ▼                                            │ │
│  │   Output: "Merhaba, AWS'e hoş geldiniz."                  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Automatic speech recognition (ASR)                         │
│  • 100+ dil desteği                                           │
│  • Real-time transcription                                    │
│  • Speaker identification                                      │
│  • Custom vocabulary                                           │
│  • Redaction (hassas bilgi maskeleme)                        │
│                                                                 │
│  Transcribe Medical:                                           │
│  • Tıbbi diksiyon                                             │
│  • HIPAA uyumlu                                               │
│                                                                 │
│  Transcribe Call Analytics:                                    │
│  • Call center analitiği                                      │
│  • Duygu analizi                                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon Translate ve Lex

```
┌─────────────────────────────────────────────────────────────────┐
│                 AMAZON TRANSLATE ve LEX                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AMAZON TRANSLATE                                              │
│  ─────────────────                                             │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   "Hello, welcome to AWS"  ───►  "Merhaba, AWS'e hoşgeldiniz"│
│  │                                                            │ │
│  │   • Nöral makine çevirisi                                  │ │
│  │   • 75+ dil çifti                                          │ │
│  │   • Real-time ve batch translation                         │ │
│  │   • Custom terminology                                      │ │
│  │   • Active Custom Translation (kendi modelinle)           │ │
│  │                                                            │ │
│  │   Kullanım Alanları:                                       │ │
│  │   • Web sitesi lokalizasyonu                               │ │
│  │   • Müşteri iletişimi                                      │ │
│  │   • Cross-language search                                  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  AMAZON LEX                                                    │
│  ──────────                                                    │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   "Alexa'yı güçlendiren teknoloji"                        │ │
│  │                                                            │ │
│  │   ┌───────────────────────────────────────────────────┐   │ │
│  │   │                  CHATBOT                           │   │ │
│  │   │                                                    │   │ │
│  │   │   User: "Yarın için uçuş rezervasyonu yap"        │   │ │
│  │   │                     │                              │   │ │
│  │   │           ┌─────────▼─────────┐                   │   │ │
│  │   │           │       LEX         │                   │   │ │
│  │   │           │  ┌─────────────┐  │                   │   │ │
│  │   │           │  │   Intent:   │  │                   │   │ │
│  │   │           │  │ BookFlight  │  │                   │   │ │
│  │   │           │  │             │  │                   │   │ │
│  │   │           │  │   Slot:     │  │                   │   │ │
│  │   │           │  │ Date:Yarın  │  │                   │   │ │
│  │   │           │  └─────────────┘  │                   │   │ │
│  │   │           └─────────┬─────────┘                   │   │ │
│  │   │                     │                              │   │ │
│  │   │           ┌─────────▼─────────┐                   │   │ │
│  │   │           │   Lambda/Backend  │                   │   │ │
│  │   │           │   Integration     │                   │   │ │
│  │   │           └───────────────────┘                   │   │ │
│  │   │                                                    │   │ │
│  │   └───────────────────────────────────────────────────┘   │ │
│  │                                                            │ │
│  │   Özellikler:                                             │ │
│  │   • Natural language understanding (NLU)                  │ │
│  │   • Voice & text chatbots                                 │ │
│  │   • Multi-turn conversations                              │ │
│  │   • Lambda entegrasyonu                                   │ │
│  │   • Connect entegrasyonu (call center)                   │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon Textract ve Kendra

```
┌─────────────────────────────────────────────────────────────────┐
│                 AMAZON TEXTRACT ve KENDRA                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AMAZON TEXTRACT                                               │
│  ───────────────                                               │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   "Belgelerden metin, form ve tablo çıkarımı"             │ │
│  │                                                            │ │
│  │   ┌─────────────┐      ┌─────────────────────────────┐    │ │
│  │   │   📄 PDF    │ ───► │  Textract                    │    │ │
│  │   │   📷 Image  │      │  ┌───────────────────────┐  │    │ │
│  │   │   📑 Scan   │      │  │ • Raw text            │  │    │ │
│  │   └─────────────┘      │  │ • Tables (structured) │  │    │ │
│  │                        │  │ • Forms (key-value)   │  │    │ │
│  │                        │  │ • Signatures          │  │    │ │
│  │                        │  └───────────────────────┘  │    │ │
│  │                        └─────────────────────────────┘    │ │
│  │                                                            │ │
│  │   OCR'dan fazlası:                                        │ │
│  │   • Tablo yapısını anlar                                  │ │
│  │   • Form alanlarını eşler                                 │ │
│  │   • Handwriting recognition                               │ │
│  │   • ID document parsing                                   │ │
│  │   • Expense/Invoice analysis                              │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  AMAZON KENDRA                                                 │
│  ─────────────                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   "ML destekli akıllı arama servisi"                      │ │
│  │                                                            │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │                    KENDRA                          │  │ │
│  │   │                                                    │  │ │
│  │   │   Veri Kaynakları:                                │  │ │
│  │   │   ┌────┐ ┌────┐ ┌─────┐ ┌───────┐ ┌────────┐     │  │ │
│  │   │   │ S3 │ │RDS │ │Share│ │Service│ │Confluenc│    │  │ │
│  │   │   │    │ │    │ │Point│ │ Now   │ │        │     │  │ │
│  │   │   └────┘ └────┘ └─────┘ └───────┘ └────────┘     │  │ │
│  │   │              │                                    │  │ │
│  │   │              ▼                                    │  │ │
│  │   │   Query: "What is the refund policy?"            │  │ │
│  │   │              │                                    │  │ │
│  │   │              ▼                                    │  │ │
│  │   │   Result: Relevant excerpts + document links     │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  │   Özellikler:                                             │ │
│  │   • Natural language queries                              │ │
│  │   • Semantic search (anlam bazlı)                        │ │
│  │   • 14+ data source connectors                           │ │
│  │   • Document ranking                                      │ │
│  │   • FAQ support                                           │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Diğer AI Servisleri

```
┌─────────────────────────────────────────────────────────────────┐
│                    DİĞER AI SERVİSLERİ                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Amazon Personalize                                            │
│  ──────────────────                                            │
│  • Kişiselleştirilmiş öneri sistemi                           │
│  • "Bu ürünü alanlar şunları da aldı"                         │
│  • E-commerce, medya, retail                                  │
│  • Amazon.com tavsiye motoruyla aynı teknoloji               │
│                                                                 │
│  Amazon Forecast                                               │
│  ────────────────                                              │
│  • Zaman serisi tahminleme                                    │
│  • Satış tahmini, talep planlama                             │
│  • Envanter optimizasyonu                                     │
│  • Amazon'un retail tahmini teknolojisi                      │
│                                                                 │
│  Amazon Fraud Detector                                         │
│  ─────────────────────                                         │
│  • Sahtecilik tespiti                                         │
│  • Yeni hesap dolandırıcılığı                                 │
│  • Ödeme sahtekarlığı                                         │
│  • Amazon'un 20+ yıllık deneyimi                             │
│                                                                 │
│  Amazon DevOps Guru                                            │
│  ──────────────────                                            │
│  • ML destekli operasyon izleme                              │
│  • Anomali tespiti                                            │
│  • Proaktif uyarılar                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## AWS Analytics Servisleri

### Amazon Athena

```
┌─────────────────────────────────────────────────────────────────┐
│                       AMAZON ATHENA                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "S3 üzerindeki verileri SQL ile sorgula"                      │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │                    ATHENA                          │  │ │
│  │   │                                                    │  │ │
│  │   │   SELECT * FROM logs                              │  │ │
│  │   │   WHERE date > '2024-01-01'                       │  │ │
│  │   │   AND status_code = 500                           │  │ │
│  │   │                                                    │  │ │
│  │   └───────────────────────┬────────────────────────────┘  │ │
│  │                           │                                │ │
│  │                           ▼                                │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │                 S3 BUCKET                          │  │ │
│  │   │                                                    │  │ │
│  │   │    logs/2024/01/01/data.parquet                   │  │ │
│  │   │    logs/2024/01/02/data.parquet                   │  │ │
│  │   │    logs/2024/01/03/data.parquet                   │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • SERVERLESS - altyapı yönetimi yok                         │
│  • Pay per query (taranan veri başına)                        │
│  • Standard SQL (Presto/Trino tabanlı)                       │
│  • CSV, JSON, Parquet, ORC, Avro formatları                  │
│  • Glue Data Catalog entegrasyonu                            │
│  • QuickSight entegrasyonu                                   │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Log analizi (CloudTrail, VPC Flow Logs, ALB logs)         │
│  • Ad-hoc sorgular                                            │
│  • Business intelligence                                       │
│  • Data lake sorgulama                                        │
│                                                                 │
│  Maliyet Optimizasyonu:                                        │
│  • Columnar format kullan (Parquet)                           │
│  • Partitioning (tarih, bölge vb.)                           │
│  • Compression                                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon Kinesis

```
┌─────────────────────────────────────────────────────────────────┐
│                       AMAZON KINESIS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Real-time streaming data servisleri"                         │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                     KINESIS FAMILY                         │ │
│  │                                                            │ │
│  │  ┌─────────────────────────────────────────────────────┐  │ │
│  │  │  Kinesis Data Streams                               │  │ │
│  │  │  ───────────────────────                            │  │ │
│  │  │  • Custom real-time data processing                 │  │ │
│  │  │  • Shard bazlı ölçekleme                           │  │ │
│  │  │  • 1-365 gün veri saklama                          │  │ │
│  │  │  • Consumer: Lambda, EC2, KDA                      │  │ │
│  │  │                                                     │  │ │
│  │  │  Producers ───► Shards ───► Consumers              │  │ │
│  │  │  (apps, SDK)   (partition)  (processing)           │  │ │
│  │  └─────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  │  ┌─────────────────────────────────────────────────────┐  │ │
│  │  │  Kinesis Data Firehose                              │  │ │
│  │  │  ─────────────────────────                          │  │ │
│  │  │  • Streaming ETL servisi                           │  │ │
│  │  │  • Otomatik ölçekleme (serverless)                 │  │ │
│  │  │  • Near real-time (min 60 saniye buffer)           │  │ │
│  │  │                                                     │  │ │
│  │  │  Producers ───► Firehose ───► Destinations          │  │ │
│  │  │                    │         S3, Redshift,          │  │ │
│  │  │                Transform     OpenSearch,            │  │ │
│  │  │                (Lambda)      HTTP endpoint          │  │ │
│  │  └─────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  │  ┌─────────────────────────────────────────────────────┐  │ │
│  │  │  Kinesis Data Analytics                             │  │ │
│  │  │  ───────────────────────────                        │  │ │
│  │  │  • SQL veya Apache Flink ile stream analizi        │  │ │
│  │  │  • Real-time dashboards                            │  │ │
│  │  │  • Anomaly detection                               │  │ │
│  │  │  • Time-series analytics                           │  │ │
│  │  └─────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  │  ┌─────────────────────────────────────────────────────┐  │ │
│  │  │  Kinesis Video Streams                              │  │ │
│  │  │  ───────────────────────                            │  │ │
│  │  │  • Video streaming ve saklama                      │  │ │
│  │  │  • Live ve on-demand playback                      │  │ │
│  │  │  • Computer vision entegrasyonu                    │  │ │
│  │  └─────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Seçim Rehberi:                                                │
│  ┌──────────────────────┬─────────────────────────────────┐   │
│  │ İhtiyaç              │ Servis                          │   │
│  ├──────────────────────┼─────────────────────────────────┤   │
│  │ Custom processing    │ Kinesis Data Streams            │   │
│  │ S3/Redshift'e yükle  │ Kinesis Data Firehose          │   │
│  │ SQL ile analiz       │ Kinesis Data Analytics          │   │
│  │ Video streaming      │ Kinesis Video Streams           │   │
│  └──────────────────────┴─────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon QuickSight

```
┌─────────────────────────────────────────────────────────────────┐
│                     AMAZON QUICKSIGHT                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Serverless BI ve görselleştirme servisi"                     │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Veri Kaynakları                                         │ │
│  │   ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌─────┐ ┌─────┐            │ │
│  │   │ S3 │ │RDS │ │Red-│ │Athe│ │Excel│ │Sales│            │ │
│  │   │    │ │    │ │shif│ │na  │ │     │ │force│            │ │
│  │   └──┬─┘ └──┬─┘ └──┬─┘ └──┬─┘ └──┬──┘ └──┬──┘            │ │
│  │      │      │      │      │      │       │                │ │
│  │      └──────┴──────┴──────┴──────┴───────┘                │ │
│  │                        │                                   │ │
│  │                        ▼                                   │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │               QUICKSIGHT                           │  │ │
│  │   │                                                    │  │ │
│  │   │   SPICE Engine                                    │  │ │
│  │   │   (Super-fast, Parallel, In-memory                │  │ │
│  │   │    Calculation Engine)                            │  │ │
│  │   │                                                    │  │ │
│  │   └───────────────────────┬────────────────────────────┘  │ │
│  │                           │                                │ │
│  │                           ▼                                │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │              DASHBOARDS                            │  │ │
│  │   │                                                    │  │ │
│  │   │   📊 Charts   📈 Graphs   📉 KPIs   📋 Tables     │  │ │
│  │   │                                                    │  │ │
│  │   │   ┌─────────────────────────────────────────────┐ │  │ │
│  │   │   │                                             │ │  │ │
│  │   │   │    Sales Dashboard                          │ │  │ │
│  │   │   │    ┌─────┐ ┌─────┐ ┌─────────────────────┐ │ │  │ │
│  │   │   │    │$45M │ │+12% │ │      📈            │ │ │  │ │
│  │   │   │    │Total│ │ YoY │ │  Trend Chart       │ │ │  │ │
│  │   │   │    └─────┘ └─────┘ └─────────────────────┘ │ │  │ │
│  │   │   │                                             │ │  │ │
│  │   │   └─────────────────────────────────────────────┘ │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Serverless - altyapı yönetimi yok                         │
│  • SPICE in-memory engine                                     │
│  • ML Insights (anomaly detection, forecasting)              │
│  • Embedded dashboards                                        │
│  • Mobile app                                                  │
│  • Pay-per-session pricing                                    │
│  • Q: Natural language queries                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Data Lake ve ETL

### AWS Glue

```
┌─────────────────────────────────────────────────────────────────┐
│                         AWS GLUE                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Serverless ETL ve Data Catalog servisi"                      │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   1. GLUE DATA CATALOG                                    │ │
│  │   ─────────────────────                                   │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │                                                    │  │ │
│  │   │   Databases     Tables        Schemas              │  │ │
│  │   │   ┌─────────┐   ┌─────────┐   ┌─────────┐         │  │ │
│  │   │   │ sales_db│   │ orders  │   │ col: id │         │  │ │
│  │   │   │ logs_db │   │ products│   │ col: name│        │  │ │
│  │   │   │ users_db│   │ users   │   │ col: date│        │  │ │
│  │   │   └─────────┘   └─────────┘   └─────────┘         │  │ │
│  │   │                                                    │  │ │
│  │   │   Merkezi metadata deposu                         │  │ │
│  │   │   Athena, Redshift Spectrum, EMR ile entegre      │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  │   2. GLUE CRAWLERS                                        │ │
│  │   ────────────────                                        │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │                                                    │  │ │
│  │   │   S3, RDS, DynamoDB ───► Crawler ───► Data Catalog│  │ │
│  │   │                                                    │  │ │
│  │   │   • Otomatik şema keşfi                           │  │ │
│  │   │   • Partition detection                           │  │ │
│  │   │   • Schema evolution                              │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  │   3. GLUE ETL JOBS                                        │ │
│  │   ────────────────                                        │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │                                                    │  │ │
│  │   │   Source          Transform          Target        │  │ │
│  │   │   ┌─────┐        ┌─────────┐        ┌─────┐       │  │ │
│  │   │   │ S3  │ ─────► │  Glue   │ ─────► │ Red-│       │  │ │
│  │   │   │ RDS │        │  Jobs   │        │shift│       │  │ │
│  │   │   │JDBC │        │ (Python/│        │ S3  │       │  │ │
│  │   │   └─────┘        │  Scala) │        └─────┘       │  │ │
│  │   │                  └─────────┘                       │  │ │
│  │   │                                                    │  │ │
│  │   │   • Visual ETL (drag-and-drop)                    │  │ │
│  │   │   • Auto-scaling                                  │  │ │
│  │   │   • Serverless Spark                              │  │ │
│  │   │   • Data quality checks                           │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Glue DataBrew:                                                │
│  • No-code veri hazırlama                                     │
│  • Visual data profiling                                      │
│  • 250+ built-in transformations                              │
│  • Data analistler için                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### AWS Lake Formation

```
┌─────────────────────────────────────────────────────────────────┐
│                    AWS LAKE FORMATION                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Data Lake kurulum ve yönetim servisi"                        │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Geleneksel Data Lake:          Lake Formation:          │ │
│  │   ┌──────────────────┐          ┌──────────────────┐      │ │
│  │   │ ✗ Manuel ETL     │          │ ✓ Otomatik ETL   │      │ │
│  │   │ ✗ İzin yönetimi  │   ───►   │ ✓ Merkezi güvenlik│     │ │
│  │   │   karmaşık       │          │ ✓ Veri keşfi     │      │ │
│  │   │ ✗ Haftalar sürer │          │ ✓ Günler içinde  │      │ │
│  │   └──────────────────┘          └──────────────────┘      │ │
│  │                                                            │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │               LAKE FORMATION                        │  │ │
│  │   │                                                     │  │ │
│  │   │   1. Data Sources       2. Blueprints              │  │ │
│  │   │   ┌─────────────────┐   ┌─────────────────┐        │  │ │
│  │   │   │ S3, RDS, JDBC   │   │ Pre-built ETL   │        │  │ │
│  │   │   │ On-premises     │   │ workflows       │        │  │ │
│  │   │   └────────┬────────┘   └────────┬────────┘        │  │ │
│  │   │            │                     │                  │  │ │
│  │   │            └─────────┬───────────┘                  │  │ │
│  │   │                      │                              │  │ │
│  │   │   3. Data Lake (S3)  │  4. Access Control          │  │ │
│  │   │   ┌──────────────────▼──┐  ┌─────────────────────┐ │  │ │
│  │   │   │   Raw   │  Curated  │  │ Fine-grained       │ │  │ │
│  │   │   │   Zone  │   Zone    │  │ column/row level   │ │  │ │
│  │   │   └─────────────────────┘  │ permissions        │ │  │ │
│  │   │                            └─────────────────────┘ │  │ │
│  │   │                                                     │  │ │
│  │   │   5. Analytics                                      │  │ │
│  │   │   ┌────┐ ┌────────┐ ┌────┐ ┌──────────┐           │  │ │
│  │   │   │Athe│ │Redshift│ │EMR │ │QuickSight│           │  │ │
│  │   │   │na  │ │Spectrum│ │    │ │          │           │  │ │
│  │   │   └────┘ └────────┘ └────┘ └──────────┘           │  │ │
│  │   │                                                     │  │ │
│  │   └─────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Özellikler:                                                   │
│  • Merkezi güvenlik ve erişim kontrolü                        │
│  • Cross-account data sharing                                 │
│  • Data quality monitoring                                    │
│  • ML-powered data discovery                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon OpenSearch Service

```
┌─────────────────────────────────────────────────────────────────┐
│                  AMAZON OPENSEARCH SERVICE                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Yönetilen arama ve log analitiği servisi"                   │
│  (Eski adı: Amazon Elasticsearch Service)                      │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   Veri Kaynakları                                         │ │
│  │   ┌──────────┐  ┌───────────┐  ┌────────────────┐        │ │
│  │   │ App Logs │  │ CloudWatch│  │ Kinesis        │        │ │
│  │   │          │  │   Logs    │  │ Firehose       │        │ │
│  │   └────┬─────┘  └─────┬─────┘  └───────┬────────┘        │ │
│  │        │              │                │                   │ │
│  │        └──────────────┼────────────────┘                   │ │
│  │                       │                                    │ │
│  │                       ▼                                    │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │            OPENSEARCH CLUSTER                      │  │ │
│  │   │                                                    │  │ │
│  │   │   ┌─────────────────────────────────────────────┐ │  │ │
│  │   │   │              INDEX                          │ │  │ │
│  │   │   │   ┌──────┐  ┌──────┐  ┌──────┐             │ │  │ │
│  │   │   │   │Shard │  │Shard │  │Shard │             │ │  │ │
│  │   │   │   │  1   │  │  2   │  │  3   │             │ │  │ │
│  │   │   │   └──────┘  └──────┘  └──────┘             │ │  │ │
│  │   │   └─────────────────────────────────────────────┘ │  │ │
│  │   │                                                    │  │ │
│  │   │   Full-text search + Analytics                    │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                       │                                    │ │
│  │                       ▼                                    │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │         OPENSEARCH DASHBOARDS                      │  │ │
│  │   │                                                    │  │ │
│  │   │   📊 Görselleştirme   📈 Dashboard   🔍 Arama     │  │ │
│  │   │                                                    │  │ │
│  │   └────────────────────────────────────────────────────┘  │ │
│  │                                                            │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Log analytics (application/infrastructure logs)            │
│  • Full-text search (e-commerce, website search)             │
│  • Clickstream analytics                                      │
│  • Security analytics (SIEM)                                  │
│                                                                 │
│  OpenSearch Serverless:                                        │
│  • Cluster yönetimi yok                                       │
│  • Otomatik ölçekleme                                         │
│  • Pay-per-use                                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Amazon EMR

```
┌─────────────────────────────────────────────────────────────────┐
│                        AMAZON EMR                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "Elastic MapReduce - Big Data processing platformu"          │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                                                            │ │
│  │   ┌────────────────────────────────────────────────────┐  │ │
│  │   │                  EMR CLUSTER                        │  │ │
│  │   │                                                     │  │ │
│  │   │   ┌─────────────┐                                  │  │ │
│  │   │   │   Master    │                                  │  │ │
│  │   │   │    Node     │                                  │  │ │
│  │   │   └──────┬──────┘                                  │  │ │
│  │   │          │                                          │  │ │
│  │   │   ┌──────┴──────────────────────────┐              │  │ │
│  │   │   │                                 │              │  │ │
│  │   │   ▼              ▼              ▼   │              │  │ │
│  │   │ ┌────────┐  ┌────────┐  ┌────────┐ │              │  │ │
│  │   │ │ Core   │  │ Core   │  │ Task   │ │              │  │ │
│  │   │ │ Node 1 │  │ Node 2 │  │ Node 3 │ │              │  │ │
│  │   │ │        │  │        │  │        │ │              │  │ │
│  │   │ │ HDFS   │  │ HDFS   │  │(compute│ │              │  │ │
│  │   │ │+compute│  │+compute│  │ only)  │ │              │  │ │
│  │   │ └────────┘  └────────┘  └────────┘ │              │  │ │
│  │   │                                     │              │  │ │
│  │   └─────────────────────────────────────┘              │  │ │
│  │                                                         │  │ │
│  │   Desteklenen Frameworkler:                            │  │ │
│  │   ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐         │  │ │
│  │   │ Spark  │ │ Hadoop │ │ Hive   │ │ Presto │         │  │ │
│  │   └────────┘ └────────┘ └────────┘ └────────┘         │  │ │
│  │   ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐         │  │ │
│  │   │  Flink │ │  HBase │ │  Pig   │ │TensorFl│         │  │ │
│  │   └────────┘ └────────┘ └────────┘ └────────┘         │  │ │
│  │                                                         │  │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  Deployment Modları:                                           │
│  • EMR on EC2 (geleneksel)                                    │
│  • EMR on EKS (Kubernetes üzerinde)                           │
│  • EMR Serverless (altyapı yönetimi yok)                      │
│                                                                 │
│  Kullanım Alanları:                                            │
│  • Big data processing                                        │
│  • Data engineering (ETL)                                     │
│  • Machine learning                                           │
│  • Real-time streaming                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Karşılaştırma Tabloları

### AI Servisleri Karşılaştırma

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        AI SERVİSLERİ KARŞILAŞTIRMA                          │
├─────────────────────┬───────────────────────────────────────────────────────┤
│      Servis         │                    Kullanım Alanı                     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ SageMaker           │ Custom ML model training & deployment                 │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Rekognition         │ Görüntü/video: yüz tanıma, nesne tespiti, moderasyon │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Comprehend          │ NLP: duygu analizi, entity extraction, topic modeling│
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Polly               │ Text → Speech (konuşma sentezi)                       │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Transcribe          │ Speech → Text (konuşma tanıma)                        │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Translate           │ Metin çevirisi (75+ dil)                              │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Lex                 │ Chatbot / Conversational AI (Alexa teknolojisi)       │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Textract            │ Belge OCR: form, tablo, el yazısı                     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Kendra              │ Akıllı enterprise arama (ML destekli)                 │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Personalize         │ Öneri sistemi (Amazon.com teknolojisi)                │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Forecast            │ Zaman serisi tahminleme                               │
└─────────────────────┴───────────────────────────────────────────────────────┘
```

### Analytics Servisleri Karşılaştırma

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    ANALİTİKS SERVİSLERİ KARŞILAŞTIRMA                       │
├─────────────────────┬───────────────────────────────────────────────────────┤
│      Servis         │                    Kullanım Alanı                     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Athena              │ S3'teki veriyi serverless SQL ile sorgula            │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Redshift            │ Data warehouse (OLAP, petabyte ölçeği)               │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ QuickSight          │ BI dashboards, serverless görselleştirme             │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Kinesis Data Streams│ Real-time custom streaming processing                │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Kinesis Firehose    │ Streaming ETL, S3/Redshift'e yükleme                 │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Kinesis Analytics   │ SQL/Flink ile stream analizi                         │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Glue                │ Serverless ETL + Data Catalog                        │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ Lake Formation      │ Data lake kurulum ve yönetim                         │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ OpenSearch          │ Log analytics + full-text search                     │
├─────────────────────┼───────────────────────────────────────────────────────┤
│ EMR                 │ Big data processing (Spark, Hadoop)                  │
└─────────────────────┴───────────────────────────────────────────────────────┘
```

---

## Sınav Senaryoları

### Senaryo 1: Görüntü Analizi

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: E-ticaret sitesi ürün fotoğraflarındaki uygunsuz         │
│ içeriği otomatik tespit etmek istiyor. Hangi servis?           │
├─────────────────────────────────────────────────────────────────┤
│ A) Amazon Comprehend                                            │
│ B) Amazon Rekognition                                           │
│ C) Amazon Textract                                              │
│ D) Amazon SageMaker                                             │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) Amazon Rekognition                                    │
│                                                                 │
│ Neden?                                                          │
│ • "Görüntü" = Rekognition                                      │
│ • "Uygunsuz içerik tespiti" = Content Moderation API           │
│ • Comprehend = metin analizi                                   │
│ • Textract = belge OCR                                         │
│ • SageMaker = custom model (gereksiz karmaşıklık)              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 2: Log Analizi

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Şirket S3'te saklanan application log'larını analiz      │
│ etmek istiyor. Sunucu yönetmek istemiyor ve SQL kullanmak      │
│ istiyor. En uygun çözüm?                                        │
├─────────────────────────────────────────────────────────────────┤
│ A) Amazon Redshift                                              │
│ B) Amazon Athena                                                │
│ C) Amazon EMR                                                   │
│ D) Amazon RDS                                                   │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) Amazon Athena                                         │
│                                                                 │
│ Neden?                                                          │
│ • "S3'teki veri" = Athena ideal                                │
│ • "Sunucu yönetimi yok" = Athena serverless                    │
│ • "SQL" = Athena standard SQL destekler                        │
│ • Redshift = cluster yönetimi gerekir                          │
│ • EMR = cluster yönetimi gerekir                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 3: Chatbot

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: Müşteri hizmetleri için sesli ve yazılı chatbot          │
│ oluşturmak isteniyor. Mevcut backend sistemlerle               │
│ entegre olmalı. Hangi servis?                                   │
├─────────────────────────────────────────────────────────────────┤
│ A) Amazon Polly                                                 │
│ B) Amazon Lex                                                   │
│ C) Amazon Transcribe                                            │
│ D) Amazon Connect                                               │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) Amazon Lex                                            │
│                                                                 │
│ Neden?                                                          │
│ • "Chatbot" = Lex (conversational AI)                          │
│ • "Sesli ve yazılı" = Lex her ikisini destekler               │
│ • "Backend entegrasyonu" = Lex Lambda integration              │
│ • Polly = sadece text-to-speech                                │
│ • Transcribe = sadece speech-to-text                           │
│ • Connect = call center platformu (Lex ile entegre edilir)     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Senaryo 4: Real-time Analytics

```
┌─────────────────────────────────────────────────────────────────┐
│ SORU: IoT cihazlarından gelen real-time veriyi işleyip S3'e    │
│ kaydetmek isteniyor. Minimum operasyon overhead istenyor.       │
│ Hangi servis?                                                   │
├─────────────────────────────────────────────────────────────────┤
│ A) Kinesis Data Streams                                         │
│ B) Kinesis Data Firehose                                        │
│ C) Amazon SQS                                                   │
│ D) Amazon EMR                                                   │
├─────────────────────────────────────────────────────────────────┤
│ CEVAP: B) Kinesis Data Firehose                                 │
│                                                                 │
│ Neden?                                                          │
│ • "S3'e kaydet" = Firehose otomatik yapar                      │
│ • "Minimum operasyon" = Firehose fully managed                 │
│ • Kinesis Data Streams = custom processing gerekir             │
│ • SQS = mesaj kuyruğu, streaming değil                         │
│ • EMR = big data processing, cluster yönetimi gerekir          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Özet Kartları

### Hızlı Referans

```
┌─────────────────────────────────────────────────────────────────┐
│                    HIZLI REFERANS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AI SERVİSLERİ:                                                │
│  • SageMaker = Custom ML model train/deploy                    │
│  • Rekognition = Görüntü/video analizi                        │
│  • Comprehend = NLP, duygu analizi                            │
│  • Polly = Text → Speech                                       │
│  • Transcribe = Speech → Text                                  │
│  • Translate = Dil çevirisi                                   │
│  • Lex = Chatbot (Alexa)                                      │
│  • Textract = Belge OCR                                       │
│  • Kendra = Enterprise arama                                  │
│                                                                 │
│  ANALİTİKS:                                                    │
│  • Athena = S3'te serverless SQL                              │
│  • Redshift = Data warehouse (OLAP)                           │
│  • QuickSight = BI dashboards                                 │
│  • Kinesis = Real-time streaming                              │
│    - Data Streams = Custom processing                          │
│    - Firehose = S3/Redshift'e yükleme                         │
│    - Analytics = SQL/Flink analiz                              │
│  • Glue = ETL + Data Catalog                                  │
│  • EMR = Big data (Spark, Hadoop)                             │
│  • OpenSearch = Log analytics + search                        │
│                                                                 │
│  ANAHTAR KELIMELER:                                            │
│  • "Görüntü analizi" → Rekognition                            │
│  • "Metin analizi/NLP" → Comprehend                           │
│  • "Chatbot" → Lex                                            │
│  • "S3 + SQL" → Athena                                        │
│  • "Data warehouse" → Redshift                                │
│  • "Real-time streaming" → Kinesis                            │
│  • "ETL" → Glue                                               │
│  • "BI/Dashboard" → QuickSight                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Kaynaklar

- [Amazon SageMaker Documentation](https://docs.aws.amazon.com/sagemaker/)
- [Amazon Rekognition Documentation](https://docs.aws.amazon.com/rekognition/)
- [Amazon Athena User Guide](https://docs.aws.amazon.com/athena/)
- [Amazon Kinesis Documentation](https://docs.aws.amazon.com/kinesis/)
- [Amazon QuickSight User Guide](https://docs.aws.amazon.com/quicksight/)
- [AWS Glue Developer Guide](https://docs.aws.amazon.com/glue/)
- [CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

---

> **Sınav İpucu:** AI servisleri soruları genellikle anahtar kelime bazlı! "Görüntü" = Rekognition, "Metin/NLP" = Comprehend, "Ses" = Polly/Transcribe, "Chatbot" = Lex. Analytics için "S3 + SQL" = Athena, "Real-time" = Kinesis.

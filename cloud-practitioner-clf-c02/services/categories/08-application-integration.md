# Application Integration Services (In-Scope)

> **Kategori:** Uygulama Entegrasyon Servisleri

## Amazon SQS (Simple Queue Service)

**Ne Ise Yarar:** Tam yonetilen mesaj kuyrugu.

**Kuyruk Turleri:**
| Tur | Ozellik |
|-----|---------|
| Standard | Yuksek throughput, sira garantisi yok |
| FIFO | Sirali teslim, 300 msg/sn |

**Ne Zaman Kullanilir:**
- Microservice'ler arasi iletisim
- Is yuku dengeleme
- Asenkron isleme

---

## Amazon SNS (Simple Notification Service)

**Ne Ise Yarar:** Pub/Sub mesajlasma ve push bildirimleri.

**Aboneler:**
- Email
- SMS
- HTTP/HTTPS
- SQS
- Lambda

**SQS vs SNS:**
| Ozellik | SQS | SNS |
|---------|-----|-----|
| Model | Queue (pull) | Pub/Sub (push) |
| Tuketici | Tek | Coklu |
| Kalicilik | Mesaj kuyrukta kalir | Aninda iletilir |

---

## Amazon EventBridge

**Ne Ise Yarar:** Serverless event bus (olay yonlendirme).

**Olay Kaynaklari:**
- AWS servisleri
- SaaS uygulamalari
- Custom uygulamalar

**Ne Zaman Kullanilir:**
- Event-driven mimariler
- Servisler arasi entegrasyon
- Olay tabanli otomasyon

---

## AWS Step Functions

**Ne Ise Yarar:** Gorsel workflow orchestration.

**Ne Zaman Kullanilir:**
- Lambda fonksiyonlarini zincirleme
- Karmasik is akislari
- ETL pipeline'lari
- Mikroservis koordinasyonu


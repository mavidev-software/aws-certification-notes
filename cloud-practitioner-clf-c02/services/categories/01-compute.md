# Compute Services (In-Scope)

> **Kategori:** Hesaplama Servisleri

## Amazon EC2 (Elastic Compute Cloud)

**Ne İşe Yarar:** AWS'de sanal sunucular (instance) oluşturmanızı sağlar.

**Temel Özellikler:**
- İstediğiniz işletim sistemini seçebilirsiniz (Linux, Windows, macOS)
- Farklı instance tipleri: General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, Accelerated Computing
- On-Demand, Reserved, Spot, Dedicated Host gibi fiyatlandırma seçenekleri

**Ne Zaman Kullanılır:**
- Web sunucuları barındırmak
- Uygulama sunucuları çalıştırmak
- Geliştirme/test ortamları oluşturmak

**Fiyatlandırma Modelleri:**
| Model | Açıklama | İndirim |
|-------|----------|---------|
| On-Demand | Kullandığın kadar öde | - |
| Reserved Instance | 1-3 yıllık taahhüt | %72'ye kadar |
| Spot Instance | Boşta kalan kapasiteyi kullan | %90'a kadar |
| Savings Plans | Esnek taahhüt | %72'ye kadar |
| Dedicated Host | Fiziksel sunucu kiralama | Lisans uyumluluğu |

---

## AWS Lambda

**Ne İşe Yarar:** Sunucu yönetmeden kod çalıştırmanızı sağlar (Serverless).

**Temel Özellikler:**
- Sadece kod çalışma süresi kadar ödeme yaparsınız
- Otomatik ölçeklenir
- Python, Node.js, Java, Go, C#, Ruby destekler
- Maksimum 15 dakika çalışma süresi

**Ne Zaman Kullanılır:**
- Event-driven (olay tabanlı) uygulamalar
- API backend'leri
- Veri işleme pipeline'ları
- Zamanlanmış görevler (cron jobs)

---

## AWS Elastic Beanstalk

**Ne İşe Yarar:** Web uygulamalarını kolayca dağıtmanızı ve yönetmenizi sağlar (PaaS).

**Temel Özellikler:**
- Altyapıyı otomatik olarak yönetir (EC2, Load Balancer, Auto Scaling)
- Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker destekler
- Ücret sadece kullandığınız kaynaklar için alınır

**Ne Zaman Kullanılır:**
- Hızlı uygulama dağıtımı gerektiğinde
- Altyapı yönetimi ile uğraşmak istemediğinizde

---

## Amazon Lightsail

**Ne İşe Yarar:** Basit sanal özel sunucu (VPS) hizmeti sunar.

**Temel Özellikler:**
- Sabit aylık fiyatlandırma
- Önceden yapılandırılmış uygulamalar (WordPress, LAMP, Node.js vb.)
- Kolay kullanım arayüzü

**Ne Zaman Kullanılır:**
- Basit web siteleri
- Blog platformları
- Küçük e-ticaret siteleri

---

## AWS Batch

**Ne İşe Yarar:** Büyük ölçekli toplu işleme (batch processing) işlerini çalıştırır.

**Temel Özellikler:**
- Binlerce işi paralel çalıştırabilir
- Otomatik kaynak ölçekleme
- Spot Instance desteği ile maliyet optimizasyonu

**Ne Zaman Kullanılır:**
- Büyük veri analizi
- Video/görüntü işleme
- Bilimsel simülasyonlar

---

## AWS Outposts

**Ne İşe Yarar:** AWS altyapısını kendi veri merkezinizde çalıştırmanızı sağlar.

**Temel Özellikler:**
- Hybrid cloud çözümü
- AWS servisleri on-premises'te çalışır
- Düşük gecikme süresi gerektiren uygulamalar için

**Ne Zaman Kullanılır:**
- Veri yerelliği gereksinimleri
- Düşük gecikme süresi gerektiğinde

---

## AWS Fargate

**Ne İşe Yarar:** Serverless container çalıştırma.

**Temel Özellikler:**
- EC2 instance yönetimi yok
- ECS ve EKS ile kullanılır
- Kullandığın kadar öde

**Ne Zaman Kullanılır:**
- Container workload'ları
- Microservices mimarisi

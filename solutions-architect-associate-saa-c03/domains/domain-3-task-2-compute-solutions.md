# Domain 3 — Task Statement 2: Design High-Performing and Elastic Compute Solutions

---

## 2. Önemli Keywords (Sınav Odaklı)

### 3 Compute Biçimi

| Biçim | Servisler | Özellik |
|-------|-----------|---------|
| **Instances** | EC2 | Sanal sunucular, farklı aile/boyut, tam kontrol |
| **Containers** | ECS, EKS, Fargate | Hafif, taşınabilir, mikro servisler için ideal |
| **Functions** | Lambda | Sunucusuz, olay odaklı, süre bazlı faturalandırma |

### Amazon EC2

| Keyword | Sınav Notu |
|---------|-----------|
| **Instance Families** | Compute, memory, storage, accelerated, general purpose — iş yüküne göre seçin |
| **Doğası gereği ölçeklenebilir DEĞİL** | Auto Scaling + ELB ile ölçeklenebilir hale getirilir |
| **Instance type seçimi** | CPU, bellek, ağ bant genişliği, depolama türü — uygulama ihtiyacına göre |
| **Her türü ezberleme** | Aile mantığını anla, tek tek türleri değil |

### Konteynerler (ECS / EKS)

| Keyword | Sınav Notu |
|---------|-----------|
| **ECS** | AWS'nin kendi konteyner orkestrasyonu |
| **EKS** | AWS üzerinde Kubernetes |
| **ECS + EC2** | Bilişim ortamı üzerinde tam kontrol istiyorsan |
| **ECS + Fargate** | Sunucusuz konteyner — altyapı yönetimi yok |
| **ALB + port mapping** | Konteynerlerle entegrasyon |

### Lambda (Functions)

| Keyword | Sınav Notu |
|---------|-----------|
| **15 dakika limit** | Daha uzun süre → Step Functions |
| **Olay odaklı (event-driven)** | Bir olay tetikler, Lambda çalışır |
| **Süre bazlı faturalandırma** | Yürütme süresi kadar ödeme |
| **Doğası gereği ölçeklenebilir** | Ölçeklendirme tasarımı gerekmez |
| **Lambda@Edge** | CloudFront edge location'larında çalıştırma — düşük gecikme |

### Ne Zaman Hangisi?

| Senaryo | Seçim |
|---------|-------|
| Tam kontrol, özel yapılandırma, uzun çalışan süreçler | **EC2** |
| Mikro servisler, taşınabilirlik, tutarlı ortam | **ECS/EKS** |
| Altyapı yönetimi istemiyorsan + konteyner | **Fargate** |
| Kısa süreli, olay odaklı, ≤15 dk | **Lambda** |
| 15 dk'dan uzun iş akışları | **Step Functions** |
| Global düşük gecikme + Lambda | **Lambda@Edge** |

### CloudWatch ve Ölçeklendirme Metrikleri

| Keyword | Sınav Notu |
|---------|-----------|
| **CloudWatch Metrics** | Ölçeklendirme kararlarının temeli |
| **CloudWatch Alarms** | Eşik aşılınca tetiklenir → Auto Scaling aksiyonu |
| **Varsayılan metrikler** | CPU, disk, network — bunlar otomatik gelir |
| **Bellek (memory) = varsayılan DEĞİL** | Custom metric olarak eklenmelidir — sınavda sık çıkar! |
| **Custom Metrics** | Kendiniz tanımlayabilirsiniz |
| **ELB Metrics** | HealthyHostCount, SurgeQueueLength — ölçeklendirme için kullanılabilir |

### Doğası Gereği Ölçeklenebilir vs Değil

| Ölçeklenebilir | Değil (tasarım gerektirir) |
|----------------|---------------------------|
| Lambda | EC2 |
| Fargate | — |
| SQS | — |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 3'ün ikinci görev ifadesini ele alıyor: **yüksek performanslı ve esnek bilişim çözümleri tasarlama**.

### 3 Compute Biçimi

AWS'de bilişim 3 biçimdedir: **Instances (EC2)**, **Containers (ECS/EKS/Fargate)** ve **Functions (Lambda)**. Sınavda bir senaryo verilip hangi bilişim türünün en uygun olduğu sorulacak. Her birinin faydalarını, sınırlamalarını ve ideal kullanım senaryolarını bilmelisiniz.

### EC2 Ölçeklendirme

EC2 doğası gereği ölçeklenebilir değildir — **ELB + Auto Scaling** ile ölçeklenebilir hale getirilir. Instance family seçimi performansı doğrudan etkiler. Her türü ezberlemeye gerek yok ama aile mantığını (compute-optimized, memory-optimized vb.) anlayın.

### Lambda'nın Süper Güçleri ve Sınırları

Lambda doğası gereği ölçeklenebilir — tasarım gerekmez. Ancak **15 dakika yürütme limiti** var. Daha uzun süreçler için **Step Functions** kullanın. **Lambda@Edge** ile CloudFront edge location'larında çalıştırarak global düşük gecikme elde edilebilir.

### CloudWatch Metrikleri — Kritik Sınav Konusu

Ölçeklendirme kararları CloudWatch metriklerine dayanır. **Memory/bellek kullanımı varsayılan olarak mevcut DEĞİLDİR** — custom metric olarak eklenmelidir. Bu sınavda en sık çıkan tuzaklardan biridir. ELB metrikleri (HealthyHostCount, SurgeQueueLength) de ölçeklendirme için kullanılabilir.

> **Sınav İpucu:** 
> - "15 dakikadan uzun" → Step Functions
> - "Bellek bazlı ölçeklendirme" → Custom metric gerekli
> - "Gün boyunca değişken trafik + HA" → EC2 + ELB + Auto Scaling
> - "Altyapı yönetimi yok + konteyner" → Fargate
> - "Olay odaklı, kısa süreli" → Lambda

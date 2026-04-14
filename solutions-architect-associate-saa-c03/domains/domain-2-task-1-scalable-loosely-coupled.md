# Domain 2 — Task Statement 1: Design Scalable and Loosely Coupled Architectures

---

## Önemli Keywords (Sınav Odaklı)

### Ölçeklendirme Kavramları
| Keyword | Açıklama |
|---------|----------|
| **Vertical Scaling** | Mevcut kaynağın boyutunu artırma (scale up/down) |
| **Horizontal Scaling** | Kaynak sayısını artırma/azaltma (scale out/in) |
| **Elasticity** | Otomasyon + yatay ölçeklendirme ile kapasitenin talebe otomatik eşleşmesi |
| **AWS Auto Scaling** | Birden fazla servisi kapsayan ölçeklendirme |
| **EC2 Auto Scaling** | EC2'ye özel ölçeklendirme, farklı scaling policy türleri |
| **Launch Configuration** | Auto Scaling için instance şablonu |

### Compute Seçenekleri
| Keyword | Açıklama |
|---------|----------|
| **Containers vs Serverless vs EC2** | Ne zaman hangisi uygun — sınav favori konusu |
| **Placement Groups** | EC2 instance'ları için yerleşim stratejileri (cluster, spread, partition) |
| **HPC (High Performance Computing)** | Özel instance türleri ve ağ yapılandırmaları |
| **Enhanced Networking** | Belirli EC2 instance türleri için gelişmiş ağ |

### Veritabanı ve Önbellekleme
| Keyword | Neden Önemli |
|---------|-------------|
| **Read Replica vs Multi-AZ** | Read Replica = performans + kullanılabilirlik; Multi-AZ = YALNIZCA yüksek kullanılabilirlik, okuma ölçeklendirmez |
| **Read Replica ≠ Cache** | Replica sorgulamak hâlâ DB overhead'i taşır |
| **RDS Proxy** | Ölçeklenebilirlik + dayanıklılık + güvenlik |
| **DynamoDB** | NoSQL, düşük gecikme, aşırı ölçek |
| **Aurora** | Cloud-native ilişkisel DB |
| **Redshift** | Veri ambarı |
| **ElastiCache** | In-memory caching |
| **DAX (DynamoDB Accelerator)** | DynamoDB için özel cache |
| **CloudFront** | CDN, edge caching |

### Edge ve Ağ Servisleri
| Keyword | Açıklama |
|---------|----------|
| **CloudFront** | CDN, veri şifreleme, edge caching |
| **Route 53** | DNS, ağ atlamalarını azaltma |
| **Global Accelerator** | Geliştirilmiş gecikme, ağ optimizasyonu |
| **AWS Transfer Family** | Yönetilen dosya transfer servisi, 3 AZ desteği, auto scaling |

### Mimari Kalıplar
| Keyword | Açıklama |
|---------|----------|
| **SOA (Service-Oriented Architecture)** | Bileşenleri servis arayüzleriyle yeniden kullanılabilir yapma |
| **Microservices** | SOA'dan daha küçük ve basit bileşenler |
| **API-driven** | API üzerinden iletişim |
| **Event-driven** | Olay tabanlı mimari |
| **Data Streaming** | Veri akışı kalıbı |
| **Loosely Coupled (Gevşek Bağlı)** | Bileşenler birbirinden bağımsız çalışır |
| **Decoupling** | Bileşenlerin özerk ve birbirinden habersiz kalması |

### Serverless Ekosistemi
| Keyword | Sınav Notu |
|---------|-----------|
| **Serverless tanımı** | Altyapı yok, tüketim bazlı ölçekleme, değer bazlı fatura, yerleşik HA/FT |
| **Lambda** | Olay odaklı, sunucusuz compute; concurrency ve transactions önemli |
| **API Gateway** | Otomatik ölçeklenir, Lambda ile entegrasyon |
| **SQS** | Mesaj kuyruğu, yüksek throughput, asenkron ayrıştırma |
| **EventBridge** | Karmaşık olay odaklı yapılar |
| **Fargate** | Sunucusuz konteyner çalıştırma |
| **ECS / EKS** | Konteyner orkestrasyon servisleri |

### Ayrıştırma (Decoupling) Kavramları
| Keyword | Açıklama |
|---------|----------|
| **Synchronous Integration** | Her iki bileşen her zaman kullanılabilir olmalı |
| **Asynchronous Integration** | Bileşenler bağımsız çalışır, mesaj kuyruğu ile |
| **SQS ile ayrıştırma** | Frontend-backend bağımsız ölçeklendirme |
| **Durable Message Store** | SQS, DynamoDB — istek alımını işlemden ayırma |

---

## Genel Özet ve Anlatım

Bu video, SAA-C03 sınavının Domain 2'sindeki ilk görev ifadesini ele alıyor: **ölçeklenebilir ve gevşek bağlı mimariler tasarlama**.

### Temel Mesajlar

**Ölçeklendirme iki türdür:** Dikey (kaynağı büyütme) ve yatay (kaynak sayısını artırma). Sınavda ikisinin farkını ve maliyet etkilerini bilmeniz bekleniyor. **Elasticity** kavramı özellikle önemli — otomasyon ile yatay ölçeklendirmeyi birleştirerek kapasitenin talebe otomatik uyum sağlamasıdır.

**Compute seçimi kritik:** Konteyner, serverless ve EC2 arasındaki doğru tercihi yapabilmelisiniz. HPC için placement groups ve enhanced networking detaylarını bilin.

**Veritabanı tarafında en kritik ayrım:** Read Replica vs Multi-AZ. Multi-AZ sadece yüksek kullanılabilirlik sağlar, okuma performansını ölçeklendirmez. Read Replica hem performans hem kullanılabilirlik sağlar ama cache'in yerini tutmaz. Caching için CloudFront, ElastiCache ve DAX seçeneklerini bilin. RDS Proxy ise ölçeklenebilirlik, dayanıklılık ve güvenlik için önemli bir araçtır.

**Mimari kalıplar:** SOA ve microservices arasındaki farkı, API-driven / event-driven / data streaming kalıplarını anlayın. Sınavda bir senaryo verilip hangi kalıbın en uygun olduğu sorulacaktır.

**Gevşek bağlama (loose coupling) ve ayrıştırma (decoupling)** bu task'ın kalbidir. Bileşenlerin birbirinden bağımsız çalışması, tek bir arıza noktasının tüm sistemi çökertmemesi hedeflenir. Senkron vs asenkron entegrasyon farkını bilin. SQS ile frontend ve backend'i ayırarak bağımsız ölçeklendirme yapılabilir.

**Serverless tanımını ezberleyin:** Altyapı yönetimi yok, tüketim bazlı otomatik ölçekleme, değer bazlı faturalandırma, yerleşik kullanılabilirlik ve hata toleransı.

### Bilinmesi Gereken Servisler
API Gateway, Lambda, SQS, EventBridge, Transfer Family, CloudFront, Route 53, Global Accelerator, RDS Proxy, ElastiCache, DAX, DynamoDB, Aurora, Redshift, Fargate, ECS, EKS, Secrets Manager, ALB.

> **Sınav İpucu:** Bu task statement, sadece tanım bilgisi değil, servislerin gerçek senaryolarda nasıl birlikte çalıştığını anlama becerisi test eder. "Hangi servis bu sorunu çözer?" sorusuna cevap verebilmelisiniz.

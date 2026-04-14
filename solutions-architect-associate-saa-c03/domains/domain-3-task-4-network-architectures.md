# Domain 3 — Task Statement 4: Determine High-Performing and/or Scalable Network Architectures

---

## 2. Önemli Keywords (Sınav Odaklı)

### VPC Bileşenleri ve Oluşturma Sırası

| Sıra | Bileşen | Açıklama |
|------|---------|----------|
| 1 | **VPC** | Bölgesel dayanıklı, varsayılan olarak özel |
| 2 | **Subnets** | Alt ağlar (public/private) |
| 3 | **Route Tables** | Yönlendirme tabloları |
| 4 | **Internet Gateway** | İnternete çıkış |
| 5 | **NACL** | Ağ erişim kontrol listesi (stateless) |
| 6 | **Security Groups** | Güvenlik grupları (stateful) |
| 7+ | **NAT Gateway, Peering, Endpoints** | İhtiyaca göre özelleştirme |

### Hibrit Bağlantı Seçenekleri

| Servis | Özellik | Ne Zaman |
|--------|---------|----------|
| **Site-to-Site VPN** | Şifreli internet üzerinden bağlantı | Hızlı kurulum, düşük-orta bant genişliği |
| **Direct Connect** | Dedike özel bağlantı | Yüksek bant genişliği, tutarlı gecikme, uyumluluk |
| **Transit Gateway** | Merkezi hub — birden fazla VPC + uzak ağ bağlantısı | Karmaşık çoklu VPC mimarileri |
| **AWS CloudHub** | Hub-and-spoke modeli | Birden fazla uzak ağı bağlama |

### VPC'ler Arası Bağlantı

| Servis | Kullanım |
|--------|----------|
| **VPC Peering** | İki VPC arası doğrudan bağlantı |
| **Transit Gateway** | Birden fazla VPC'yi merkezi olarak bağlama |
| **Direct Connect Gateway** | On-premises'den birden fazla VPC'ye |
| **PrivateLink** | VPC'ler arası özel uygulama erişimi — internet/peering olmadan |

### VPC Endpoints

| Tür | Kullanım | Örnek |
|-----|----------|-------|
| **Gateway Endpoint** | Özel subnet'ten AWS genel servislerine erişim | **S3, DynamoDB** |
| **Interface Endpoint (PrivateLink)** | VPC'ler arası özel servis erişimi | Diğer AWS servisleri |

### Route 53 Yönlendirme Politikaları

| Politika | Kullanım Senaryosu |
|----------|-------------------|
| **Simple** | Tek kaynak |
| **Weighted** | Trafik dağılım yüzdesi |
| **Latency-based** | En düşük gecikmeye yönlendir |
| **Failover** | Birincil çökerse ikincile yönlendir |
| **Geolocation** | Kullanıcının bulunduğu ülke/kıtaya göre |
| **Geoproximity** | Kullanıcıya coğrafi olarak en yakın Region'a yönlendir |
| **Multivalue Answer** | Birden fazla sağlıklı kaynak döndür |

### Performans Optimizasyon Servisleri

| Servis | Ne Yapar |
|--------|----------|
| **CloudFront** | Edge caching — varlıkları son kullanıcıya yakınlaştır |
| **Global Accelerator** | AWS global ağı üzerinden optimum yönlendirme, S3 Multi-Region Access Points |
| **S3 Transfer Acceleration** | Edge location'lar üzerinden hızlı upload |
| **Enhanced Networking** | EC2 için geliştirilmiş ağ performansı |
| **EBS-Optimized Instances** | Dedike EBS bant genişliği |

### Veri Aktarım ve Geçiş Servisleri

| Servis | Kullanım |
|--------|----------|
| **AWS DataSync** | Online veri aktarımı (on-prem ↔ AWS) |
| **Snow Family** | Fiziksel cihazlarla büyük veri transferi (Snowcone, Snowball, Snowmobile) |
| **Transfer Family** | Yönetilen dosya transfer servisi (SFTP, FTPS, FTP) |
| **Database Migration Service (DMS)** | Veritabanı geçişi |

### Load Balancer ve OSI Katmanları

| Load Balancer | OSI Katmanı | Kullanım |
|---------------|-------------|----------|
| **ALB (Application)** | Layer 7 (HTTP/HTTPS) | URL/path bazlı yönlendirme, mikro servisler |
| **NLB (Network)** | Layer 4 (TCP/UDP) | Ultra düşük gecikme, yüksek throughput |
| **GLB (Gateway)** | Layer 3 (Network) | Güvenlik araçları, trafik inceleme |
| **CLB (Classic)** | Layer 4/7 | Eski, artık önerilmiyor |

### Küresel Sunucusuz Mimari Örneği

| Bileşen | Servis |
|---------|--------|
| Statik içerik | S3 Website Hosting |
| DNS / Yönlendirme | Route 53 (latency + failover) |
| Veritabanı | DynamoDB Global Tables |
| Caching / CDN | CloudFront |
| Ağ optimizasyonu | Global Accelerator (S3 Multi-Region Access Points) |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 3'ün en kapsamlı görev ifadesini ele alıyor: **yüksek performanslı ve ölçeklenebilir ağ mimarileri**.

### VPC Temelleri

VPC bölgesel dayanıklı bir servistir ve varsayılan olarak özeldir. Oluşturma sırası: VPC → Subnets → Route Tables → IGW → NACL → Security Groups → özelleştirme (NAT GW, peering, endpoints).

### Hibrit Bağlantı: VPN vs Direct Connect

Sınavda en sık karşılaşılan soru: "On-premises'i AWS'ye nasıl bağlarsınız?"
- **VPN:** Hızlı kurulum, internet üzerinden şifreli, daha düşük maliyet ama değişken performans
- **Direct Connect:** Dedike bağlantı, tutarlı gecikme, yüksek bant genişliği, uyumluluk gereksinimleri
- **Transit Gateway:** Birden fazla VPC'yi tek bir hub'dan yönetme — karmaşık ağlarda basitleştirici

### Özel Bağlantılar: PrivateLink ve VPC Endpoints

Özel subnet'ten S3 veya DynamoDB'ye erişmek için **Gateway Endpoint** kullanın. VPC'ler arası özel uygulama erişimi için **PrivateLink** — internet veya peering gerekmez.

### Route 53 — Sınavın Gözdesi

7 farklı yönlendirme politikası var. Sınavda senaryo verilip "hangi politika" sorulur:
- Coğrafi yakınlık → **Geoproximity**
- En düşük gecikme → **Latency-based**
- Failover → **Failover routing**

### Load Balancer OSI Katmanları

Sınav ipucu: Her LB'nin OSI katmanını bilin. ALB = Layer 7, NLB = Layer 4, GLB = Layer 3.

### Veri Aktarım Servisleri

Veri miktarı, türü ve kaynağa göre doğru servisi seçin: online küçük veri → DataSync, fiziksel büyük veri → Snow Family, dosya transferi → Transfer Family, DB geçişi → DMS.

### Küresel Sunucusuz Mimari

Video bir örnek mimari sunuyor: S3 (statik) + Route 53 (latency/failover) + DynamoDB Global Tables + CloudFront + Global Accelerator. Bu kombinasyon hem yüksek ölçeklenebilir hem düşük maliyetli.

> **Sınav İpucu:**
> - "Özel subnet'ten S3'e erişim" → Gateway Endpoint
> - "VPC'ler arası özel erişim, internet yok" → PrivateLink
> - "On-prem + AWS, yüksek bant genişliği" → Direct Connect
> - "Birden fazla VPC merkezi bağlantı" → Transit Gateway
> - "Kullanıcıya en yakın Region" → Route 53 Geoproximity
> - "Her LB'nin OSI katmanı" → ALB=L7, NLB=L4, GLB=L3

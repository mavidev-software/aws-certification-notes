# Domain 4 — Task Statement 4: Design Cost-Optimized Network Architectures

---

## 2. Önemli Keywords (Sınav Odaklı)

### Veri Aktarım Maliyeti Kuralları

| Kural | Açıklama |
|-------|----------|
| **Aynı AZ içi** | Genellikle ücretsiz |
| **AZ'ler arası** | Veri aktarım ücreti var |
| **Region'lar arası** | Daha yüksek veri aktarım ücreti |
| **AWS Origin → CloudFront Edge** | Ücretsiz (S3, EC2, ELB) |
| **Gateway Endpoint → S3/DynamoDB** | Aynı Region'da ücretsiz |
| **İnternete çıkış** | Ücretli (data transfer out) |

### Bağlantı Maliyet Karşılaştırması

| Bağlantı | Maliyet | Ne Zaman |
|----------|---------|----------|
| **Site-to-Site VPN** | Düşük | Maliyet öncelikli, orta bant genişliği |
| **Direct Connect** | Yüksek | Yüksek throughput veya güvenlik gerekli |
| **VPC Peering** | Düşük | VPC'ler arası basit bağlantı |
| **Transit Gateway** | Daha yüksek | Çoklu VPC hub, peering'den pahalı |

### Direct Connect Failover Stratejisi

| Seçenek | Maliyet | Süre |
|---------|---------|------|
| İkinci Direct Connect | Yüksek | Uzun kurulum |
| **Site-to-Site VPN (failover)** | **Düşük** | **Hızlı kurulum** |

> **Sınav İpucu:** "Direct Connect failover + minimum maliyet" → Site-to-Site VPN

### EC2 Erişim Yöntemleri ve Maliyet

| Yöntem | Maliyet Etkisi |
|--------|---------------|
| **SSH / RDP** | Bastion host gerekebilir → ek instance maliyeti |
| **Systems Manager Session Manager** | Bastion host gereksiz, ek maliyet yok |
| **EC2 Instance Connect** | Geçici SSH, basit |

### NAT Gateway Maliyet Optimizasyonu

| Ortam | Strateji |
|-------|----------|
| **Üretim** | Her AZ'de ayrı NAT Gateway (HA) |
| **Geliştirme** | Paylaşımlı tek NAT Gateway (maliyet tasarrufu) |

### CloudFront Maliyet Avantajları

| Avantaj | Açıklama |
|---------|----------|
| **Origin → Edge ücretsiz** | S3/EC2/ELB'den CloudFront'a aktarım ücretsiz |
| **Regional Edge Cache** | Ek ücret yok |
| **S3 data transfer out azaltma** | Cache ile API çağrıları ve çıkış trafiği düşer |
| **Origin yükü azaltma** | Daha az istek = daha düşük işletim maliyeti |

### S3 Maliyet Bileşenleri

| Bileşen | Açıklama |
|---------|----------|
| **Depolama** | Saklanan veri miktarı |
| **API çağrıları** | PUT, GET, LIST vb. |
| **Data transfer out** | Kovadan dışarı aktarım |
| **Optimizasyon** | CloudFront ile API çağrıları ve çıkış trafiğini azalt |

### API Gateway Throttling

| Özellik | Açıklama |
|---------|----------|
| **Usage Plans** | Müşteri bazlı limit ve kota tanımlama |
| **API Keys** | Müşteri kimliği ve erişim kontrolü |
| **Throttling** | TPS limitleri → gereksiz istek maliyetini önle |

### Genel Maliyet Azaltma İpuçları

| İpucu | Açıklama |
|-------|----------|
| **Aynı AZ'de tut** | AZ'ler arası transfer maliyetinden kaçın |
| **Aynı Region'da tut** | Region'lar arası transfer daha pahalı |
| **Ucuz Region seç** | Bazı Region'lar daha ucuz |
| **Free Tier kullan** | Birçok servisin ücretsiz katmanı var |
| **Gateway Endpoint** | S3/DynamoDB erişiminde transfer ücreti yok |
| **CloudFront** | Origin'den edge'e transfer ücretsiz |
| **VPN > Direct Connect** | Maliyet öncelikli → VPN tercih et |

### İzleme Araçları

| Araç | Kullanım |
|------|----------|
| CloudWatch | Metrik ve alarm |
| Health Dashboard | Servis durumu |
| VPC Reachability Analyzer | Ağ bağlantı sorunları |
| Transit Gateway Network Manager | Transit GW izleme |
| Config | Yapılandırma değişiklik takibi |
| Route Analyzer | Yönlendirme analizi |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 4'ün son görev ifadesini ele alıyor: **maliyet optimize edilmiş ağ mimarileri tasarlama**.

### Veri Aktarım Maliyetini Anlayın

AWS'de en çok gözden kaçan maliyet kalemi **veri aktarım ücretleridir**. Temel kurallar:
- Aynı AZ içi genellikle ücretsiz
- AZ'ler arası ücretli
- Region'lar arası daha pahalı
- AWS Origin → CloudFront Edge **ücretsiz**
- Gateway Endpoint ile S3/DynamoDB erişimi aynı Region'da **ücretsiz**

### VPN vs Direct Connect — Maliyet Açısından

Genel kural: Direct Connect daha pahalı ama yüksek throughput/güvenlik sağlar. Maliyet öncelikli senaryolarda **VPN tercih edin**. Direct Connect failover'ı için ikinci Direct Connect yerine **VPN kullanın** — daha ucuz ve hızlı kurulum.

### CloudFront — Maliyet Optimize Edici

CloudFront sadece performans değil, **maliyet optimizasyonu** aracıdır:
- AWS Origin'lerden Edge'e transfer ücretsiz
- S3'ten çıkış trafiğini azaltır (cache)
- Regional edge cache ek ücret yok
- Origin'lerdeki operasyonel yükü düşürür

### NAT Gateway Stratejisi

Üretimde her AZ'de ayrı NAT Gateway (HA için). Geliştirmede **paylaşımlı tek NAT Gateway** ile tasarruf.

### Genel İlkeler

Verileri mümkün olduğunca **aynı AZ ve Region'da** tutun. Ucuz Region'ları tercih edin. Free Tier'dan yararlanın. API Gateway throttling ile gereksiz istek maliyetini kontrol edin.

> **Sınav İpucu:**
> - "Direct Connect failover + minimum maliyet" → Site-to-Site VPN
> - "S3 data transfer out azaltma" → CloudFront
> - "Özel subnet → S3 erişimi + maliyet" → Gateway Endpoint (ücretsiz)
> - "Dev ortamı NAT maliyet" → paylaşımlı tek NAT Gateway
> - "VPC peering vs Transit Gateway maliyet" → Peering daha ucuz
> - "Origin → CloudFront" → ücretsiz transfer

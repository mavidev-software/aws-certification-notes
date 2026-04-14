# Domain 4 — Task Statement 2: Design Cost-Optimized Compute Solutions

---

## 2. Önemli Keywords (Sınav Odaklı)

### 5 Maliyet Optimizasyonu Sütunu

| # | Sütun | Açıklama |
|---|-------|----------|
| 1 | **Right-Sizing** | Doğru instance türü ve boyutu seçimi |
| 2 | **Elasticity (Esneklik)** | Kaynakları sadece gerektiğinde kullan, kullanılmadığında kapat |
| 3 | **Pricing Model** | Doğru fiyatlandırma modeli seçimi |
| 4 | **Storage Matching** | Depolamayı kullanıma uygun boyutlandır |
| 5 | **Continuous Improvement** | İzleme ve ölçme ile sürekli optimizasyon |

### EC2 Fiyatlandırma Modelleri

| Model | Ne Zaman | Tasarruf | Kesinti Riski |
|-------|----------|---------|---------------|
| **On-Demand** | Kısa süreli, tahmin edilemez iş yükleri | Yok | Yok |
| **Savings Plans** | 1-3 yıl taahhüt, EC2/Fargate/Lambda esnekliği | %72'ye kadar | Yok |
| **Reserved Instances** | 1-3 yıl taahhüt, belirli instance türü | %75'e kadar | Yok |
| **Spot Instances** | Kesinti tolere edebilen iş yükleri | %90'a kadar | **Var** |
| **Dedicated Hosts** | Lisans gereksinimleri, uyumluluk | Değişken | Yok |
| **Dedicated Instances** | Donanım izolasyonu | Orta | Yok |
| **Capacity Reservations** | Belirli AZ'de kapasite garantisi | On-Demand fiyat | Yok |

### Spot Instances İdeal Kullanım

| Kullanım | Neden |
|----------|-------|
| **Stateless web sunucuları** | Sunucu kaybedilebilir, yerine yenisi gelir |
| **Batch processing** | İş bölünebilir, kesinti tolere edilir |
| **HPC / Big Data** | Büyük paralel iş yükleri |
| **CI/CD build işleri** | Kısa süreli, tekrarlanabilir |

### Sınav Sorusu Kalıpları

| Senaryo | Cevap |
|---------|-------|
| "Kesinti tolere edebilir + minimum maliyet" | **Spot Instances** |
| "Kesinti tolere edemez + tasarruf + EC2/Fargate/Lambda esnekliği" | **Savings Plans** |
| "Belirli instance türü + 1-3 yıl" | **Reserved Instances** |
| "Lisans gereksinimleri + fiziksel sunucu" | **Dedicated Hosts** |

### Maliyet Azaltma Stratejileri

| Strateji | Açıklama |
|----------|----------|
| **Yönetilen servisler** | Sunucu yönetimi yerine managed services (Aurora, RDS) |
| **Lisans eliminasyonu** | Aurora/RDS ile DB lisans maliyetini kaldır |
| **CloudFront** | Veri aktarım maliyetini azalt |
| **Daha fazla küçük instance** | Birkaç büyük yerine → daha granüler ölçeklendirme |
| **ASG + off-hours scale-down** | İş saatleri dışında küçült |
| **Tagging stratejisi** | Maliyet takibi ve etiket bazlı otomatik yönetim |

### Hibrit ve Edge Computing

| Servis | Maliyet Notu |
|--------|-------------|
| **Outpost** | Kullanıma dayalı (instance saat), EC2/EBS hariç |
| **Snowball Edge** | Uzak bölgelerde edge computing |
| **CloudFront** | Edge caching — veri merkezi maliyeti yok |

### ELB + Auto Scaling Maliyet Optimizasyonu

| Özellik | Açıklama |
|---------|----------|
| **RequestCountPerTarget** | ALB metriği → ASG ölçeklendirme tetikleyicisi |
| **ELB Health Checks** | Sağlıksız instance'ları tespit ve değiştir |
| **HealthyHostCount alarmı** | Sağlıklı host sayısı düşerse CloudWatch bildirimi |
| **Right-size + ELB** | Küçük instance + yatay ölçeklendirme = maliyet optimize |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 4'ün ikinci görev ifadesini ele alıyor: **maliyet optimize edilmiş bilişim çözümleri tasarlama**.

### 5 Maliyet Optimizasyonu Sütunu

Video, compute maliyet optimizasyonunu 5 sütun üzerinden anlatıyor:

1. **Right-Sizing:** İlk ve en önemli adım. Doğru instance ailesi (compute-optimized, memory-optimized vb.) ve boyutu seçin. Gereksinimden fazlasını sağlamayın.

2. **Elasticity:** Kullanılmadığında kaynakları kapatın. Daha fazla küçük instance + Auto Scaling = iş saatleri dışında scale-down. CloudFormation + tagging ile yönetim.

3. **Pricing Model:** En kritik sınav konusu. Spot = en ucuz ama kesinti riski, Savings Plans = kesinti yok + EC2/Fargate/Lambda esnekliği, Reserved = belirli instance taahhüdü. Soruda "kesinti tolere edebilir" görürseniz → Spot.

4. **Storage Matching:** Compute ortamı için depolamayı doğru boyutlandır.

5. **Continuous Improvement:** CloudWatch, Cost Explorer, tagging ile sürekli izleme ve optimizasyon.

### Fiyatlandırma Seçimi

Sınavda en sık çıkan kalıp:
- **"Batch processing + kesinti OK + minimum maliyet"** → Spot
- **"Kesinti yok + esneklik + tasarruf"** → Savings Plans
- **"Lisans gereksinimi"** → Dedicated Hosts
- **"Kısa süreli, tahmin edilemez"** → On-Demand

### Maliyet Azaltma İpuçları

- Aurora/RDS ile **lisans maliyetini ortadan kaldır**
- CloudFront ile **veri aktarım maliyetini azalt**
- Birkaç büyük instance yerine **çok sayıda küçük instance** + ASG
- **RequestCountPerTarget** metriği ile akıllı ölçeklendirme

> **Sınav İpucu:**
> - "Kesinti tolere edebilir" → Spot Instances
> - "EC2 + Fargate + Lambda esnekliği + tasarruf" → Savings Plans
> - "Lisans + fiziksel sunucu" → Dedicated Hosts
> - "Büyük instance boşta kalıyor" → küçük instance + ASG ile right-size
> - "Off-hours maliyet" → ASG scheduled scaling ile iş dışı saatlerde küçült

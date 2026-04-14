# Domain 4 — Task Statement 3: Design Cost-Optimized Database Solutions

---

## 2. Önemli Keywords (Sınav Odaklı)

### Veritabanı Maliyet Optimizasyonu Stratejileri

| Strateji | Açıklama |
|----------|----------|
| **Doğru veri deposu seçimi** | Her şeyi RDS'e koymayın — erişim kalıbına göre seçin |
| **Veri alt kümelerini taşıma** | Büyük nesneler → S3, NoSQL uygun veriler → DynamoDB |
| **Aurora Serverless** | Seyrek/tahmin edilemez iş yükleri için Aurora'dan daha ucuz |
| **Read Replica / Cache** | Scale-up yerine → daha ucuz yatay ölçeklendirme |
| **Storage Auto Scaling** | RDS depolamasını otomatik boyutlandır |
| **Snapshot retention politikası** | Gereksiz snapshot'ları silme → depolama maliyeti azalt |
| **Managed services** | Sunucu yönetimi yükünü kaldır, bulut ölçeğinde düşük maliyet |

### Veri Geçişi Maliyet Optimizasyonu

| Kaynak Veri | Hedef | Neden |
|-------------|-------|-------|
| İlişkisel DB (tümü) | EC2 veya RDS | Yönetilen servise taşı |
| Büyük nesneler (BLOB) | **S3** | DB'den büyük dosyaları çıkar → maliyet düşer |
| NoSQL uygun alt küme | **DynamoDB** | Yönetim yükü yok, otomatik ölçekleme |
| Sabit şema gereken veri | RDS'te tut | İlişkisel kısıtlamalar gerekli |

### Veritabanı Seçim Kriterleri

| Kriter | Soru |
|--------|------|
| **Erişim kalıpları** | Veri nasıl okunuyor/yazılıyor? |
| **Beklenen ölçek ve büyüme** | Veri ne kadar büyüyecek? |
| **Erişim sıklığı** | Ne sıklıkla erişilecek? |
| **DB-specific özellikler** | İlişkisel kısıtlamalar, joins gerekli mi? |
| **Şema esnekliği** | Şema değişecek mi? (evet → NoSQL düşün) |

### Ölçeklendirme: Scale-Up vs Read Replica/Cache

| Yaklaşım | Maliyet | Performans |
|----------|---------|------------|
| **Scale-Up (dikey)** | Yüksek — daha büyük instance | İyi ama pahalı |
| **Read Replica (yatay)** | Düşük — mevcut instance + replica | Okuma trafiğini dağıtır |
| **Cache (ElastiCache/DAX)** | Düşük — in-memory | En hızlı okuma |

> **Sınav Kuralı:** Okuma ağırlıklı trafik + maliyet optimizasyonu = Read Replica veya Cache, scale-up değil.

### Aurora Serverless Maliyet Avantajı

| Özellik | Maliyet Etkisi |
|---------|---------------|
| **Sıfıra inebilir ACU** | Kullanılmadığında sadece depolama ücreti |
| **Otomatik ölçekleme** | İhtiyaca göre büyür/küçülür |
| **Seyrek iş yükleri** | Standart Aurora'dan çok daha ucuz |

### Yedekleme ve Retention

| Konu | Maliyet İpucu |
|------|-------------|
| **RPO'ya uygun yedekleme sıklığı** | Çok sık = gereksiz maliyet, çok seyrek = veri kaybı riski |
| **Point-in-time recovery** | Hangi servisler destekliyor bilin (RDS, DynamoDB) |
| **Snapshot retention** | Yararlı ömrünün ötesinde saklama = gereksiz depolama maliyeti |

---

## 3. Genel Özet ve Anlatım

Bu video, Domain 4'ün üçüncü görev ifadesini ele alıyor: **maliyet optimize edilmiş veritabanı çözümleri tasarlama**.

### Her Şeyi RDS'e Koymayın

Videonun ana mesajı: tüm verileri ilişkisel veritabanında saklamak hem performans sorunlarına hem de yüksek maliyete yol açar. Erişim kalıplarına bakarak verileri doğru yere taşıyın:
- Büyük nesneler (dosyalar, resimler) → **S3**
- Key-value veya esnek şema gereken veriler → **DynamoDB**
- İlişkisel kısıtlamalar gereken veriler → **RDS'te tutun**

### Scale-Up Yerine Yatay Ölçeklendirme

Okuma ağırlıklı trafik sorununda instance boyutunu artırmak (scale-up) pahalıdır. Daha uygun maliyetli yaklaşım:
1. **Read Replica** ekle — okuma trafiğini dağıt
2. **Cache** ekle (ElastiCache/DAX) — sık erişilen verileri bellekte tut
3. Her ikisi de scale-up'tan daha ucuz

### Aurora Serverless

Seyrek veya tahmin edilemez iş yükleri için standart Aurora yerine **Aurora Serverless** düşünün. ACU sıfıra inebilir → kullanılmadığında sadece depolama ücreti.

### Snapshot ve Yedekleme Maliyeti

Snapshot'ları yararlı ömürlerinin ötesinde saklamak gereksiz depolama maliyeti oluşturur. Retention politikası tasarlayın ve RPO gereksinimlerine uygun yedekleme sıklığı belirleyin.

### Yönetilen Servisler

Mümkün olduğunda yönetilen servisler kullanın — sunucu bakım yükü yok, bulut ölçeğinde daha düşük işlem başına maliyet.

> **Sınav İpucu:**
> - "Okuma ağırlıklı + maliyet" → Read Replica veya Cache (scale-up değil)
> - "Seyrek iş yükü + maliyet" → Aurora Serverless
> - "Büyük nesneler DB'de + maliyet" → S3'e taşı
> - "NoSQL uygun veri + maliyet" → DynamoDB (yönetim yükü yok)
> - "Eski snapshot'lar + maliyet" → Retention politikası, gereksiz snapshot'ları sil

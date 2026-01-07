# Migration & Transfer Services (In-Scope)

> **Kategori:** Goc ve Transfer Servisleri

## AWS Migration Hub

**Ne Ise Yarar:** Goc projelerini merkezi izleme.

**Temel Ozellikler:**
- Tek panelden tum gocleri izleme
- Ilerleme takibi
- Coklu arac entegrasyonu

---

## AWS Application Migration Service

**Ne Ise Yarar:** Sunuculari AWS'e goc ettirme (lift-and-shift).

**Temel Ozellikler:**
- Otomatik sunucu replikasyonu
- Minimum kesinti
- Test ve cutover

---

## AWS Database Migration Service (DMS)

**Ne Ise Yarar:** Veritabani gocu ve replikasyonu.

**Desteklenen Senaryolar:**
- Ayni veritabani motoru (MySQL -> MySQL)
- Farkli motor (Oracle -> Aurora)
- Surekli replikasyon

---

## AWS Schema Conversion Tool (SCT)

**Ne Ise Yarar:** Veritabani semasini donusturme (Oracle -> PostgreSQL gibi).

**Ne Zaman Kullanilir:**
- Farkli veritabani motorlari arasi goc
- Stored procedure donusumu
- DMS ile birlikte

---

## AWS Snow Family

**Ne Ise Yarar:** Fiziksel veri transfer cihazlari.

| Cihaz | Kapasite | Kullanim |
|-------|----------|----------|
| Snowcone | 8-14 TB | Kucuk transferler, edge |
| Snowball Edge | 80 TB | Orta olcekli transferler |
| Snowmobile | 100 PB | Cok buyuk transferler (tir) |

**Ne Zaman Kullanilir:**
- Zayif internet baglantisi
- Buyuk veri transferi
- Edge computing

---

## AWS Application Discovery Service

**Ne Ise Yarar:** On-premises altyapi kesfi.

**Kesfedilenler:**
- Sunucu konfigurasyonu
- Performans verileri
- Bagimlilliklar

---

## Migration Evaluator

**Ne Ise Yarar:** Goc maliyet analizi.

**Sagladiklarri:**
- Is senaryosu
- Maliyet karsilastirmasi
- TCO analizi


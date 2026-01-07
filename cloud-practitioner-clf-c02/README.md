# AWS Cloud Practitioner (CLF-C02) Study Guide

AWS Certified Cloud Practitioner sınavına hazırlık için Türkçe çalışma notları.

## Sınav Bilgileri

| Bilgi | Detay |
|-------|-------|
| Sınav Kodu | CLF-C02 |
| Soru Sayısı | 65 (50 puanlı + 15 puansız) |
| Süre | 90 dakika |
| Geçme Puanı | 700/1000 |
| Sınav Ücreti | $100 USD |
| Geçerlilik | 3 yıl |

## Domain Ağırlıkları

| Domain | Konu | Ağırlık |
|--------|------|---------|
| 1 | Cloud Concepts | %24 |
| 2 | Security & Compliance | %30 |
| 3 | Cloud Technology & Services | %34 |
| 4 | Billing, Pricing & Support | %12 |

```
Domain 2 + Domain 3 = %64 (Sınavın büyük çoğunluğu!)
```

## Domain Notları

| Domain | Dosya | Durum |
|--------|-------|-------|
| Domain 1: Cloud Concepts | [domain-1-cloud-concepts.md](domains/domain-1-cloud-concepts.md) | Tamamlandı |
| Domain 2: Security & Compliance | [domain-2-security-compliance.md](domains/domain-2-security-compliance.md) | Tamamlandı |
| Domain 3: Technology & Services | [domain-3-technology-services.md](domains/domain-3-technology-services.md) | Hazırlanıyor |
| Domain 4: Billing & Support | [domain-4-billing-pricing-support.md](domains/domain-4-billing-pricing-support.md) | Hazırlanıyor |

## Servis Referansları

- [Tüm In-Scope Servisler](services/all-services.md) - 100+ servisin detaylı açıklamaları
- [Kategori Bazlı Servisler](services/README.md) - 12 kategoride organize edilmiş servisler
- [Out-of-Scope Servisler](services/out-of-scope-services.md) - Sınavda çıkmayan servisler

## Pratik Sorular

- [Practice Questions Kaynakları](practice-questions/README.md) - Tüm pratik soru kaynakları ve referanslar

## Hızlı Referans

### En Kritik Konular

```
1. Elasticity = UP + DOWN (Scalability değil!)
2. Security Group = Stateful, NACL = Stateless
3. CloudTrail = WHO did WHAT (API logs)
4. CloudWatch = HOW (monitoring)
5. Shared Responsibility = IN vs OF the cloud
6. Well-Architected = 6 pillar (O-S-R-P-C-S)
7. AWS CAF = 6 perspective
8. Migration = 7 R's
```

### Sık Karıştırılan Kavramlar

| Kavram A | Kavram B | Fark |
|----------|----------|------|
| Elasticity | Scalability | Up+Down vs Grow |
| Security Group | NACL | Stateful vs Stateless |
| CloudTrail | CloudWatch | API logs vs Metrics |
| IAM Role | IAM User | Temporary vs Permanent |
| Reserved Instance | Savings Plans | Specific vs Flexible |

## Çalışma Planı

### Hafta 1-2: Temel Kavramlar
- [ ] Domain 1: Cloud Concepts
- [ ] Domain 4: Billing & Support

### Hafta 3-4: Güvenlik ve Servisler
- [ ] Domain 2: Security & Compliance
- [ ] Domain 3: Technology & Services (Part 1)

### Hafta 5-6: Pratik ve Tekrar
- [ ] Domain 3: Technology & Services (Part 2)
- [ ] Tüm servisleri gözden geçir
- [ ] Pratik sınavlar çöz

## Resmi Kaynaklar

| Kaynak | Link |
|--------|------|
| Exam Guide | [CLF-C02 Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html) |
| In-Scope Services | [Service List](https://docs.aws.amazon.com/aws-certification/latest/examguides/clf-02-in-scope-services.html) |
| AWS Skill Builder | [Free Courses](https://skillbuilder.aws) |
| Official Practice | [Practice Questions](https://aws.amazon.com/certification/certification-prep/) |

## Pratik Sınav Kaynakları

> Detaylı liste için: [Practice Questions README](practice-questions/README.md)

### Resmi AWS (Ücretsiz)
- [Official Practice Question Set (20 soru)](https://explore.skillbuilder.aws/learn/course/external/view/elearning/14050/aws-certified-cloud-practitioner-official-practice-question-set-clf-c02-english)
- [Official Practice Exam](https://skillbuilder.aws/learn/JSJ5VBDBRG/official-practice-exam-aws-certified-cloud-practitioner-clfc02--english/FHCY1FNYXJ)
- [Cloud Practitioner Essentials Course](https://explore.skillbuilder.aws/learn/course/external/view/elearning/134/aws-cloud-practitioner-essentials)

### Ücretsiz (3. Parti)
- [ExamTopics (800+ soru)](https://www.examtopics.com/exams/amazon/aws-certified-cloud-practitioner-clf-c02/) - Yorumları oku!
- [Tutorials Dojo Sample (10 soru)](https://tutorialsdojo.com/aws-certified-cloud-practitioner-clf-c02-sample-exam-questions/)
- [Kananinirav GitHub (300+ soru)](https://kananinirav.com/practice-exam/exams.html)

### Ücretli (Önerilen)
- [Tutorials Dojo (~$15)](https://tutorialsdojo.com/aws-certified-cloud-practitioner/) - En çok önerilen
- [Whizlabs (~$20)](https://www.whizlabs.com/aws-certified-cloud-practitioner/)
- [Udemy - Stephane Maarek (~$15)](https://www.udemy.com/course/practice-exams-aws-certified-cloud-practitioner/)

## Sınav Günü İpuçları

1. **Zaman yönetimi:** 65 soru / 90 dk = soru başına ~1.5 dk
2. **Bilmediğini işaretle:** Flag özelliğini kullan, sonra dön
3. **Eleyerek cevapla:** Yanlış seçenekleri ele
4. **Anahtar kelimelere dikkat:** "most cost-effective", "least operational overhead"
5. **Boş bırakma:** Negatif puanlama yok, tahmin et

---

> **Hedef:** Pratik sınavlarda sürekli **%80+** alana kadar çalış, sonra gerçek sınava gir.

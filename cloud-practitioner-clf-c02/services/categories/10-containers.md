# Container Services (In-Scope)

> **Kategori:** Konteyner Servisleri

## Amazon ECR (Elastic Container Registry)

**Ne Ise Yarar:** Docker container image'larini depolar.

**Temel Ozellikler:**
- Tam yonetilen container registry
- Guvenlik taramasi
- Lifecycle policies
- Cross-region replikasyon

---

## Amazon ECS (Elastic Container Service)

**Ne Ise Yarar:** Container orchestration hizmeti.

**Launch Turleri:**
| Tur | Yonetim |
|-----|---------|
| EC2 Launch Type | EC2 instance'lari yonetirsiniz |
| Fargate Launch Type | Serverless, AWS yonetir |

**Ne Zaman Kullanilir:**
- Microservices mimarisi
- Batch processing
- Web uygulamalari

---

## Amazon EKS (Elastic Kubernetes Service)

**Ne Ise Yarar:** Yonetilen Kubernetes hizmeti.

**ECS vs EKS:**
| Ozellik | ECS | EKS |
|---------|-----|-----|
| Platform | AWS native | Kubernetes |
| Ogrenme | Daha kolay | Daha zor |
| Portability | AWS'e bagli | Multi-cloud |

**Ne Zaman Kullanilir:**
- Kubernetes ecosystem kullanimi
- Multi-cloud stratejisi
- Kubernetes bilgisi olan ekipler

---

## AWS Fargate

**Ne Ise Yarar:** Serverless container calistirma.

**Ozellikler:**
- EC2 instance yonetimi yok
- ECS ve EKS ile kullanilir
- Kullandigin kadar ode

**Ne Zaman Kullanilir:**
- Serverless container workload'lari
- Operasyonel yuku azaltma
- Degisken is yukleri


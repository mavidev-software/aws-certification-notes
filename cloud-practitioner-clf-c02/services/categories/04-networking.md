# Networking & Content Delivery Services (In-Scope)

> **Kategori:** Ağ ve İçerik Dağıtım Servisleri

## Amazon VPC (Virtual Private Cloud)

**Ne İşe Yarar:** AWS'de izole sanal ağ oluşturmanızı sağlar.

**Temel Bileşenler:**
| Bileşen | Açıklama |
|---------|----------|
| Subnet | VPC içinde IP aralığı bölümü |
| Route Table | Trafik yönlendirme kuralları |
| Internet Gateway | İnternete çıkış |
| NAT Gateway | Private subnet'ten internete çıkış |
| Security Group | Instance seviyesinde güvenlik duvarı (stateful) |
| Network ACL | Subnet seviyesinde güvenlik duvarı (stateless) |

**Public vs Private Subnet:**
| Özellik | Public Subnet | Private Subnet |
|---------|---------------|----------------|
| Internet erişimi | Doğrudan (IGW) | NAT Gateway üzerinden |
| Kullanım | Web sunucuları | Veritabanları, backend |

---

## Amazon Route 53

**Ne İşe Yarar:** DNS (Domain Name System) web hizmeti.

**Routing Policy'leri:**
| Policy | Açıklama |
|--------|----------|
| Simple | Tek kaynak |
| Weighted | Ağırlıklı dağıtım |
| Latency-based | En düşük gecikme |
| Failover | Aktif/pasif |
| Geolocation | Coğrafi konum |
| Multi-value | Birden fazla değer |

---

## Amazon CloudFront

**Ne İşe Yarar:** Global CDN (Content Delivery Network) hizmeti.

**Temel Özellikler:**
- 400+ Edge Location
- DDoS koruması (AWS Shield entegrasyonu)
- SSL/TLS sertifikası
- Lambda@Edge ile edge computing

---

## AWS Direct Connect

**Ne İşe Yarar:** Veri merkezinizden AWS'e özel bağlantı sağlar.

**Direct Connect vs VPN:**
| Özellik | Direct Connect | VPN |
|---------|----------------|-----|
| Bağlantı | Özel hat | İnternet üzerinden |
| Kurulum süresi | Haftalar | Dakikalar |
| Performans | Tutarlı | Değişken |

---

## AWS VPN (Site-to-Site VPN & Client VPN)

**Ne İşe Yarar:** Şifreli VPN bağlantısı sağlar.

**Türleri:**
- **Site-to-Site VPN:** Veri merkezi ile AWS arasında
- **Client VPN:** Bireysel kullanıcılar için uzaktan erişim

---

## Amazon API Gateway

**Ne İşe Yarar:** API oluşturma, yayınlama ve yönetme hizmeti.

**Temel Özellikler:**
- RESTful API ve WebSocket API desteği
- Throttling ve rate limiting
- Lambda, EC2 entegrasyonu

---

## AWS Global Accelerator

**Ne İşe Yarar:** AWS global ağını kullanarak performans artırır.

**CloudFront vs Global Accelerator:**
| Özellik | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| Kullanım | Statik/dinamik içerik | TCP/UDP uygulamaları |
| Cache | Var | Yok |
| IP tipi | DNS-based | Static Anycast IP |

---

## AWS Transit Gateway

**Ne İşe Yarar:** VPC'leri ve on-premises ağlarını merkezi olarak bağlar.

---

## AWS PrivateLink

**Ne İşe Yarar:** AWS servislerine özel bağlantı sağlar (internet üzerinden değil).

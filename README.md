# Sunucu Taraflı Ölçüm Altyapısı (Meta CAPI · Google Ads · GA4)

**Sorun:** Ödeme harici bir ödeme sayfasında (link ile ödeme) tamamlandığı için tarayıcı pikselleri satın almayı çoğu zaman göremiyordu. Reklam platformları eksik ve kirli veriyle optimize ediyordu: ödenmemiş siparişler dönüşüm sayılıyor, botlar ve iç test trafiği "müşteri" gibi görünüyordu.

**Çözüm:** WooCommerce için sunucu taraflı, ödemeye bağlı ve tekilleştirilmiş bir ölçüm katmanı.

## Modüller
| Modül | Ne yapar |
|---|---|
| **Meta Conversions API – Purchase** | Gerçek ödeme onayı (ödeme sağlayıcı callback'i) geldiğinde sunucudan Purchase gönderir. Checkout anında fbc/fbp, IP, UA saklanır; e-posta/telefon hash'lenir. Piksel ile aynı `event_id` → çift sayım yok. |
| **Meta CAPI – AddPaymentInfo** | Sipariş oluşturulduğu anda yüksek eşleşme kalitesiyle (EMQ) ara olay. |
| **Mesaj kaynaklı satış sinyali** | DM'den kapanan satışları Meta'ya `business_messaging` Purchase olarak bildirir → "mesaj atacak kişi" yerine "satın alacak kişi" optimizasyonu. |
| **Google Ads – ödenmiş doğrulaması** | gclid/wbraid/gbraid'i siparişe kaydeder, dönüşümü yalnızca *ödenmiş* siparişte tetikler. |
| **Google Ads offline dönüşüm akışı** | Zamanlanmış HTTPS yüklemesi için salt-okunur CSV akışları (satın alma + düzeltme). |
| **GA4 boşluk doldurma** | Mevcut entegrasyonun atlamadığı view_item / begin_checkout olaylarını ekler, Purchase'ı çiftlemez. |
| **İç trafik & bot filtresi** | Kendi testlerini (çerez/parametre) ve tarayıcı olmayan istekleri tüm reklam piksellerinden hariç tutar. |

## Teknoloji
PHP 8, WooCommerce hook'ları (HPOS uyumlu), Meta Graph API, Google Ads offline conversions, GA4.

## Pazarlama açısından neden önemli
Algoritmalar ne ölçerse onu optimize eder. Temiz, ödemeye bağlı ve tekilleştirilmiş sinyal; daha doğru ROAS okuması ve daha iyi teklif optimizasyonu demektir.

---

## 🔒 Kaynak kod

Kaynak kod bu depoda **AES-256 ile şifrelenmiş** olarak durur (`kaynak.tar.gz.enc`). API anahtarları, mağaza kimlikleri ve iş ortaklarının bilgileri şifrelemeden önce koddan temizlendi.

Kodu incelemek isteyen işverenler ve ekipler şifreyi doğrudan benden alabilir:
**[LinkedIn](https://www.linkedin.com/in/emirhanterzi/)** · **[Profil](https://www.motogate.com.tr/emirhan-terzi/)**

```bash
openssl enc -d -aes-256-cbc -pbkdf2 -iter 300000 -in kaynak.tar.gz.enc | tar xz
```

© Emirhan Terzi. Tüm hakları saklıdır. Kod lisanslı değildir; izinsiz kopyalanamaz, kullanılamaz.

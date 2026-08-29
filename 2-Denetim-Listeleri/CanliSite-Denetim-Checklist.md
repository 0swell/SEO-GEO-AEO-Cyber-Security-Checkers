# 🚀 Canlı Site Denetim Checklist

> **NE ZAMAN:** Site **yayına alındıktan sonra.** Yayın öncesi çalıştırılamaz —
> SSL sertifikası, www yönlendirmesi, güvenlik başlıklarının gerçekten
> gönderilmesi, formun üretim ortam değişkeniyle çalışması ve arama konsolu
> hiçbiri `localhost`'ta doğrulanamaz.
>
> `CyberSecurity-Checklist.md`'de `[98] yayın sonrası` bırakılan maddeler de
> burada kapatılır.

Amaç, geliştirmede görünmeyen ve yalnızca canlı ortamda ortaya çıkan
sorunları yakalamak. Tamamlananları `[x]` işaretle.

### Ne zaman çalıştırılır?

```
geliştirme      →  CyberSecurity (sürekli, yazarken uygulanır)
                     ↓
yayın öncesi    →  SEO/GEO/AEO denetimi + CyberSecurity kanıtı toplanır
                     ↓
              D E P L O Y
                     ↓
yayın sonrası   →  CanliSite-Denetim + CyberSecurity'nin [98] maddeleri
```

| Liste | Zaman | Neyi denetler |
|-------|-------|---------------|
| `CyberSecurity-Checklist.md` | Geliştirme + yayın öncesi | Uygulamanın **içini** |
| `CanliSite-Denetim-Checklist.md` | **Yayından sonra** | Yayına alınmış **halini** |


**Öncelik:** 🔴 Kritik · 🟠 Yüksek · 🟡 Orta &nbsp;|&nbsp; **⭐ = ölçümle doğrulanmalı**

**Kural:** Her madde **canlı URL üzerinden** doğrulanır. `localhost` ve
`next build` çıktısı yeterli değildir — CDN, sertifika, yönlendirme ve
üçüncü taraf betikleri yalnızca canlıda görünür.

---

## 1. 🌐 Erişim & Ayakta Kalma
> Site açılmıyorsa geri kalan hiçbir maddenin anlamı yok.

- [ ] 🔴 Ana sayfa **HTTP 200** dönüyor.
- [ ] 🔴 Sitemap'teki **her URL 200** dönüyor (301/404/500 yok). ⭐
- [ ] 🔴 `https://` çalışıyor, sertifika geçerli ve süresi dolmamış.
- [ ] 🔴 `http://` → `https://` yönleniyor.
- [ ] 🔴 `www` ve köksüz alan adından **yalnız biri** asıl; diğeri **301/308**
      ile ona yönleniyor. İkisi de 200 dönüyorsa duplicate content olur.
- [ ] 🟠 Olmayan bir yol **404** döndürüyor (200 ile "bulunamadı" sayfası **değil**). ⭐
- [ ] 🟠 404 sayfası marka dilinde; ana sayfaya ve önemli sayfalara link veriyor.
- [ ] 🟠 Yönlendirme zinciri yok (A→B→C değil, A→C).
- [ ] 🟡 Sunucu yanıt süresi (TTFB) < 600 ms. ⭐
- [ ] 🟡 Bakım/hata durumunda haber veren bir izleme kurulu (uptime monitor).

## 2. 🔗 Alan Adı & DNS

- [ ] 🔴 DNS kayıtları barındırma sağlayıcısının istediği değerlerle **birebir** aynı. ⭐
- [ ] 🟠 Kayıt firmasında **otomatik yenileme açık** — süresi dolan alan adı
      geri alınamayabilir.
- [ ] 🟠 Alan adı **işletme/kişi adına** kayıtlı; geliştiricinin üstünde değil.
- [ ] 🟡 WHOIS gizliliği açık (kişisel adres/telefon herkese görünmesin).
- [ ] 🟡 E-posta gönderiyorsan **SPF / DKIM / DMARC** kayıtları tanımlı. ⭐

## 3. 🔒 Güvenlik Başlıkları
> Tarayıcıya "bu siteyi nasıl çalıştıracaksın" diyen katman.

- [ ] 🔴 `Content-Security-Policy` tanımlı ve gerçekten uygulanıyor. ⭐
- [ ] 🔴 `Strict-Transport-Security` (HSTS) gönderiliyor.
- [ ] 🟠 `X-Frame-Options: DENY` veya CSP'de `frame-ancestors 'none'`
      (clickjacking).
- [ ] 🟠 `X-Content-Type-Options: nosniff`.
- [ ] 🟠 `Referrer-Policy` tanımlı (`strict-origin-when-cross-origin` yeterli).
- [ ] 🟡 `Permissions-Policy` ile kullanılmayan cihaz izinleri kapatılmış.
- [ ] 🟡 Sunucu/framework sürümünü sızdıran başlıklar kapalı
      (`X-Powered-By`, ayrıntılı `Server`).
- [ ] 🟡 securityheaders.com veya Mozilla Observatory taraması yapıldı. ⭐

## 4. 🔍 SEO Temelleri

- [ ] 🔴 `robots.txt` erişilebilir ve **yanlışlıkla `Disallow: /` yok**.
- [ ] 🔴 Yayındaki sayfalarda **kazara `noindex` yok**. ⭐
- [ ] 🔴 `sitemap.xml` erişilebilir, doğru URL'leri listeliyor ve arama
      konsoluna gönderildi.
- [ ] 🟠 Her sayfada **tek ve doğru `canonical`** (kendi URL'sini gösteriyor).
- [ ] 🟠 Her sayfa **benzersiz `<title>`** (≤ 60 karakter) ve
      **benzersiz `description`** (≤ 155 karakter). Tekrar eden var mı? ⭐
- [ ] 🟠 Sayfa başına **tek `<h1>`**; başlık hiyerarşisi atlamıyor.
- [ ] 🟠 Çok dilli site ise `hreflang` üçlüsü (`tr` · `en` · `x-default`)
      her sayfada **karşılıklı** tanımlı; hiçbiri 404'e işaret etmiyor. ⭐
- [ ] 🟡 Tüm görsellerde **anlamlı `alt`** var; süs görselleri `alt=""`.
      Çok dilli sitede alt metin **ziyaretçinin dilinde**.
- [ ] 🟡 Arama konsolunda önemli sayfalar tek tek indekslemeye verildi.
- [ ] 🟡 Yönetim paneli / test rotaları hem `robots.txt`'te **hem sayfada**
      `noindex` ile kapalı (yalnız disallow yetmez).

## 5. 🧩 Yapısal Veri (Schema.org)

- [ ] 🟠 JSON-LD **sözdizimsel olarak geçerli** (her blok parse ediliyor). ⭐
- [ ] 🟠 Rich Results Test ve Schema Validator'dan **hatasız** geçiyor. ⭐
- [ ] 🟠 Schema'daki her bilgi sayfada **görünür** — gizli/sahte schema yok.
- [ ] 🟡 `@id` referansları karşılığı olan düğümlere işaret ediyor.
- [ ] 🟡 Yerel işletmede `LocalBusiness` bilgileri (isim, telefon, konum)
      işletme profiliyle **birebir aynı**.
- [ ] 🟡 `dateModified` gerçek bir tarih taşıyor; her derlemede değişmiyor.

## 6. ⚡ Performans & Core Web Vitals

- [ ] 🔴 **Mobil** Lighthouse ölçümü yapıldı (masaüstü tek başına yanıltır). ⭐
- [ ] 🟠 LCP < 2.5 sn · CLS < 0.1 · INP < 200 ms. ⭐
- [ ] 🟠 LCP öğesi ne, biliniyor mu? Gecikme **yükleme** mi **render** mi? ⭐
- [ ] 🟠 Görseller modern formatta (WebP/AVIF), boyutları sayfadaki ölçüye
      uygun, `width`/`height` verilmiş (CLS = 0).
- [ ] 🟠 Üçüncü taraf betiklerin (analitik, chat, reklam) maliyeti ölçüldü;
      kritik yoldan çıkarıldı. **Tek bir analitik betiği 150+ KiB olabilir.** ⭐
- [ ] 🟡 Font `display: swap` ile geliyor, self-host edilmiş.
- [ ] 🟡 Statik varlıklar CDN'den ve uzun `Cache-Control` ile servis ediliyor. ⭐
- [ ] 🟡 Gerçek kullanıcı ölçümü (RUM) kurulu — tek seferlik lab ölçümü
      gerçeği yansıtmaz.

## 7. ♿ Erişilebilirlik

- [ ] 🟠 Metin kontrastı WCAG AA (normal metin 4.5:1, arayüz sınırı 3:1). ⭐
- [ ] 🟠 **Saydam/blur zemin üzerindeki metinler ayrıca ölçüldü** — token
      değeri yeterli görünse de gerçek arka plan farklı olabilir. ⭐
- [ ] 🟠 Dokunma hedefleri ≥ 44 px ve aralarında boşluk var. ⭐
- [ ] 🟠 Klavye ile tüm site gezilebiliyor; odak halkası görünür.
- [ ] 🟡 "İçeriğe atla" bağlantısı var.
- [ ] 🟡 Form alanlarının `<label>`'ı var; hata mesajı alana bağlı
      (`aria-describedby`).
- [ ] 🟡 `prefers-reduced-motion` tercihine uyuluyor.
- [ ] 🟡 Sayfa dili `<html lang>` ile doğru bildirilmiş.

## 8. 🔗 İçerik & Bağlantı Bütünlüğü

- [ ] 🟠 **Kırık iç link yok** — tüm sayfalardaki bağlantılar taranarak. ⭐
- [ ] 🟠 Dış bağlantılar açılıyor; `target="_blank"` olanlarda
      `rel="noopener noreferrer"` var.
- [ ] 🟠 Yer tutucu metin kalmadı (`lorem ipsum`, `TODO`, `örnek@mail.com`). ⭐
- [ ] 🟡 Telefon ve e-posta **tıklanabilir** (`tel:` / `mailto:`) ve doğru.
- [ ] 🟡 Sosyal medya bağlantıları doğru hesaba gidiyor.
- [ ] 🟡 Paylaşım kartı (OG image) üretiliyor ve doğru görünüyor —
      WhatsApp/LinkedIn'de test edildi. ⭐

## 9. 📊 Ölçüm & İzleme

- [ ] 🟠 Analitik kurulu ve **gerçek zamanlı raporda kendi ziyaretin görünüyor**. ⭐
- [ ] 🟠 Arama konsolu mülkü doğrulandı, sitemap gönderildi.
- [ ] 🟠 **Dönüşüm olayları tanımlı** (form gönderimi, telefon/WhatsApp
      tıklaması). Ölçülmeyen dönüşüm yönetilemez.
- [ ] 🟡 Hata izleme kurulu (sunucu hataları ve istemci istisnaları).
- [ ] 🟡 Harcama/kota uyarısı kurulu (barındırma, e-posta servisi, API).

## 10. 📨 Form & Dönüşüm Akışı
> En sık gözden kaçan yer: form canlıda sessizce çalışmıyor olabilir.

- [ ] 🔴 **Canlıda gerçek bir form gönderimi yapıldı ve mesaj ulaştı.** ⭐
- [ ] 🔴 Gerekli ortam değişkenleri (API anahtarı vb.) üretim ortamında
      tanımlı — geliştirmede çalışıp canlıda susmak en yaygın hata.
- [ ] 🟠 Anahtar eksikse form **sessizce başarılı görünmüyor**; kullanıcıya
      hata gösteriliyor.
- [ ] 🟠 Gönderim sonrası kullanıcıya net geri bildirim veriliyor.
- [ ] 🟠 Spam koruması çalışıyor (honeypot / hız sınırı).
- [ ] 🟡 Gelen bildirimlerin düştüğü kutu **takip ediliyor** (spam klasörü
      kontrol edildi).

## 11. ⚖️ Yasal & Güven

- [ ] 🔴 Kişisel veri toplanıyorsa **aydınlatma metni / gizlilik sayfası** var
      (Türkiye'de KVKK, AB'de GDPR).
- [ ] 🔴 Gizlilik metni **gerçeği anlatıyor** — çerez veya üçüncü taraf servis
      eklendiyse metin de güncellendi. Yanlış beyan, metnin hiç olmamasından
      daha kötü.
- [ ] 🟠 Hangi üçüncü taraf servislerin veri gördüğü sayılmış
      (analitik, e-posta gönderim servisi, barındırma).
- [ ] 🟠 Fiyat, süre ve kapsam bilgileri gerçek ve güncel.
- [ ] 🟡 Sahte referans, uydurma istatistik, hayali müşteri yorumu **yok**.
- [ ] 🟡 İletişim bilgileri (isim/adres/telefon) tüm platformlarda
      **harfi harfine aynı**.

## 12. 🔁 Yayın Sonrası Rutin

- [ ] 🟡 **48 saat sonra:** arama konsolu sitemap'i okudu mu, indeksleme
      hatası var mı?
- [ ] 🟡 **1 hafta sonra:** analitikte veri akıyor mu, dönüşüm olayları
      düşüyor mu?
- [ ] 🟡 **Aylık:** hedef anahtar kelimelerde ortalama sıra ve tıklama takibi.
- [ ] 🟡 **Aylık:** bağımlılık güncellemesi ve güvenlik taraması.
- [ ] 🟡 **Her içerik değişikliğinden sonra:** görünür güncelleme tarihi,
      `dateModified` ve sitemap `lastmod` **üçü birden** ilerletildi mi?
- [ ] 🟡 Sertifika ve alan adı bitiş tarihleri takvime işlendi.

---

<sub>Bu liste `CyberSecurity-Checklist.md` ile birlikte kullanılır: o dosya
uygulamanın **içini**, bu dosya yayına alınmış **halini** denetler.
⭐ maddeler göz kararıyla değil, komut veya araç çıktısıyla doğrulanır.</sub>

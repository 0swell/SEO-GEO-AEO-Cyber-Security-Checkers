# 🛡️ Cyber Security Checklist

> **NE ZAMAN:** Geliştirme boyunca ve **yayına almadan önce.** Uygulamanın
> içini denetler — kod, sır yönetimi, girdi doğrulama, bağımlılıklar.
> Bu maddelerin çoğu yazarken zaten uygulanır; yayın öncesi kanıt toplanır.
>
> **İSTİSNA:** §1 HTTPS ve §5 güvenlik başlıkları yalnızca canlı ortamda
> doğrulanabilir. Onlar `[98] yayın sonrası` işaretlenir ve
> `CanliSite-Denetim-Checklist.md` ile kapatılır.

Önem sırasına göre kategorilere ayrılmıştır. Tamamlananları `[x]` işaretle.

**Öncelik:** 🔴 Kritik · 🟠 Yüksek · 🟡 Orta &nbsp;|&nbsp; **⭐ = listeye eklenen ek madde**

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


---

## 1. 🔑 Sırlar & Yapılandırma
> Sızarsa uygulama **anında** ele geçirilir — en kritik katman.

- [ ] 🔴 `.env` ve tüm sırlar repo/public'te **değil** (`.gitignore`'da).
- [ ] 🔴 API key / servis anahtarları frontend'de **değil**, yalnızca sunucuda.
- [ ] 🔴 Sızmış sırları **git geçmişinden sil** ve mutlaka **yenile (rotate)**. ⭐
- [ ] 🟠 Prod/dev ayrı config; mümkünse **secret manager** kullan. ⭐

## 2. 🔐 Kimlik Doğrulama & Yetkilendirme

- [ ] 🔴 Yetki/izin kontrolü **sunucuda** yapılır; frontend'e asla güvenme.
- [ ] 🔴 **RLS (Row Level Security)** açık — erişim kuralı DB seviyesinde.
- [ ] 🔴 Şifreler **hash'li** (bcrypt/argon2), düz metin değil.
- [ ] 🟠 Admin paneli korumalı + **rol tabanlı (RBAC)** erişim.
- [ ] 🟠 Girişe **rate limit / deneme sınırı** (brute-force koruması).
- [ ] 🟠 **Zayıf şifreye izin yok** (min. uzunluk + karmaşıklık).
- [ ] 🟠 **E-posta doğrulama** (email verification) aktif.
- [ ] 🟠 Token `localStorage`'da **değil** → `httpOnly` + `Secure` çerez.
- [ ] 🟡 **2FA / MFA** desteği. ⭐
- [ ] 🟡 Oturum yönetimi: logout'ta geçersiz kıl, timeout. ⭐

## 3. 🧪 Girdi Doğrulama & Injection

- [ ] 🔴 **Server-side validation** (frontend doğrulamasına güvenme).
- [ ] 🔴 SQL Injection → **parametreli sorgu / ORM** kullan.
- [ ] 🔴 XSS → ham HTML **render etme**, çıktıyı escape/sanitize et.
- [ ] 🟠 Dosya upload'ta **tip / boyut / uzantı** doğrulaması.
- [ ] 🟠 **Mass assignment**: body'yi direkt kaydetme, alanları whitelist'le.
- [ ] 🟠 Tahmin edilebilir ID yerine **UUID / rastgele** kimlik.
- [ ] 🟡 **CSRF** koruması (token). ⭐
- [ ] 🟡 İstek gövde/boyut limiti (DoS'a karşı). ⭐

## 4. 🌐 API & CORS

- [ ] 🔴 Her endpoint **kimlik doğrulama + yetkilendirme** ile korunur; nesne/fonksiyon seviyesi yetki açığı yok (**BOLA / BFLA** — OWASP API Top 10). Debug/test/kullanılmayan endpoint'ler production'da **kapalı**. ⭐
- [ ] 🟡 API endpoint'ler **doğru bağlanmış** (integration): çağrılan tüm uçlar mevcut, doğru metod/yola bağlı ve beklenen yanıtı dönüyor — kırık/yanlış bağlantı yok. ⭐
- [ ] 🔴 CORS `*` **değil** — yalnızca güvenilen **domain whitelist**.
- [ ] 🟠 Webhook'larda **imza doğrulama** (signed webhook).
- [ ] 🟡 API için genel **rate limiting**. ⭐

## 5. 🔒 Transport & Güvenlik Başlıkları

- [ ] 🔴 **HTTPS zorunlu** (tüm HTTP → HTTPS yönlendir).
- [ ] 🟠 Güvenlik başlıkları: **HSTS, CSP, X-Frame-Options, X-Content-Type-Options**.
- [ ] 🟠 IP bazlı **rate limiting / throttling** — hacimsel (DDoS) ve kaba kuvvet saldırılarına karşı; edge/WAF veya reverse-proxy (ör. Cloudflare) katmanında uygulanır. ⭐

## 6. 📉 Hata & Log Yönetimi

- [ ] 🟠 Production'da **stack trace gösterme**; hata mesajlarını kısıtla.
- [ ] 🟡 Logları temizle; log'a **sır / PII yazma**.
- [ ] 🟡 Güvenlik olaylarını logla (audit trail). ⭐

## 7. 📦 Bağımlılık & Operasyon

- [ ] 🟠 Eski **dependency'leri güncelle**, `npm audit` benzeri denetim yap.
- [ ] 🟡 Lockfile + otomatik güvenlik taraması (Dependabot / SCA). ⭐

## 8. 💾 Yedek, Kurtarma & İzleme

- [ ] 🟠 **Otomatik yedek** + düzenli **geri yükleme testi**.
- [ ] 🟡 Hesap/veri **gerçekten silinmeli** (kalıcı silme / KVKK-GDPR).
- [ ] 🟡 **Harcama / anomali uyarısı** kur (beklenmedik trafik-maliyet).
- [ ] 🟡 **Saldırgan gibi test et** (pentest · OWASP Top 10).

---

<sub>Kaynak: iki topluluk listesi (20 "hacklenme sebebi" + 23 "yayın öncesi adım") birleştirilip önceliklendirildi; ⭐ maddeler ek olarak eklendi.</sub>

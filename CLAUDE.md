# Proje Kılavuzu ve Geliştirme Standartları — {{PROJE_ADI}}

> **BU BİR ŞABLONDUR.** `{{...}}` ile işaretli her yer projeye göre doldurulur.
> Doldurulmadan bırakılan bir alan varsa iş başlamadan sorulur, tahmin edilmez.
> Şablonun nasıl kullanılacağı: `0-BASLA.md`

---

## 1. Proje Özeti

- **Proje Adı:** {{PROJE_ADI}}
- **Canlı Adres:** `{{ALAN_ADI}}`
- **Repo:** {{REPO_URL}} — {{public / private}}. Hassas veri asla commit edilmez;
  hepsi `.env.local`'de tutulur ve `.gitignore`'a eklenir.
- **Amaç:** {{Tek cümleyle iş hedefi. Örn: "X şehrindeki yerel işletmelerin
  ‹anahtar kelime› aramasında ilk sırada çıkmak ve ziyaretçiyi WhatsApp
  üzerinden teklif talebine dönüştürmek."}}
- **Ana dönüşüm:** {{WhatsApp tıklaması / form gönderimi / satın alma / kayıt}}
- **Kimlik:** {{Kişisel marka mı, kurumsal mı? Ton nasıl?}}
  Sahte kurumsallık ("ekibimiz", olmayan tecrübe yılı) **kullanılmaz**.
- **Dil:** {{Tek dil / TR + EN}}. Çok dilliyse: birincil dil kök URL'de,
  ikincil dil prefix altında (`/en/`). Detay §4.3.
- **Hedef Kitle:** {{Yaş, meslek, teknik seviye. Örn: "30-55 yaş yerel işletme
  sahibi; teknik terim bilmez, güven ve fiyat şeffaflığı arar."}}

### Hedef Kitle Kuralı (her içerik kararında geçerli)

Bir cümle yazarken *"Bunu {{tipik müşteri}} anlar mı?"* testini uygula.

Teknik terimler public metinlerde geçmez. Karşılıkları:

| Yazma | Yaz |
|-------|-----|
| responsive | telefonda da düzgün görünür |
| SSR / SSG / hydration | (hiç geçmez) |
| deployment | yayına alırım |
| SEO optimizasyonu | Google'da bulunur |
| stack / framework | (hiç geçmez) |

Teknik derinlik yalnızca `/hakkimda` benzeri yetkinlik sayfasında, kanıt
olarak yer alır.

---

## 2. Başarı Kriterleri

| Kriter | Hedef |
|--------|-------|
| {{birincil anahtar kelime}} organik sıra | İlk sayfa → ilk 3 |
| Lighthouse (mobil) | Performance ≥ 95, SEO 100, A11y ≥ 95 |
| LCP / CLS / INP | < 2.5s / < 0.1 / < 200ms |
| Ana dönüşüm | {{ölçülecek olay}} |
| İlk yayın süresi | {{hedef}} — SEO saati erken başlasın |

**Kural:** Bu tablodaki her satır ölçülebilir olmalı. Ölçülemeyen hedef
yazılmaz.

---

## 3. Teknolojiler (Tech Stack)

Aşağıdaki seçimler bu şablonun çıktığı projede doğrulanmıştır. Yeni projede
farklı bir seçim yapılacaksa gerekçesi buraya yazılır.

- **Framework & Dil:** Next.js (App Router) + TypeScript.
- **Render Stratejisi:** **%100 statik (SSG).** Tüm public sayfalar build-time'da
  üretilir; hiçbir sayfa istemci tarafı veri çekmeye bağlı olmaz.
  Gerekçe: SEO + LCP + YZ botlarının JS render etmemesi (§4.5).
- **Stil & UI:** Tailwind CSS. Renkler CSS variable (token) olarak tanımlanır;
  component içine hardcoded hex yazılmaz.
- **İçerik:** `content/` klasöründe JSON/MDX — dosya bazlı. Metin JSX içine
  gömülmez.
- **Çok dillilik:** Sözlük dosyaları (`src/i18n/`) + çift dilli içerik JSON'u.
  Detay §4.3 ve §6.3.
- **İkonlar:** `lucide-react` (UI); `react-icons` (marka ikonları).
- **Animasyon:** Seçici ve hafif. Giriş (fade/slide-up) ve vitrin dışında yok.
  Scroll-hijack, parallax, ağır efekt **YOK**.
- **Form:** Server Action → e-posta servisi (Resend vb.). Spam koruması:
  honeypot + hız sınırı. reCAPTCHA yok (LCP ve UX bedeli).
- **Analitik:** {{Vercel Analytics / GA4}}. Google Search Console **zorunlu**.
- **Veritabanı:** {{YOK / gerekçesiyle var}}. Gerekmiyorsa açılmaz.
- **Deploy:** {{Vercel / Cloudflare Pages}}.
  > **Ticari kullanım uyarısı:** Vercel Hobby sözleşmesi ticari kullanımı
  > yasaklıyor. Müşteri projeleri **Cloudflare Pages**'te barındırılır
  > (ücretsiz, ticari kullanıma açık). Kod, platforma özel API'lere bağımlı
  > yazılmaz — taşınabilir kalır.

---

## 4. SEO Stratejisi — Projenin Kalbi

### 4.1. Anahtar Kelime Haritası

Her sayfanın **tek bir birincil hedefi** vardır. Aynı kelime iki sayfaya
verilmez (keyword cannibalization).

| Sayfa | Birincil Hedef | İkincil |
|-------|----------------|---------|
| `/{{sayfa-1}}` | {{birincil kelime}} | {{ikincil kelimeler}} |
| `/` | {{ana sayfa hedefi — hizmet sayfalarından FARKLI olmalı}} | |
| `/{{sayfa-2}}` | | |

**Kelime sırası kararı:** Hedef kitlenin gerçekte ne yazdığını araştır,
sektör jargonunu değil. (Örn: halk "web sitesi" yazar, "web tasarım" değil.)

**Yakın anlamlı iki kelime için ayrı sayfa AÇILMAZ.** Aynı arama niyetini
karşılayan iki kelime tek sayfada toplanır; iki sayfaya bölünürse link gücü
ikiye ayrılır ve **ikisi de geriye düşer.**

Tek sayfanın iki kelimeyi taşıma biçimi:

| Yer | İçerik |
|-----|--------|
| `<title>` | Her iki ifadeyi de içerir |
| `<h1>` | Birincil kelime |
| Giriş paragrafı (ilk 100 kelime) | Birincil kelime geçer |
| Bir `<h2>` | İkincil kelimeyi soru biçiminde taşır |
| Gövde | Eş anlamlılar doğal akışta geçer |

Kelime yığma yapılmaz — her ifade cümlenin doğal parçası olur.

**Cannibalization denetimi:** Yayın öncesi tüm sayfaların `<title>`,
`<h1>` ve `description` alanları yan yana konur; iki sayfa aynı ifadeyle
açılıyorsa biri değiştirilir.

### 4.2. Bölgesel Kapsam Kuralı — ÖNEMLİ

**Her şehir/bölge için ayrı sayfa AÇILMAZ.** Şehir başına klonlanmış sayfalar
Google tarafından **doorway page** sayılır ve cezalandırılır.

İkincil bölgeler şuralarda geçer:

- Ana sayfadaki "Hizmet Bölgesi" bölümü
- JSON-LD `areaServed` alanı
- İletişim ve hakkımda metinleri
- Blog yazıları

**Bu bölüm sonradan kaldırılmaz:** schema'daki `areaServed` "yalnızca sayfada
görünen bilgi yazılır" kuralına buradan dayanır.

### 4.3. Teknik SEO Zorunlulukları

- **Metadata API:** Her sayfa unique `title` (≤60 karakter) + `description`
  (≤155 karakter) + `canonical` + Open Graph.
  > Uzunluk ölçerken HTML entity kaçışlarına dikkat: `&#x27;` kaynakta 1
  > karakter, HTML'de 6 görünür. Ham string üzerinden ölç.
- **JSON-LD (`schema.org`):** Tek `@graph`, düğümler `@id` ile bağlı.
  - **Kalıcı düğümler** (root layout, her sayfa): `Person`/`Organization` →
    `LocalBusiness`/`ProfessionalService` → `WebSite`
  - **Sayfa düğümleri:** `WebPage` · `BreadcrumbList` · `FAQPage` · `Service` ·
    `BlogPosting`
  - `sameAs` ile diğer profiller bağlanır → arama motoru aynı kişiye eşler.
  - **KURAL:** Schema'ya yalnızca sayfada **görünen** bilgi yazılır. Gizli veya
    sahte schema yok. `aggregateRating` yalnızca sitede görünen gerçek yorum
    varsa eklenir.
  - `dateModified` gerçek bir tarih taşır (§4.7).
- **`sitemap.ts` + `robots.ts`:** Otomatik üretim. Çok dilliyse sitemap her
  sayfanın tüm dillerini `xhtml:link` ile listeler.
- **`hreflang` — çok dillilik zorunluluğu:** Her sayfa kendi karşılığına ve
  kendine işaret eder: `{{dil1}}` · `{{dil2}}` · `x-default`. Eksik veya
  karşılıksız `hreflang` **duplicate content** riskidir.
  Diller arasında **slug'lar farklı** olur (`/urun-listesi` ↔ `/en/products`);
  aynı slug iki dilde kullanılmaz. Her sayfanın `canonical`'ı kendi dilindeki
  URL'dir.
- **Görünür breadcrumb:** `BreadcrumbList` schema'sı basılan her sayfada
  **görünür breadcrumb da olmak zorunda** — Google "schema görünür içeriği
  temsil etmeli" diyor. SERP'te URL yerine geçtiği için tıklamayı da yükseltir.
- **Yönetim/test rotaları indekslenmez:** `robots.ts`'te `disallow` **ve**
  sayfada `noindex`. Yalnız disallow indekslemeyi garantili engellemez.
- **Başlık hiyerarşisi:** Sayfa başına **tek `<h1>`**, bölümler `<h2>`, alt
  başlıklar `<h3>`. Atlama yok.
- **Semantik DOM:** `<main>`, `<section>`, `<article>`, `<nav>`, `<footer>`.
- **Görseller:** `next/image`, AVIF/WebP, gerçek `width`/`height` (CLS = 0),
  betimleyici `alt` — **ziyaretçinin dilinde**.
- **Font:** `next/font` ile self-host. En fazla 2 aile. `display: swap`.
- **İç linkleme:** Gövde içi bağlamsal link, navbar/footer linkinden daha
  değerlidir. **Anchor text hedef kelimeyi taşır** — "Detaylar" hiçbir sinyal
  vermez, "{{Şehir}} Web Sitesi" verir.
- **404:** Olmayan yol gerçekten **HTTP 404** döner (200 ile "bulunamadı"
  sayfası değil) ve marka dilinde bir sayfa gösterir.

### 4.4. Site Dışı — Bunlar Olmadan İlk Sıra Gelmez

Yerel aramalarda organik sonuçların üstünde **harita paketi** çıkar.
Site tek başına yetmez:

1. **Google İşletme Profili** — ücretsiz. Kategori doğru seçilir, adres/telefon
   doğrulanır, hizmet listesi ve fotoğraflar eklenir, düzenli gönderi atılır.
   **İlk sıranın en büyük tek faktörü budur.**
2. **NAP tutarlılığı** — İsim / Adres / Telefon üçlüsü site, GBP ve tüm
   dizinlerde **harfi harfine aynı** yazılır. Tek karakter farkı (tire, kısaltma,
   boşluk) sıralamayı böler.
3. **Google Search Console** — sitemap gönderimi, indeksleme takibi.
4. **Yerel backlink** — ticaret odası, yerel haber, üniversite, dernek.
5. **Müşteri yorumları** — gerçek yorum. Sahte yorum **kesinlikle yok**.

### 4.5. GEO & AEO — YZ ve Cevap Motorları

**GEO** = YZ motorları (ChatGPT/Perplexity/Gemini) kaynak göstersin.
**AEO** = öne çıkan snippet ve sesli asistan doğrudan cevap seçsin.

- **`robots.txt`'te YZ botları engellenmez:** `GPTBot`, `PerplexityBot`,
  `Google-Extended`, `ClaudeBot`, `CCBot` açık.
- **`public/llms.txt`** — site kökünde; ne yaptığını, hizmetleri ve bölgeyi düz
  metin özetler. **Bağlantılar markdown biçiminde** yazılır (`[metin](url)`) —
  çıplak URL denetim araçlarında "bağlantı yok" sayılıyor.
- **JS olmadan okunabilirlik:** %100 SSG olduğu için içerik HTML'de hazır gelir.
  Çoğu YZ botu JS render etmez — statik render kararının ikinci gerekçesi.
- **Soru-cevap formatı (AEO'nun kalbi):** SSS ve bölüm başlıkları **doğal dil
  sorusu** olarak yazılır. Cevap **hemen altında, ilk cümlede, 40-60 kelime**,
  ters piramit (önce net sonuç, sonra detay).
- **Alıntılanabilirlik:** "binlerce" değil **net rakam/tarih/süre**. Uydurma
  istatistik yasak (§4.6) — gerçek olan söylenir.
- **Taranabilirlik:** kısa paragraf (2-4 cümle), madde listeleri,
  karşılaştırmalarda gerçek `<table>`, önemli tanımlar **bold**.
  > Tablo, YZ motorlarının en güvenilir çözümlediği yapıdır. Kart düzeni güzel
  > görünür ama makine için tablo kadar net değildir — ikisi birlikte konur.
- **E-E-A-T:** Yazar/kurum kimliği gerçek bir sayfayla desteklenir; `Person`
  schema, unvan ve `sameAs` ile bağlanır.

### 4.6. Yasaklar

- Keyword stuffing, gizli metin, doorway page, otomatik üretilmiş sahte içerik.
- Uydurma referans, sahte müşteri yorumu, gerçek olmayan istatistik.
- Kopyalanmış içerik. Tüm metinler özgün.
- Sunulmayan bir hizmeti sunuluyor gibi göstermek. Henüz hazır değilse
  **"Yakında" etiketi + soluk görünüm + tıklanamayan buton** kullanılır.

### 4.7. Tazelik Sinyali

Üç yer **aynı tarihi** göstermek zorunda:

1. Sayfada görünür "Son güncelleme" satırı
2. JSON-LD `WebPage.dateModified`
3. `sitemap.xml` içindeki `lastmod`

Tek bir sabit değişkenden beslenir (`src/config/guncelleme.ts` gibi) ve içerik
değiştikçe **elle** ilerletilir.

> **Neden `new Date()` kullanılmaz:** her derlemede değişir; Google hiç
> değişmeyen bir sayfada sürekli değişen lastmod görünce alanı bütünüyle yok
> sayar.

Fiyat yayınlayan sitede bu ayrıca güven meselesidir — ziyaretçi rakamın
ne zamanki olduğunu bilmek ister.

---

## 5. Sayfa Yapısı ve UI

### 5.1. Sayfa Haritası

```text
/                          {{Ana sayfa}}
/{{hizmet-1}}              {{...}}
/{{hizmet-2}}              {{...}}
/{{ornekler}}              {{Vitrin}}
/{{fiyatlar}}              {{Paketler}}
/{{hakkimda}}              {{Güven sayfası}}
/{{iletisim}}              {{Form + doğrudan iletişim}}
/gizlilik                  {{KVKK/GDPR aydınlatma metni}}
/blog  /blog/[slug]        {{Varsa}}
```

Çok dilliyse ikincil dil `/{{dil}}/` prefix altında, **slug'lar farklı**.

### 5.2. Ana Sayfa Akışı (dönüşüm sırasına göre)

1. **Hero** — Statik render; LCP elemanı burada. Kimlik + net vaat + birincil CTA.
   - `<h1>` **isim değil, hizmet + yer** olur. Marka adı `<p>` içinde durur.
   - Görsel `priority` ile yüklenir, sabit `width`/`height` (CLS = 0).
2. **Problem/Görünürlük** — Ziyaretçinin derdini onun diliyle anlat.
   Eksikle değil, **hedefiyle** karşıla ("şunu istiyor musunuz?" > "şunu
   yapamıyorsunuz").
3. **Hizmetler** — Kartlar, ilgili sayfaya link. Anchor text kelime taşır.
4. **Vitrin / Örnek çalışma** — İkna gücünün ana kaynağı. Gerçek iş yoksa
   **açıkça "örnek tasarımdır"** yazılır.
5. **Süreç** — Adımlar. Başlık soru kalıbında olur ("… nasıl işliyor?").
6. **Fiyat** — "başlayan fiyat" ile. Detay fiyat sayfasında.
7. **Hizmet bölgesi** — §4.2.
8. **SSS** — `FAQPage` schema ile. **Ana sayfada kısaltılmış liste**, tamamı
   fiyat/SSS sayfasında — aynı blok iki URL'de birebir tekrarlanmaz.
9. **Kapanış CTA.**

### 5.3. Uzun Metin — Akordiyon Düzeni

Metin ağırlıklı sayfalar duvar gibi olmaz. Açılır-kapanır bölümler kullanılır.

**SEO şartları — bunlar bozulursa akordiyon zarar verir:**

- İçerik **HTML'de hazır** basılır; tıklayınca fetch edilmez.
- `<details>`/`<summary>` kullanılır — JS gerekmez, klavye ile açılır, kapalı
  bölümlerin metni DOM'da kalır ve botlar okur.
- **İlk bölüm açık gelir** — snippet ve YZ alıntısı oradan gider.
- Başlık hiyerarşisi akordiyon yüzünden bozulmaz.
- Aynı sayfada iki akordiyon varsa `name` farklı olur.

### 5.4. Global UI Elemanları

- **Sabit birincil CTA** (WhatsApp/telefon) — tüm sayfalarda, mobilde başparmak
  erişiminde. Ön-doldurulmuş mesaj sayfaya göre değişir.
- **Sticky Navbar** — mobilde sadeleşir.
- **Dil değiştirici** — bulunduğu sayfanın diğer dildeki **karşılığına** gider,
  ana sayfaya atmaz. **Bayrak kullanılmaz** (bayrak dili değil ülkeyi temsil
  eder). Otomatik dil yönlendirmesi **yapılmaz** — botu yanlış dile atmak
  indekslemeyi bozar.
- **Dark / Light tema** — kontrast iki temada ayrı ayrı kontrol edilir.
- **Toast**, **Scroll-to-Top**, **Reduced Motion** desteği.

### 5.5. Tasarım Sistemi

- **Yön:** {{Örn: "Güven veren modern" — net, ferah, bol beyaz alan, büyük
  tipografi, seçici animasyon.}} "Havalı"lık düzen ve kalite hissiyle kurulur,
  efektle değil.
- **Kontrast:** WCAG AA (normal metin 4.5:1, arayüz sınırı 3:1).
  > **Saydam/blur zemin üzerindeki metinleri ayrıca ölç.** Token değeri yeterli
  > görünse de cam yüzeyin ve arkadaki renkli ışımanın etkisiyle gerçek oran
  > düşer; Lighthouse gerçek arka planla ölçer.
- **Renk Kuralı (60-30-10):** %60 zemin, %30 kart/yüzey, %10 vurgu.
  Vurgu rengi CTA'lara ayrılır; her yerde kullanılırsa vurgu olmaktan çıkar.
- **Palet & Tipografi:** {{belirlenecek — `ui-ux-pro-max` skill ile seçilir,
  Tailwind config'de token olarak tanımlanır}}
- **Tutarlılık:** Spacing, radius ve gölgelerde Tailwind scale dışına çıkılmaz.
- **Mobile-first:** Kararlar önce 375px'te verilir.
- **Dokunma hedefi:** ≥ 44 px. Footer ve breadcrumb linkleri dahil.
- **Metin boyutu:** Gövde ≥ 16 px hedeflenir; 12 px'in altı kullanılmaz.

### 5.6. Görsel Varlıklar

| Varlık | Kural |
|--------|-------|
| Profil/marka fotoğrafı | Kare kırp → WebP + AVIF → hedef < 60 KB. Ham dosya `public/`'e konmaz |
| Favicon | `src/app/icon.tsx` ile build-time üretilir. **96×96** — Google arama sonucunda favicon için 48'in katı kare ister |
| OG image | `opengraph-image.tsx` ile build-time üretilir. **Kaynak PNG olmak zorunda** — `next/og` (Satori) WebP çözemez |
| Mockup ekranları | Kod ile çizilir; görsel dosyası indirilmez, her boyutta net kalır, LCP'ye yük binmez |

- Tüm görseller `next/image` üzerinden; ham `<img>` kullanılmaz.
- Yuvarlaklık CSS ile verilir, görselin kendisi kare kalır.

---

## 6. Mimari ve Geliştirme Standartları

### 6.1. Kod Standartları

- **Atomic Design:**
  - `atoms/` — Button, Input, Badge, Container, Section, JsonLd
  - `molecules/` — kart, akordiyon, breadcrumb, tema tuşu, dil tuşu
  - `organisms/` — Navbar, Footer, Hero, bölümler, form
  - `sayfalar/` — birden çok rotanın paylaştığı sayfa gövdeleri
  - Templates katmanı yok — iskelet işini App Router `layout.tsx` üstlenir.
- **Server / Client ayrımı:** Varsayılan Server Component. `"use client"`
  sadece state/etkileşim gerektirende ve **"client kabuk + server içerik
  (children)"** kalıbıyla.
- **Modülerlik:** Tekrar eden mantık `hooks/` veya `utils/`'e çıkar.
- **Tek doğruluk kaynağı:** İletişim bilgileri, hizmet listesi, fiyatlar ve SSS
  `src/config/site.ts` + `content/`'te tutulur. Metin JSX içine gömülmez.
- **Performans:** İlk ekranda görünmeyen ağır bileşenler `next/dynamic` ile
  lazy yüklenir. **`ssr: false` kullanılmaz** (SEO).
- **Yorum dili:** {{Türkçe}}. Yorum *ne* yaptığını değil **neden** öyle
  yapıldığını anlatır — özellikle sezgiye aykırı kararlarda.

### 6.2. Güvenlik

`2-Denetim-Listeleri/CyberSecurity-Checklist.md` geliştirme boyunca uygulanır.
Statik sitede en kritik dördü:

- **Güvenlik başlıkları** `next.config.ts`'te: CSP, HSTS, X-Frame-Options,
  X-Content-Type-Options, Referrer-Policy, Permissions-Policy.
  `poweredByHeader: false`, `productionBrowserSourceMaps: false`.
  > **CSP yalnızca üretimde gönderilir.** Dev sunucusu React hata ayıklama için
  > `eval()` ve HMR için `ws://` kullanıyor; bunları politikaya eklemek üretim
  > kuralını da gevşetir.
- **Sunucu tarafı doğrulama** — istemci doğrulamasına asla güvenilmez.
- **Form spam koruması** — honeypot + hız sınırı. Sınır anahtarı **IP + kimlik
  alanı** birlikte olur; tek alana bakan sınır kolayca atlatılır.
  > Sunucusuz ortamda bellekteki sayaç soğuk başlangıçta sıfırlanır. Ciddi
  > koruma gerekiyorsa platform firewall'u veya görünmez captcha eklenir.
- **Sır yönetimi** — `.env*` gitignore'da; anahtar sohbete/ekrana yapıştırılmaz,
  yapıştırıldıysa **iptal edilip yenilenir**.

### 6.3. Çok Dillilik Mimarisi

Bu şablonun çıktığı projede doğrulanmış düzen:

```text
src/app/(tr)/layout.tsx        → <html lang="tr">, Türkçe sayfalar (kök URL)
src/app/(en)/layout.tsx        → <html lang="en">
src/app/(en)/en/...            → İngilizce sayfalar
src/app/not-found.tsx          → kök 404 (kendi <html>'ini basar)
src/i18n/diller.ts             → dil tipi + rota eşleşme tablosu + hreflang
src/i18n/sozluk.ts             → arayüz metinleri (buton, etiket)
src/i18n/sayfalar.ts           → sayfa gövde metinleri
content/settings/*.json        → işletme verisi, her alan { "tr": "", "en": "" }
```

Kurallar:

- **Rota eşleşmesi tek tabloda** (`rotalar`). Dil tuşu, hreflang ve sitemap
  üçü de oradan beslenir. Yeni sayfa eklenince tabloya da eklenir.
- Bileşenler `dil` prop'u alır; dil yapısını görmez, çözülmüş string alır.
- Diller arası eşleşme **indeksle** kurulur, metinle değil — metin dile göre
  değişir, eşleştirme kopar.
- **Bilinen sınır:** Next.js'te birden çok kök layout varken eşleşmeyen her yol
  tek bir kök `not-found.tsx`'e düşer; ikincil dilde ayrı 404 gösterilemiyor.
  Etki küçük (sayfa yine 404 döner, dil tuşu görünür).

### 6.4. Doğrulama Kuralı

"Bitti", "çalışıyor", "hazır" demeden önce **komutu çalıştır ve çıktıyı gör**:

- `next build` temiz geçmeli (uyarı dahil okunmalı)
- `tsc --noEmit` ve lint temiz olmalı
- Lighthouse **mobil** skorları §2'deki hedefleri karşılamalı
- JSON-LD Rich Results Test + Schema Validator'dan hatasız geçmeli
- Sayfalar hem light hem dark temada, hem 375px hem masaüstünde kontrol edilmeli
- **Canlı ortamda** ayrıca `2-Denetim-Listeleri/LiveSite-Checklist.md`

**Kanıt olmadan başarı iddia edilmez.**

---

## 7. Hedeflenen Dosya Yapısı

```text
{{proje}}/
├── src/
│   ├── app/
│   │   ├── (tr)/ (en)/               çok dilliyse iki kök layout
│   │   ├── sitemap.ts  robots.ts
│   │   ├── icon.tsx  opengraph-image.tsx
│   │   ├── not-found.tsx
│   │   └── globals.css               tasarım sistemi tokenleri
│   ├── components/
│   │   ├── atoms/ molecules/ organisms/
│   │   └── sayfalar/                 rotaların paylaştığı gövdeler
│   ├── config/
│   │   ├── site.ts                   NAP, sosyal, hizmet/paket verisi
│   │   ├── nav.ts                    menü + CTA bağlantıları
│   │   └── guncelleme.ts             tek tarih kaynağı (§4.7)
│   ├── i18n/                         dil tanımı, rotalar, sözlükler
│   ├── lib/
│   │   ├── schema.ts                 JSON-LD üreticileri
│   │   └── content.ts                content/ okuyucusu, dile göre çözer
│   ├── actions/                      Server Action'lar
│   ├── hooks/ types/ utils/
├── content/settings/*.json           çift dilli işletme verisi
├── public/
│   ├── llms.txt                      YZ motorları için düz metin özet
│   └── {{görseller}}
├── .claude/agents/                   gelistirici · test-uzmani · pazarlama-uzmani
├── 1-SEO-GEO-AEO/                    denetim listeleri (bu paketten)
├── 2-Denetim-Listeleri/              güvenlik + canlı site (bu paketten)
├── .env.local                        gitignore'da
└── CLAUDE.md                         bu dosya
```

---

## 8. Geliştirme Ekibi ve Araçlar

### 8.1. Agent'lar

| Agent | Rolü | Yetkisi |
|-------|------|---------|
| `gelistirici` | Frontend + backend. Sayfa, bileşen, form, schema, içerik, hata düzeltme | Kod yazar |
| `test-uzmani` | Build, konsol, kırık link, form davranışı, 375px/masaüstü, light/dark, Lighthouse, a11y | **Kod düzeltmez** — bulur, raporlar |
| `pazarlama-uzmani` | `1-SEO-GEO-AEO/` denetimi, rapor üretimi | **Kod düzeltmez** — denetler, öneri yazar |

**Akış:** `gelistirici` yazar → `test-uzmani` kırmaya çalışır →
`pazarlama-uzmani` denetler → bulgular `gelistirici`'ye döner.

Denetleyenlerin kod yazmaması bilinçli: kendi işini test eden taraf körleşir.

### 8.2. Araçlar

Herhangi bir tasarım/UI işine **sıfırdan elle CSS yazarak başlama.**

| Araç | Ne için | Kota |
|------|---------|------|
| `ui-ux-pro-max` skill | **İlk durak.** Palet, font eşleşmesi, stil yönü, UX kuralları | Ücretsiz |
| `frontend-design` skill | Jenerik "AI görünümü"nden kaçınan arayüz | Ücretsiz |
| `mcp__nanobanana__*` | Görsel/asset üretimi | Ücretsiz kota |
| `mcp__stitch__*` | Prompt'tan ekran tasarımı | ~350/ay |
| `mcp__21st__*` | shadcn/ui component üretimi | 100 kredi/ay — **kullanmadan önce sor** |
| `mcp__Claude_Browser__*` | Local preview, responsive, console/network denetimi | Ücretsiz |
| Playwright | Ekran görüntüsü, davranış testi | Ücretsiz |
| `seo-auditor` skill | Meta/okunabilirlik/link denetimi | Ücretsiz |

**Kural:** Hangi aracı kullanacağını kısaca söyle. Kotası dar olanlarda ve
ücretli serviste **kullanmadan önce sor**.

---

## 9. Kapsam Dışı (YAGNI)

Bunlar bilinçli olarak **yapılmaz** — istenirse V2'de tartışılır:

- Veritabanı, kendi yazılan auth sistemi, sunucu tarafı CMS
- {{Git tabanlı CMS — tek kişilik projede metin doğrudan kodda düzenlenir}}
- Şehir/bölge başına ayrı landing sayfası (§4.2)
- Online ödeme, chat widget, canlı destek
- Grafik kütüphanesi, ağır 3D / WebGL

---

## 10. SEO / GEO / AEO Denetimi

`1-SEO-GEO-AEO/` klasöründe ~200 maddelik kontrol listesi var:
`1.SEO.md` · `2.GEO.md` · `3.AEO.md`, akış `0.instructions.md`'de.

**Bu liste sona bırakılan bir sınav değil, geliştirme sırasında uyulacak
şartnamedir.** Yayın öncesi denetim sadece kanıt toplar. Sonda toplu düzeltme
yapmak zorunda kalınıyorsa süreç yanlış işlemiştir.

**Geliştirme sırasında sürekli geçerli maddeler:**

| Kaynak | Madde |
|--------|-------|
| SEO 1-3 | robots/sitemap/canonical, semantik HTML, tek `<h1>`, unique meta, `alt` |
| SEO 4 | sayfa başına tek birincil kelime, cannibalization yok |
| SEO 5 | LCP/CLS/INP, WebP/AVIF, lazy loading, dokunma hedefi |
| GEO 1-2 | YZ botları açık, `llms.txt`, JS'siz okunabilirlik |
| GEO 3-4 | net rakam/tarih, kaynaklı iddia, author kimliği |
| AEO 2-3 | JSON-LD, soru başlıkları, ilk cümlede 40-60 kelimelik cevap |
| AEO 4-5 | kısa paragraf, liste, `<table>`, snippet cevapları üstte |

**Yayın öncesi:** `4.Rapor-SABLON.txt` doldurulur, öneriler yazılır.
Orijinal `1/2/3` listeleri **asla** işaretlenmez.

**Yayın sonrası:** `2-Denetim-Listeleri/LiveSite-Checklist.md`.

---

## 11. Sonradan Öğrenilenler — Tekrar Yaşanmasın

Bu bölüm önceki projelerde **bedeli ödenerek** öğrenilmiş şeylerdir.
Yeni projede baştan uygulanır.

### Tailwind / CSS

| Konu | Karar |
|------|-------|
| **Tailwind v4 sözdizimi** | `rounded-[--radius]` v4'te kaldırıldı → **`rounded-(--radius)`** kullanılır. Eski biçim hata vermez, **sessizce düşer** — her buton köşesiz kalır ve fark edilmez |
| **`@theme` içinde kendine referans** | `--shadow-xs: var(--shadow-xs)` döngü kurar ve değeri sessizce boşaltır. Kök değişkenler farklı adla tanımlanır (`--sh-*`) |
| **Dinamik sınıf adı** | Sınıf adını dışarıdan alan yardımcılarda (`vurgula(metin, renk)`), verilen sınıf kaynakta **başka bir yerde de geçmeli** — JIT tarama string birleştirmesini görmez |
| **`backdrop-filter` + değişken** | `blur(var(--x))` derleyici tarafından sessizce düşürülebiliyor; literal değer yazılır |
| **Layer önceliği** | Utility sınıfı bir component sınıfını ezemiyorsa (`border-dashed` vs `.card`), ayrı bir component sınıfı yazılır |

### Next.js

| Konu | Karar |
|------|-------|
| **`next/og` görsel formatı** | Satori WebP çözemez → OG kaynağı **PNG** olmalı, yoksa build "not iterable" ile düşer |
| **Favicon boyutu** | Google arama sonucunda favicon için **48'in katı kare** ister; 32×32 yetmez |
| **Çoklu kök layout** | `app/layout.tsx` kaldırılıp `(dil)` grupları kendi `<html>`'ini basar. Kök `not-found.tsx` de kendi `<html>`'ini basmak ve `metadataBase`'i kendi tanımlamak zorunda |
| **Metadata şablonu** | `title.template` sayfa başlığında zaten olan markayı ikinci kez ekliyor olabilir — render edilen çıktı kontrol edilir |
| **CSP ve dev sunucusu** | React dev modu `eval()`, HMR `ws://` kullanır. CSP yalnız üretimde gönderilir |
| **Server Action gövde limiti** | Varsayılan 1 MB. Dosya yükleme eklenirse tip/boyut/uzantı doğrulaması zorunlu |

### İçerik ve SEO

| Konu | Karar |
|------|-------|
| **Aynı blok iki URL'de** | SSS veya fiyat bloğu hem ana sayfada hem alt sayfada birebir duruyorsa iki URL önemli ölçüde aynı metni taşır. Ana sayfada kısaltılır, tamamı alt sayfada kalır |
| **SSS cevap uzunluğu** | Metin her değiştiğinde **40-60 kelime** aralığı yeniden ölçülür. Kısa cevap snippet şansını düşürür |
| **Tarih içeren ifadeler** | "X ayı itibarıyla şu hizmet aktif değil" gibi cümleler net tarih taşır; durum değişince taranıp güncellenir |
| **Anchor text** | "Detaylar", "Devamı", "Tıklayın" hiçbir sinyal vermez. Link metni hedef kelimeyi taşır |
| **Çok dilli `alt` metni** | İkincil dilde birincil dilin alt metni basılıyor olabilir — kontrol edilir |
| **Üçüncü taraf betik maliyeti** | Tek bir analitik betiği 150+ KiB ve 150+ ms getirebilir. `lazyOnload` ile kritik yoldan çıkarılır; preconnect eklenir |
| **Gizlilik metni ile gerçek durum** | Analitik/çerez eklendiğinde KVKK metni de güncellenir. Yanlış beyan, metnin hiç olmamasından kötüdür |

### Süreç

| Konu | Karar |
|------|-------|
| **Metin sahipliği** | Kullanıcı metinleri tek tek düzenlediyse, ajan kendi kararıyla metin değiştirmez — **önce önerir, onay alır** |
| **Ölçüm olmadan iddia yok** | "Kontrast yeterli" demeden önce hesapla; "sayfa hızlı" demeden önce ölç |
| **Denetim bulgusu ≠ emir** | Rapor "şunu yap" der; tasarım/marka kararı kullanıcınındır. Bulgu açıklanır, maliyeti söylenir, karar sorulur |
| **Sır yapıştırıldıysa** | API anahtarı sohbete/ekrana geldiyse **iptal edilip yenilenir** — sadece dosyaya yazmak yetmez |

---

## 12. Bu Projeye Özel Kararlar

> Yeni projede bu bölüm **boş başlar** ve geliştirme sırasında doldurulur.
> Buraya yalnızca yukarıdaki bölümlerden **türetilemeyen** kararlar yazılır:
> neden öyle yapıldığı, neyin denenip geri alındığı, hangi sınırın kabul
> edildiği.

| Konu | Karar | Tarih |
|------|-------|-------|
| | | |

# 📦 Yeni Proje Paketi

> Yeni bir web sitesi projesine başlarken bu klasörü projenin köküne kopyala.
> İçinde geliştirme şartnamesi ve üç denetim listesi var — hepsi birbirine
> bağlı, tek sistem olarak çalışıyor.

---

## Ne var burada?

```text
yeniproje-paketi/
├── 0-BASLA.md                        bu dosya — senin için
├── KONU-VERILDIGINDE.md              yapay zekâ için talimat
├── CLAUDE.md                         geliştirme şartnamesi (ŞABLON)
├── 3-Agentlar/                       gelistirici · test-uzmani · pazarlama-uzmani
├── 1-SEO-GEO-AEO/
│   ├── 0.instructions.md             denetim akışı
│   ├── 1.SEO.md                      ~90 madde  (boş şablon, İŞARETLENMEZ)
│   ├── 2.GEO.md                      ~40 madde  (boş şablon, İŞARETLENMEZ)
│   ├── 3.AEO.md                      ~50 madde  (boş şablon, İŞARETLENMEZ)
│   └── 4.Rapor-SABLON.txt            denetim buraya doldurulur
└── 2-Denetim-Listeleri/
    ├── CyberSecurity-Checklist.md    uygulamanın İÇİNİ denetler
    └── CanliSite-Denetim-Checklist.md yayına alınmış HALİNİ denetler
```

---

## Nasıl kullanılır?

### 1. Kopyala

Paketin içindekileri yeni projenin köküne al:

- `CLAUDE.md` → proje kökü
- `1-SEO-GEO-AEO/` → proje kökü
- `2-Denetim-Listeleri/` → proje kökü (veya kişisel dosyalarına)

`0-BASLA.md` kopyalanmaz, o bu paketin kılavuzu.

### 2. Yapay zekâya devret

Tek cümle yeterli:

> "Konu şu: … `KONU-VERILDIGINDE.md`'yi oku ve ona göre ilerle."

`KONU-VERILDIGINDE.md` ajanın talimatıdır: sana hangi soruları soracağını,
`CLAUDE.md`'yi nasıl dolduracağını ve denetimi nasıl çalıştıracağını anlatır.
Sen sadece sorulara cevap verirsin.

Ajan şunları soracak (cevabı konudan çıkarabildiklerini sormayacak):

| Ne | Örnek |
|----|-------|
| İş hedefi | "X şehrinde ‹kelime› aramasında ilk sırada çıkmak" |
| Ana dönüşüm | WhatsApp tıklaması / form / satın alma |
| Hedef kitle | yaş, meslek, teknik seviye |
| Anahtar kelime haritası | hangi sayfa hangi kelimeyi hedefliyor |
| Sayfa listesi | kaç sayfa, hangileri |
| Dil | tek dil mi, çok dilli mi |
| Barındırma | Vercel / Cloudflare Pages |
| Yerel iş mi | belirli bir şehir/bölge hedefleniyor mu |

### 3. Geliştirirken

`CLAUDE.md` §10'daki "sürekli geçerli maddeler" tablosu her sayfa yazılırken
uygulanır. Denetim listeleri **sona bırakılan bir sınav değil**, yazarken
uyulacak şartnamedir.

`CLAUDE.md` §11 "Sonradan Öğrenilenler" bölümünü işe başlamadan bir kez oku —
orada yazan her madde daha önce zaman kaybettirmiş bir tuzaktır.

### 4. Yayın öncesi

`1-SEO-GEO-AEO/0.instructions.md`'deki akışı uygula:
kontrol → `4.Rapor-SABLON.txt`'i doldur → önerileri uygula → tekrar kontrol.

`2-Denetim-Listeleri/CyberSecurity-Checklist.md` gözden geçirilir.

### 5. Yayın sonrası

`2-Denetim-Listeleri/CanliSite-Denetim-Checklist.md` — bunlar yalnızca canlı
ortamda ortaya çıkan sorunlardır. Özellikle:

- Form gerçekten mail gönderiyor mu (ortam değişkeni üretimde tanımlı mı)
- Alan adı, sertifika, www yönlendirmesi
- Sitemap arama konsoluna gönderildi mi
- Analitikte veri akıyor mu

---

## Sırayla yapılacaklar (yeni proje)

```text
□  KONU-VERILDIGINDE.md okundu, soru turu yapıldı
□  CLAUDE.md {{...}} alanları dolduruldu ve onaylandı
□  3-Agentlar/ → projenin .claude/agents/ klasörüne kopyalandı
□  Anahtar kelime haritasını netleştir — cannibalization kontrolü
□  Tasarım yönü ve palet (ui-ux-pro-max skill)
□  Klasör yapısı + bağımlılıklar
□  site.ts / i18n / schema iskeleti
□  Sayfaları yaz (SEO maddeleri yazarken uygulanır)
□  next build + tsc + lint temiz
□  Lighthouse mobil ölçümü
□  SEO/GEO/AEO denetimi → rapor → düzeltme → tekrar denetim
□  Güvenlik listesi
□  Deploy
□  Alan adı + SSL + www yönlendirmesi
□  Search Console + sitemap + indeksleme isteği
□  Analitik + dönüşüm olayları
□  Form canlıda test
□  Google İşletme Profili (yerel işse — en yüksek etkili madde)
□  Canlı site denetim listesi
```

---

## Notlar

- Listelerdeki **⭐** işareti "gözle değil, komut veya araç çıktısıyla doğrula"
  demektir.
- Öncelik işaretleri: 🔴 kritik · 🟠 yüksek · 🟡 orta.
- `1.SEO.md` / `2.GEO.md` / `3.AEO.md` dosyaları **asla işaretlenmez** — boş
  şablon kalır, her projede yeniden kullanılır.

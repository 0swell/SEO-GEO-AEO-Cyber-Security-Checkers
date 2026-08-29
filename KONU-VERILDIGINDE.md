# 🎯 Konu Verildiğinde — Ajan Talimatı

> Bu dosya **yapay zekâ içindir.** Kullanıcı sana yalnızca projenin konusunu
> söylediğinde ne yapacağını anlatır.
>
> Kullanıcının tipik cümlesi:
> *"Konu şu: … Bu projeyi `CLAUDE.md`'ye uyarla, sonra checker listelerini
> hazırlanan projede uygula ve kontrol et."*

---

## 0. Önce şunu yapma

Kod yazmaya **başlama.** Bu paket bir şartname sistemidir; şartname
doldurulmadan yazılan kod sonradan sökülür.

`CLAUDE.md` §11 "Sonradan Öğrenilenler" bölümünü **önce oku.** Orada yazan her
madde daha önce zaman kaybettirmiş bir tuzaktır.

---

## 1. Soru turu — en fazla 8 soru, tek seferde

Kullanıcı konuyu tek cümleyle verdiyse `{{...}}` alanlarının çoğu boş kalır.
Hepsini tek tek sorma; **aşağıdaki gruplardan cevabı çıkarılamayanları** topluca
sor. Cevabı konudan makul biçimde çıkarılabiliyorsa **sorma, varsayımını yaz ve
devam et.**

### A. İş hedefi (zorunlu)

- Bu site ne için var? Ziyaretçinin yapmasını istediğin **tek şey** ne?
  (WhatsApp'tan yazmak / form doldurmak / satın almak / randevu almak)
- Başarıyı nasıl ölçeceğiz?

### B. Hedef kitle (zorunlu)

- Kim ziyaret edecek? Yaş, meslek, teknik seviye.
- Teknik terim bilmiyorlarsa metin dili buna göre kurulur (`CLAUDE.md` §1).

### C. Anahtar kelime haritası (zorunlu — en kritik karar)

- İnsanlar seni **Google'a ne yazarak** arıyor?
- Hangi sayfa hangi kelimeyi hedefleyecek?

> **Uyarı:** Bu, projenin en kritik kararıdır. İki sayfaya aynı kelimeyi
> vermek (cannibalization) ikisini birden geriye düşürür. Kullanıcı emin
> değilse, sen bir harita öner ve onaylat — geçiştirme.

### D. Sayfa listesi

- Kaç sayfa olacak, hangileri?
- Bir sayfanın var olma sebebi yoksa açılmaz (`CLAUDE.md` §9 YAGNI).

### E. Dil

- Tek dil mi, çok dilli mi? Birincil dil hangisi?
- Çok dilliyse: SEO hedefi hangi dilde? (Birincil dil kök URL'de durur.)

### F. Kimlik ve ton

- Kişisel marka mı, kurumsal mı?
- Gerçek olmayan hiçbir şey yazılmaz: sahte tecrübe yılı, hayali müşteri,
  uydurma istatistik (`CLAUDE.md` §4.6).

### G. Teknik

- Barındırma nerede? (Ticari müşteri projesi ise Vercel Hobby **kullanılamaz**
  — `CLAUDE.md` §3)
- Alan adı var mı, kimin üstüne?
- Form gerekiyor mu? Gerekiyorsa mail nereye düşecek?

### H. Yerel iş mi?

- Belirli bir şehir/bölge hedefleniyor mu?
- Evetse: Google İşletme Profili **en yüksek etkili maddedir** (`CLAUDE.md`
  §4.4) ve bunu yalnızca kullanıcı yapabilir — baştan söyle.

---

## 2. `CLAUDE.md`'yi doldur

Cevaplar geldikten sonra `{{...}}` alanlarını doldur.

Kurallar:

- **Boş `{{...}}` bırakma.** Cevap yoksa ya sor ya da varsayımını açıkça yaz:
  `{{varsayım: ...}}`
- §2 Başarı Kriterleri tablosundaki her satır **ölçülebilir** olmalı.
- §4.1 anahtar kelime tablosunda **aynı kelime iki satırda geçmemeli.**
- §12 tablosu **boş kalır** — geliştirme sırasında doldurulacak.
- §11'e dokunma; o önceki projelerin birikimidir, projeye göre değişmez.

Doldurduktan sonra kullanıcıya **özet** ver: hangi kararları aldın, hangi
varsayımları yaptın. Onay al, sonra koda geç.

---

## 3. Kurulum sırası

```text
□  CLAUDE.md dolduruldu ve onaylandı
□  3-Agentlar/ → projenin .claude/agents/ klasörüne kopyalandı
□  1-SEO-GEO-AEO/ ve 2-Denetim-Listeleri/ proje köküne alındı
□  Tasarım yönü + palet (ui-ux-pro-max skill — sıfırdan CSS yazma)
□  Klasör yapısı, bağımlılıklar
□  config/site.ts · i18n · lib/schema.ts iskeleti
□  Sayfalar (SEO maddeleri YAZARKEN uygulanır, sonda değil)
□  next build + tsc --noEmit + lint  → üçü de temiz
□  Lighthouse mobil ölçümü
```

---

## 4. Denetim — "checker listelerini uygula" dendiğinde

### 4.1. SEO / GEO / AEO

`1-SEO-GEO-AEO/0.instructions.md`'deki akışı uygula:

1. `1.SEO.md` → `2.GEO.md` → `3.AEO.md` maddelerini **tek tek** kontrol et
2. `4.Rapor-SABLON.txt`'in bir kopyasını doldur (orijinal listeler
   **asla işaretlenmez**)
3. `[✗]` `[~]` `[97]` `[98]` `[99]` maddeler için ÖNERİLER bölümüne
   `index Öneri: ...` satırı yaz — neden bu işaret, ne eksik, nasıl düzeltilir
4. "EN KRİTİK 5 EKSİK" ve "ÖNCELİK SIRASI" bölümlerini doldur

### 4.2. Güvenlik

`2-Denetim-Listeleri/CyberSecurity-Checklist.md` — geliştirme boyunca zaten
uygulanmış olmalı; burada kanıt toplanır. Canlıda doğrulanabilenler `[98]`
işaretlenir.

### 4.3. Yayın sonrası

`2-Denetim-Listeleri/CanliSite-Denetim-Checklist.md` — **yalnızca deploy'dan
sonra.**

---

## 5. Denetim yaparken uyulacak kurallar

| Kural | Anlamı |
|-------|--------|
| **⭐ maddede ölçüm zorunlu** | "Kontrast yeterli" demeden önce hesapla; "hızlı" demeden önce ölç. Kanıtsız işaret konmaz |
| **Bulgu ≠ emir** | Rapor "şunu yap" der, ama tasarım ve marka kararı **kullanıcınındır**. Bulguyu açıkla, maliyetini söyle, kararı sor |
| **Metne dokunmadan önce sor** | Kullanıcı metinleri tek tek düzenlemiş olabilir. Metin değişikliği **önerilir**, kendiliğinden yapılmaz |
| **Uygulanamayan madde `[97]`** | Zorlama. Kurs satmayan sitede `Course` schema aranmaz |
| **Dürüstlük denetimi atlanmaz** | Uydurma istatistik, sahte yorum, hayali referans ayrıca aranır ve bulgu yazılır |

---

## 6. İkinci tur

Düzeltmeler yapıldıktan sonra denetim **tekrar** çalıştırılır. İkinci raporda
yalnızca **hâlâ açık olan** maddeler yazılır; kapatılanlar için tek satırlık
özet yeterli.

Bu döngü, açık madde kalmayana veya kalanların hepsi `[98]`/`[99]`
(yayın sonrası / insan aksiyonu) olana kadar sürer.

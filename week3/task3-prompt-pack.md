# Week 3 — Task 3: Hızlı Prompt Benchmark Paketi

## A. Metin Modelleri İçin Tek Kopyala-Yapıştır Prompt (MIHENK `quiz_public_tr.md` — İlk 10 Soru)

Aşağıdaki bloğu **yeni sohbette**, **aynı ayarlarla** en az **5 farklı metin modeline** (ör. ChatGPT, Gemini, Claude, Grok, DeepSeek veya `arena.ai`) kopyalayıp yapıştır:

```text
Aşağıdaki 10 soruyu yanıtla. Çoktan seçmeli sorularda YALNIZCA doğru şıkkın harfini (A–D) yaz; kısa cevaplı sorularda YALNIZCA cevabı (en fazla 7 kelime) yaz. Açıklama ekleme. Her yanıtı soru numarasıyla ver (örn. 1. A).

1. [Bilimsel Makale Anlama · L1] Özet: Bir çalışmada 100 domates bitkisi iki gruba ayrıldı; bir gruba yeni bir gübre, diğerine normal gübre verildi. Sekiz hafta sonunda yeni gübre grubunun ortalama verimi %15 daha yüksek bulundu. Bu çalışmanın temel bulgusu nedir?
A) Yeni gübre verimi artırmıştır
B) Domatesler sekiz haftada büyümez
C) Normal gübre zararlıdır
D) İki gübre de aynıdır

2. [Bilimsel Makale Anlama · L2] Bir araştırmacının, çalışmadan önce test etmek üzere öne sürdüğü öngörüye ne ad verilir? (tek kelime — kısa cevap)

3. [Biyoloji · L1] İnsan vücudunda oksijeni dokulara taşıyan kan hücresi hangisidir?
A) Akyuvar
B) Alyuvar
C) Trombosit
D) Plazma

4. [Biyoloji · L2] Hücrede enerji (ATP) üretiminin gerçekleştiği organel hangisidir? (kısa cevap)

5. [Coğrafya · L1] Aşağıdaki enlemlerden hangisi Ekvator'a en yakındır?
A) 60° Kuzey
B) 23° Kuzey
C) 45° Güney
D) 80° Kuzey

6. [Coğrafya · L2] Türkiye'nin en yüksek dağı hangisidir? (iki kelime olabilir — kısa cevap)

7. [Debugging · L1] Aşağıdaki döngünün 1'den 5'e kadar (5 dahil) sayıları yazdırması bekleniyor ama 5 hiç yazılmıyor:
for i in range(1, 5):
    print(i)
Hatayı düzeltmek için ne yapılmalı?
A) range(1, 6) yazılmalı
B) range(0, 5) yazılmalı
C) while döngüsü kullanılmalı
D) Kodda hata yok

8. [Debugging · L2] Bir a listesini referansla paylaşmak yerine kopyalamak için a.____() biçiminde hangi metot çağrılır? (metot adı — kısa cevap)

9. [Ekonomi · L1] Bir malın arzı sabitken talebi artarsa, fiyatı genellikle nasıl değişir?
A) Artar
B) Azalır
C) Değişmez
D) Sıfırlanır

10. [Ekonomi · L2] Maliyeti 200 TL olan bir ürün %10 kârla satılırsa satış fiyatı kaç TL olur? (kısa cevap)
```

### Önceden Belirlenmiş Cevap Anahtarı & Ölçüt
- **Cevap Anahtarı:** `1. A` · `2. Hipotez` · `3. B` · `4. Mitokondri` · `5. B` · `6. Ağrı Dağı` · `7. A` · `8. copy` · `9. A` · `10. 220` (veya `220 TL`)
- **Ölçüt:** (1) Doğruluk (`/10`), (2) Süre (saniye), (3) Biçime uyum (`Tam`: sadece numara + kısa cevap, açıklama yok / `Kısmi`: açıklama ekledi), (4) Maliyet (ücretsiz / API tahmini).
- **İlgili Mevcut Benchmark:** [MIHENK Benchmark](https://github.com/gorkemergune/mihenk-benchmark) (800 soruluk Türkçe–İngilizce çok disiplinli değerlendirme seti).

---

## B. Fotoğraf (3 Model) ve Video (2 Model) İçin Sabit Prompt (16:9)

**Görsel & Video Prompt'u:**
```text
A first-year computer engineering student studying late at night at a minimalist wooden desk, laptop showing clean code on screen, a small glowing digital Pomodoro timer on the desk clearly displaying the text "25:00", warm amber desk lamp casting soft light, rain droplets on the dark window behind, cinematic realism, 16:9 widescreen aspect ratio.
```

**Önceden Belirlenmiş Puanlama Ölçütleri (1–5):**
1. **Prompt'a sadakat:** Ahşap masa, laptopta kod, "25:00" yazan Pomodoro sayacı, sıcak masa lambası, yağmurlu pencere.
2. **Yazı ve yüzler:** "25:00" yazısının okunabilirliği, öğrencinin yüz/el anatomisi bozulmadan doğal mı?
3. **Hareket & tutarlılık (Video için):** Yağmur damlalarının akışı, ekran/timer titremesi olmadan sahne tutarlılığı.
4. **Maliyet & hız:** Ücretsiz kota / üretim süresi.

**Durum:**
- [x] **Fotoğraf Modeli 1 (Google Imagen / Nano Banana):** Üretildi → `C:\Users\LENOVO\MahmutGok.github.io\assets\week3\img-google-nano-banana.jpg`
- [ ] **Fotoğraf Modeli 2 (ör. ChatGPT / GPT Image veya Grok Imagine):** Görseli indirip `C:\Users\LENOVO\MahmutGok.github.io\assets\week3\` klasörüne koy.
- [ ] **Fotoğraf Modeli 3 (ör. `arena.ai` / FLUX / Grok / Ideogram):** Görseli indirip aynı klasöre koy.
- [ ] **Video Modeli 1 (ör. Kling / MiniMax Hailuo / Wan / `arena.ai`):** 5 sn 16:9 video indir veya linkini not et.
- [ ] **Video Modeli 2 (farklı bir video modeli):** 5 sn 16:9 video indir veya linkini not et.

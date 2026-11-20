# Week 3 — Task 3: Prompt Benchmark Sonuçları (Metin + Fotoğraf + Video)

- **Referans Benchmark:** [MIHENK Benchmark](https://github.com/gorkemergune/mihenk-benchmark) (`quizzes/quiz_public_tr.md`, ilk 10 soru · 5 disiplin, L1–L2)
- **Canlı Portföy Galerisi:** [https://mahmutgok.github.io/#week3](https://mahmutgok.github.io/#week3)

---

## A. Metin Modelleri Karşılaştırması (5 Model · Aynı Prompt, Yeni Sohbet)

**Önceden Belirlenen Ölçüt:**
- **Cevap Anahtarı:** `1. A` · `2. Hipotez` · `3. B` · `4. Mitokondri` · `5. B` · `6. Ağrı Dağı` · `7. A` · `8. copy` · `9. A` · `10. 220` / `220 TL`
- **Biçim Kuralı:** Çoktan seçmelide sadece harf, kısa cevapta ≤7 kelime, açıklama yok, numarayla listeli.

| Model | Doğruluk (Strict TR) | Anlam Doğruluğu | Süre | Maliyet | Biçime Uyum | Notlar |
| --- | :---: | :---: | :---: | :---: | :---: | --- |
| **GPT-OSS 20B** (OVH / Pollinations) | **8 / 10** | 10 / 10 | 4.86 sn | Ücretsiz | Tam | 2. soruya `"hypothesis"`, 4. soruya `"mitochondria"` diye İngilizce cevap verdi (dil sapması). |
| **Gemini Flash-Lite** | **10 / 10** | 10 / 10 | ~3 sn | Düşük / Ücretsiz katman | Tam | En hızlı tam isabetli Türkçe çıktı; sıfır gereksiz açıklama. |
| **Gemini Flash** | **10 / 10** | 10 / 10 | ~8 sn | Düşük | Tam | 10/10 birebir Türkçe ve format uyumu. |
| **Gemini Pro** | **10 / 10** | 10 / 10 | ~44 sn | Orta-Yüksek | Tam | 10/10 doğru ancak L1–L2 sorularda fazladan gecikme maliyeti var. |
| **Gemini 4 Neon** | **10 / 10** | 10 / 10 | ~72 sn | Yüksek | Tam | 10/10 doğru; derin akıl yürütme L1–L2 sorularda Flash-Lite'a göre ~24 kat gecikme ekledi. |

---

## B. Fotoğraf (4 Model) ve Video (16:9) Karşılaştırması

**Sabit Prompt (16:9):**
> *"A first-year computer engineering student studying late at night at a minimalist wooden desk, laptop showing clean code on screen, a small glowing digital Pomodoro timer on the desk clearly displaying the text '25:00', warm amber desk lamp casting soft light, rain droplets on the dark window behind, cinematic realism, 16:9 widescreen."*

**Önceden Belirlenen Ölçütler (1–5):** Prompt'a sadakat · Yazı ("25:00") ve yüz/el doğruluğu · Hareket/tutarlılık (video) · Süre/maliyet.

| Tür | Model | Oran / Süre | Render Süresi | Prompt Sadakati | Yazı & Yüz/El | Genel Puan | Gözlem |
| --- | --- | :---: | :---: | :---: | :---: | :---: | --- |
| **Fotoğraf** | **Google Imagen / Nano Banana** | 16:9 | ~8 sn | 5 / 5 | 5 / 5 | **5 / 5 (En İyi)** | Masa üstünde net `"25:00"` sayacı, laptopta gerçekçi kod, yağmurlu gece penceresi, sıcak lamba ve doğal eller. |
| **Fotoğraf** | **FLUX.1-schnell** (Black Forest Labs) | 16:9 (1280×720) | 5.51 sn | 4 / 5 | 4 / 5 | **4 / 5** | Sinematik ışık, temiz eller, kod ekranı ve yağmur harika; ancak `"25:00"` Pomodoro sayacını tamamen atladı. |
| **Fotoğraf** | **Stable Diffusion 3.5 Large Turbo** | 16:9 (1024×576) | 5.34 sn | 3.5 / 5 | 2.5 / 5 | **3 / 5** | Masa lambası, laptop ve sayaç var; ancak rakamlar `"6:0"` diye bozuldu ve laptop ekranı izleyiciye dönük değil. |
| **Fotoğraf** | **NVIDIA SANA** (Pollinations) | 16:9 (1280×720) | 4.07 sn | 1.5 / 5 | 1.5 / 5 | **1.5 / 5 (En Kötü)** | En hızlı ve ücretsiz ama bulanık; laptop, kod ve `"25:00"` sayacı hiç yok, gece yağmuru yerine gündüz ışığı, elde bozulma var. |
| **Video 1** | **Lightricks LTX-Video Distilled** | ~16:9 (768×448, 3 sn) | 10 sn | 2.5 / 5 | 1.5 / 5 | **2 / 5** | Masa, sıcak lamba ve laptop var ama öğrenci yok; ekranda bozuk harfler ve `"25:00"` yerine `"2:0"` yazıyor; kamera/sahne neredeyse durağan. |
| **Video 2** | *(Hugging Face hesabın açılınca 1 tıkla Wan 2.1 / Grok eklenecek)* | 16:9 (3–5 sn) | — | — | — | — | ZeroGPU anonim kotası dolduğu için girişli hesapla eklenecek. |

---

## C. Değerlendirme Yorumu (6 Cümle)
Aynı 10 soruluk Türkçe MIHENK alt setinde modellerin tamamı soruları anlam olarak bilmesine rağmen, sıkı (exact-match) Türkçe puanlama `GPT-OSS 20B` modelinin 2. ve 4. sorularda İngilizce yanıt (`hypothesis`, `mitochondria`) vererek dil tutarlılığı hatasına düştüğünü ortaya çıkardı. Bu deney, Task 2 makalesindeki gibi tek bir ortalama puana bakmak yerine vaka bazlı (paired) inceleme yapmanın gizli hataları yakalamada neden şart olduğunu gösterdi. Ayrıca temel L1–L2 sorularda `Gemini Flash-Lite` (~3 sn), çok daha ağır ve yavaş olan `Gemini Pro` (~44 sn) ile aynı %100 doğruluğu vererek maliyet-hız avantajını kanıtladı. Görsel üretimde modeller arasındaki asıl uçurum **metin doğruluğu (`"25:00"`) ve çoklu kısıt takibinde** ortaya çıktı: yalnızca `Google Imagen / Nano Banana` tüm 6 sahne öğesini ve `"25:00"` rakamını hatasız ürettiği için açık ara en iyi sonucu verdi. Buna karşılık `NVIDIA SANA` (4.07 sn) hızına rağmen laptopu, gece yağmurunu ve sayacı kaybedip bulanık bir sonuç ürettiği için en kötü çıktı oldu; `FLUX.1-schnell` güçlü atmosferine rağmen sayacı unuttu, `SD 3.5 Turbo` ise sayacı `"6:0"` diye bozdu. Video tarafında ise `LTX-Video Distilled` hızlı (10 sn) üretmesine rağmen hem öğrenciyi hem de okunaklı metni koruyamadı; bu da açık ağırlıklı hızlı video modellerinin metin ve karmaşık sahne sadakatinde hâlâ fotoğraf modellerinin gerisinde kaldığını gösteriyor.

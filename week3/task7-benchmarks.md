# Week 3 — Task 7: Benchmark İncelemeleri & Hugging Face

- **Hugging Face Profili:** `https://huggingface.co/MahmutGok` *(hesap açıldıktan sonra teyit edilecek)*

---

## 1. TurkishMMLU

- **Kaynaklar:** [hf/AYueksel/TurkishMMLU](https://huggingface.co/datasets/AYueksel/TurkishMMLU) · [gh/ArdaYueksel/TurkishMMLU](https://github.com/ArdaYueksel/TurkishMMLU)

### Neyi ölçüyor, kaç soru var, hangi dilde?
TurkishMMLU, büyük dil modellerinin (LLM) **Türkçe** dil anlama, mantıksal muhakeme ve alan bilgisini ölçen, çeviri yerine doğrudan **Türkiye lise müfredatına** (AYT/TYT düzeyine yakın) göre hazırlanmış çoktan seçmeli bir değerlendirme setidir. EMNLP 2024 Findings / ACL 2024 SIGTURK çalışmasında tanıtılan tam veri seti **9 ders** (Biyoloji, Kimya, Coğrafya, Tarih, Matematik, Felsefe, Fizik, Din Kültürü ve Ahlak Bilgisi, Türk Dili ve Edebiyatı) kapsamında **10.000'den fazla soru** içerir; Hugging Face'teki açık `All/test` sürümünde ise **1.845 soru** (ve ders bazlı `dev`/`test` ayrımı) yer alır. Soruların her biri 5 şıklıdır ve `correctness_ratio`, `difficulty`, `grade` gibi üst veriler (metadata) barındırır.

### Nasıl puanlanıyor & Güncel en iyi skor?
GitHub reposu ve makalede standart **çoktan seçmeli tam eşleşme (exact match — accuracy)** yöntemiyle hem 0-shot hem 5-shot (ve Chain-of-Thought) değerlendirme yapılır; modelin seçtiği şık doğru harfle/indeksle karşılaştırılır. Orijinal 5-shot makale tablosunda **GPT-4o %83,1** (Sosyal Bilimler %95,3, Matematik %59,0) ve **Claude-3 Opus %79,1** ile liderken, açık ağırlıklı modellerde Llama-3 70B-IT %67,3 seviyesindedir; güncel liderlik tablolarında (TR-MMLU / Turkish MMLU) yeni nesil öncü modeller **%84+** bandına ulaşmıştır.

### Seçtiğim 2 Örnek Soru ve Cevabı (HF Dataset Viewer)
1. **Matematik (`Mathematics / test`, Satır 0):**
   - *Soru:* "Bir düzlemde 3'ü paralel olan 7 tane doğru vardır. Bu doğrular en çok kaç farklı noktada kesişir?"
   - *Şıklar:* A) 18 | B) 21 | C) 28 | D) 35 | E) 42
   - *Doğru Cevap:* **A (18)** — *(C(7,2) - C(3,2) = 21 - 3 = 18)*
2. **Tarih (`History / test`, Satır 0):**
   - *Soru:* Selçukluların Anadolu akınları öncesinde Bizans'ın askeri garnizon şehirleri ve Anadolu'da kalıcı hakimiyet için idari teşkilat + kültür eserlerinin gerekliliğini anlatan paragrafa göre "hangisine ulaşılamaz?"
   - *Doğru Cevap:* **D — "Bizans'ın garnizona dönüştürdüğü şehirlere özerklik tanıdığına"**

### Zayıf Yanı (Doymuşluk & Kontaminasyon Riski)
Genel ortalamada henüz GSM8K kadar tam doymamış (%83–85 bandı) olsa da **Sosyal Bilimler alt kategorisinde %95+ seviyelerine dayanmış** durumda. En büyük riski **veri kontaminasyonu (contamination)**: sorular Türkiye'deki lise/ÖSYM müfredatı kaynaklı olduğu için internette benzer veya birebir soru bankaları bulunabiliyor; ayrıca Türkçe'nin sondan eklemeli yapısı nedeniyle standart tokenizer'lar kelimeleri fazla parçaladığı için model başarısı sadece bilgiyi değil tokenization verimliliğini de yansıtıyor.

### MIHENK ile Farkı
TurkishMMLU yalnızca **Türkçe**, **müfredat/bilgi ağırlıklı** ve sadece **5 şıklı çoktan seçmeli** sorulardan oluşur. **MIHENK** ise **çift dilli (Türkçe–İngilizce paralel)** 800 sorudan oluşur; ezber bilgi yerine verilen metinden **akıl yürütmeyi (reasoning)** ölçer, hem çoktan seçmeli hem kısa cevaplı (≤7 kelime) format içerir ve kontaminasyonu önlemek için çoğunluğu kapalı (`private` split) tutulan sıfırdan yazılmış özgün sorular kullanır.

---

## 2. GSM8K (Grade School Math 8K)

- **Kaynaklar:** [hf/openai/gsm8k](https://huggingface.co/datasets/openai/gsm8k) · [gh/openai/grade-school-math](https://github.com/openai/grade-school-math)

### Neyi ölçüyor, kaç soru var, hangi dilde?
OpenAI tarafından 2021'de yayınlanan GSM8K, modellerin **çok adımlı matematiksel akıl yürütme (multi-step reasoning)** ve aritmetik problem çözme becerisini ölçen **İngilizce** bir ilkokul/ortaokul düzeyi sözel matematik problem setidir. `main` ve `socratic` olmak üzere iki yapılandırmaya sahiptir; her birinde **7.473 eğitim (`train`)** ve **1.319 test (`test`)** olmak üzere toplam **8.792 soru** bulunur. Çözümler tipik olarak 2 ila 8 adım gerektirir ve ara hesaplamalar `<<işlem=sonuç>>` etiketiyle işaretlenir.

### Nasıl puanlanıyor & Güncel en iyi skor?
GitHub reposundaki `grade_school_math/dataset.py` (`is_correct` ve `extract_answer`) incelendiğinde puanlamanın **kural tabanlı kesin eşleşme (regex exact match)** ile yapıldığı görülür: model çıktısındaki `####` işaretinden hemen sonra gelen sayısal değer (`r"#### (\-?[0-9\.\,]+)"`) çekilir, virgüller temizlenir ve gerçek cevapla birebir karşılaştırılır. Güncel öncü modeller (ve yeni nesil akıl yürütme modelleri) GSM8K'da **%95–%99** bandında (kimi değerlendirmelerde %97,5+ / %99'a yakın) skorlar almaktadır.

### Seçtiğim 2 Örnek Soru ve Cevabı (HF Dataset Viewer — `main / test`)
1. **Satır 0 (Janet'ın Ördek Yumurtaları):**
   - *Soru:* Janet's ducks lay 16 eggs per day. She eats 3 for breakfast and bakes muffins with 4 every day. She sells the remainder for \$2 each. How much in dollars does she make every day?
   - *Çözüm & Cevap:* `16 - 3 - 4 = 9` yumurta, `9 * 2 = 18` → **`#### 18`**
2. **Satır 2 (Ev Alım-Satım Kârı):**
   - *Soru:* Josh buys a house for \$80,000 and puts in \$50,000 in repairs. This increased the value of the house by 150%. How much profit did he make?
   - *Çözüm & Cevap:* Toplam maliyet `80,000 + 50,000 = 130,000`; değer artışı `80,000 * 1.5 = 120,000` → yeni değer `200,000`; kâr `200,000 - 130,000 = 70,000` → **`#### 70000`**

### Zayıf Yanı (Doymuşluk & Kontaminasyon Riski)
GSM8K artık ciddi biçimde **doymuş (saturated)** bir benchmark'tır: modern modellerin neredeyse tamamı **%95+** bandına yığıldığı için modeller arasındaki gerçek muhakeme farkını ayırt etme gücü kalmamıştır. Üstelik 2021'den beri tamamen açık olduğu için **yüksek kontaminasyon riski** taşır; *GSM1k* gibi temiz veya sayıları değiştirilmiş test setlerinde modellerin puanlarının belirgin şekilde (%10'a kadar) düştüğü gösterilmiştir — bu da yüksek skorların bir kısmının akıl yürütmeden çok ezberden geldiğini gösterir.

### MIHENK ile Farkı
GSM8K tek bir alana (İngilizce sözel aritmetik) odaklanır, tamamen açık olduğu için kontamine olmuştur ve %95+ ile doymuştur. **MIHENK** ise 20 farklı disiplini (hukuk, debugging, grafik analizi, mantık vb.) **Türkçe ve İngilizce** paralel biçimde ölçer, 4 zorluk seviyesi (L1–L4) sunar ve soruların büyük bölümünü gizli (`private`) tutarak GSM8K'nın yaşadığı kontaminasyon sorununu engellemeye çalışır.

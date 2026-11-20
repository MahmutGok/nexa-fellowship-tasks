# Week 3 — Task 5: Oyun Analizi (`1in8b.life` — 8 Milyarda 1)

- **İncelenen Site:** [https://1in8b.life](https://1in8b.life) (Sürüm: `v0.10.60 beta` · Geliştirici: Jin's Earth · 20 dil, Türkçe dahil)

---

## 1. Mekanikler Nasıl İşliyor?
`1in8b.life` (8 Milyarda 1), tarayıcıda kayıt gerektirmeden çalışan, gerçek küresel demografi verilerine dayalı bir **yaşam simülasyonu**. Oyunun ana döngüsü şu mekanikler üzerine kurulu:
- **Rastgele Doğum Ataması:** Saniyede dünyada yaklaşık 4 bebek doğduğu gerçeğinden yola çıkarak kullanıcıyı **doğum oranlarına göre ağırlıklandırılmış olasılıkla** (veya isteğe bağlı olarak "şu an yaşayan 8 milyar" modunda) **177 ülke ve bölgeden** birinde dünyaya getiriyor. Oyuncu nerede, hangi gelir düzeyinde veya kime doğacağını seçemiyor.
- **Zaman Akışı ve Dönem Seçimi:** Varsayılan modda bir insan ömrü yaklaşık 1 saat (yılda 40–60 saniye) sürüyor; ancak 30 dk / 1 saat / 1.5 saat seçenekleri ve `16x`'e kadar hızlandırma (`1–6` tuşları, `Space` ile duraklatma) sunuluyor. Ayrıca **Bugün**, **Geçmiş (1930–2025)**, **Gelecek (BM projeksiyonları)** ve **Herhangi Bir Dünya** seçenekleri bulunuyor.
- **Karar Kartları ve Olasılık Dengesi:** Yıllar akarken eğitim, iş, aile ve yaşam dönemeçlerinde **karar kartları (decision cards)** çıkıyor. Ancak oyuncunun kararları her şeyi belirlemiyor; doğum yeri, gelir dağılımı (Gini katsayısı) ve sağlık/güvenlik istatistikleri seçeneklerin sınırlarını ve olasılıklarını doğrudan şekillendiriyor.
- **Sonuç & Paylaşım:** Hayat sona erdiğinde ücretsiz tam **ölüm kaydı (death record)** ve paylaşım kartı üretiliyor; oyuncular yaşadıkları hayatları isimsiz olarak "Shared lives" bölümünde paylaşabiliyor.

## 2. Veri ile Hikâye Nasıl Bağlanıyor?
Oyunu klasik metin tabanlı simülasyonlardan ayıran en güçlü nokta, rastgeleliğin uydurma sayılarla değil **doğrulanabilir uluslararası kurum verileriyle** beslenmesi:
- **Kullanılan Veri Kaynakları:** UN World Population Prospects 2024 (doğum, nüfus, yaşam beklentisi), UN IGME (çocuk ölümleri), WHO & UN MMEIG (anne ölümleri, tütün, trafik kazaları), UNDP & UNESCO (eğitim), World Bank (gelir, Gini eşitsizliği, sağlık harcaması), ILO (tarım istihdamı), UNAIDS, UNODC, UNICEF ve World Happiness Report.
- **Hikâyeleştirme Köprüsü:** Kuru istatistik tabloları yerine her sayısal olasılık (örneğin bir ülkenin çocuk ölüm oranı, eğitim erişimi ya da 1930–2025 arası savaş/tarih verisi) o yıl yaşanan **tekil bir insan hikâyesine ve karar kartına** tercüme ediliyor. Üstelik pek çok olgunun yanında doğrudan veri kaynağı gösteriliyor ve kullanıcı `/en/c/` sayfasından 177 ülkenin ham istatistiklerini inceleyebiliyor.

## 3. Tasarımın Artıları ve Eksileri
### Artıları
- **Sürtünmesiz Giriş (Zero-Friction Onboarding):** Hesap açma, reklam veya oyun içi pay-to-win mekaniği yok. Veriler tarayıcıda yerel tutuluyor, cihaz değiştirmek isteyenler için `Move my lives` kodu ve PWA ("Ana Ekrana Ekle") desteği sunuluyor.
- **Erişilebilirlik ve Duygusal Güvenlik:** Ekran okuyucu uyumu, tam klavye navigasyonu (`Space`, `1–6`, `Esc`), `prefers-reduced-motion` desteği, 2 saat üzeri/gece kullanımda mola hatırlatması ve ağır temaları gizleyen "Leave out frightening scenes" / sınıf modu çok düşünceli tasarlanmış.
- **Şeffaf AI Kullanımı ve Gelir Modeli:** Hakkında sayfasında kod ve metinlerin **Anthropic Claude** desteğiyle, operatör denetiminde yazıldığı açıkça paylaşılmış; tek ücretli unsur hayatı etkilemeyen isteğe bağlı \$3'lık hatıra paketi (*keepsake pass*).

### Eksileri
- **Tempo / Sabır Eşiği:** Varsayılan 1 saatlik akış (yılda 40–60 sn) hızlı tüketim alışkanlığı olan yeni kullanıcılar için başlangıçta yavaş hissettirebilir; `16x` hızlandırma kontrolünü fark etmeyen kullanıcı sıkılabilir.
- **Metin Yoğunluğu ve Kontrol Hissi:** Görsel grafiklerden çok metin ve istatistik kartlarına yaslandığı için yüksek etkileşimli oyun arayanlara pasif gelebilir; olasılık temelli ani olaylar (erken hastalık/kaza) oyuncuda "kararlarım yeterince etkili değil" hissi yaratabilir — her ne kadar oyunun ana felsefesi tam olarak bu şans eşitsizliğini göstermek olsa da.

## 4. Kendi Projemize Neler Taşınabilir?
1. **Hesapsız Başlangıç + Yerel Kalıcılık:** `StudySprint` uygulamamızda yaptığımız `localStorage` yaklaşımını bir adım ileri taşıyıp ileride `Move my lives` benzeri **dışa/içe aktarılabilir tek satırlık yedekleme kodu** ekleyebiliriz.
2. **Klavye-Öncelikli ve Erişilebilir UI:** `1in8b.life`'ın `Space` (duraklat), rakam tuşları (hız/seçim) ve ekran okuyucu (`aria-live`) disiplinini kendi web araçlarımıza standart olarak koyabiliriz (bu hafta `StudySprint`'e `Space` ve `R` kısayollarını ekledim).
3. **Veriyi Hikâyeye Çeviren Arayüz:** Soyut verileri (örneğin çalışma istatistikleri veya benchmark skorları) sadece ham tablo olarak vermek yerine, yanında kaynağı açık ve kullanıcıya anlamlı özet kartlar sunmak ürünün etkisini katlıyor.

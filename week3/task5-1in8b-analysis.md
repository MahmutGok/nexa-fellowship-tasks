# Week 3 — Task 5: Oyun Analizi (`1in8b.life` — 8 Milyarda 1)

- **İncelenen Site:** [https://1in8b.life](https://1in8b.life) (Sürüm: `v0.10.60 beta` · Geliştirici: Jin's Earth · 21 dil, Türkçe dahil)
- **Canlı Oynanış Gözlemi (3 Tam Hayat):** Cibuti'de kırsalda doğan Hassan (51 yaşında romatizmal kalp hastalığıyla vefat), Ekvador'da doğan Adriana (76 yaş, inme; 16 yaşında erken evlilik baskısı, \$764 hastane borcu) ve ABD/Los Angeles'ta doğan Evelyn (69 yaş, zatürre; serebral palsi ile yaşam).

---

## 1. Mekanikler Nasıl İşliyor?
- **Şansa Dayalı Doğum & "Aynı Saniyede Doğanlar":** Kullanıcı kayıt olmadan **177 ülke ve bölgeden** birinde (güncel sürümde 172'den 177'ye çıkmış) doğum payına göre ağırlıklı rastgele doğuyor. Oyunun başında ICAO tarzı bir doğum kartı (ülke, gelir dilimi 1–5, ebeveyn mesleği, din/köken) ve aynı saniyede farklı ülkelerde doğan **4 paralel akran (cohort peer)** atanıyor.
- **Zaman Akışı, Odak & Dönüm Noktaları:** Varsayılan 1 saat (veya 30 dk / 1.5 saat) temposunda `1x–16x` hız, `Space` ile duraklatma, `Next »` ve **Auto mode** var. Yaşlara göre değişen yıllık odak çipleri (*Study*, *Focus on work*, *Health care*, *Rest*) ile 4 temel istatistik (`0–100`: **Health, Happiness, Knowledge, Relationships**) ve **PPP gelir / net varlık** takip ediliyor. **5, 15, 30, 50 ve 70 yaşlarında** oyun duraklayıp akranların durumunu ve hayatta kalma oranlarını karşılaştırıyor.
- **Seçimler vs. Olasılık (RNG):** Seçimler sonucu garanti etmiyor, sadece olasılığı kaydırıyor (ör. Ekvador'daki Adriana `Relationships 99` olmasına rağmen %22 çocuk evliliği oranlı ülkede 16 yaşında ailesini ikna edemedi). Ölüm ise ortalama ömre sabitlenmiyor; her yıl ülkeye ve cinsiyete özel yaşam tablolarından (life tables) zar atılarak belirleniyor.
- **Kapanış ("The Last Moments" & "Record of Death"):** Ölümde kültüre/dine uygun piksel-art sahnesi açılıyor; ardından ölüm nedeni, kohort sıralaması, 4 davranış ekseninden hesaplanan **16 "grain" + özel renk kartı**, 1 yıl = 1 kare mutluluk haritası (*Life map*) ve miras dağılımı sunuluyor.

## 2. Veri ile Hikâye Nasıl Bağlanıyor?
- **Kapsamlı Gerçek Kaynaklar:** UN World Population Prospects 2024, UN IGME, WHO (Global Health Estimates 2021), World Bank (WDI, Gini, Enterprise Surveys), UNDP/UNESCO, ILO, UNICEF, UNAIDS, UNODC, World Happiness Report 2024, EM-DAT ve OECD verileri kullanılıyor.
- **Makro Veriden Mikro Hikâyeye:** Kuru istatistik tabloları yerine her ülke verisi o yıl yaşanan kişisel olaya dönüşüyor (ör. Ekvador'da cepten sağlık harcaması %29 olduğu için hastaneden \$764 borç çıkması; Cibuti'de ILO kayıt dışı istihdam oranına göre iş seçeneklerinin daralması). Üstelik olay kartlarının hemen altında **tek satırlık ampirik kaynak dipnotları** ve hassas konularda (alkol, şiddet) doğrudan `findahelpline.com` yardım bağlantısı yer alıyor.

## 3. Tasarımın Artıları ve Eksileri
### Artıları
- **Görsel ve Duygusal Anlatım:** Canlı piksel-art sahne günün saatine, hava durumuna, ev tipine ve karakterin yaşlanmasına göre anlık güncelleniyor; koyu tema + serif başlıklar "Record of Death" ekranına belgesel ciddiyeti katıyor.
- **Sürtünmesiz Giriş & Erişilebilirlik:** Hesap/reklam yok, yerel tarayıcı kaydı (`Move my lives` aktarım kodu + PWA), ekran okuyucu uyumu, klavye kısayolları (`Space`, `1–6`, `Esc`), `prefers-reduced-motion`, 2 saatlik mola uyarısı, **"Leave out frightening scenes"** ve **Sınıf Modu (Classroom mode)** bulunuyor.
- **Şeffaf AI ve Etik Gelir Modeli:** Hakkında sayfasında Anthropic Claude desteğiyle geliştirildiği açıkça belirtilmiş; tek ödeme oynanışı etkilemeyen isteğe bağlı hatıra paketi (*Keepsake pass*).

### Eksileri (Canlı Testte Yakalanan Somut Sorunlar)
- **Mobil Modal ve Toast Çakışması:** `420px` dar ekranda uzun karar ve dönüm noktası modallarında **çift dikey kaydırma çubuğu** oluşuyor ve altta beliren beyaz bildirim (`<status>` toast) 3. seçeneğin ve *"Hide decision cards"* butonunun üstünü kapatıyor.
- **Yapışkan Çubuk & Kontrast Sorunları:** "Record of Death" ekranındaki sabit alt çubuk (*Share / Be born again*) alttaki tablo satırlarını örtüyor; 21 yaş karar modalı yarı saydam açıldığı için arka plan yazıları okunabilirliği düşürüyor.
- **Kırpılan Etiketler ve Dar Masaüstü Düzeni:** Üst durum çubuğunda uzun odak çipleri varken etiketler `Hea...`, `Hap...`, `Bon...` diye kesiliyor; `1280px` masaüstü ekranda bile oyun alanı `~405px` tek sütuna kilitli kalıyor. Ayrıca her sayfa yüklemesinde Cloudflare `beacon.min.js` nedeniyle konsolda CSP `script-src 'self'` hatası düşüyor.

## 4. Bizim Projemize Neler Taşınabilir?
1. **Satır İçi Mikro-Kaynaklar (*Inline Citations*):** Benchmark veya veri odaklı web sayfalarımızda kaynakları en alta gömmek yerine ilgili kartın/sayının hemen altında tek satırla göstermek güveni artırır.
2. **Aynı Saniye / Yan Yana Karşılaştırma:** `1in8b.life`'ın 4 akranı aynı anda simüle etmesi gibi, portföyümüzdeki Task 3 model karşılaştırmalarında çıktıları yan yana aynı eksende göstermek farkı anında görünür kılıyor.
3. **Hesapsız Yerel Kayıt + Klavye-Öncelikli Erişilebilirlik:** `StudySprint`'teki `localStorage` yapısına `Move my lives` tarzı dışa aktarma kodu eklenebilir; `Space`/`R` klavye kısayolları ve temiz `aria-live` bildirimleri tüm projelerimizde standart yapılabilir.

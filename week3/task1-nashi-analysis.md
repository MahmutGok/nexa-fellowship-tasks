# Week 3 — Task 1: Repo Analizi (`orkunisitmak/nashi-app`)

- **Orijinal Repo:** `https://github.com/orkunisitmak/nashi-app`
- **Fork Linkim:** `https://github.com/MahmutGok/nashi-app`
- **Test Edilen Sürüm:** `v0.7.17-48` (`Nashi-Windows-x64.zip`, 91.6 MB · Windows 11 x64)

---

## 1. Bu Uygulama Hangi Problemi Çözüyor? Kimin İçin?
Günlük kullanımda görsel dönüştürme, kırpma, PDF birleştirme/bölme, video kesme, QR okuma veya web'den medya indirme gibi işler için kullanıcılar genelde güvensiz online sitelere dosya yüklemek ya da 5–6 farklı masaüstü programı kurmak zorunda kalıyor. **Nashi**, bu dağınık dosya işlemlerini tek bir yerel (offline-first) masaüstü aracında toplayarak **46 girdi formatı ve 27 aracı** sürükle-bırak ve **araç çarkı (wheel)** etkileşimiyle saniyeler içinde çözmeyi hedefliyor. İçerik üreticileri, öğrenciler ve sık sık PDF/görsel/medya formatı dönüştüren bilgisayar kullanıcıları için geliştirilmiş.

## 2. Repo Yapısı ve AI ile Nasıl Yazılmış?
Repo incelemesinde projenin yapay zekâ destekli modern bir mühendislik akışıyla geliştirildiği çok net görülüyor:
- **AI İzleri (Commit & PR Geçmişi):** Son commit'lerin tamamına yakınında `Co-Authored-By: Claude <noreply@anthropic.com>` ve doğrudan `Claude-Session` bağlantıları bulunuyor (ör. `#824`, `#823`, `#822`, `c01675b`). Topluluktan gelen PR'lar (ör. `#135`, `#636`, `#757`) Claude Code oturumları üzerinden ana dala (`main`) uyarlanarak alınmış ve gerçek katkıcılar `Co-authored-by` ile belirtilmiş.
- **Otomasyon ve Kalite Kapıları:** Kök dizinde `.coderabbit.yaml` (AI kod inceleme aracı) ve `.mergify.yml` (otomatik PR yönetim kuralları) mevcut. Ayrıca `Tests/` altında `NashiCoreTests`, `NashiUpdatesTests` ve Windows tarafında `Nashi.Verify` ile gerçek UI/klavye testi yapan `Nashi --smoke-test` altyapısı kurulmuş.
- **Sürüm Notları & Dokümantasyon:** Hem İngilizce (`README.md`) hem Türkçe (`README.tr.md`) dokümantasyon, `windows/README.md`, `FEATURE-PARITY.md` ve her sürümde `SHA256SUMS.txt` doğrulama dosyası tutuluyor.

## 3. Kurulum, Çalıştırma ve Takıldığım Nokta
- Windows için `v0.7.17-48` sürümünün `Nashi-Windows-x64.zip` (91.6 MB) paketi indirildi ve ayrı bir klasöre çıkarılıp `Nashi.exe` ile çalıştırıldı (.NET 10 self-contained olduğu için ayrıca .NET kurulumu gerekmedi).
- **Kurulum Bulgusu:** README'de de belirtildiği gibi Windows Authenticode yayıncı imzası bulunmadığı için işletim sistemi ilk açılışta güvenlik onayı isteyebiliyor; ayrıca temel görsel/PDF işlemleri hemen çalışırken medya/Office araçları ilk açılışta ek bağımlılık indirmesi gerektiriyor.
- **Test Edilen Özellikler:** *(Uygulamayı açıp denediğin 3 özelliği — ör. dosya ekleme & `Ctrl+Space` araç çarkı, görsel dönüştürme/kırpma, PDF veya QR okuma — ve ekran görüntünü buraya ekle.)*

## 4. 3 Güçlü Yan
1. **Çift Platform & Yerel Mimari:** macOS'ta Swift, Windows'ta .NET 10 / WPF ile yerel performans sunarken ortak 27 araç / 46 format kataloğunu koruyor; kullanıcıya ayrıca web üzerinden kurulumsuz deneyebileceği etkileşimli bir playground sunuyor.
2. **Güvenlik ve Doğrulama Bilinci:** İndirilen harici araçların (FFmpeg, Office vb.) SHA-256 hash'leri kod içinde sabitlenmiş (`Toolchain.cs`), sürümlerde `SHA256SUMS.txt` paylaşılıyor ve hatalı görsel dönüşümünün başka dosyaları silmesini önlemek gibi (`#824` / `c01675b`) regresyon testli düzeltmeler yapılıyor.
3. **Topluluk ve Çok Dilli Dokümantasyon:** Türkçe/İngilizce README, net kısayol rehberi, `CONTRIBUTING.md`, `SECURITY.md` ve topluluk PR'larına açıkça kredi verilmesi açık kaynak kültürü açısından örnek düzeyde.

## 5. 3 Zayıf Yan veya Bulduğum Hata
1. **Sürüm Notunda Unutulmuş AI Kontrol Yorumu (Doğrudan Bulgu):** En son yayınlanan `v0.7.17-48` GitHub Release açıklamasının en başında şu gizli HTML yorumu unutulmuş: `<!-- Before publishing: the "Tested with" paragraph is carried over from 0.7.16 and I did not run or confirm any of these checks for this commit... -->`. Bu durum, AI ile üretilen sürüm notunun yayın öncesi son insan kontrolünden kaçtığını gösteriyor.
2. **Windows Paket Boyutu ve İmza Eksikliği:** macOS sürümü 14.2 MB iken self-contained .NET 10 WPF paketi nedeniyle Windows ZIP dosyası **91.6 MB** (6 kattan büyük) ve Authenticode imzası olmadığı için Windows SmartScreen uyarısı tetikliyor.
3. **Windows Arayüz Pürüzleri ve Açık Issue Yoğunluğu:** Repoda 153 açık issue var ve Windows'a özgü görsel/etkileşim hataları (ör. açık `#819` — araç çarkındaki başlıkların harf ortasından bölünmesi) macOS sürümünün biraz gerisinde kaldığını gösteriyor.

## 6. Kendi Projemde Kopyalayacağım Alışkanlıklar
- **Anlamlı Commit + Regresyon Testi:** `c01675b` commit'indeki gibi "hata neydi, nasıl düzelttim, bunu kilitleyen test nedir?" yaklaşımını kendi projelerime (özellikle `StudySprint` ve gelecek projelere) taşıyacağım.
- **Doğrulanabilir README ve Sürüm Notları:** Kısayolları, sınırlamaları ve bilinen farkları (`FEATURE-PARITY.md` gibi) gizlemeden şeffafça yazmak; AI destekli dokümanları yayınlamadan önce mutlaka insan gözüyle kontrol etmek.

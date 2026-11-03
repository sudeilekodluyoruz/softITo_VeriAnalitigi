📶 İstanbul | İBB Wi-Fi Kullanıcı Analizi

İBB Açık Veri Portalı'ndaki Wi-Fi yeni kullanıcı verisini kullanarak hazırladığım tek sayfalık Power BI raporu. Rapor, "Yabancı abonelerin oranı yaz aylarında kıştan yüksektir" önermesini 2015–2021 verisi üzerinden test ediyor.

🎯 Önerme ve Sonuç

Önerme: Yabancı abonelerin oranı yaz aylarında (Haziran–Ağustos) kış aylarından (Aralık–Şubat) yüksektir.

Sonuç: ❌ Önerme çürütüldü.

Mevsim	Yabancı Abone Oranı
Yaz	%17,40
Kış	%22,53
Fark	5,12 yüzde puan (kış lehine)
Seyrelme etkisi: Yazın yerli abone sayısı yabancı abonelerden daha hızlı arttığı için yabancı oranı düşüyor.
İlçe bağlamı: Yabancı abone yoğunluğu Fatih ve Beyoğlu ilçelerinde öne çıkıyor.
📊 Rapor İçeriği

Rapor tek sayfadan oluşuyor (Kış-Yaz Analizi, 1920×1080) ve iki bölüme ayrılıyor:

A — Mevsim Karşılaştırması

Yaz / Kış yabancı oranı ve fark (yüzde puan) kartları
Mevsime göre yabancı oranı sütun grafiği
Yabancı–yerli abone dağılımı (%100 yığılmış çubuk)
Mevsim × yıl bazında yabancı oranı

B — Zaman İçinde

Aylara göre yabancı abone oranı çizgi grafiği
Ay × mevsim yılı ısı haritası (matris)
İlçelere göre 10 bin kişiye düşen yabancı abone (2015–2021 toplamı)
Yıl ve ilçe dilimleyicileri (slicer)
🗂️ Veri Kaynakları
Veri Seti	Kaynak	Kullanım
Günlük İBB Wi-Fi Yeni Kullanıcı Verisi (2015–2021)	İBB Açık Veri Portalı	Yerli/yabancı abone sayıları
Nüfus Bilgileri	İBB Açık Veri Portalı (TÜİK)	İlçe nüfusu, 10 bin kişiye oran hesabı
🧱 Veri Modeli

Yıldız şemasına yakın bir model kurdum: iki olgu (fact) tablosu, iki boyut (dimension) tablosu.

        DimYil ──────┐
                     ├──► FactWifi
        DimIlce ─────┤
                     └──► FactNufus
Tablo	Tür	Açıklama
FactWifi	Olgu	Wi-Fi yeni kullanıcı kayıtları
FactNufus	Olgu	İlçe bazlı nüfus verisi
DimIlce	Boyut	İlçe listesi
DimYil	Boyut	Yıl listesi
Ölçüler (Measures)

Yabanci Orani · Yaz Orani · Kis Orani · Fark (yp) · Yabanci Abone · Yerli Abone · 10 Bin Kisiye Yabanci Abone

🚀 Nasıl Açılır?
Bu repodaki Power BI dosyasını indir.
Power BI Desktop'ı aç (ücretsiz).
Dosya → Aç ile raporu yükle.

Not: Rapor Power BI Desktop'ta hazırlandı; tarayıcıda doğrudan önizleme için Power BI Service'e yayınlanması gerekir.

🛠️ Kullanılan Araçlar
Power BI Desktop
Power Query (veri dönüştürme)
DAX (ölçüler)

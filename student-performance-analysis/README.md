# 📊 Student Performance Analysis

## 📌 Proje Hakkında

Bu proje, öğrencilerin günlük sınav sonuçları ve performans değerlendirme puanlarını analiz etmek amacıyla Python kullanılarak geliştirilmiştir.

Proje kapsamında 50 öğrencinin, 3 farklı gruptaki ve 13 günlük performans verileri incelenmiştir. Elde edilen veriler çeşitli istatistiksel yöntemlerle değerlendirilmiş ve grafiklerle görselleştirilmiştir.

## 🎯 Projenin Amacı

- Öğrencilerin başarı durumlarını analiz etmek.
- Grupların performanslarını karşılaştırmak.
- En başarılı öğrencileri belirlemek.
- Günlük sınav ortalamalarındaki değişimleri incelemek.
- İstatistiksel analizler gerçekleştirmek.
- Veriler arasındaki ilişkileri araştırmak.

## 🛠️ Kullanılan Teknolojiler ve Kütüphaneler

- **Python**
- **Pandas:** Veri okuma, düzenleme ve analiz.
- **NumPy:** Matematiksel işlemler.
- **Matplotlib:** Veri görselleştirme.
- **Seaborn:** İstatistiksel grafikler.
- **OpenPyXL:** Excel dosyalarını okuma.
- **Jupyter Notebook:** Analiz ortamı.

## 📂 Proje İçeriği

| Dosya | Açıklama |
|---|---|
| `degerlendirme_analizi.ipynb` | Veri analizi ve görselleştirme kodları |
| `degerlendirme_veri.xlsx` | Analizde kullanılan veri seti |

## 📈 Gerçekleştirilen Analizler

1. Öğrencilerin gruplara göre dağılımı.
2. Eksik verilerin kontrol edilmesi.
3. Min-Max normalizasyon yöntemi.
4. Başarı puanlarının hesaplanması.
5. En başarılı 5 öğrencinin belirlenmesi.
6. En başarılı grubun tespit edilmesi.
7. Grupların günlük başarı değişimlerinin incelenmesi.
8. Günlük sınav ortalamalarının hesaplanması.
9. Tanımlayıcı istatistiklerin incelenmesi.
10. Araştırma sorularının korelasyon ve istatistiksel yöntemlerle değerlendirilmesi.

## 🧮 Başarı Puanı Hesaplama

Öğrenci başarı puanları, normalize edilmiş sınav ve günlük değerlendirme puanları kullanılarak hesaplanmıştır.

**Kullanılan formül:**

Başarı Puanı = (Normalize Sınav Notu × 0.70) + (Normalize Değerlendirme Puanı × 0.30)

Normalizasyon işleminde Min-Max yöntemi kullanılmıştır:

X_norm = (X_i - X_min) / (X_max - X_min)

## 🚀 Projeyi Çalıştırma

1. Repository'yi bilgisayarınıza indirin veya klonlayın.

2. Gerekli kütüphaneleri yükleyin:

```bash
pip install pandas numpy matplotlib seaborn openpyxl jupyter
```

3. Excel veri setini notebook dosyasıyla aynı klasöre yerleştirin.

4. Jupyter Notebook'u başlatın:

```bash
jupyter notebook
```

5. `degerlendirme_analizi.ipynb` dosyasını açarak hücreleri sırasıyla çalıştırın.

## 📚 Kazanımlar

Bu proje sayesinde Python ile veri analizi, veri ön işleme, normalizasyon, istatistiksel değerlendirme ve veri görselleştirme konularında uygulamalı çalışmalar gerçekleştirilmiştir.

---

**Geliştiren:** Sude Nur Kılıç

**Alan:** Bilgisayar Programcılığı | Veri Analitiği

**Konu:** Python ile Veri Analizi ve Görselleştirme

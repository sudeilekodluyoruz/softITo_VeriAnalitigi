##  11. Gün: Power BI'a Giriş, Sanal Makine ve Veri Modelleme (26 Ekim 2026)

Bugün Veri Analitiği eğitiminde görselleştirme ve iş zekası (Business Intelligence) süreçlerinin endüstri standardı olan **Microsoft Power BI**'a geçiş yaptık. 

Kullandığım macOS işletim sistemine Power BI Desktop'ı kurabilmek için öncelikle sanal makine (Virtual Machine) aracılığıyla **Windows ortamı kurulumu** gerçekleştirdik. Windows ortamının stabil çalışmasının ardından Power BI Desktop kurulumunu tamamlayıp veri çekme, dönüştürme ve modelleme çalışmalarına başladık.

### 1. Power BI'a Veri Aktarımı (Get Data)
Bir analiz projesine başlarken ilk adım, verileri doğru şekilde içeri aktarmaktır (Get Data). Bugün `demo` klasörümüzden farklı formatlardaki verileri Power BI içerisine çektik:
*   **Klasörden Veri Çekme (Folder):** Çok sayıda CSV dosyasını tek tek almak yerine, `Sales_Data_csv_files` klasörünü topluca içe aktararak verileri tek bir tablo halinde birleştirdik.
*   **Excel ve Metin Dosyaları:** `Country_Order_Log.xlsx`, `orders.csv` ve `products.txt` gibi farklı yapılandırılmış dosyaları ilgili konektörler üzerinden projeye dahil ettik.

### 2. Power Query ile Veri Temizleme (Transform Data)
Veriler içe aktarıldıktan sonra, raporlamaya ve analize uygun hale getirmek için **Power Query Editor** arayüzü kullanıldı:
*   **Sütun Veri Tiplerini Belirleme (Data Types):** Sipariş tarihleri (`Order Date`) *Date*, sayısal değerler (`Age in Yrs`, `Quantity`, `Sales`) *Whole Number/Decimal*, metinler (`Category`, `City`, `Gender`) ise *Text* formatına dönüştürüldü.
*   **Veri Kalitesi Kontrolü:** Power Query arayüzündeki *Column Quality*, *Column Distribution* ve *Column Profile* (View sekmesi) araçları kullanılarak verilerdeki eksik (Empty), hatalı (Error) veya geçerli (Valid) kayıt oranları analiz edildi. Sütunların büyük çoğunluğunda **%100 Valid** (Geçerli) oranına ulaşıldığı ve örneğin `products` tablosunda 26 benzersiz ürünün hatasız aktarıldığı doğrulandı.
*   **Adımların Kaydedilmesi (Applied Steps):** Power Query'de yapılan her işlemin (örn: *Promoted Headers*, *Changed Type*) sağ taraftaki "Applied Steps" bölmesinde kronolojik olarak kaydedildiği, bu sayede işlemlerin esnek bir şekilde geri alınabildiği veya düzenlenebildiği görüldü.

### 3. Veri Modelleme (Data Modeling - Relationships)
Power Query'deki işlemler tamamlandıktan sonra (`Close & Apply`), sol taraftaki **Model Görünümü (Model View)** ekranına geçerek bağımsız tablolar arasında ilişkiler kurduk.
*   `Order_Log`, `orders`, `products`, `country` ve `Countries` tabloları model çalışma alanına yerleştirildi.
*   İlişkisel veritabanı (RDBMS) mantığında olduğu gibi, tablolar arasındaki ortak sütunlar (Örn: `orders` tablosundaki kategori veya ID bilgileri ile `products` tablosu) sürükle-bırak yöntemiyle birleştirilerek **İlişkiler (Relationships)** oluşturuldu.
*   Kurulan bu ilişkisel ağ (Star Schema mantığı) sayesinde, ilerleyen aşamalarda farklı tablolardaki verileri tek bir görsel (Visual) veya DAX (Data Analysis Expressions) formülü içinde sorunsuzca birleştirip analiz edebileceğiz.

  ##  12. Gün: Power BI İleri Veri Modelleme (Star & Snowflake), Veri Birleştirme ve Görselleştirme (27 Ekim 2026)

Bugün Power BI üzerinde veri modelleme konseptlerini derinleştirdik. Çeşitli açık veriler (zaman serileri, üretim istatistikleri ve anket verileri) üzerinden daha karmaşık modelleme yapılarını ve şema tasarımlarını inceledik, ardından elde ettiğimiz modeli görselleştirdik.

### 1. Veri Modelleme Kavramları ve Şema Türleri
Raporların performanslı ve doğru çalışabilmesi için tablolar arasındaki ilişkinin doğru tasarlanması gerekir. Analiz çalışmalarımızda iki ana şema yapısını teorik ve pratik olarak ele aldık:

*   **Star Schema (Yıldız Şeması):** Merkezde bir Fact (Gerçek) tablosunun yer aldığı ve ona doğrudan bağlı Dimension (Boyut) tablolarından oluşan, yıldıza benzeyen en ideal ve performanslı modelleme türüdür.
*   **Snowflake Schema (Kar Tanesi Şeması):** Boyut tablolarının kendi içlerinde de başka alt boyut tablolarına bağlandığı (normalize edildiği) yapıdır. `79-81` gibi ana tablolara bağlanan alt boyut tablolarını (Örn: `age code`) Model Görünümünde (Model View) kurgulayarak bu şema yapısını pratik ettik. Veri tekrarını azaltır ancak tablo ilişkileri (join) uzadığı için sorgu performansını yavaşlatabilir.

### 2. Fact (Gerçek) ve Dimension (Boyut) Tabloları
Model yapısını kurarken verileri işlevlerine göre iki ana türe ayırdık:
*   **Fact Tabloları:** İşletmenin ölçülebilir olaylarını barındıran tablolardır. Genellikle çok sayıda satır içerir ve sayısal verilerden, metriklerden oluşur (Örn: Çalışan Sayıları, Sipariş Miktarları).
*   **Dimension Tabloları:** Fact tablosunu "Kim, Ne, Ne Zaman, Nerede?" sorularıyla filtrelememizi ve analiz etmemizi sağlayan açıklayıcı (metinsel ağırlıklı) tablolardır (Örn: `Meaning of Establishment age code`, `manufacturing_data`).

### 3. Power Query ile İleri Düzey Birleştirme İşlemleri
Dağınık dosyaları tek bir modele oturtmak için Power Query Editor'ın (Queries menüsü altındaki `ana [2]` ve `Other Queries [4]` klasörleriyle) iki önemli birleştirme fonksiyonunu kullandık:
*   **Sorguları Ekleme (Append Queries):** Yıllara göre ayrı ayrı tutulan `Timeseries_1979`, `Timeseries_1980`, `Timeseries_1981` gibi verileri alt alta ekleyerek (SQL'deki `UNION ALL` mantığı) tek ve bütün bir zaman serisi tablosu (`1979_1980`) haline getirdik.
*   **Sorguları Birleştirme (Merge Queries):** Ortak bir anahtar sütunu bulunan tabloları SQL'deki `LEFT JOIN` mantığına benzer şekilde yan yana birleştirerek modeli sadeleştirdik.

### 4. Raporlama ve Görselleştirme (Data Visualization)
Kurduğumuz modeli Rapor Görünümünde (Report View) test ettik:
*   **Kart (Card) Görseli:** Bir şirketin veya verisetinin anahtar performans göstergesini (KPI) tek bir büyük sayı ile vurgulamak için kullanılır. Toplam çalışan sayısını 187M (187 Milyon) olarak bu görselde yansıttık.
*   **Yatay Çubuk Grafik (Clustered Bar Chart):** Kurum yaşı kodlarına (`Meaning of Establishment age code`) göre çalışan sayılarını (`Sum of Number of employees`) dağıtarak kategorik analizler gerçekleştirdik.
*   **Harita (Map) Görseli:** Konumsal verileri (`Geographic Area Name`) kullanarak Amerika Birleşik Devletleri haritası üzerinde veri yoğunluğunu görselleştirdik. Harita görsellerinde konumsal (location) sütunlarının Power Query'de uygun veri tipinde (örn: State/Province) işaretlenmesi büyük önem taşır.

### 5. İlişki Kuralları ve Kardinalite (Cardinality)
Model görünümünde tabloları birbirine bağlarken ilişki türlerine ve veri bütünlüğüne dikkat ettik:
*   **Bire-Çok İlişki (1:*):** Dimension (1) tablosundan Fact (*) tablosuna doğru kurulan en yaygın ilişki türüdür. Model ekranında bu ilişki `1` ve `*` simgeleri ile görülebilir.
*   Filtreleme akışının (Cross filter direction) her zaman Dimension tablosundan Fact tablosuna doğru (tek yönlü oklu çizgi) ilerlemesini sağladık. Hatalı hesaplamaların önüne geçmek için çift yönlü (Both) filtrelemeden zorunlu kalmadıkça kaçındık.

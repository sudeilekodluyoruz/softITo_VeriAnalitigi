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

  ## 13. Gün: Power BI'da Veri Modelleme: Yıldız (Star) ve Kar Tanesi (Snowflake) Şemaları (28 Ekim 2026)

Bugün Power BI üzerinde veri modeli optimizasyonları ve şema tasarımları üzerine çalıştık. Geniş (wide) formatta gelen ham verileri daha performanslı olan *Olgu (Fact)* ve *Boyut (Dimension)* tablolarına ayırarak **Yıldız Şema (Star Schema)** ve **Kar Tanesi Şeması (Snowflake Schema)** yapılarını adım adım kurguladık.

### 1. Veriyi Olgulara ve Boyutlara Ayırma (Yıldız Şema)
Geniş veri setlerini (`Establishment Survey.csv`) doğrudan kullanmak yerine, verileri ölçülebilir olgular (Fact) ve açıklayıcı boyutlar (Dimension) olarak ikiye ayırdık. Bu işlem Power BI'da sorgu performansını büyük ölçüde artırır.
* **Boyut Tablosu Oluşturma:** `Establishment Survey` sorgusunu Power Query'de çoğaltıp (Duplicate) `Industry` (Sektör) adını verdik. Yalnızca sektörel hiyerarşi sütunlarını (`NAICS code`, `Sector`, `Subsector` vb.) bırakıp **yinelenen değerleri (duplicates) temizledik**.
* **Olgu Tablosunu Sadeleştirme:** Ana olgu (Fact) tablomuz olan `Establishment Survey` içindeki açıklayıcı metin sütunlarını sildik. İki tabloyu birbirine bağlamak için sadece **`NAICS code`** sütununu birincil anahtar (Primary Key) olarak koruduk.
* **Görselleştirme:** Elde ettiğimiz bu yapıdan Model görünümünde ilişki kurduk. Ardından Rapor görünümünde `Subsector`'ü eksen (Axis), `Average Number of employees`'i değer (Values) olarak atayarak Küme Çubuk Grafiği (Clustered Bar Chart) oluşturduk.

### 2. Modele Dışarıdan Yeni Boyutlar (Dimensions) Ekleme
Sadece olgu tablosundan türettiğimiz boyutlarla kalmayıp, modele dışarıdan açıklayıcı veri setleri dahil ettik:
* **Zaman Boyutu (Time Dimension):** `Time.txt` dosyasını içe aktarıp `Year` (Yıl) sütunu üzerinden olgu tablosuna bağladık. Bu, verileri yalnızca yıla göre değil, on yıllık (Decade) veya yüzyıllık periyotlarla da analiz edip dilimleyici (Slicer) eklememizi sağladı.
* **Yaş Boyutu (Age Dimension):** Verilerdeki anlamsız "Age code" sayılarını açıklamak için `EstablishmentAge.csv` dosyasını `Age` boyutu olarak modele dahil ettik. `Age code` üzerinden ilişki kurarak analizleri anlamlı metinlerle (`Establishment Age`) filtreleyebilir hale geldik.

### 3. Boyut Hiyerarşileri
Boyut (Dimension) tabloları genellikle kendi içlerinde bir mantıksal hiyerarşi barındırır.
* `Industry` boyut tablomuzu incelediğimizde sektörlerin genelden özele doğru bir kırılımı (hiyerarşisi) olduğunu gördük: **`Sector > Subsector > Industry group > NAICS Code description`**. 
* Bu hiyerarşileri görselleştirmelerde sırasıyla kullanarak "Matris (Matrix)" veya "Tablo" görsellerinde detaya inme (Drill-down) yeteneği kazanıyoruz.

### 4. Kar Tanesi Şeması (Snowflake Schema) Tasarımı
Power BI temelde Yıldız (Star) şema için optimize edilmiş olsa da, boyutların diğer boyutlara bağlandığı hiyerarşik durumlarda tabloları kendi içlerinde bölerek (normalize ederek) **Kar Tanesi (Snowflake)** şemasına geçiş yaptık:
* **Hiyerarşiyi Parçalama:** `Industry` boyutunu Power Query'de 4 farklı sorguya çoğalttık ve her birini hiyerarşideki seviyelere göre adlandırdık: `Sector`, `Subsector`, `Industry group` ve `NAICS code`.
* **İlişkileri Zincirleme Bağlama:** Her alt boyut tablosunu bir üst hiyerarşinin kodu ile birbirine (zincirleme şeklinde) bağladık. En uçtaki (en detaylı) `NAICS code` boyutunu da `Establishment Survey` olgu tablosuna bağladık.
* **İlişkiyi Aktif Etme:** Eğer bağlantılarda noktalı (kesik) çizgiler oluşursa (Inactive Relationship), ilişkinin *Properties* (Özellikler) menüsünden **"Make this relationship active"** (Bu ilişkiyi etkinleştir) seçeneğiyle ilişkileri aktif hale getirmeyi deneyimledik.

##  14. Gün: Veri Bilimi ve Makine Öğrenmesine (Machine Learning) Giriş (30 Ekim 2026)

Bugün Veri Analitiği ve İş Zekası süreçlerinden bir adım öteye geçerek, **Veri Biliminin (Data Science)** kalbi olan **Makine Öğrenmesi (Machine Learning - ML)** konularına teorik bir giriş yaptık.

### 1. Veri Bilimi (Data Science) Nedir?
Veri Bilimi tek bir disiplin değil, üç temel yetkinliğin kesişim kümesidir:
*   **Bilgisayar Bilimleri (Coding / Computer Science):** Veriyi işleme, temizleme ve algoritmaları kodlama (Python, SQL vb.).
*   **Matematik ve İstatistik (Stats / Calculus / Linear Algebra):** Modellerin arkasındaki olasılık teorileri, hata hesaplamaları ve doğrusal cebir.
*   **Etki Alanı Uzmanlığı (Domain Knowledge):** Verinin ait olduğu sektörü (finans, sağlık, perakende vb.) anlamak.
*   Makine Öğrenmesi, Kodlama ve İstatistik alanlarının kesiştiği yerde bulunur. Eğer sadece kodlama ve alan bilgisi varsa bu "Tehlike Bölgesi (Danger Zone)" olarak adlandırılır çünkü istatistiksel doğrulama olmadan yapılan kodlamalar yanıltıcı sonuçlar verebilir.

### 2. Geleneksel Programlama vs. Makine Öğrenmesi
İki yaklaşım arasındaki temel felsefe farkını inceledik:
*   **Geleneksel Programlama (Traditional Programming):** İnsanın yazdığı **Kurallar (Program/Mantık)** ve **Veri (Data)** bilgisayara verilir, bilgisayar bu kuralları işleterek bir **Çıktı (Output)** üretir.
*   **Makine Öğrenmesi (Machine Learning):** Bilgisayara **Veri (Data)** ve olması gereken **Çıktılar (Observed Output)** verilir. Bilgisayar eğitim (training) aşamasında bu ikisi arasındaki ilişkiyi çözerek kendi **Kurallarını (Program/Model)** üretir. Bu üretilen model daha sonra test edilir ve yeni verilerle tahminler yapmak için kullanılır.

### 3. Yapay Zeka (AI) Çemberi
Bu kavramların birbiriyle olan hiyerarşik ilişkisini netleştirdik:
1.  **Yapay Zeka (Artificial Intelligence - AI):** En dış ve en geniş halkadır. Makinelerin insan zekasını taklit etme kapasitesini kapsayan en genel kavramdır.
2.  **Makine Öğrenmesi (Machine Learning - ML):** Yapay zekanın bir alt dalıdır. Sistemlerin açıkça programlanmadan, veriden öğrenerek (istatistiksel yöntemlerle) performansını artırmasıdır.
3.  **Derin Öğrenme (Deep Learning):** Makine öğrenmesinin bir alt dalıdır. Çok katmanlı yapay sinir ağları (Neural Networks) kullanarak, insan beynine benzer şekilde (özellikle NLP, görüntü işleme gibi karmaşık işlerde) öğrenme sürecidir.

### 4. Makine Öğrenmesinin Ana Kategorileri
Makine öğrenmesi algoritmaları verinin yapısına ve ulaşılmak istenen hedefe göre iki ana dala ayrılır:

**A. Gözetimli Öğrenme (Supervised Learning)**
Elimizde etiketli (sonucu bilinen) veriler varsa kullanılır. İkiye ayrılır:
*   **Sınıflandırma (Classification):** Çıktı kategorik bir değişkense (Örn: E-posta Spam mi değil mi?). Kullanılan algoritmalar: *Support Vector Machines (SVM), Naive Bayes, Nearest Neighbor, Discriminant Analysis*.
*   **Regresyon (Regression):** Çıktı sürekli (sayısal) bir değişkense (Örn: Evin fiyatı ne kadar olacak?). Kullanılan algoritmalar: *Linear Regression (GLM), SVR, Decision Trees, Ensemble Methods (Random Forest vb.), Neural Networks*.

**B. Gözetimsiz Öğrenme (Unsupervised Learning)**
Elimizde etiketli (sonucu bilinen) bir veri *yoksa*, verinin kendi içindeki gizli yapıları veya örüntüleri (patterns) bulmak için kullanılır.
*   **Kümeleme (Clustering):** Verileri benzerliklerine göre gruplara ayırır (Örn: Müşteri segmentasyonu). Kullanılan algoritmalar: *K-Means, K-Medoids, Fuzzy C-Means, Hierarchical Clustering, Gaussian Mixture, Hidden Markov Model*.
*   *(Not: Sinir Ağları (Neural Networks) hem regresyon hem de kümeleme/sınıflandırma gibi farklı alanlarda kullanılabilen esnek bir yapıya sahiptir).*

### 5. Makine Öğrenmesi İş Akışı
Temel bir makine öğrenmesi modeli kurma süreci şu şekilde akar:
1.  **Data (Veri):** Geçmiş veriler toplanır.
2.  **Algorithm (Algoritma):** Veri uygun bir makine öğrenmesi algoritmasına (Örn: Lineer Regresyon) beslenir.
3.  **Model:** Algoritma veriden öğrenerek bir matematiksel "Model" inşa eder.
4.  **New Data (Yeni Veri):** Daha önce hiç görülmemiş yeni veriler bu eğitilmiş modele sokulur.
5.  **Prediction (Tahmin):** Model, yeni veriler için sonuçları (tahminleri) üretir.

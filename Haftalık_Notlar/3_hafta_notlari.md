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

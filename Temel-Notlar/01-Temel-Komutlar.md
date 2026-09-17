# SQL İsimlendirme Standartları ve Köşeli Parantez `[]` Kullanımı

Veritabanı nesnelerini (tablo, sütun, vb.) tanımlarken belirli standartlara uymak ve özel durumlarda sınırlandırıcı (delimiter) kullanmak, hatasız ve sürdürülebilir kod yazımı için kritiktir.

---

## 1. Veri ve Nesne İsimlendirme Kuralları (Naming Conventions)

SQL sorgularında ve veritabanı tasarımlarında karışıklığı ve veritabanı harf seti (collation) hatalarını önlemek için şu kurallara uyulmalıdır:

* **İngilizce Karakter Kullanımı:** Türkçe karakterler (`ı, ş, ğ, ü, ö, ç` veya `I, Ğ, Ü, Ö, Ç`) veritabanı sürücülerinde karakter kodlama hatalarına yol açabileceğinden kullanılmamalıdır.
  * *Hatalı:* `müşteri_adı`
  * *Doğru:* `musteri_adi`
* **Boşluk Bırakmama:** Kelimeler arasında boşlık bırakılmamalıdır. Boşluk yerine yaygın iki standarttan biri tercih edilmelidir:
  * **Snake Case:** `musteri_id`, `kayit_tarihi`
  * **Pascal / Camel Case:** `MusteriID`, `KayitTarihi`
* **Sayı ve Özel Karakter Sınırı:** İsimler sayı ile başlamamalıdır ve alt çizgi (`_`) haricinde özel karakterler (`!`, `@`, `#`, `$`, `%`, `-`) içermemelidir.
  * *Hatalı:* `1.musteri`, `musteri-adi`
  * *Doğru:* `musteri1`, `musteri_adi`
* **Ayrılmış Kelimeleri (Reserved Words) Kullanmama:** `SELECT`, `WHERE`, `TABLE`, `ORDER`, `GROUP` gibi SQL komut isimleri tablo veya sütun adı olarak seçilmemelidir.

---

## 2. Köşeli Parantez `[]` (Delimiter) Kullanımı

**MS SQL Server (T-SQL)** ortamında köşeli parantez `[]`, veritabanı motoruna ilgili ifadenin birleşik ve tek bir nesne ismi olduğunu bildirmek için kullanılır. 

*(Not: MySQL ortamında aynı işlem için ters tırnak `` ` ``, PostgreSQL ve Oracle ortamında ise çift tırnak `""` tercih edilir).*

### Kullanım Senaryoları :

#### A. İsimde Boşluk Bulunması
Tasarımsal bir hata sonucu sütun veya tablo isminde boşluk bırakılmışsa, sorgunun hata vermemesi için `[]` kullanımı zorunludur.

```sql
-- Hatalı (SQL Server 'Adı' kısmını farklı bir komut/takma ad sanır ve syntax hatası verir):
SELECT Müşteri Adı FROM musteriler;

-- Doğru Kullanım:
SELECT [Müşteri Adı] FROM musteriler;
```

#### B. SQL Komut İsimlerinin (Reserved Words) Nesne Adı Yapılması
Sütun veya tablo ismi olarak SQL'in kendi komut kelimelerinden biri verildiğinde çakışmayı önlemek için kullanılır.

```sql
-- Hatalı (Order kelimesi ORDER BY komutu ile çakışır):
SELECT Order FROM siparisler;

-- Doğru Kullanım:
SELECT [Order] FROM siparisler;
```

#### C. İsimde Özel/Türkçe Karakter Bulunması
Veritabanı harf seti uyuşmazlıklarında ayrıştırma sorunlarının önüne geçmek için tercih edilebilir.

```sql
SELECT [İletişim Numarası] FROM [Müşteri Detay];
```

> **NOT :**
> Doğru tasarlanmış bir veritabanında köşeli parantez `[]` kullanma ihtiyacı doğmamalıdır. Tablo ve sütun isimleri İngilizce, küçük harflerle ve boşluksuz (`customer_id`, `first_name`) oluşturulduğunda temiz ve ek sembol gerektirmeyen SQL kodları yazılır.


---

# USE Komutu (Veritabanı Seçimi)

`USE` komutu, veritabanı sunucusu üzerinde bulunan birden fazla veritabanı arasından **hangisinde işlem yapacağını (aktif veritabanını)** belirlemek için kullanılır.

### Sözdizimi

```sql
USE veritabani_adi;
```

---

# DDL Komutları (CREATE, ALTER, DROP)

**DDL** (*Data Definition Language* - Veri Tanımlama Dili), veritabanının yapısını, tablolarını ve diğer veritabanı nesnelerini (index, view, schema vb.) oluşturmak, değiştirmek veya silmek için kullanılan SQL komutları kümesidir.

---

## 1. CREATE Komutu

Yeni bir veritabanı, tablo veya veritabanı nesnesi **oluşturmak** için kullanılır.

### Veritabanı Oluşturma Sözdizimi 
```sql
CREATE DATABASE e_ticaret_db;
);
```

### Tablo Oluşturma Sözdizimi 
```sql
CREATE TABLE tablo_adi (
    sutun1_adi veri_tipi kisitlamalar,
    sutun2_adi veri_tipi kisitlamalar,
    ...
);
```

#### Örnek Kullanım :
`Müşteriler` tablosu oluşturma 
```sql
CREATE TABLE musteriler (
    musteri_id INT PRIMARY KEY,
    ad VARCHAR(50) NOT NULL,
    soyad VARCHAR(50) NOT NULL,
    eposta VARCHAR(100) UNIQUE,
    kayit_tarihi DATE
);
```

### İndeks Oluşturma Sözdizimi 
```sql
CREATE INDEX idx_musteri_ad 
ON musteriler(ad);
);
```

> **İPUCU :**
> Sık sorgulanan veya `WHERE` koşulunda sıkça kullanılan sütunlara indeks eklemek sorgu süresini ciddi oranda kısaltır.

---

## 2. ALTER Komutu

**ALTER** komutu; daha önce oluşturulmuş olan **Veritabanı (Database)**, **Tablo (Table)** veya **İndeks (Index)** gibi veritabanı nesnelerinin yapısını **güncellemek** ve **değiştirmek** için kullanılır.

### Veritabanı Yapısını Değiştirme 
```sql
ALTER DATABASE e_ticaret_db RENAME TO e_ticaret_v2;
```

### Tabloya Yeni Sütun Ekleme 
```sql
ALTER TABLE musteriler 
ADD telefon VARCHAR(15);
```

### Sütunun Veri Tipini Değiştirme 
```sql
-- PostgreSQL / MS SQL
ALTER TABLE musteriler 
ALTER COLUMN soyad VARCHAR(100);

-- MySQL
ALTER TABLE musteriler 
MODIFY COLUMN soyad VARCHAR(100);
```

### Tablodan Sütun Silme 
```sql
ALTER TABLE musteriler 
DROP COLUMN kayit_tarihi;
```

### Sütun Adını Değiştirme 
```sql
ALTER TABLE musteriler 
RENAME COLUMN soyad TO musteri_soyadi;
```

### İndeks Yapısını Değiştirme
```sql
ALTER INDEX idx_musteri_ad REBUILD;
```

---

## 3. DROP Komutu

Bir veritabanını, tabloyu veya diğer veritabanı nesnelerini **tamamen ve geri dönülemez şekilde silmek** için kullanılır.

> **NOT :**
> `DROP` komutu tablonun sadece içindeki verileri değil, tablonun yapısını da tamamen veritabanından kaldırır.

### Veritabanı Silme
```sql
DROP DATABASE e_ticaret_v2;
```

### Tablo Silme
```sql
DROP TABLE musteriler;
```

> **İPUCU :**
> Silmeye çalıştığınız tablo başka bir veritabanı nesnesinde kullanılıyorsa hata almamak için varlık kontrolü `(IF EXISTS)` eklenebilir :
> ```sql
> DROP TABLE IF EXISTS musteriler;
> ```

### Tablodan Sütun Silme
```sql
ALTER TABLE musteriler 
DROP COLUMN kayit_tarihi;
```

### İndeks Silme
```sql
-- PostgreSQL / MS SQL
DROP INDEX idx_musteri_ad ON musteriler;

-- MySQL
DROP INDEX idx_musteri_ad ON musteriler;
```

---

## 4. TRUNCATE Komutu

Bir tablonun yapısını, sütunlarını ve kısıtlamalarını koruyarak **içindeki tüm verileri tek hamlede ve kalıcı olarak silmek** için kullanılan DDL komutudur.

### Sözdizimi
```sql
TRUNCATE TABLE musteriler;
```

### TRUNCATE Komutunun Önemli Özellikleri

* **Yapıyı Korur:** Tablonun kendisi (`CREATE TABLE` ile tanımlanan sütunlar, veri tipleri vb.) kalır, sadece içindeki tüm satırlar temizlenir.
* **Sayaçları Sıfırlar:** Tabloda otomatik artan bir kimlik sütunu (`AUTO_INCREMENT` / `IDENTITY`) varsa, sayacı varsayılan ilk değerine (ör. 1) sıfırlar.
* **Hızlıdır:** `DELETE` komutu gibi verileri satır satır silmek yerine tablonun veri sayfalarını doğrudan boşalttığı için çok daha hızlı çalışır.
* **Koşul Almaz:** `WHERE` yan cümleciği kullanılamaz; yani sadece belirli satırları silmek için kullanılamaz, her zaman tablonun tamamını temizler.

---

## 💡 TRUNCATE vs. DROP Karşılaştırması

| Özellik | TRUNCATE | DROP |
| :--- | :--- | :--- |
| **Kategori** | DDL | DDL |
| **Sildiği Yapı** | Tüm Satırlar | Tablo + Veriler (Her Şey) |
| **Tablo Yapısı Kalır mı?** | Evet | Hayır (Tablo tamamen silinir) |
| **`WHERE` Kullanılabilir mi?** | Hayır | Hayır |
| **Çalışma Hızı** | Çok Hızlı | Çok Hızlı |
| **ID Sayacını Sıfırlar mı?** | Evet | N/A (Tablo yok olur) |

---

# DQL (Data Query Language - Veri Sorgulama Dili)

**DQL**, veritabanındaki tabloların yapısını veya içeriğini değiştirmeden, mevcut verileri **sorgulamak**, **filtrelemek**, **gruplamak** ve **raporlamak** için kullanılan SQL komut grubudur.

## DQL Yapı Taşları ve Yan Cümlecikleri

DQL'in ana komutu **`SELECT`**'tir. Bu komut, aşağıdaki yan cümlecikler (*clauses*) ve yapılarla birlikte kullanılarak karmaşık sorgular oluşturulur:

> **NOT :**
> Burada açıklamalar üzerinden anlamanız zor olacaktır. Lütfen örnekle beraber aynı anda inceleyiniz.

* **`SELECT`:** Listelenmek istenen sütunları belirler.
* **`FROM`:** Verinin çekileceği tablo veya tabloları belirtir.
* **`AS` (Alias):** Sütunlara veya tablolara geçici takma isimler vererek çıktı okunabilirliğini artırır.
* **`DISTINCT`:** Sorgu sonucundaki tekrar eden (yinelenen) kayıtları temizleyerek sadece benzersiz verileri getirir.
* **`WHERE`:** Belirli şartlara/koşullara göre satırları filtreler.
* **`GROUP BY`:** Verileri belirli sütunlara göre gruplar (Aggregate fonksiyonlarla kullanılır) (Başka bölümde detaylı anlatılacak).
* **`HAVING`:** Gruplanmış veriler üzerinde filtreleme yapar.
* **`ORDER BY`:** Sorgu sonucunu artan (`ASC`) veya azalan (`DESC`) sırada sıralar.
* **`LIMIT` / `TOP` / `FETCH`:** Dönen sonuç kümesinden kaç satır getirileceğini kısıtlar. 
* **`JOIN` Yapıları:** Birden fazla tabloyu birleştirerek tek bir sorguda veri çekmeyi sağlar (`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`) (Başka bölümde anlatılacak).

## `LIMIT` / `TOP` / `FETCH` farkı
| Komut | Destekleyen Veritabanları | Örnek Sözdizimi |
| :--- | :--- | :--- |
| **`LIMIT`** | PostgreSQL, MySQL, SQLite | `SELECT * FROM musteriler LIMIT 10;` |
| **`TOP`** | MS SQL Server (T-SQL) | `SELECT TOP 10 * FROM musteriler;` |
| **`FETCH`** | Oracle, PostgreSQL, MS SQL Server (ANSI SQL Standardı) | `SELECT * FROM musteriler FETCH FIRST 10 ROWS ONLY;` |

---

## SELECT

**SELECT** komutu; veritabanından veri okumak ve istenen formatta listelemek için kullanılan temel sorgulama komutudur.

### 🔍 SELECT Kullanım Senaryoları ve Örnekler

### Tablodaki Tüm Sütunları Çekme (`*`)
```sql
SELECT * 
FROM musteriler;
```

### Belirli Sütunları Seçme
```sql
SELECT ad, soyad, eposta 
FROM musteriler;
```

### Sütunlara Takma Ad Verme
```sql
SELECT 
    ad AS musteri_adi, 
    eposta AS iletisim_adresi 
FROM musteriler;
```

### Tekrarlayan Verileri Tekilleştirme
```sql
SELECT DISTINCT sehir 
FROM musteriler;
```

### Hesaplama ve Metinsel İşlemler
```sql
SELECT 
    urun_adi, 
    fiyat, 
    (fiyat * 1.20) AS kdvli_fiyat 
FROM urunler;
```

### Koşula Göre Satırları Filtreleme
```sql
SELECT * 
FROM musteriler 
WHERE sehir = 'İstanbul';
```

### Verileri Belirli Sütunlara Göre Gruplama
```sql
SELECT sehir 
FROM musteriler 
GROUP BY sehir;
```

### Gruplanmış Veriler Üzerinde Filtreleme Yapma
```sql
SELECT sehir 
FROM musteriler 
GROUP BY sehir 
HAVING sehir = 'İstanbul';
/*
where ile having farkı :
where = gruplamadan önce filtreleme
having = gruplamadan sonra filtreleme
*/
```

### Sorgu Sonucunu Sıralama
```sql
SELECT ad, soyad, kayit_tarihi 
FROM musteriler 
ORDER BY kayit_tarihi DESC;

SELECT ad, soyad, kayit_tarihi 
FROM musteriler 
ORDER BY kayit_tarihi ASC;
```

### Getirilecek Satır Sayısını Kısıtlama
```sql
SELECT * 
FROM musteriler 
LIMIT 10;

SELECT TOP 5 ad, soyad 
FROM musteriler;

SELECT * 
FROM musteriler 
FETCH FIRST 5 ROWS ONLY;
```


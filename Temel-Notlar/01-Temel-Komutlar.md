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

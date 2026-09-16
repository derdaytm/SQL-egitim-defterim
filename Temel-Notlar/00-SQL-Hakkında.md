# SQL Nedir?

**SQL** (*Structured Query Language* - Yapılandırılmış Sorgu Dili):

- **Veritabanı Dilidir:** İlişkisel veritabanı yönetim sistemlerinde (RDBMS) verileri yönetmek, sorgulamak ve düzenlemekiçin kullanılır.
- **Global Standarttır:** Bütün yapısal veritabanlarının ortak global sorgu dilidir.
- **Kritik Yetenektir:** Veri analizinde öğrenilmesi gereken ilk ve en önemli yetenektir.

## SQL Ne İşe Yarar?

SQL kullanarak veritabanları üzerinde şu temel işlemleri gerçekleştirebiliriz:

* **Veri Çekme (Querying):** Veritabanından istenen kriterlere uygun verileri listeleme.
* **Veri Ekleme (Insert):** Veritabanına yeni kayıtlar ekleme.
* **Veri Güncelleme (Update):** Mevcut verileri değiştirme veya güncelleme.
* **Veri Silme (Delete):** Gereksiz veya eski verileri veritabanından kaldırma.
* **Veritabanı Yapısı Oluşturma (DDL):** Yeni tablolar, veritabanları veya dizinler (index) tanımlama.

## SQL Bir Programlama Dili Midir?

SQL; C#, Java veya Python gibi genel amaçlı bir **programlama dili değildir**. SQL bir **sorgu ve veri bildirim dilidir** (*Declarative Language*).

---

# Veritabanı (Database) Nedir?

**Veritabanı**, bilgilerin düzenli, güvenli ve kolay erişilebilir bir şekilde dijital ortamda saklandığı sistemdir.

## Veritabanı Nelerden Oluşur?

İlişkisel bir veritabanı temel olarak şu yapı taşlarından meydana gelir:

* **Tablo (Table):** Verilerin mantıksal gruplar halinde saklandığı ana yapılardır (ör. `Kullanıcılar`, `Siparişler`).
* **Sütun / Alan (Column / Field):** Tablodaki verilerin türünü ve niteliğini belirten dikey yapılardır (ör. `Ad`, `Soyad`, `E-posta`).
* **Satır / Kayıt (Row / Record):** Tablodaki her bir tekil veri girdisidir (ör. Ahmet Yılmaz'a ait müşteri bilgisi).
* **Hücre (Cell):** Bir satır ile bir sütunun kesiştiği noktadaki tekil veri değeridir.
* **Anahtarlar (Keys):** 
  * **Primary Key (Birincil Anahtar):** Her satırı benzersiz (unique) kılan kimlik sütunudur (ör. `kullanici_id`).
  * **Foreign Key (Yabancı Anahtar):** Tabloları birbirine bağlayan ve ilişki kuran sütunlardır.
* **Şema (Schema):** Veritabanının tüm tablo, sütun ve ilişki yapısını tanımlayan mimari taslaktır.

## Veritabanı Sunucusu (Database Server) Nedir?

**Veritabanı Sunucusu**, veritabanı yönetim yazılımlarını (RDBMS) üzerinde barındıran, verileri saklama, işleme, güvenliğini sağlama ve istemcilerden (client) gelen sorguları yanıtlayarak veri iletimi yapma görevini üstlenen güçlü bir donanım/yazılım sistemidir.

## Nasıl Çalışır? (İstemci - Sunucu Mimarisi)

Veritabanı sunucuları genellikle **Client-Server (İstemci-Sunucu)** mimarisiyle çalışır:

1. **İstemci (Client):** Kullanıcı, mobil uygulama veya web sitesidir. Sunucuya bir veri talebi (SQL sorgusu) gönderir.
2. **Sunucu (Database Server):** Gelen SQL sorgusunu işler, gerekli veriyi bulur, güvenlik/yetki kontrollerini yapar ve sonucu istemciye geri yanıt (response) olarak döner.

---

# İlişkisel Veritabanı (RDBMS) Nedir?

**İlişkisel Veritabanı** (*Relational Database*), verileri **satır** (row) ve **sütunlardan** (column) oluşan mantıksal **tablolarda** saklayan ve bu tabloları birbirleriyle belirli kurallar (ilişkiler) çerçevesinde bağlayan veritabanı mimarisidir.

Bu yapıları yönetmek ve işletmek için kullanılan yazılımlara ise **RDBMS** (*Relational Database Management System* / İlişkisel Veritabanı Yönetim Sistemi) denir.

## İlişkisel Veritabanının Avantajları

* **Veri Tekrarını Önler (Normalizasyon):** Aynı veriyi birden fazla yerde tutmak yerine tabloları birbirine bağlayarak depolama tasarrufu ve düzen sağlar.
* **Veri Bütünlüğü (Data Integrity):** Yabancı anahtarlar (*Foreign Key*) sayesinde silinen veya güncellenen verilerin ilişkili diğer tablolarda tutarsızlık yaratması engellenir.
* **ACID Kuralları:** İşlemlerin (*Transaction*) güvenli, eksiksiz ve hatasız bir şekilde tamamlanmasını garanti eder.
* **Esnek Sorgulama:** **SQL** dili kullanılarak birden fazla tablodaki veriler tek bir sorguda birleştirilerek (`JOIN`) kolayca çekilebilir.

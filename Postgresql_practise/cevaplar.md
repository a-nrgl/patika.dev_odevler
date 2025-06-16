## Aşağıdaki sorgu senaryoları, Ödev 8 hariç PostgreSQL üzerinde `dvdrental` örnek veri tabanı kullanılarak hazırlanmıştır.

---

## İçindekiler

- [Ödev 1](#ödev-1)
- [Ödev 2](#ödev-2)
- [Ödev 3](#ödev-3)
- [Ödev 4](#ödev-4)
- [Ödev 5](#ödev-5)
- [Ödev 6](#ödev-6)
- [Ödev 7](#ödev-7)
- [Ödev 8](#ödev-8)
- [Ödev 9](#ödev-9)
- [Ödev 10](#ödev-10)
- [Ödev 11](#ödev-11)
- [Ödev 12](#ödev-12)


---
## Ödev 1
### 1. Soru  
**Film tablosunda bulunan `title` ve `description` sütunlarındaki verileri sıralayınız.**

```sql
SELECT title, description FROM film;
```

---

### 2. Soru  
**Film tablosunda bulunan tüm sütunlardaki verileri film uzunluğu (`length`) 60'dan büyük VE 75'ten küçük olma koşullarıyla sıralayınız.**

```sql
SELECT * FROM film
WHERE length > 60 AND length < 75;
```

---

### 3. Soru  
**Film tablosunda bulunan tüm sütunlardaki verileri `rental_rate` 0.99 VE `replacement_cost` 12.99 VEYA 28.99 olma koşullarıyla sıralayınız.**

```sql
SELECT * FROM film
WHERE rental_rate = 0.99 AND (replacement_cost = 12.99 OR replacement_cost = 28.99);
```

---

### 4. Soru  
**Customer tablosunda bulunan `first_name` sütunundaki değeri 'Mary' olan müşterinin `last_name` sütunundaki değeri nedir?**

```sql
SELECT last_name FROM customer
WHERE first_name = 'Mary';
```

---

### 5. Soru  
**Film tablosundaki uzunluğu (`length`) 50'den büyük OLMAYIP aynı zamanda `rental_rate` değeri 2.99 veya 4.99 OLMAYAN verileri sıralayınız.**

```sql
SELECT * FROM film
WHERE NOT (length > 50) AND rental_rate NOT IN (2.99, 4.99);

```

## Ödev 2

---

### 1. Soru  
**Film tablosunda bulunan tüm sütunlardaki verileri `replacement_cost` değeri 12.99’dan büyük eşit VE 16.99’dan küçük olma koşuluyla sıralayınız.  
(`BETWEEN ... AND` yapısını kullanınız.)**

```sql
-- NOT: BETWEEN her iki sınırı da dahil eder.
-- Bu nedenle soruda istenileni karşılamak için üst sınırı 16.99 yerine 16.98 olarak verdim. Böylece 16.99 dahil olmaz.
SELECT * FROM film
WHERE replacement_cost BETWEEN 12.99 AND 16.98;

-- Alternatif olarak operatör kullanarak soruda istenilen sonucu sorgulayabiliriz.

SELECT * FROM film
WHERE replacement_cost >= 12.99 AND replacement_cost < 16.99;
--Bu ifade soruda istenildiği gibi 12.99 dahil, 16.99 dahil değildir.

```


---

### 2. Soru  
**Actor tablosunda bulunan `first_name` ve `last_name` sütunlarındaki verileri,  
`first_name` 'Penelope' veya 'Nick' veya 'Ed' olması koşuluyla sıralayınız.  
(`IN` operatörünü kullanınız.)**

```sql
-- IN operatörü ile çoklu "or" koşullarını sade bir şekilde yazabiliriz.  

SELECT first_name, last_name FROM actor
WHERE first_name IN ('Penelope', 'Nick', 'Ed');

-- Bu sorgu actor tablosundan Sadece first_name sütunu 'Penelope' veya 'Nick' veya 'Ed' olan satırları seçer.
-- Her biri için first_name ve last_name sütunlarını getirir
```

---

### 3. Soru  
**Film tablosunda bulunan tüm sütunlardaki verileri `rental_rate` 0.99, 2.99, 4.99 VE  
`replacement_cost` 12.99, 15.99, 28.99 olma koşullarıyla sıralayınız.  
(`IN` operatörünü kullanınız.)**

```sql
SELECT * FROM film
WHERE rental_rate IN (0.99, 2.99, 4.99)
AND replacement_cost IN (12.99, 15.99, 28.99);
```

## Ödev 3

---

### 1. Soru 
**Country tablosunda bulunan `country` sütunundaki `ülke`, isimlerinden 'A' karakteri ile başlayıp 'a' karakteri ile sonlananları sıralayınız.**

```sql
SELECT country
FROM country
WHERE country LIKE 'A%a';
```
---

### 2. Soru
**Country tablosunda bulunan `country` sütunundaki `ülke` isimlerinden en az 6 karakterden oluşan ve sonu 'n' karakteri ile sonlananları sıralayınız.**

```sql
SELECT country
FROM country
WHERE LENGTH(country) >= 6
AND country LIKE '%n';
```
---
### 3. Soru
**`Film`tablosunda bulunan `title` sütunundaki film isimlerinden en az 4 adet, büyük ya da küçük harf farketmesizin 'T' karakteri içeren film isimlerini sıralayınız.**

```sql
SELECT title
FROM film
WHERE title ILIKE '%t%'
LIMIT 10;
```
---
### 4. Soru
**`Film` tablosunda bulunan tüm sütunlardaki verilerden `title` 'C' karakteri ile başlayan ve uzunluğu (length) 90 dan büyük olan ve rental_rate 2.99 olan verileri sıralayınız.**

```sql
SELECT *
FROM film
WHERE title LIKE 'C%'
  AND length > 90
  AND rental_rate = 2.99;
```
---
## Ödev 4

### 1. Soru
**Film tablosunda bulunan `replacement_cost` sütununda bulunan birbirinden farklı değerleri sıralayınız.**

```sql
SELECT DISTINCT replacement_cost
FROM film
ORDER BY replacement_cost;
--Distinct fonksiyonu replacement_cost sütununda tekrar eden değerleri gizler, aynı olan değerleri bir kez gösterir.
```
---
### 2. Soru
**Film tablosunda bulunan replacement_cost sütununda birbirinden farklı kaç tane veri vardır?**

```sql
SELECT COUNT(DISTINCT replacement_cost)
FROM film;
```
---
### 3. Soru
**Film tablosunda bulunan film isimlerinde (title) kaç tanesini T karakteri ile başlar ve aynı zamanda rating 'G' ye eşittir?**
```sql
SELECT COUNT(*)
FROM film
WHERE title LIKE 'T%'
AND rating = 'G';
```
---
### 4. Soru
**Country tablosunda bulunan ülke isimlerinden (country) kaç tanesi 5 karakterden oluşmaktadır?**
```sql
SELECT COUNT(*) 
FROM country
WHERE LENGTH(country) = 5;
```
---
### 5. Soru
**City tablosundaki şehir isimlerinin kaç tanesi 'R' veya r karakteri ile biter?**
```sql
SELECT COUNT(*)
FROM city
WHERE city ILIKE '%r';
```
---
## Ödev 5

### 1. Soru
**Film tablosunda bulunan ve film ismi (title) 'n' karakteri ile biten en uzun (length) 5 filmi sıralayınız.**
```sql
SELECT title, length
FROM film
WHERE title LIKE '%n'
ORDER BY length DESC
LIMIT 5;
```
---
### 2. Soru
**Film tablosunda bulunan ve film ismi (title) 'n' karakteri ile biten en kısa (length) ikinci(6,7,8,9,10) 5 filmi(6,7,8,9,10) sıralayınız.**
```sql
SELECT title, length
FROM film
WHERE title LIKE '%n'
ORDER BY length ASC
OFFSET 5
LIMIT 5;

-- Not: Bu sorgu, sonu 'n' harfiyle biten film isimleri arasından en kısa olanlara odaklanır.
-- "ORDER BY length ASC" ifadesiyle filmler uzunluklarına göre küçükten büyüğe sıralanır.
-- "OFFSET 5" ilk 5 en kısa filmi atlar, böylece 6-10 arasında sıralanan filmleri getirir.
-- "LIMIT 5" ise bu sıralamadan sadece 5 filmi getirir.
```
---
### 3. Soru
**Customer tablosunda bulunan last_name sütununa göre azalan yapılan sıralamada store_id= 1 olmak koşuluyla ilk 4 veriyi sıralayınız.**
```sql
SELECT *FROM customer
WHERE store_id = 1
ORDER BY last_name DESC
LIMIT 4;
```
---
## Ödev 6

### 1. Soru
**Film tablosunda bulunan rental_rate sütunundaki değerlerin ortalaması nedir?**
```sql
SELECT AVG(rental_rate) AS ortalama
FROM film;
```
---

### 2. Soru
**Film tablosunda bulunan filmlerden kaç tanesi 'C' karakteri ile başlar?**
```sql
SELECT COUNT(*) AS c_ile_baslayan_sayi
FROM film
WHERE title LIKE 'C%';
```
---
### 3. Soru
**Film tablosunda bulunan filmlerden rental_rate değeri 0.99 a eşit olan en uzun (length) film kaç dakikadır?**
```sql
SELECT MAX(length) AS en_uzun_sure
FROM film
WHERE rental_rate = 0.99;
```
---
### 4.Soru
**Film tablosunda bulunan filmlerin uzunluğu 150 dakikadan büyük olanlarına ait kaç farklı replacement_cost değeri vardır?**
```sql
SELECT COUNT(DISTINCT replacement_cost) AS farkli_cost_sayisi
FROM film
WHERE length > 150;
```
---
## Ödev 7

### 1. Soru
**Film tablosunda bulunan filmleri rating değerlerine göre gruplayınız.**
```sql
SELECT rating, COUNT(*) AS film_sayisi
FROM film
GROUP BY rating;
```
---
### 2. Soru
**Film tablosunda bulunan filmleri replacement_cost sütununa göre grupladığımızda film sayısı 50 den fazla olan replacement_cost değerini ve karşılık gelen film sayısını sıralayınız.**
```sql
SELECT replacement_cost, COUNT(*) AS film_sayisi
FROM film
GROUP BY replacement_cost
HAVING COUNT(*) > 50
ORDER BY film_sayisi DESC;  

-- Having, Group BY sonrası kullanılarak filtreleme işlemi yapar. 
```
---

### 3. Soru
**Customer tablosunda bulunan store_id değerlerine karşılık gelen müşteri sayılarını nelerdir?** 
```sql
SELECT store_id, COUNT(*) AS musteri_sayisi
FROM customer
GROUP BY store_id;
```
---
### 4. Soru
**City tablosunda bulunan şehir verilerini country_id sütununa göre gruplandırdıktan sonra en fazla şehir sayısı barındıran country_id bilgisini ve şehir sayısını paylaşınız.**
```sql
SELECT country_id, COUNT(*) AS sehir_sayisi
FROM city
GROUP BY country_id
ORDER BY sehir_sayisi DESC
LIMIT 1;
```
---
## Ödev 8

### 1.Soru
**Test veritabanınızda employee isimli sütun bilgileri id(INTEGER), name VARCHAR(50), birthday DATE, email VARCHAR(100) olan bir tablo oluşturalım.**
```sql
CREATE TABLE employee (
    id        INTEGER        PRIMARY KEY,
    name      VARCHAR(50)    NOT NULL,
    birthday  DATE           NOT NULL,
    email     VARCHAR(100)   UNIQUE
);
```
---
### 2. Soru
**Oluşturduğumuz employee tablosuna 'Mockaroo' servisini kullanarak 50 adet veri ekleyelim.**
```sql
/* Mockaroo servisini kullanarak "employee" tablosu için 50 satırlık örnek veri oluşturdum. 
Alanlar: id, name, birthday, email
CSV formatında indirilen dosyanın adı: MOCK_DATA.csv
PgAdmin 4 arayüzündeki "Import/Export" özelliği kullanılarak veri tabloya yüklendi.

Import işlemi şu adımlarla yapıldı:
1. PgAdmin 4'te employee tablosuna sağ tıklayıp "Import/Export" seçeneği açıldı.
2. Dosya yolu olarak "C:/Users/Lenovo/Downloads/MOCK_DATA.csv" girildi.
3. Format: CSV
4. Delimiter: ,
5. Header (başlık satırı) kutucuğu işaretli bırakıldı.
6. Sütun eşlemesi: id, name, birthday, email
7. OK butonuna basılarak veriler tabloya aktarıldı. */

--Diğer bir yol ise INSERT INTO kullanarak verileri eklemek olabilir. Bu yöntemi kullanarak eklediğim ilk 10 veri aşağıdaki gibidir.
```
---
```sql
INSERT INTO employee (id, name, birthday, email) VALUES
(1, 'Vergil Oliphant', '2025-03-15', 'voliphant0@furl.net'),
(2, 'Nolly Barrable', '2024-11-04', 'nbarrable1@tinyurl.com'),
(3, 'Ulick Rodd', '2025-02-03', 'urodd2@fema.gov'),
(4, 'Rivi Paolacci', '2025-01-29', 'rpaolacci3@lycos.com'),
(5, 'Ricki Hadaway', '2025-01-02', 'rhadaway4@woothemes.com'),
(6, 'Sallee Surmon', '2025-01-18', 'ssurmon5@surveymonkey.com'),
(7, 'Raddy Naisby', '2024-08-12', 'rnaisby6@globo.com'),
(8, 'Aime Tucknutt', '2024-09-01', 'atucknutt7@phpbb.com'),
(9, 'Arley Parham', '2024-10-10', 'aparham8@topsy.com'),
(10, 'Alvis Mingard', '2025-03-03', 'amingard9@toplist.cz');
```
---
### 3. Soru
**Sütunların her birine göre diğer sütunları güncelleyecek 5 adet UPDATE işlemi yapalım**
```sql
-- 1.güncelleme: id’ye göre  name, birthday, email değiştirildi.

UPDATE employee
SET name = 'Updated Name',
    birthday = '1999-03-18',
    email = 'updated1@example.com'
WHERE id = 1;

--2. güncelleme: name’e göre  id, birthday, email değiştirildi.

UPDATE employee
SET id = 216,
    birthday = '1997-06-021',
    email = 'updated2@example.com'
WHERE name = 'Nolly Barrable';

 --3. güncelleme: birthday’e göre  id, name, email değiştirildi.

 UPDATE employee
SET id = 300,
    name = 'john Stone',
    email = 'updated3@example.com'
WHERE birthday = '2025-02-03';

--4. güncelleme: email’e göre  id, name, birthday değiştirildi.

UPDATE employee
SET id = 400,
    name = 'Rhoda Creus',
    birthday = '2003-01-29'
WHERE email = 'rpaolacci3@lycos.com';

--5. güncelleme: 4 sütun olduğu için name’e göre  id, birthday, email güncellemesi bir başkası için tekrar yapıldı.

UPDATE employee
SET id = 500,
    birthday = '1991-09-15',
    email = 'updated5@example.com'
WHERE name = 'Ricki Hadaway';
```
---
### 4. Soru
**Sütunların her birine göre ilgili satırı silecek 5 adet DELETE işlemi yapalım.**
```sql
--1. id’ye göre silme işlemi

DELETE FROM employee
WHERE id = 10;

--2. name’e göre silme işlemi

DELETE FROM employee
WHERE name = 'Arley Parham';

--3. birthday’e göre silme işlemi

DELETE FROM employee
WHERE birthday = '2025-01-02';

--4. email’e göre silme işlemi

DELETE FROM employee
WHERE email = 'ssurmon5@surveymonkey.com';

DELETE FROM employee
WHERE email= 'atucknutt7@phpbb.com';
```
---

## Ödev 9

### 1. Soru
**City tablosu ile country tablosunda bulunan şehir (city) ve ülke (country) isimlerini birlikte görebileceğimiz INNER JOIN sorgusunu yazınız.**
```sql
SELECT country.country, city.city
FROM country
INNER JOIN city ON country.country_id = city.country_id
ORDER BY country.country ASC;
```
---
### 2. Soru 
**Customer tablosu ile payment tablosunda bulunan payment_id ile customer tablosundaki first_name ve last_name isimlerini birlikte görebileceğimiz INNER JOIN sorgusunu yazınız.**
```sql
SELECT 
  payment.payment_id, 
  customer.first_name, 
  customer.last_name
FROM customer
INNER JOIN payment ON customer.customer_id = payment.customer_id;
  ```
  ---
### 3. Soru
**Customer tablosu ile rental tablosunda bulunan rental_id ile customer tablosundaki first_name ve last_name isimlerini birlikte görebileceğimiz INNER JOIN sorgusunu yazınız.**
```sql
SELECT 
  rental.rental_id,
  customer.first_name,
  customer.last_name
FROM rental
INNER JOIN customer ON rental.customer_id = customer.customer_id;
```
---
## Ödev 10

### 1. Soru
**City tablosu ile country tablosunda bulunan şehir (city) ve ülke (country) isimlerini birlikte görebileceğimiz LEFT JOIN sorgusunu yazınız.**
```sql
SELECT 
  city.city,
  country.country
FROM city
LEFT JOIN country ON city.country_id = country.country_id;
```
---
### 2. Soru
**Customer tablosu ile payment tablosunda bulunan payment_id ile customer tablosundaki first_name ve last_name isimlerini birlikte görebileceğimiz RIGHT JOIN sorgusunu yazınız.**
```sql
SELECT 
  payment.payment_id,
  customer.first_name,
  customer.last_name
FROM customer
RIGHT JOIN payment ON customer.customer_id = payment.customer_id;
```
---
### 3. Soru
**Customer tablosu ile rental tablosunda bulunan rental_id ile customer tablosundaki first_name ve last_name isimlerini birlikte görebileceğimiz FULL JOIN sorgusunu yazınız.**
```sql
SELECT 
  rental.rental_id,
  customer.first_name,
  customer.last_name
FROM customer
FULL JOIN rental ON customer.customer_id = rental.customer_id;
```
---
## Ödev 11

### 1. Soru
**Actor ve customer tablolarında bulunan first_name sütunları için tüm verileri sıralayalım.**
```sql
SELECT first_name FROM actor
UNION
SELECT first_name FROM customer;
```
---
### 2. Soru
**Actor ve customer tablolarında bulunan first_name sütunları için kesişen verileri sıralayalım.**
```sql
SELECT first_name FROM actor
INTERSECT
SELECT first_name FROM customer;
```
---
### 3. soru 
**Actor ve customer tablolarında bulunan first_name sütunları için ilk tabloda bulunan ancak ikinci tabloda bulunmayan verileri sıralayalım.**
```sql
SELECT first_name FROM actor
EXCEPT
SELECT first_name FROM customer;
```
---
### 4. soru 
**İlk 3 sorguyu tekrar eden veriler için de yapalım.**
```sql
--1.soru için
SELECT first_name FROM actor
UNION ALL
SELECT first_name FROM customer;

--2. soru için
SELECT first_name FROM actor
INTERSECT ALL
SELECT first_name FROM customer;


-- 3. soru için
SELECT first_name FROM actor
EXCEPT ALL
SELECT first_name FROM customer;
```
---
## Ödev 12

### 1. Soru
**Film tablosunda film uzunluğu length sütununda gösterilmektedir. Uzunluğu ortalama film uzunluğundan fazla kaç tane film vardır?**
```sql
SELECT COUNT(*) AS uzun_film_sayisi
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
---
### 2. Soru
**Film tablosunda en yüksek rental_rate değerine sahip kaç tane film vardır?**
```sql
SELECT COUNT(*) AS en_yuksek_rate_filmler
FROM film
WHERE rental_rate = (SELECT MAX(rental_rate) FROM film);
```
---
### 3. Soru
**Film tablosunda en düşük rental_rate ve en düşün replacement_cost değerlerine sahip filmleri sıralayınız.**
```sql
SELECT * FROM film
WHERE rental_rate = (SELECT MIN(rental_rate) FROM film)
AND replacement_cost = (SELECT MIN(replacement_cost) FROM film);
```
---
### 4. Soru
**Payment tablosunda en fazla sayıda alışveriş yapan müşterileri(customer) sıralayınız.**
```sql
SELECT 
  c.customer_id, 
  c.first_name, 
  c.last_name, 
  COUNT(p.payment_id) AS toplam_odeme_sayisi
FROM payment AS p
INNER JOIN customer AS c ON p.customer_id = c.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name
ORDER BY toplam_odeme_sayisi DESC;
```







































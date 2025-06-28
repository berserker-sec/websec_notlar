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

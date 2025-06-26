# 0x01
Sql injection notları burada yer almaktadır.
## **SQL Injection nedir?**
Sql injection, saldırganların veri tabanı içeren uygulamalarda kendi sorgularını yazabilmesidir.
## **Veri Tabanı Mantığını Anlamak**
Veri Tabanı Sorguları
Veritabanında "SELECT 1" sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibidir.

![Image](https://github.com/user-attachments/assets/c5d098b7-acec-46d6-830a-900456a2a9b6)

Veritabanında "SELECT 2-1" sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibi çıkarma işlemi gerçekleşmektedir.

![Image](https://github.com/user-attachments/assets/201e156a-c5de-471b-84d2-5989259b8b5e)

Veritabanında "SELECT 2+1" sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibi toplama işlemi gerçekleşmektedir.

![Image](https://github.com/user-attachments/assets/eb9d9625-234b-4f59-8239-349e387a083c)

Veritabanında " SELECT '2-1' " sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibidir. Bu sefer çıkarma işlemi gerçekleşmemiştir. Çünkü bu ifade string'tir.

![Image](https://github.com/user-attachments/assets/d4965dc5-c89c-472c-80eb-7e6628225cf9)

Veritabanında " SELECT '2'-'1' " sorgusunu çalıştırdığımızda verdiği sonuç aşağıdaki gibidir. Bu seferki sorgu, '2' ve '1' stringlerini integer'a çevirir ve işlem yapar.

![Image](https://github.com/user-attachments/assets/279243ff-8469-4e10-b342-998e548ae62c)

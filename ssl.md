# **0x0E | SSL temelde nedir? HSTS**

## **SSL tanımı**

Ssl(Secure Sockets Layer), İnternet gibi bir bilgisayar ağı üzerinden iletişim güvenliği sağlamak üzere tasarlanmış bir şifreleme protokolüdür.

## **MITM ve SSL**

Kullanıcı ile hedef site arasında bir ortadaki adamın olduğunu düşünün. Ortadaki adam kullanıcı ile hedef site arasındaki tüm trafiği izlemektedir. Bir sunucuya HTTP ile
istek atıldığında, çoğu zaman sunucu size bir 301 (Moved Permanently) döndürerek aynı kaynağın HTTPS versiyonuna yönlendirir. Bunu gören ortadaki adam https versiyonuyla
istek atar. Https'e karşılık bir cevap döner ve ortadaki adam da bu cevabı kurbana gönderir. Hedef sitede ssl olsa bile kullanıcı böyle bir durumdayken ssl bağlantısı 
kuramaz.

<img width="1469" height="479" alt="image" src="https://github.com/user-attachments/assets/0e34c670-6312-43bb-a147-c7b314d8af64" />

Peki ortadaki adam veya herhangi bir kullanıcı kendi ssl sertifikasını üretebilir mi? Cevap evet.

[kodun kaynağı](https://stackoverflow.com/questions/10175812/how-can-i-generate-a-self-signed-ssl-certificate-using-openssl)
```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365
```

Hal böyleyken bu durum için de tedbir olarak sertifikanın doğrulanması gerekmektedir. 

## **Sertifika doğrulaması**
Browser, sertifikanın bir CA tarafından imzalanıp imzalanmadığını kontrol ederek ssl sertifikasının güvenilirliğini test etmiş olur.

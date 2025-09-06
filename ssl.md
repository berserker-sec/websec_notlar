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

Browser, sertifikanın bir CA otoritesi tarafından imzalanıp imzalanmadığını kontrol ederek ssl sertifikasının güvenilirliğini test etmiş olur. Haliyle x.com'un sahibinin ise para karşılığı ssl sertifikasını bir CA otoritesine imzalatması gerekir. Pek çok browser hizmetinin güvendiği sertifika otoritelerinin listesi bulunmakta. Herkes sertifika üretebildiği için browserlar güvendikleri CA tarafından imza bekler. Yani CA'lar kritik bir işlem yapmakta. Peki sertifika otoriteleri bu doğrulamayı nasıl yapıyor?

## **Sertifika Otoritelerinin Yöntemleri**

### **1. Yöntem**

Ucuz olan yöntemler çoğunlukla böyle olur. CA sertifikayı imzalar ama doğrudan size vermez. Bu sertifikayı örneğin info@x.com'a mail atar ve eğer siz bu domain'in sahibiyseniz sertifikanızı alabilirsiniz. Eğer x.com mail sunucusu hacklenirse belirli bir ücret karşılığında hacker ssl sertifikasına sahip olabilir.

### **2. Yöntem**

Bu yöntemde CA x.com'a http get request'i yollar. Eğer siz x.com domainine sahipseniz x.com/cokgizlidosya.txt path'ine dosya koyabileceğinizi varsayar. Bu http get talebi internette uzun bir yolculuk yaptıktan sonra bize ulaşır. Halihazırda ssl teknolojisine sahip olmayan bir kişiye bu http get talebinin nasıl güvenli bir şekilde ulaşacağı da bir soru işaretidir.

### **3. Yöntem**

Pahalı bir yöntemdir ve bu yöntemde sertifikayı elden teslim alırsınız. 


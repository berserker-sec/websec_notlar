# **0x03 CSRF Zafiyeti ve Same Site Cookie Önlemi**

CSRF (Cross-Site Request Forgery), Türkçesiyle Siteler Arası İstek Sahteciliği, bir web güvenlik zafiyetidir. Kullanıcının oturumunun açık olduğu bir web uygulamasında,
istemeden ve habersizce zararlı bir işlem yapmasına neden olan bir saldırı türüdür.

## **HTTP Hakkında**

Açılımı Hyper Text Transfer Protocol(Türkçe: Hiper-Metin Transfer Protokolü)'dür. Http, world wide web için veri iletişiminin temelidir. Http, tasarlanırken ilk başta bazı üniversitelerin doküman paylaşması için geliştirildi ve günümüz için oldukça yetersiz kalan bir protokoldür. Http'de yaşanan herşey aslında sadece bir request response'tan ibarettir. 

Farklı OSI katmanlarındaki protokolleri kıyaslamak doğru olmasa da http dendiğinde akla TCP 3-way handshake gelebilir.

![image](https://github.com/user-attachments/assets/9d855c7b-0c33-45e9-a58d-b08381b060ac)

https://afteracademy.com/blog/what-is-a-tcp-3-way-handshake-process/

## **TCP 3-Way Handshake**

3-way handshake kısaca istemcinin ve sunucunun iletişim kurma niyetinin doğrulanarak aralarındaki etkileşimin uygunluğunun sağlanmasıdır. 

Örneğin;

Client: “Merhaba, seninle konuşmak istiyorum.”

Server: “Merhaba benimle konuşmak istediğini duydum ve seninle konuşmaya müsaitim”

Client: “Merhaba, ben seninle konuşmak istediğimi söylemiştim sen de bunu duymuşsun ve benimle konuşmak için müsait olduğunu duydum. Hadi konuşalım”

Burada 3'lü paket gönderilmektedir. En büyük katkısı karşı tarafın kontrol edilmesidir.

Http ile kıyaslandığında http'nin doğrulamasının olmaması göze çarpar. Yani protokolde authentication(kimlik doğrulama) yoktur. Bir diğer konu da statelerdir.

Burada bir http post requesti bulunmaktadır. Request, header ve bodyden oluşmaktadır.

![image](https://github.com/user-attachments/assets/dbb65000-ed73-4889-8049-97cc94550e31)

## **Header ve Body'nin Önemi**

Header'da önemli bilgiler bulunmaktadır(örneğin cookie) diğer bilgiler ise body kısmındadır. Örneğin insanlar birbirlerine bakarken baş kısımlarına bakarlar çünkü yüz gibi kişinin kim olduğunun doğrulanmasını sağlayan şey baştadır. 

Bilinmesi gereken 4 önemli terim var; requestin bodysi, headerı ve response bodysi ile headerı.

## **Http vs Https**

Protokol olarak hiçbir farkları bulunmaz. Tek farkları https, http trafiğinin şifrelenmiş halidir.

## **Http authentication içermiyorsa, authentication mekanizması nasıl ilerliyor?**

Örnek bir request

![image](https://github.com/user-attachments/assets/569d58bf-a1ef-4355-952f-2e7ad72d134f)

Requeste karşılık gelen response.

![image](https://github.com/user-attachments/assets/c00818f3-5f02-4411-9599-3b75540a283b)

Bu request ve response'u alan browserdır. Bir authentication mekanizması olmadığı için cookie'ye ihtiyaç duyulur. Bu cookie'nin veri tabanında kaydedilmesi gerekir. Bir önceki responseta browsera söylenen bilgi 2. requestte otomatik olarak eklenir. Bu sayede sürekli kullanıcı adı ve parolaya gerek kalmaz.

![image](https://github.com/user-attachments/assets/167cd386-a51b-4ef1-9b5a-8035db9580f8)

Browserlar, sonraki requeste bazı bilgiler eklemesi gerekmekte. Peki bu işlem neye göre gerçekleşmekte? 

```
http://www.mdisec.com:80/
```

Browser’ların baktığı kısım burada “protocol”+”domain”+”port” kısımlarıdır. Bir web HTTP Request’i gittikten sonra HTTPS ile girdiğimizde oturum kayıplarıyla karşılaşmış olabilirsiniz. Sebebi bu durumdan kaynaklanmaktadır.

## **Cookie ve Session Hakkında**

Kullanıcı, sunucuya ilk giriş yaptığında sunucu bir session id üretir. Bu id genellikle bir cookie olarak tarayıcıya gönderilir. Tarayıcı bu cookieyi her defasında sunucuya gönderir. Sunucu bu id'ye göre oturum bilgilerini bulur. Cookie; bir anahtar, bir kimlik gibi düşünülebilir. Session, bu kimliğe dair bilgilerin yazıldığı ve tutulduğu bir bilgi topluluğudur ve bu da %90 sunucu tarafında.

### **Cookie nerede saklanır?**

![image](https://github.com/user-attachments/assets/767af416-f537-4fc4-b8d3-3d5c58c2fed6)

#### **1. Yöntem/Diskte Tutma**

Diskte kendi üzerinde tutabilir. Ama diskte tutmanın bazı dezavantajları var. Kullanıcıdan gelen her requestte diskteki hangi session'ın dosyasına ait olduğunu bulmak gerek ve bir değişiklik yapılacağı zaman dosyayı update etmek gerek. Sürekli diskte birşeyler yazıp silerek yapılan bu işlemler, request ve responseları yavaşlatır. 

Web application'ın sayısı birden ikiye çıkarsa ön tarafa gelen requestleri dağıtmak için bir reverse proxy'nin sessionları senkronize etmesi gerekmetedir(görselde olduğu gibi). Buradan da anlaşılacağı üzere diskte tutmak efektif bir yöntem değildir.

#### **2. Yöntem/DB'de Tutma**

DB'de saklanma durumunda cookie, SELECT sorgusu ile session tablosuna sorulur ve sessiondaki tüm bilgiler databaseden alınır. Diskte tutma yöntemine göre daha iyidir çünkü diskteki senkronizasyon problemi db'de yaşanmaz. Günümüzde çok kullanılan bir yöntem olmasına karşın yük ve performanstan dolayı en iyi yöntem değildir.

#### **3. Yöntem/Redis**

Redis; hiçbir diskte tutulmayan, sadece bir işletim sistemi aracılığıyla memory'de key-value şeklinde değer tutan bir servis. Session ile ilgili bilgilerin tutulup kontrol edildiği yer redis ise hem memory kullandığı için hızlı olur hem de veri kaybı yaşanırsa en kötü 302 status kodunu verir. Yeniden login ile cookie kolayca yenilenir.

#### **4. Yöntem/Cookie Based Session**

Cookie bilgisinin client'ta tutulmasına cookie based session denir. 

Cookie Based Session'da bir json objesi oluşturulur. Bir IV (Initialization Vector) değerine rastgele başlangıç değeri atanır ve json objesine eklenir. Daha sonra bir Checksum oluşturulur. Burada Symetric Encryption yapılacağı için IV değeri bize lazım olacaktır. Daha sonra session data’sının tamamını şifreliyoruz. Bunu şifrelerken kullandığımız şey sunucu tarafında config dosyasında bulunan SYMC ENC SUPER SECRET KEY değerini kullanırız. Daha sonra da verimizde herhangi bir değişiklik olmadığından emin olmak için HMAC ile objenin kendisi ve KEY değerini birleştirerek bir Signature oluştururuz. Daha sonra da bu değeri Base64 ile encode ederiz. Ve artık user’a bu değeri veririz. Session’ı artık kullanıcı taşıyacaktır.

```
SUNUCU TARAFINDA CONFıG DOSYASINDA BULUNAN SYMC ENC SUPER SECRET KEY ...!!!

username=mehmet&password=twitch

HTTP 302 OK
Location: mdisec.com/dashboard
Set-Cookie: {
'IV':'RANDOM_DEGER',
'session_data':ENC(['email','user_id'])}|CHECKSUM --SIGNATURE --> BASEE64ENCODE
```

## **CSRF Hakkında**

Bir e-ticaret uygulamasında adresin silinmeye çalışılması durumunda ortaya çıkan request.

<img width="816" height="324" alt="image" src="https://github.com/user-attachments/assets/1b2532e2-305a-42b0-ae06-e01d83e200ba" />

# **0x13 | Host Header Manipulation**

# **Bazı Temel Kavramların Açıklanması**

Bir ağdaki cihaz veya sunucuya host denir. Http request'inde hangi alan adına erişilmek istendiğini belirten başlığa ise host header denir. 

# **Web'de Yaşananlar ve Saldırı Zihniyeti**

Apache, Nginx ve Tomcat gibi web servislerinin veya ters proxy servislerinin aslında host alanı ile önemli ilişkisi vardır. Bir kullanıcı, bir web sitesine http 
requesti gönderir ve bu requestin içinde bir host alanı geçer. Ön tarafta bulunan web servisi, host alanına bakarak arka taraftaki hangi uygulamaya map edeceğini 
belirler. Atarak vektörleri açısından bakıldığında ise dikkat edilmesi gereken yer framework'tur. Çünkü frameworklerin de hostları kullandığı durumlar mevcuttur. Kullanıcının request ettiği urllerin backend tarafında kullanıldığı durumlar olabilir. Mesela .NET'te 'Request.Url.ToString()' fonksiyonu buna örnektir. Framework'te bulunan bu tarz yardımcı metotlar aslında Http request'in host alanındaki 'example.com' bilgisini alırlar. Eğer saldırganın yazdığı host alanı değeri bir yere takılmazsa, uygulamaya requestin map edilmesi sağlanırsa ve bu uygulamada host alanı kullanırak print edilen bir değer varsa xss zafiyeti ortaya çıkabilir. Hatta uygulamanın kullanıcıya döndüğü html içerik eğer cacheleniyorsa ve içerik birden fazla kullanıcıya sunuluyorsa, cache poison edilerek stored xss gibi daha kritik bir zafiyete yol açabilir. 

Nginx web servisi üzerinden örnek verelim. Nginx, belirli domainlere sahip requestleri uygulamaya yönlendirir.

```
GET /sayfa HTTP/1.1
Host: uygulama.com
```

Eğer bu başlık manipüle edilirse

```
GET /sayfa HTTP/1.1
Host: local-uygulama.com
```

Nginx, bu manipüle edilmiş Host başlığına göre isteği farklı bir uygulamaya yönlendirebilir. Bu durumda, aslında dış dünyadan erişilememesi gereken bir local uygulamaya erişim kazanmış olabilirsiniz. Bu yüzden Nginx veya başka bir reverse proxy kullanılırsa bu manipülasyonlara karşı konfigürasyonların doğru yapılması gerekmektedir.

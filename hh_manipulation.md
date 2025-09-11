# **0x13 | Host Header Manipulation**

# **Bazı Temel Kavramların Açıklanması**

Bir ağdaki cihaz veya sunucuya host denir. Http request'inde hangi alan adına erişilmek istendiğini belirten başlığa ise host header denir. 

# **Web'de Yaşananlar ve Saldırı Zihniyeti**

Apache, Nginx ve Tomcat gibi web servislerinin veya ters proxy servislerinin aslında host alanı ile önemli ilişkisi vardır. Bir kullanıcı, bir web sitesine http 
requesti gönderir ve bu requestin içinde bir host alanı geçer. Ön tarafta bulunan web servisi, host alanına bakarak arka taraftaki hangi uygulamaya map edeceğini 
belirler. Atarak vektörleri açısından bakıldığında ise dikkat edilmesi gereken yer framework'tur. Çünkü frameworklerin de hostları kullandığı durumlar mevcuttur. Kullanıcının request ettiği urllerin backend tarafında kullanıldığı durumlar olabilir. Mesela .NET'te 'Request.Url.ToString()' fonksiyonu buna örnektir. Framework'te bulunan bu tarz yardımcı metotlar aslında Http request'in host alanındaki 'example.com' bilgisini alırlar. Eğer saldırganın yazdığı host alanı değeri bir yere takılmazsa, uygulamaya requestin map edilmesi sağlanırsa ve bu uygulamada host alanı kullanırak print edilen bir değer varsa xss zafiyeti ortaya çıkabilir. 

# **0x13 | Host Header Manipulation**

# **Bazı Temel Kavramların Açıklanması**

Bir ağdaki cihaz veya sunucuya host denir. Http request'inde hangi alan adına erişilmek istendiğini belirten başlığa ise host header denir. 

# **Modern Web'de Neler Oluyor?**

Apache, Nginx ve Tomcat gibi web servislerinin veya ters proxy servislerinin aslında host alanı ile önemli ilişkisi vardır. Bir kullanıcı, bir web sitesine http 
requesti gönderir ve bu requestin içinde bir host alanı geçer. Ön tarafta bulunan web servisi, host alanına bakarak arka taraftaki hangi uygulamaya map edeceğini 
belirler. Atarak vektörleri açısından bakıldığında ise dikkat edilmesi gereken yer framework'tur. 

# **0x16 | SSRF**

# **SSRF(server side request forgery) nedir?**

Eskiden web uygulamaları başka bir kaynağa ihtiyaç duymuyordu. Ama artık teknolojinin ilerlemesi ve web'in de gelişmesinden dolayı uygulamaları daha kompleks bir yapıya
sahip olmaya başladı. Mesela artık web uygulaması dediğimiz şey aslında şuan bir application proxy olmaya başladı. Çünkü mikroservislerle iletişim kurması gerekiyor. O 
yüzden modern uygulamaları anlamak için kullandığımız her web sitesinin nasıl çalıştığını kafamızda canlandırmalıyız.

SSRF'e gelinecek olursa CSRF ile isim benzerliğine sahiptir ama bu iki zafiyet birbirinden farklıdır. SSRF'in açılımı "Server Side Request Forgery"dir yani sunucu 
taraflı istek sahteciliği. SSRF deyince aklımıza backend'in client gibi davrandığı noktalar aklımıza gelmelidir. Bir uygulama düşünelim. Bu uygulama, sunucular 
bölgesinin dışına çıkıp bazı harici kaynaklara istek atmaktadır. Peki biz ssrf ile bu uygulamanın sunucular bölgesinde bulunan dahili bir kaynağa istek atmasını 
sağlarsak ne olur? 

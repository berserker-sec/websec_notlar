# **0x1E | HTTP Request Smuggling ve HTTP/2 Downgrade Saldırısı**

Http request smuggling aynı zamanda http desync diye de geçer ve bir web zafiyetidir. Web zafiyetleri deyince akla genelde web uygulamaları gelir fakat bu zafiyet web 
uygulamaları ile değil direkt web'in kendisiyle ilgilidir. 

Http request'i load balancer'a gelir, load balancer bunu nginx'e, nginx de bunu uygulama servisine verir. Cevaplar da aynı şekilde aynı yoldan döner. Http 1.0 ve 
1.1'in yapısı budur. Request'imizin geçtiği yollar bunlar ve requestimiz load balancer'da farklı nginx'te farklı yorumlanır.

<img width="1459" height="212" alt="image" src="https://github.com/user-attachments/assets/1fe9c259-d9b6-4cf7-abb2-3793e3e8feb4" />

3-way handshake sonrası tcp oturumunda biz bir http request'i göndeririz. Request'in içinde yer alacak text ifadeye bir örnek aşağıda bulunmaktadır.

<img width="250" height="98" alt="image" src="https://github.com/user-attachments/assets/49dbd549-e215-4bda-996e-ac59240bdcc3" />

Bu ifadenin gidebilmesi için karşı tarafa tcp paketlerinin gönderilmesi gerekmektedir. Karşı taraf bu paketleri bekleyecek ve `//` gördüğünde artık biz transport katmanından bir üst katmana request göndermeyi başarabiliriz. Veriler karşı tarafa gittikten sonra bir cevap dönecek. Tcp 1.0'de cevap alınana kadar ikinci istek atılamaz. Ama Tcp 2.0'de bu özellik yoktur. Peki biz request gönderirken başkası da request göndermiyor mu? Elbette gönderiyor. Başkasıymış gibi request göndermemiz durumunda veya başkasına giden cevapları etkileyebilmemiz durumunda bazı sorunlar ortaya çıkar. HTTP Request Smuggling, esasen bir aracı sunucuyu (proxy, load balancer) kandırarak, aynı TCP bağlantısı üzerinden birden fazla kullanıcının isteklerinin sırasını ve yorumlanma şeklini manipüle etmeye odaklanır. HTTP/2 Downgrade Saldırısında ise HTTP/2 protokolünü HTTP/1.1'e düşürerek, saldırgan HTTP/1.1 Request Smuggling gibi saldırılara zemin hazırlar. Çünkü HTTP/2, genellikle daha güvenli ve katı çerçeveleme kurallarına sahiptir.

Http requestlerinde content lenght ve transfer encoding diye iki tane header var. Bunlardan gelen değere göre http servisi davranış gerçekleştirir. Request'imizin geçtiği yollarda, requestimizin load balancer'da farklı nginx'te farklı yorumlandığından bahsettik. Örneğin "Load balancer'da transfer encoding dikkate alınacak mı?" veya "Nginx'te content lenght dikkate alınacak mı? bunlarla ilgili net bir kural yoktur. Bu yüzden her http servisi kendi kuralını üretir ve kurallar arasında uyuşmazlıklar olursa sorun ortaya çıkar. 

# **HTTP 2 hakkında**

HTTP 2, network seviyesinde binary bir protokoldür. Http 2'de her requestin bir id'si vardır biz de bu id ile ilgili cevabı alırız. Bu yüzden istediğimiz kadar http 2 requestini cevabını beklemeden çıkartabiliriz. Http 2; protokol bazlı, async ve hızlı bir imkan getirmektedir. 

Http 2 binary olduğu için content lenght okumamıza gerek yoktur. Çünkü http'nin alt katında protokol seviyesinde frameler vardır. Framelerin içinde de kaç byte okunacağı yazar. 

Http request smuggling zafiyeti de http 2'de yoktur. Bu yüzden yazının başlarında bahsettiğimiz gibi downgrade ile http 2 düşürülür ve request smuggling daha düşük http versiyonlarında sömürülür.

# **Farklı versiyonların beraber kullanımı**

Bir istemciden load balancer'a http 2.0 requesti gelsin. Daha sonra load balancer'dan arka taraftaki sunucuya http 1.1 requesti gitsin. Bu durumda tcp paketlerinde bölünme olacaktır. Çünkü http 2'de bir tcp bağlantısı üzerinden birden fazla tcp paketi gider. Ama http 1.1'de ayrı tcp bağlantıları kullanılır. Yani load balancer'a gelen bir paket birden fazla paket olarak çıkacaktır. Bu durumda load balancer'a arka taraftan gelen responselar başkasının http requesti ile eşleşebilir.

<img width="1250" height="616" alt="image" src="https://github.com/user-attachments/assets/fd65cc5b-02da-4335-a6e7-0665e0fb19b7" />

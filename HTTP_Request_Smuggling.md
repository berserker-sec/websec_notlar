# **0x1E | HTTP Request Smuggling**

Http request smuggling aynı zamanda http desync diye de geçer ve bir web zafiyetidir. Web zafiyetleri deyince akla genelde web uygulamaları gelir fakat bu zafiyet web 
uygulamaları ile değil direkt web'in kendisiyle ilgilidir. 

Http request'i load balancer'a gelir, load balancer bunu nginx'e, nginx de bunu uygulama servisine verir. Cevaplar da aynı şekilde aynı yoldan döner. Http 1.0 ve 
1.1'in yapısı budur. Request'imizin geçtiği yollar bunlar ve requestimiz load balancer'da farklı nginx'te farklı yorumlanır.

<img width="1459" height="212" alt="image" src="https://github.com/user-attachments/assets/1fe9c259-d9b6-4cf7-abb2-3793e3e8feb4" />

3-way handshake sonrası tcp oturumunda biz bir http request'i göndeririz. Request'in içinde yer alacak text ifadeye bir örnek aşağıda bulunmaktadır.

<img width="250" height="98" alt="image" src="https://github.com/user-attachments/assets/49dbd549-e215-4bda-996e-ac59240bdcc3" />

Bu ifadenin gidebilmesi için karşı tarafa tcp paketlerinin gönderilmesi gerekmektedir. Karşı taraf bu paketleri bekleyecek ve `//` gördüğünde artık biz transport katmanından bir üst katmana request göndermeyi başarabiliriz. Veriler karşı tarafa gittikten sonra bir cevap dönecek. Tcp 1.0'de cevap alınana kadar ikinci istek atılamaz. Ama Tcp 2.0'de bu özellik yoktur. Peki biz request gönderirken başkası da request göndermiyor mu? Başkasıymış gibi request gönderebilir miyiz? Başkasına giden cevapları etkileyebilir miyiz?

Http requestlerinde content lenght ve transfer encoding diye iki tane header var. Bunlardan gelen değere göre http servisi davranış gerçekleştirir. Request'imizin geçtiği yollarda, requestimizin load balancer'da farklı nginx'te farklı yorumlandığından bahsettik. Örneğin "Load balancer'da transfer encoding dikkate alınacak mı?" veya "Nginx'te content lenght dikkate alınacak mı? bunlarla ilgili net bir kural yoktur. Bu yüzden her http servisi kendi kuralını üretir ve kurallar arasında uyuşmazlıklar olursa sorun ortaya çıkar. 

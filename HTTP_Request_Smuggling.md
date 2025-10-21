# **0x1E | HTTP Request Smuggling**

Http request smuggling aynı zamanda http desync diye de geçer ve bir web zafiyetidir. Web zafiyetleri deyince akla genelde web uygulamaları gelir fakat bu zafiyet web 
uygulamaları ile değil direkt web'in kendisiyle ilgilidir. 

Http request'i load balancer'a gelir, load balancer bunu nginx'e, nginx de bunu uygulama servisine verir. Cevaplar da aynı şekilde aynı yoldan döner. Http 1.0 ve 
1.1'in yapısı budur. Request'imizin geçtiği yollar bunlar ve requestimiz load balancer'da farklı nginx'te farklı yorumlanır.

<img width="1459" height="212" alt="image" src="https://github.com/user-attachments/assets/1fe9c259-d9b6-4cf7-abb2-3793e3e8feb4" />


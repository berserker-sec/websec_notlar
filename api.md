# 0x26 | API SECURITY

Uygulamalar, frontend ve backend olarak iki kısma ayrıldıktan sonra backend dünyasının içerisinde çok detayları konular vardır. Uygulamaların arka planında birçok 
servis çalışmaktadır. Bu yapıların temelini öğrendikten sonra konu tamamiyle yoruma kalıyor. 

Böyle bir requestimiz olduğunu düşünelim.

<img width="212" height="98" alt="image" src="https://github.com/user-attachments/assets/2d6423b1-74d5-44d0-be78-fde7600cff47" />

Bu requesti karşılayan bir load balancer, ters proxy ya da api gateway olabilir. Bu request, buralardan geçip bir web uygulamasına ulaşacaktır. Bu gibi istekler, 
frameworkler arasında çeşitli değişkenlikler gösterebilir fakat api kullanıldığında bazı standart formatlar kullanılmaktadır. Bu standartlar, farklı frameworkler 
arasında bile uyumluluk sağlar. 

Diyelim ki bir uygulamanın aşağıdaki gibi bir user objesi ve username ve firstname isminde değişkenleri var.

````
GET /api/mdisec HTTP/2.0
Host: website.com
User-Agent: [Your User Agent]

username: "a-z100",
firstname: "[Desired First Name]"
````

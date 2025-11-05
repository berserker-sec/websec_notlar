# 0x26 | API SECURITY

Uygulamalar, frontend ve backend olarak iki kısma ayrıldıktan sonra backend dünyasının içerisinde çok detayları konular vardır. Uygulamaların arka planında birçok 
servis çalışmaktadır. Bu yapıların temelini öğrendikten sonra konu tamamiyle yoruma kalıyor. 

Böyle bir requestimiz olduğunu düşünelim.

<img width="212" height="98" alt="image" src="https://github.com/user-attachments/assets/2d6423b1-74d5-44d0-be78-fde7600cff47" />

Bu requesti karşılayan bir load balancer, ters proxy ya da api gateway olabilir. Bu request, buralardan geçip bir web uygulamasına ulaşacaktır. Bu gibi istekler, 
frameworkler arasında çeşitli değişkenlikler gösterebilir fakat api kullanıldığında bazı standart formatlar kullanılmaktadır. Bu standartlar, farklı frameworkler 
arasında bile uyumluluk sağlar. 

Diyelim ki bir uygulamanın aşağıdaki gibi bir user objesi ve username ve firstname isminde değişkenleri var.

```
GET /api/mdisec HTTP/2.0
Host: website.com
User-Agent: [Your User Agent]

username: "a-z100",
firstname: "[Desired First Name]"
```

"mdisec" olarak verilen değer burada username değişkenine atanmaktadır. Yazılımcılar, User.find($username) gibi bir ifade ile istedikleri işlemi gerçekleştirir. Eğerki güncelleme işlemi yapılacaksa da eskiden get ve post requestleri vardı. Peki böyle bir durumda create ve update'in farkı nasıl belirlenecek? İkisi de post ile gönderiliyor. Bunları ayırt etmek için "_method" kullanılırdı ve name kısmında eğerki "_method" varsa value'ya atanan değere göre bir ayırt etme işlemi gerçekleşirdi. Eğer value'ya atanan değer "put" ise arka tarafta create işlemi gerçekleşirdi ama eğer value'ya atanan değer "delete" ise arka tarafta silme işlemi gerçekleşirdi. Günümüzde ise yazılım mimarileri artık aşağıdaki gibidir.

```
class UserController extend Controller:
	def get():
	
	def delete():
	
	def update():
```


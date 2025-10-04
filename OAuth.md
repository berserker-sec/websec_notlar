# **0x1A | OAuth zafiyetleri**

## **OAuth nedir?**

OAuth, bir kişinin iznini alarak başka bir web sitesindeki bilgisine erişme imkanı sağlayan bir protokoldür. Örneğin bir kullanıcının yeni bir web sitesine kayıt olduğunu düşünün. Kayıt olurken bir form doldurması gerekiyor ama bunun yerine halizhazırda kayıtlı olduğu bir web sitesinden bilgileri alınıp kaydı yapılabilir. İşte bu, OAuth'tur. 

## **OAuth sorunları**

Kullanıcının halihazırda verilerini barındıran uygulamaya "provider" diyelim. Yeni kaydolduğu sitenin sunucusuna da "x" diyelim. X'e istek gönderip cevap alan kullanıcının, halihazırda bilgilerini tutan provider'a yönlenmesi gerekmektedir. Ama x ile provider'ın arasında bir bağlantı yok. Bunun için provider'ın, kendisiyle iletişim kuran kullanıcıyı x sitesine tekrar yönlendirmesi lazım. Kullanıcı, x sitesine tekrar geldiğinde bilgilerinin nasıl doğrulanıp doğrulanmadığının anlaşılması da lazımdır. Bunun için provider, client'a x sitesine tekrar yönleneceğini bildirmeli ve beraberinde bazı bilgiler barındırmalıdır. 

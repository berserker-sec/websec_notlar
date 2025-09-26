# **0x17 | SSTI**

Bir istemci, bir web uygulamasına erişmek istediğinde sunucuya http request'i gönderir ve sunucu da istemciye bir http response döner. Uygulamanın da bir mvc 
mimarisi ile geliştirildiğini düşünelim. Bu durumda, requestin karşılandığı bir controller ve controller'ın da döneceği bir view var. Eski usül basit 
uygulamalardan ziyade bu tarz framework algısına girmeye başladığımız zaman farklı atak vektörleri ortaya çıkmakta. Çünkü artık bir html implementasyonundan ziyade 
bir template implementasyonu yapmaya başlıyoruz yani özelleşmiş html gibi. Günün sonunda kullanıcının browser'ına bir html içerik dönülmekte ve bu içerik dış 
dünyadan inputla kontrol edilebilir.

Buradaki MDISEC içeriğini bir input olarak düşünelim.

```
<html>
    <div>
        Merhaba MDISEC
    </div>
</html>
```

Bu input, request ile uygulama tarafından konuyor olabilir ya da bir db'den de geliyor olabilir. Yukarıdaki örneğe nazaran aşağıdaki kod örneğinde daha dinamik bir 
yapı bulunmakta. Backend'deki template engine kütüphanesi, "return view('home.html', name)" kısmındaki name parametresinden aldığı değeri html'de görüntülemektedir.


```
return view('home.html', name)

<html>
    <div>
        Merhaba {{ name }}
    </div>
</html>
```

Burada sadece name değişkeninin olduğu yer dinamik tanımlanmıştır. Peki template'in de dinamik olduğu durumda ne olacak? Mesela kullanıcıdan alınan verinin template
olması gibi bir durum. Bu durum, template'in kullanıcı girdilerinden etkilendiği bir durumdur.

```
mditemplate = """"
<html>
    <div>
        Merhaba {{ name }}
    </div>
</html>
"""" 

return view(mditemplate, name)
```

Yukarıdaki kod, buna bir örnektir. 

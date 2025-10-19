# **Web Cache Nedir?**

Cache'in tanımını yapacak olursak cache(önbellek), veri depolayan bir yazılım veya donanım bileşenidir. Web cache ise, HTML sayfası ve resimleri gibi belgeleri geçici 
olarak depolamak, bant genişliği kullanımını, sunucu yükünü ve algılanan gecikmeyi azaltmak için oluşturulmuş bir mekanizmadır. Web cache mekanizmasını şablon üzerinden
inceleyelim.

Bir kullanıcının bir web server'ına /home adresi üzerinden erişeceğini varsayalım. Başka bir kullanıcı da /home adresi üzerinden bu server'a erişmek istediğinde aynı 
işlem iki defa tekrarlanmış olur. 

<img width="1230" height="684" alt="image" src="https://github.com/user-attachments/assets/9d2b0bdc-9535-4503-81c6-14f899e24aeb" />

İşte bunun önüne geçmek için web cache var. Web cache'in olduğu senaryoda kullanıcı, /home adresine gitmek istediğinde önce web cache'e gelecek, ondan sonra web server'a gidilecek, ondan sonra da web server bu request için içerik üretecek ve içerik kullanıcıya verilecek. Bu olayların hepsi http tarafında gerçekleşir ve başka kullanıcılar bu işlemleri gerçekleştireceği zaman web cache, web sunucusunu bu yükten kurtarmış olacak.

<img width="1200" height="676" alt="image" src="https://github.com/user-attachments/assets/7625a920-704d-40c0-9326-bb00d85ec228" />

Eğer ki bir şey cache'leniyor ise bu şeyin bir key'i ve value'su olmalıdır. Value zaten content'tir, peki key ne? Key; web cache'in protokol, domain, path ve query string ile ürettiği string'tir. Yani şuna benzer:

```
http    |x.com |/home|reklamlar=1
protocol|domain| path|query string
```

Biz bu requesti ilk gönderdiğimizde cache server, bu mekanizmanın kendi içerisinde bir key üretiyor. Kullanıcının elinde böyle bir key yoksa bu request sunucuya gönderilmek zorunda. Sunucudan içerik dönüleceği zaman ise bu içeriğin cachelenebilir olup olmadığı kontrol edilmelidir. Mesela /privatekey cachelenemez. Bu yüzden web cache'e nelerin cachelenebileceği bildirilmelidir. Bunu da bildirirken aslında header'la bildiriyoruz. Yani response'a bir takım header value'ları yazmış oluyoruz. Bu response'u da web cache alır. Value ise key ile eşleştirilen gerçek önbellek içeriğidir.

# **Web Cache Riskleri**

`http://www.x.com/home/reklamlar=1` şeklinde bir adresle bir request gönderdiğimizi düşünelim. Daha sonra bize bir response dönecektir. Bize dönen response'un body'sine birşey enjekte edebilirsek ve endpoint de cachelenebiliyorsa bizden sonra bu endpointi ziyaret eden adama sunulacak içeriğe müdahele edebiliriz. Saldırgan perspektifiyle baktığımızda ciddi zararlara yol açabilecek şeylerdir bunlar. Web cache üzerinden bir istismar gerçekleşecekse bu durumda aklımıza endpoint zaten gelir. Peki endpointe bir request gönderildiğinde bunun cache üzerinden dönüp dönmediği nasıl anlaşılır?

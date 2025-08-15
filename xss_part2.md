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


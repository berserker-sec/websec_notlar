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


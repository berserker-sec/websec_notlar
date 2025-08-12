# **XSS Devam Part 2**

Xss, browserda yaşanan bir zafiyettir ve backend tarafıyla doğrudan bir alakası yoktur. Js, sayfadaki hareket kabiliyetlerini şekillendiren bir dildir. Xss zafiyeti 
ile de js'e yönelik payloadlar yazılır. Peki js kodlarının, user'ın kontrol edebildiği inputları kullanma durumu nasıl gerçekleşecek?

## **DOM XSS**

Burada user'ın kontrol edebildiği inputlar direkt developer'ın yazdığı kodun içindedir.

```
<html>
  <div id="msgArea">
  </div>
  <script>
    user = getUsername(); //API'den mevcut user'ın isminin getirilmesi
    var username = document.getElementById()

    $("#msgArea").html('Merhaba' + username);
  </script>
 </html>
```

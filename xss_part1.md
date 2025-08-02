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


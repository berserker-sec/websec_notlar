# 0x02 | IDOR 

# **GİRİŞ**

IDOR'u anlamak için önce web uygulamasının nasıl çalıştığını anlamak gerek. Siber güvenlikteki en kritik nokta inputtur. Inputta uygulamanın kullanıcıdan aldığı direktiftir.
Mesela bir e-ticaret web uygulaması düşünelim. Bu uygulamaya birden fazla adres girebiliriz. Sipariş verirken bu adreslerden birini seçebiliriz. Ama eğerki başkasının adresini 
görebilirsek bu bir IDOR açığına örnektir.

# **IDOR tanımı**

Bir web sitesine girdiğimizde uygulamalara nesneler üzerinden erişiriz ama eğer ki bir saldırgan başka kullanıcıların nesnelerine yetkisiz erişim sağlarsa biz buna IDOR (Insecure Direct Object Reference) deriz. Türkçesi güvenli olmayan doğrudan nesne referansı'dır.

Web uygulamaları kullanıcıdan aldığı verilerle veritabanında bazı işlemler yapar(silme, ekleme, güncelleme gibi). Uygulamanın bu işlemleri yapabilmek için bazı kuralları vardır.
Mesela başka kullanıcıların verisine erişmeme gibi. Eğer erişilebiliyorsa IDOR zafiyeti var demektir.
Örneğin;

Burada mdi 1 user ve mdi 2 user isimli kullanıcılar var ve bu kullancıların adreslerini buradan ekleyebiliyoruz.

![image](https://github.com/user-attachments/assets/7966f014-721c-45c6-9b6e-ef6001cd1eed)

Mdi 1 kullanıcısının adresini silmeye çalıştığımda karşıma çıkan request. 

![image](https://github.com/user-attachments/assets/aa1e65e8-34e0-4b92-b250-75be87edf6a2)

Bu requeste baktığımda address isimli controller'ın delete metodu var ve bu metodun da 15 integer
değerini alan bir function parametresi var. Bu 15, adresin ifade edildiği id değeri anlamına gelir.

Not: Burada delete işlemi get talebi ile yapılmıştır. Ssrf token validation olmadığı için ssrf üzerinden adres bilgisi sildirilebilir.

```
Addressler //adresler tabosu

id AUTO INC //bu tabloya ait alanlar
user_id
title
adres_bilgisi
sehir_id

12 | MDI-2 Adresi | ?
15 | MDI-1 Adresi | dsfdfdsfs
```

Peki 15 yerine 12 yazarsam ne olur? Bana ait olan bir veriyi değil de başkasına ait bir veriyi silmeye çalıştım. Eğer ki uygulama bu veriyi silme yetkimin olup olmadığını
kontrol etseydi 302 yerine 404 hatası verirdi.

![image](https://github.com/user-attachments/assets/b5fef8fd-4cdd-4457-8bdf-b24b6634be13)

Sitede "Authorization Failde" hatasını görüyoruz. Bu veriyi silemediğim anlamına gelmez. Bu yüzden kontrol ediyorum.

![image](https://github.com/user-attachments/assets/27cac7ab-0097-45f7-84b1-f6be73185789)

Veri tabanında var olmayan bir id'yi girersem...

![image](https://github.com/user-attachments/assets/5f476a44-92e1-4c3d-895a-14e359539fd5)

Uygulamanın davranışlarından yola çıkarak yazılmış kod.

```php

class AddressController extend Controller {
  public function delete($address_id){
     if(!AddressModel->chechaddress($address_id)){ // adress_id'nin kontrolünün yapıldığı yer. eğer böyle bir id yoksa 404 hatası verecek.
       redirect('/',404)
     }

     AddressModel->deleteAddress($address_id); // adresin silindiği yer
  }
}
```

Eğer adresi silebilseydim bu, IDOR zafiyeti var demekti.

## **Missing Fuction Level Access ve IDOR**

İkisinin de tanımı:
IDOR (Insecure Direct Object Reference), saldırganların başka kullanıcıların nesnelerine yetkisiz erişimidir. Missing Fuction Level Access Controll(Fonksiyon Seviyesinde Yetki Kontrolü Eksikliği) ise, kullanıcıların kısıtlanması gereken işlevleri gerçekleştirmelerine veya korunması gereken kaynaklara erişmelerine olanak tanır. 

Id'si 12 olan adresi silmeye yarayan request bu şekildeydi. Bu uygulamada delete yerine farklı fonksiyonlar da kullanılıyor olabilir.

![image](https://github.com/user-attachments/assets/d13b0355-21b7-478c-9510-2ba56adc1438)

Burp suite'te intruder kısmına delete yerine başka ifade koyuluyor. Bu sayede sistemin ne gösterdiği görülebilir. 

![image](https://github.com/user-attachments/assets/76b5b09e-ce05-4538-ab07-f35e3b4b2c9d)

Delete yerine yazılabilecek fieldname(alan adı) listesi.

![image](https://github.com/user-attachments/assets/f916c1c3-5999-4c1d-997d-48f9879c1854)

Delete ifadesini fieldname listesinden kaldırıp diğer fonksiyonları deneyince edit'in farklı bir sonuç verdiğini görülüyor.

![image](https://github.com/user-attachments/assets/0a88e7c5-4b02-4ef3-bf92-53531316b9bc)

Yeni bir adres oluşturup adresin id'sini edit fonksiyonu ile beraber girildiğinde böyle bir sonuç görülmekte. Yani uygulamanın kullanıcıya sunmadığı bir özelliğe erişilmesi durumu söz konusudur bu da missing function level access controll zafiyetinin varolduğunu gösterir.

![image](https://github.com/user-attachments/assets/381feac7-53bd-460a-9a43-f2dbb7309819)

Eğer 17 yerine 5 yazılırsa burada başkasına ait veriler gözükmekte. Yani bu ise bir IDOR açığıdır.

![image](https://github.com/user-attachments/assets/20ec76bc-bcc5-4cbb-b66b-70a679c22fda)

Edit fonksiyonunun varolduğu bilindiğine göre tahmini kod yapısı aşağıdaki gibidir.

```
class AddressController extend Controller {

 public function edit($address_id){
$address = AddressModel->find($address_id);
dd($address);
}

  public function delete($address_id){
     if(!AddressModel->chechaddress($address_id)){
       redirect('/',404)
     }

     AddressModel->deleteAddress($address_id);
  }
}
```

## **Başka IDOR zafiyetleri**

Sipariş detay sayfasına geldiğimizde adres bilgisini görülüyor.

![image](https://github.com/user-attachments/assets/bea66f0a-53ca-41e4-8a3c-164324758108)

Checkout sayfasına girildiğinde böyle bir dropdown menü görülüyor.

![image](https://github.com/user-attachments/assets/d0d9a978-e62b-4de4-8a73-7e5fa337b54b)

Checkout sayfası incelediğinde adrese ait bir id değeri ve bu değerinin 17 olduğu görülüyor.

![image](https://github.com/user-attachments/assets/a5dbe226-f6cc-4e7d-823a-1d0674a196ba)

Adreslerin olduğu sayfa geri dönülüp sayfa incelendiğinde yine aynı adrese ait 17 değeri görülüyor. Yani uygulamanın her yerinde bu id kullanılıyor.

![image](https://github.com/user-attachments/assets/637c5c63-37af-45dc-b2be-db25897b0ca6)

Checkout sayfasına giden requestte 17 idsini görüyorülüyor. Bu, 18 ile değiştirilirse ne olur?

![image](https://github.com/user-attachments/assets/62729e4f-2eeb-4cd5-a2fe-232091327a33)

Siparişin show details kısmına gelindiğinde idsi 18 olan adres görülüyor.

![image](https://github.com/user-attachments/assets/94dd03c5-1dfd-4f58-b5db-93cdf40c03c7)

Bu işlemin literatürdeki tam adı da Second Order Insecure Direct Object Reference şeklindedir. Adresi seçtiğimiz endpoint ile değiştirdiğimiz id değerini gördüğümüz kısım farklıdır çünkü. Olay tek bir request-response döngüsü içinde yaşanmamaktadır.

# **IDOR, neden en zorlu zafiyet tipi?**

Zafiyetler, teknik zafiyetler ve bussiness logic zafiyetler olarak iki ana kategoriye ayrılabilir. Teknik zafiyetlerde(xss,sqli gibi) bir payload görülür ama bussiness logic zafiyetlerde görülmez. Yapılan örneklere bakıldığında da yapılan işin bir id'yi değiştirmek olduğu görülüyor, bunu da kodu incelerken fark etmek oldukça zor.  

Günümüzde, IDOR’un en çok karşılaşılan ve etkisi en kritik zafiyetlerden olmasının sebebi; artık developerların daha büyük ve gelişmiş uygulamalar geliştirmesidir.

# **Bu Tür Zafiyetler Nasıl ve Neden Ortaya Çıkmaktadır ? — Mikroservis Mimarilerine Bakış**

Bir kurumdan örnek verilecek olursa. Bu kurumda bir user var ve bu user bilgisine erişmesi gereken birçok uygulama var. Her uygulamanın veri tabanına erişip kendi sorgularını çalıştırması sorun olacaktır. Sorun gerçekleşmemesi için içeride user bilgilerini dönen bir service yazılmalıdır. Görselde de görüldüğü gibi artık birbirinden farklı çalışan service'ler bulunmakta ve bu mimarinin geliştirilmesinde alınan kararlar işin güvenlik kısmına etki etmekte. 

![image](https://github.com/user-attachments/assets/4a5083da-b58c-4ad2-9a97-c743731e5b07)

https://medium.com/@OlabodeAbesin/microservice-architecture-the-complete-guide-357bf7131cf1

Örneğin:

Görseldeki gibi id'yi hem 5 hem de 6 yapıp istek atıldığında python yukarıdaki ifadeyi alırken herhangi x dili aşağıdaki ifadeyi alabilir. Mikroservis ve API gateway'in farklı programlama dilleri ile çalışması json'ı farklı şekilde yorumlamalarına örnekteki gibi sebep olabilir.

![image](https://github.com/user-attachments/assets/c7cc5686-f739-4efd-8a3a-0da4ef9d3c7d)

# **Authmatrix**

AuthMatrix, web uygulamalarında ve web servislerinde yetkilendirmeyi test etmenin basit bir yolunu sağlayan bir Burp Suite uzantısıdır. Bu araç sayesinde isten
len kullanıcılar eklenerek her kullanıcı için ayrı bir pencerede işlemler yürütülebilir. 

![image](https://github.com/user-attachments/assets/74995032-63fd-49da-9299-e7d4788d8532)

# **Autochrome**

https://github.com/nccgroup/autochrome

# **Kaynakça**

https://www.youtube.com/watch?v=TsJ2XPuGe1k

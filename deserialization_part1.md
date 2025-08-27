# **0x0C Deserialization Zafiyeti**

Deserialization, kelime anlamı olarak "seriden çıkarma" demektir. Deserialization işlemi serialization'ın(serileştirmenin) tam tersidir. Serialization bir nesneyi ham 
veriye çevirirken deserialization ham veriyi nesneye çevirir. Deserialization işlemleri sırasında kaynaktan veri okuma ve kullanıcı girdisini doğrulamadan geçirme gibi 
durumlar zafiyete sebep olur. 

## **Web Uygulamalarında Neler Oluyor?**

Bilindiği üzere bir http request-response cycle'ında session bilgisi bulunmaktadır. Session bilgisinin her döngüde eklenmesi gerekir çünkü bu bilgi http protokolünde 
tutulmaz. İşte serialization devreye girer. Örneğin bir programlama dilinde bir sınıf üzerindeki verileri temsil eden bir string oluşturulur daha sonra bu string, nesneye
çevrilebilir hale getirilebilir.

### **Kod Örneği**

Bu kodda User isminde bir sınıf tanımlanmıştır. "firstname" ve "lastname" adında iki adet property tanımlanmıştır. Yapıcı bir metot olan constructor tanımlanmıştır. $user adında bir User tanımlanmıştır ve buna da "Mehmet","İnce" değerleri atanmıştır.

```
<?php

class User{
    var $firstname;
    var $lastname;

    function __construct($firstname="",$lastname="")
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
    }
}

$user = new User("Mehmet","İnce");
```

$user objesi bir yerde tutulmalıdır. Yoksa request response döngüsünde her seferinde gerekli işlemler tekrar edilmek zorunda. 


Kodda kullanıcı adının terminale yazdırılmasını sağlayan değişikliklerin yapılmış hali.

```
<?php

class User{
    var $firstname;
    var $lastname;

    function __construct($firstname= "", $lastname=""){
        $this->firstname=$firstname;
        $this->lastname=$lastname;
    }
    function __toString(){
        return $this->firstname." ".$this->lastname."\n";
    }
}
// User'a ait bilgiler db'den sessionId ile elde edildi. 
// ve User sınıfı oluşturuldu.
$user = new User("Mehmet","İnce");

echo $user;
```

Kodun çıktısı

<img width="411" height="131" alt="image" src="https://github.com/user-attachments/assets/290887ac-523a-4bdf-8b42-e2612fbd3b7d" />

Kod, her çalıştırdında bu fonksiyonlar tekrar çalışacaktır. Session'dan istediğimiz veriyi elde edip tekrardan sınıfa dönmemiz lazım. İşte serialization'ın işe yaradığı kısım burasıdır. 

```
<?php

class User{
    var $firstname;
    var $lastname;

    function __construct($firstname="",$lastname="")
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
    }
    function __toString(){
        return $this->firstname." ".$this->lastname."\n";
    }
}

$user = new User("Mehmet","Ince");

$store_somewhere = serialize($user);

echo $store_somewhere;

```

Kodun yeni halinin çıktısı.

<img width="784" height="112" alt="image" src="https://github.com/user-attachments/assets/761cd1b0-f70d-45ba-acd4-d719373e48bf" />

Tanımladığımız sınıfta bir "User" objesi oluşturduk. Sınıfa ait 2 tane property tanımladık. Yapıcı metot tanımladık. Çıktıdaki 'O:4' ifadesi bir php objesi olduğunu ve bu objenin 4 karakterden oluştuğunu ifade eder. Çıktıdaki '2' ise 2 tane property olduğu anlamına gelir. Parantezlerin içerisindeki değerler 'string:karakter sayisi:property ismi' şeklindedir.

Yeni dosyalarımızı oluşturup inceleyelim

#### **serialize.php dosyası**
```
serialize.php
<?php
require_once("user_class.php");

$user = new User("Mehmet","İnce");

$store_somewhere = serialize($user);

echo $store_somewhere;
```

#### **user_class.php dosyası**
```
<?php

class User{
    var $firstname;
    var $lastname;

    function __construct($firstname= "", $lastname=""){
        $this->firstname=$firstname;
        $this->lastname=$lastname;
    }
    function __toString(){
        return $this->firstname." ".$this->lastname."\n";
    }
}
```

#### **deserialize.php dosyası**
```
<?php
require_once("user_class.php");

$deserialize_str = 'O:4:"User":2:{s:9:"firstname";s:6:"Mehmet";s:8:"lastname";s:4:"Ince";}';

$user = unserialize($ $deserialize_str);

echo $user;
```

Deserialization işleminin gerçekleştiğini gösteren çıktı.

<img width="1215" height="117" alt="image" src="https://github.com/user-attachments/assets/cf7135e7-4f52-4c0c-8342-6bbda55b45ed" />

Kodlarda, objenin içindeki "User"dan User sınıfını bulduk. Bu sınıfa erişen bir objeyi bulduğumuz için serialize edip saklıyoruz. Bu sayede sürekli aynı işlemleri yapmaya gerek kalmıyoruz. 

Koda destruct metodu ekleyelim.

```
<?php

class User{
    var $firstname;
    var $lastname;

    function __construct($firstname="",$lastname="")
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
    }
    function __toString(){
        return $this->firstname." ".$this->lastname."\n";
    }
    function __destruct() {
        echo "Object destruction:".$this->firstname."".$this->lastname."\n";
    }
}

$user = new User("Mehmet","Ince");

$store_somewhere = serialize($user);



echo $store_somewhere;
```

Yeniden bir serialization işlemi gerçekleştirdiğimizde herhangi bir şekilde ‘__destruct’ fonksiyonunu çağırmasak bile bu fonksiyon çalıştırılacaktır. PHP burada bu sınıfı oluşturduktan sonra yani User sınıfı ile işi bittikten sonra __destruct() fonksiyonunu çağırmaktadır. Yani __destruct() fonksiyonu otomatik olarak çağrılmaktadır. Çıktı ise aşağıdaki gibi olmaktadır.

<img width="493" height="86" alt="image" src="https://github.com/user-attachments/assets/7be37bce-1b76-477f-a123-4f8e35bea4ff" />

Şimdi ise sınıf oluşturulurken çağrılan bir metot olan '__wakeup()' metodunu yazalım.

```
<?php

class User{
    var $firstname;
    var $lastname;

    function __construct($firstname="",$lastname="")
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
    }
    function __toString(){
        return $this->firstname." ".$this->lastname."\n";
    }
    function __destruct(){
        echo "Object destruction: ".$this->firstname." ".$this->lastname;
    }
    function __wakeup(){
        echo "SINIF UYANDIRILDI! ";
    }
}

$user = new User("Mehmet","Ince");

$store_somewhere = serialize($user);
```

Kodun çıktısı bu şekilde olacaktır.

<img width="522" height="74" alt="image" src="https://github.com/user-attachments/assets/b5e1f9a6-eae9-419c-8a0a-2c5c9d3855ab" />

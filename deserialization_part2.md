# **Deserialization Zafiyeti Part 2 | 0x0C**

Part 1'in devamıdır.

# **Property Oriented Programing (POP)**

Permission isminde bir class ekleyelim.

```
<?php

class Permission{
    var $permissionarr = array();
}

class User{
    var $firstname;
    var $lastname;
    var $is_admin = 0;
    function __construct($firstname="",$lastname="")
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
    }
    function __toString(){
        return $this->firstname." ".$this->lastname."\n";
    }
    function __destruct(){
       //echo "Object destruction: ".$this->firstname." ".$this->lastname;
    }
    function __wakeup(){
        //echo "SINIF UYANDIRILDI! ";
    }
    public function is_Admin(){
        if ($this->is_admin){
            return True;
        return False;
    }
}

}

$user = new User("Mehmet","Ince");

$store_somewhere = serialize($user);
```

Payload'ın önceki halinde User sınıfına ait bir is_admin property'si vardı. Bir sınıfın property'si başka bir sınıfın property'si olabileceği için biz bu kısımda 
başka bir sınıf oluşturtacağız. Aşağıdaki kod da buna örnek verilebilir. 

```
$this -> is_admin = new Permissions();
```

Deserialize.php kodundaki payload'ı güncelliyoruz. Artık "Permissions" isimli property'i de payload'a eklemiş olduk.

```
<?php

require_once("user_class.php");

class SecretObject{

    var $filename;
    function __wakeup(){
        system("");
    }
}

$payload = "";

$payload = 'O:4:"User":3:{s:9:"firstname";s:0:"";s:0:"";s:0:"";s:8:"is_admin";O:11;"Permissions"}';

$user = unserialize( $payload);

echo $user->is_Admin();
```

Yeni halinin çıktısı.

```
PS C:\Users\Samed\php_proje\php_kodları> php deserialize.php

Deprecated: Creation of dynamic property User::$ is deprecated in C:\Users\Samed\php_proje\php_kodları\deserialize.php on line 17 

Warning: unserialize(): Error at offset 66 of 85 bytes in C:\Users\Samed\php_proje\php_kodları\deserialize.php on line 17

Fatal error: Uncaught Error: Call to a member function is_Admin() on false in C:\Users\Samed\php_proje\php_kodları\deserialize.php:19
Stack trace:
#0 {main}
  thrown in C:\Users\Samed\php_proje\php_kodları\deserialize.php on line 19
```

Bu hatayı gidermek için manuel olarak serialize işlemini yapmalıyız. Fakat öncesinde user_class.php dosyasında bazı değişiklikler yapmalıyız. 

```
<?php

class Permission{
    var $permissionarr = array();
}

class User{
    var $firstname;
    var $lastname;
    var $is_admin = 0;

    function __construct($firstname="",$lastname="")
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
        $this->is_admin = new Permission(); // değişikliğin yapıldığı kod parçası.
    }
    function __toString(){
        return $this->firstname." ".$this->lastname."\n";
    }
    function __destruct(){
       //echo "Object destruction: ".$this->firstname." ".$this->lastname;
    }
    function __wakeup(){
        //echo "SINIF UYANDIRILDI! ";
    }
    public function is_Admin(){
        if ($this->is_admin){
            return True;
        return False;
    }
}

}

$user = new User("Mehmet","Ince");

$store_somewhere = serialize($user);
```

Serialize işlemini gerçekleştirdiğimizde aşağıdaki çıktıyı alıyoruz. Permission sınıfı için permissionarr isminde bir property bulunmakta ve bizim hata alma sebebimiz de bu property'e payload'da yer vermememizden.

```
//serialize.php
O:4:"User":3:{s:9:"firstname";s:6:"Mehmet";s:8:"lastname";s:5:"İnce";s:8:"is_admin";O:10:"Permission":1:{s:13:"permissionarr";a:0:{}}}
```

Permission sınıfımıza yeni bir magic metot ekliyoruz.

```
function __wakeup()
    {
        echo "ben uyandım mdi";
    }
```

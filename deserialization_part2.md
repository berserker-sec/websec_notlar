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

Deserialize.php kodundaki payload'ı güncelliyoruz.

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

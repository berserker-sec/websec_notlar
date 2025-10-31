# **0x0F | OS Command Injection**

# **Web Uygulamalarının İşletim Sistemleri ile İlişkisi**

Programlama dilleri, işletim sistemleri ile iletişime geçerken bazı şeylere ihtiyaç duyar. Bir sunucu veya bilgisayarda bazı programlar bulunur ve bir de bu 
bilgisayardan erişilen bir web uygulaması bulunur. Bu web uygulaması, belli durumlarda işletim sistemindeki diğer programlarla etkileşime geçer. Bu sayede web 
uygulaması üzerinden işletim sisteminde bazı kodlar ve programlar çalıştırılabilir. 

Kullanıcı, bir sunucuya request gönderdiğinde cevap vermek 60 saniyeden uzun sürüyorsa web uygulaması bunu bir "Background job" olarak yapma eğilimindedir. Bu işler sizi normalde bekletecek işler iken, asenkron bir şekilde farklı bir iş parçacığında gerçekleştirilerek beklemeniz önlenir.

# **Yazılım Mimarisi ve Güvenlik Tasarımı**

Bir web uygulamasının tasarlanmasındaki her aşamada işin güvenlik kısmı daima yer alır. Ama bu uygulamanın iletişim halinde olacağı diğer sistemlerle birlikte oluşturacağı en büyük yapıya gelindiğinde; "Bu uygulama ne yapacak?", "Özellikleri neler?" ve "Bu özellikler nasıl implemente edilecek?" gibi sorular doğar. Yani bir asenkron iletişim var ama buradaki tüm bu süreçler nasıl planlanıyor? Bunlar bilinirse command injection daha iyi anlaşılır.

# **Command Injection olabilecek yerler**

İşletim sisteminin yapabileceği şeylerin web uygulaması tarafından yapılabileceği yerler command injection zafiyeti barındırabilecek yerlerdir. Her dilde işletim sisteminde komut çalıştırmaya imkan sağlayan fonksiyon vardır. Örneğin `nmap 192.168.73.1 -Pn -n -p` şeklinde bir komut çalıştırdığımızı varsayalım. Buradaki `192.168.73.1` bir parametredir ve bu parametre kullanıcıdan alınırsa ve komutun içerisine güvensiz bir şekilde buraya bir parametre girilirse OS command injection zafiyetiyle karşılaşılmış olur. Güvensiz yerde girilen inputun outputu; html içerikte görüntüleniyorsa ve encoding yoksa xss, sql sorgusunda kullanılıyorsa ve parameter binding gibi bir işlem yapmıyorsa sqli ve işletim sisteminde çalıştıralacak komutun içerisinde ise OS command injection ortaya çıkabilmektedir.

# **Kod Üzerinden İnceleme**

```
static public function Logs_StartTrace($params) {
    $trace_id = 0;
    require_once('MailCleaner/Config.php');
    $mcconfig = MailCleaner_Config::getInstance();
    if (!isset($params['regexp'])
    || !$params['datefrom'] || !preg_match('/^\d{8}$/', $params['datefrom'])
    || !$params['dateto'] || !preg_match('/^\d{8}$/', $params['dateto']) ) {
      return array('trace_id' => $trace_id);
    }
    $cmd = $mcconfig->getOption('SRCDIR')."/bin/search_log.pl ".$params['datefrom']." ".$params['dateto']." '".$params['regexp']."'";
    if (isset($params['filter']) && $params['filter'] != '') {
      $cmd .= " '".$params['filter']."'";
    }
                if (isset($params['hiderejected']) && $params['hiderejected']) {
                    $cmd .= ' -R ';
                }
    if (isset($params['trace_id']) && $params['trace_id']) {
      $trace_id = $params['trace_id'];
    } else {
      $trace_id = md5(uniqid(mt_rand(), true));
    }
                $cmd .= " -B ".$trace_id;
    $cmd .= "> ".$mcconfig->getOption('VARDIR')."/run/mailcleaner/log_search/".$trace_id." &";
    $res = `$cmd`;
    return array('trace_id' => $trace_id, 'cmd' => $cmd) ;
  }
```

Koda ilk baktığımızda bizi ilk karşılayan şey, adından da anlaşılacağı üzere `static public function Logs_StartTrace($params)` yani logları yakından izleme işlemini başlatan bir fonksiyondur.

Biraz daha aşağı indiğimizde ise kullanıcıdan alınan 'regexp' parametresiyle match işlemleri gerçekleştiriliyor.
```
    if (!isset($params['regexp'])
    || !$params['datefrom'] || !preg_match('/^\d{8}$/', $params['datefrom'])
    || !$params['dateto'] || !preg_match('/^\d{8}$/', $params['dateto']) ) {
      return array('trace_id' => $trace_id);
```

Daha sonra burada ise işletim sisteminde bir pearl dosyası çalıştırılacağı görülüyor ve bu kod satırındaki parametreleri biz kontrol edebiliyoruz.

```
$cmd = $mcconfig->getOption('SRCDIR')."/bin/search_log.pl ".$params['datefrom']." ".$params['dateto']." '".$params['regexp']."'";
```

En aşağıda ise `$res = `$cmd`;` ile yukarıdaki komut çalıştırılıyor. Eğer bu koddaki parametlerden birini kontrol edebilirsek bu komuta kendi parametremizi enjekte edebiliriz. Aşağıda buna bir örnek var.

<img width="420" height="181" alt="image" src="https://github.com/user-attachments/assets/622922d1-e490-4fd4-b619-cc10da3634c6" />

" işaretleri içinde olan kısım kullanıcının müdahele edebildiği kısım. Eğerki oraya isim yerine bir dolar işareti ve parantezler içinde komut yazarsak komut çalışacaktır. " değilde ' tırnak içinde yazılırsa komut çalışmayacaktır.

<img width="418" height="61" alt="image" src="https://github.com/user-attachments/assets/5a311ca1-d233-4de2-bd98-8f409806147e" />

Fakat tek tırnak içindeki tüm parametrenin içine yine tek tırnaklarla komut yazılırsa çalışır.

<img width="746" height="58" alt="image" src="https://github.com/user-attachments/assets/b87a0770-a461-407f-8b40-d11cfabcc55f" />


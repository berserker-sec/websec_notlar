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

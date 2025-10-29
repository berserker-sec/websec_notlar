# **0x0F | OS Command Injection**

# **Web Uygulamalarının İşletim Sistemleri ile İlişkisi**

Programlama dilleri, işletim sistemleri ile iletişime geçerken bazı şeylere ihtiyaç duyar. Bir sunucu veya bilgisayarda bazı programlar bulunur ve bir de bu 
bilgisayardan erişilen bir web uygulaması bulunur. Bu web uygulaması, belli durumlarda işletim sistemindeki diğer programlarla etkileşime geçer. Bu sayede web 
uygulaması üzerinden işletim sisteminde bazı kodlar ve programlar çalıştırılabilir. 

Kullanıcı, bir sunucuya request gönderdiğinde cevap vermek 60 saniyeden uzun sürüyorsa web uygulaması bunu bir "Background job" olarak yapma eğilimindedir. Bu işler sizi normalde bekletecek işler iken, asenkron bir şekilde farklı bir iş parçacığında gerçekleştirilerek beklemeniz önlenir.

# **Yazılım Mimarisi ve Güvenlik Tasarımı**

Bir web uygulamasının tasarlanmasındaki her aşamada işin güvenlik kısmı daima yer alır. Ama bu uygulamanın iletişim halinde olacağı diğer sistemlerle birlikte oluşturacağı en büyük yapıya gelindiğinde; "Bu uygulama ne yapacak?", "Özellikleri neler?" ve "Bu özellikler nasıl implemente edilecek?" gibi sorular doğar. Yani bir asenkron iletişim var ama buradaki tüm bu süreçler nasıl planlanıyor? Bunlar bilinirse command injection daha iyi anlaşılır.

# **Command Injection olabilecek yerler**

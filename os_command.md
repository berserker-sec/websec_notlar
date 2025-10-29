# **0x0F | OS Command Injection**

# **Web Uygulamalarının İşletim Sistemleri ile İlişkisi**

Programlama dilleri, işletim sistemleri ile iletişime geçerken bazı şeylere ihtiyaç duyar. Bir sunucu veya bilgisayarda bazı programlar bulunur ve bir de bu 
bilgisayardan erişilen bir web uygulaması bulunur. Bu web uygulaması, belli durumlarda işletim sistemindeki diğer programlarla etkileşime geçer. Bu sayede web 
uygulaması üzerinden işletim sisteminde bazı kodlar ve programlar çalıştırılabilir. 

Kullanıcı, bir sunucuya request gönderdiğinde cevap vermek 60 saniyeden uzun sürüyorsa web uygulaması bunu bir "Background job" olarak yapma eğilimindedir. 

# **0x1F | File Upload Zafiyetleri**

## **File Upload nedir?**
File upload, uygulama sunucusuna bir takım dosyalar yüklemektir. Burada çıkan zafiyetler geleneksel mimarilerdeki uygulamalarda çıkan zafiyetlerdendir. Bir kullanıcı, 
örneğin bir jpg dosyasını yüklerken sunucunun `/var/www/users/uploads/` dizinine yükler. Böylece bu dosya tekrardan kullanıcıya gösterilebilir. 

## **File Upload Zafiyetleri**

File upload zafiyetlerini ikiye ayırabiliriz. Backdoor ve stored xss. Bir fotoğraf yüklenen uygulamanın /upload dizinine x.jpg yerine x.php dosyası yükleyebilirsek backdoor tipinde bir file upload zafiyeti mevcuttur. Böyle bir zafiyetin olması durumunda sunucuda kendi php kodumuzu çalıştırabileceğimiz anlamına gelir. Sunucuda kendi php kodumuzu çalıştırmamız, sunucunun dizinlerinde gezebilmemiz ve sunucuda komut çalıştırabilmemize olanak tanır. 

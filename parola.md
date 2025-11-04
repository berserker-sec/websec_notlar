# Web Security 0x23 | Parolalar Nasıl Saklanmalı 101

Şifrelerimizi açık bir şekilde tutmamak önemlidir. Burada şifreleme devreye girer. Şifreleme, anahtar kilit mantığına benzer çalışır. Gerçek hayatta, anahtarınızla 
kilitmele işlemini yaptıktan sonra anahtarınızı saklarsınız daha sonra kilidi açmak istediğinizde anahtarı sakladığınız yerden çıkarıp kilidi açarsınız. 

# Kriptoloji ve Şifreleme

Kriptoloji, kelime anlamı olarak şifre bilimi anlamına gelmektedir ve bazı alt dallara ayrılır.

<img width="720" height="416" alt="image" src="https://github.com/user-attachments/assets/3522f86b-2b22-4738-8af3-efbef36918ed" />

## Kriptografi

Kriptografi, şifreleme anlamına gelmektedir. Bilgiyi istenmeyen kişilerin okuyamayacağı hâle dönüştürme, yani şifreleme ve bu şifreyi çözme (deşifre etme) teknikleridir. Kriptografiyi 3 alt başlıkta inceleyebiliriz.

### Simetrik (Tek Anahtarlı) Kriptografi
Simetrik kriptoloji, hem şifreleme (encryption) hem de şifre çözme (decryption) işlemleri için aynı gizli anahtarın kullanıldığı şifreleme yöntemlerini ifade eder.

-Prensip: Anahtarın gizliliğine dayanır. Eğer anahtar bilinirse, şifre kolayca çözülür.

-Kullanım Alanı: Yüksek hızlı veri şifrelemesi (örneğin dosya şifreleme, VPN tünelleri, büyük veri trafiği).

-Popüler Algoritmalar: AES, DES, 3DES, Blowfish.

### Asimetrik (Açık Anahtarlı) Kriptografi

Bu sistem, şifreleme ve şifre çözme için matematiksel olarak ilişkili olan ancak birbirinden farklı iki anahtarın (bir çift) kullanıldığı yöntemdir.

- Anahtar Çifti:

    * Açık Anahtar (Public Key): Herkesle paylaşılabilir. Şifreleme ve kimlik doğrulamada kullanılır.

    * Gizli Anahtar (Private Key): Sadece sahibi tarafından gizli tutulur. Şifre çözme ve dijital imza atmada kullanılır.

- Prensip: Gizliliğin yanı sıra, güvenli anahtar paylaşımını ve inkâr edilemezliği sağlar.

- Kullanım Alanı: Güvenli anahtar değişimi (simetrik anahtarı güvenli iletmek için), dijital imzalar, SSL/TLS bağlantıları (web sitelerinde).

Popüler Algoritmalar: RSA, ECC (Eliptik Eğri Kriptografisi).

### Kriptografik Protokoller

Kriptografik protokoller, kriptografik bileşenlerin (simetrik şifreleme, asimetrik şifreleme, karma fonksiyonları vb.) uygulamaya konulduğu ve yönetildiği çerçevedir. Kriptografik protokoller, güvenli bir görevi yerine getirmek için iki veya daha fazla tarafın izlemesi gereken bir dizi kural veya adımdır. Bir nevi, güvenliği sağlamak için kullanılan "tarifler" gibidirler.

- Protokol Örnekleri
Bunlar, günlük hayatımızda kullandığımız ve kriptografinin tüm alt dallarını (simetrik, asimetrik, karma) birleştiren en yaygın örneklerdir:

* TLS/SSL (Transport Layer Security / Secure Sockets Layer): İnternet üzerindeki en yaygın protokoldür. Web tarayıcınız ile bir sunucu (HTTPS) arasındaki iletişimin şifreli ve doğrulanmış olmasını sağlar. (Hem asimetrik hem de simetrik şifrelemeyi kullanır.)

* IPsec (Internet Protocol Security): İnternet Protokolü (IP) seviyesinde şifreleme ve kimlik doğrulama sağlar. VPN'lerde yaygın olarak kullanılır.

* PGP/GPG (Pretty Good Privacy / GNU Privacy Guard): E-posta ve veri şifrelemesi ve dijital imzalama için kullanılır.

* SSH (Secure Shell): İki bilgisayar arasında güvenli bir komut satırı oturumu oluşturmak için kullanılır.

## Kriptanaliz (Cryptanalysis) Nedir?

Kriptanaliz, şifreli mesajları çözme ve kriptografik sistemlerin güvenliğini değerlendirme bilimidir. En basit tanımıyla, kriptanaliz, şifre kırma bilimidir.

- Temel Amaç ve Odak Noktaları
Şifreli Metni Çözmek: Bir şifreleme algoritması ve ilgili anahtar hakkında bilgi sahibi olmadan, sadece şifreli metni (ciphertext) analiz ederek orijinal düz metne (plaintext) ulaşmaya çalışmak.

- Kriptografik Sistemleri Analiz Etmek: Şifreleme algoritmalarının, karma fonksiyonlarının veya protokollerin matematiksel ve uygulama zayıflıklarını bularak bunların güvenliğini kırmak veya azaltmak.

- Anahtarı Keşfetmek: Şifreli metin üzerinden yola çıkarak şifrelemede kullanılan gizli anahtarı ele geçirmeye çalışmak.

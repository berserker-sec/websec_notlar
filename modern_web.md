# **Bir Hacker'ın Gözünden Modern Web**

Bilgisayar açıldığında dhcp'den ip alırız.

<img width="1139" height="570" alt="image" src="https://github.com/user-attachments/assets/e04e4ca0-867c-4519-95ac-b784208d3aa3" />

Görseldeki pc'lerin birbiriyle veya modemle ip vasıtasıyla iletişime geçmeden evvel mac adreslerinin bilinmesi lazım. Bunun için layer 2 katmanının protokolü olan 
ARP(address resolution protocol) devreye girer. Arp ile mac adreslerini öğrenirken ağdaki tüm cihazlara bir broadcast yapılır. Yani gerçek hayattan bir örnek verilecek 
olursa eeeyyyy "10.0.0.17" ben seninle iletişime geçmek istiyorum" şeklinde gerçekleşir.

## **ARP(address resolution protocol)**

200 kişinin bulunduğu bir ortamda Ayşe isimli birini arıyoruz ama Ayşe'nin yüzünü bilmiyoruz. "Ayşe, benimle iletişime geç!" diye seslendiğimizde Ayşe, bizi bulup iletişime geçme talebimize karşılık verecektir. Artık Ayşe'yi tanıyoruzdur ve tekrar iletişime geçmek istediğimizde tüm sınıfa seslenmeye gerek yoktur. O yüzden PC-1, PC-2'nin MAC adresini öğrendiğinde kendi üzerinde bir ARP tablosu adı verilen yere yazar.

<img width="1229" height="592" alt="image" src="https://github.com/user-attachments/assets/4901b189-7319-4512-87f2-0f59c8edfbce" />

## **ARP Poisoning**

Sınıfta bir öğrenci "eeeyyy Ayşe" diye seslendi ve birisi gelip bize Ayşe'ymiş gibi karşılık verdi. Peki bu kişi gerçekten Ayşe mi? İşte arp poisoning burada yaşanıyor. Görseldeki gibi 10.0.0.17 ip'sinin X'e değil de Y'ye aitmiş gibi gözükmesi o ipye asıl sahip olan asıl pc ile değilde öbür pc ile konuşmasına sebep olur. 

<img width="1196" height="609" alt="image" src="https://github.com/user-attachments/assets/6f71c652-f4c7-478d-9ef9-fc54b63f69b1" />

İnternete bağlanmamız için yerel ağın dışına çıkılması gerekmektedir.

<img width="1687" height="544" alt="image" src="https://github.com/user-attachments/assets/1593bf4e-c774-4692-83c1-2a2384299e22" />

## **Subnet**

Aşağıdaki iki ip aynı ağda mı diye sorulsa ne cevap verirsiniz? Sorunun cevabı bilmiyorum olmalıdır çünkü aynı ağda oladabilir olmayadabilir. Aynı ağda olmaları subnetlere bağlıdır. 

```
10.0.0.5
1.3.3.7
```

Subnet alt ağ demektir. Tanımı ise bir ip ağının alt bölümüdür. 

Buraya kadar yerel ağdan bahsedildi peki internette bir websitesine erişmeye çalışıldığında neler oluyor?

## **DNS(Domain Name System)**

Dns internette erişilmeye çalışılan alan adlarını ip'ye çeviren bir sistemdir. Kullanıcı bir websitesine gitmeden linux'ta etc/hosts dosyasında domain çözümlendirmesinin ne olacağını belirler. İlgili site ile ilgili dosyada eğer bir kayıt yoksa dns sunucusuna başvurulur. 

<img width="1852" height="701" alt="image" src="https://github.com/user-attachments/assets/289876b1-8b53-4d9e-a300-80248d6e0076" />

Dns sunucusuyla tcp veya udp vasıtasıyla haberleşilir. 8.8.8.8 dns'ine gidilerek girilmek istenen web sitesinin ip'si sorgulanır. Eğer bu bilgi, resolver dns'te yer almıyorsa route dns'e gidilerek sorgulanır. Eğer burada da yoksa Tld(Top level domain)'ye ulaşılır. Tld'nin işi *.com'ları bulmaktır. Eğer burada da yoksa authoritative dns'e yönlendirir. Burası dns kayıtlarının tutulduğu yerdir. 

<img width="1847" height="688" alt="image" src="https://github.com/user-attachments/assets/c4edf8c3-a113-48cb-bb6d-45df0bed9f01" />

Dns bilgileri ile ilgili örnek komut ve çıktı

<img width="813" height="540" alt="image" src="https://github.com/user-attachments/assets/9fefa03b-6255-4645-b510-7c8f4db8765b" />

## **NAT(Network address translation)**

Network Address Translation(ağ adresi çevirisi), TCP/IP ağındaki bir bilgisayarın yönlendirme cihazı ile başka bir ağa çıkarken adres uzayındaki bir IP ile yeniden haritalandırma yaparak IP paket başlığındaki ağ adres bilgisini değiştirme sürecidir. Ipv4'te ip sayısı kısıtlı olduğundan Nat kullanımı gereklidir.

## **Http'ye çıkma**

3-way handshake'ten sonra http'ye çıkılabilir. Http, OSI modelinde en üst yani 7. kat olan uygulama katmanındadır. Bir http requesti gönderildiğinde bir tane de response alınır. 7. katmanda sadece bu iki paket varken alt katmanlarda yüzlerce paket gönderilmektedir.

## **Firewall Hakkında**

80 portu ile firewall'un üzerinden geçeriz. Firewall bir güvenlik duvarıdır, yaptığı iş ise ağa gelen giden paketleri kontrol etmektir. Bir çeşit ağ yönetim aracı gibi düşünülebilir.

<img width="1862" height="672" alt="image" src="https://github.com/user-attachments/assets/2a375679-f19a-448e-8b91-61786f356b0a" />

Firewall'un üzerindeki kurallardan geçildikten sonra web sunucusuna erişilebilir. Sağ taraftaki kısım sunucular bölgesidir yani DMZ.

<img width="1741" height="684" alt="image" src="https://github.com/user-attachments/assets/8aaa2ab7-04c0-4118-a8a8-423740765012" />

## **Syn flood attack**

TCP 3-Way Handshake'in suistimali sonucu TCP-syn atağı gerçekleşmektedir. Saldırgan rastgele bir kaynak ip'si ile syn paketi gönderir. Web sunucusu bu rastgele ip'ye syn-ack gönderir ama cevap alamaz. Bu durumda sunucu yarım kalmış bu bağlantılarla dolmaya başlar ve sunucunun kaynakları tükenir. Firewall'lar ise sunuculara nazaran çok daha güçlü kaynaklara sahiptir ve bu saldırılara özel tedbirleri vardır. Dolayısıyla web sunucusu olası bu tarz saldırılara karşı daha fazla bileşene ihtiyaç duymaz, bu görevi firewall üstlenir. 

## **Web Sunucusu**

Burası web sunucusu. Günümüzde tüm işlemlerin ve teknolojilerin bu şekilde tek bir web sunucusunda bulunması pek mümkün değildir.

<img width="949" height="742" alt="image" src="https://github.com/user-attachments/assets/313da87d-ed65-4603-8457-bb96373ccf19" />

## **Virtual Hosting**

Virtual hosting (sanal barındırma), bir web sunucusunun aynı anda birden fazla web sitesine hizmet verebilmesini sağlayan bir teknolojidir.

<img width="1533" height="842" alt="image" src="https://github.com/user-attachments/assets/59e85590-acda-4b9b-ae9e-5c2866077b14" />

Http'nin gönderdiği request'teki host alanındaki host adı yazıyorsa cihazın çağırdığı dosyayı host içerisinden vermektedir. Örnekte olduğu gibi sunucu bu isteği aldığında "site1.com için hangi dizin kullanılacak?" sorusunun cevabını arar.

```
GET /index.html HTTP/1.1
Host: site1.com
```

Eğer konfigürasyonda site1.com için şöyle bir tanım varsa:

```
<VirtualHost *:80>
    ServerName site1.com
    DocumentRoot /var/www/site1
</VirtualHost>

Sunucu, bu durumda /var/www/site1/index.html dosyasını döner.
```

## **Sunucu Yetmediği Zaman Ne Yapılır? — Reverse Proxy**

Bir e-ticaret sitesi indirim günlerinde yoğunluk yaşar. Bu durumlarda mevcut sunucu yetersiz kalır.

Bunun için mevcut sunucudan 3 tane veri tabanından da 2 tane kurulduğunu varsayalım. Firewall'dan gelen reqestleri bunlara göndermemiz için bir reverse proxy ya da load balancer gerekmekte. Reverse proxy, istemciler (kullanıcılar) ile bir veya birden fazla arka uç sunucu (backend server) arasında duran bir sunucu türüdür. Kullanıcılar, doğrudan asıl sunucuya değil, reverse proxy'ye istek gönderir; reverse proxy bu isteği alır, arka uçtaki uygun sunucuya iletir ve gelen cevabı tekrar istemciye iletir. Load balancer (yük dengeleyici), gelen ağ trafiğini birden fazla sunucuya dağıtan bir sistemdir. Amaç, sistemin performansını ve erişilebilirliğini artırmak ve tek bir sunucunun aşırı yüklenmesini önlemektir. Reverse proxy ve road balancer farklı şeylerdir ancak reverse proxy aynı zamanda bir load balancer görevi de görebilir (örneğin Nginx, hem reverse proxy hem de load balancer olabilir).

Reverse Proxy’nin web sunucusuna ilettiği request bu kısma geldikten sonra bu uygulamanın çalışırken oluşturduğu session eğer diskte tutuluyorsa artık bu request’lerin her seferinde aynı sunucuya gelmesi gerekmektedir. Çünkü oluşan session sadece tek bir sunucunun diskinde olmuş olur. Diğer sunucularda session bilgisi bulunmamış olur.

<img width="939" height="724" alt="image" src="https://github.com/user-attachments/assets/e87cc5a4-5d4a-4f6e-9b2c-f58167ec9f5a" />

## **Mikroservis Hakkında**

Eğer birden fazla db varsa sql proxy'e ihtiyaç duyulabilir. SQL proxy, istemci (client) ile veritabanı sunucusu (database server) arasına yerleştirilen, gelen SQL sorgularını yakalayan, yönlendiren, filtreleyen veya değiştiren ara katman yazılımıdır. Sql proxy sayesinde hangi db müsaitse ona göre sorgulama yapılır. Mikroservislerin çıkış noktası da burasıdır. Mikroservis, büyük ve karmaşık uygulamaları daha küçük ve bağımsız çalışan servislere bölen bir mimaridir.  

<img width="1138" height="724" alt="image" src="https://github.com/user-attachments/assets/86fb70a4-a96d-4fdb-801b-df6edd3eb8c7" />

Session'lar diskte tutulursa diğer sunucular tarafından erişilemez, db'de tutulursa sorgular artar. Bu gibi durumların meydana gelmemesi için redis var. 

<img width="1118" height="757" alt="image" src="https://github.com/user-attachments/assets/54f9002f-f36b-4a60-aefe-77478def11c1" />

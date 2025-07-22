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


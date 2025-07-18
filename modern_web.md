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


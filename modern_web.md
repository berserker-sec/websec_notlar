# **Bir Hacker'ın Gözünden Modern Web**

Bilgisayar açıldığında dhcp'den ip alırız.

<img width="1139" height="570" alt="image" src="https://github.com/user-attachments/assets/e04e4ca0-867c-4519-95ac-b784208d3aa3" />

Görseldeki pc'lerin birbiriyle veya modemle ip vasıtasıyla iletişime geçmeden evvel mac adreslerinin bilinmesi lazım. Bunun için layer 2 katmanının protokolü olan 
ARP(address resolution protocol) devreye girer. Arp ile mac adreslerini öğrenirken ağdaki tüm cihazlara bir broadcast yapılır. Yani gerçek hayattan bir örnek verilecek 
olursa eeeyyyy "10.0.0.17" ben seninle iletişime geçmek istiyorum" şeklinde gerçekleşir.

## **ARP(address resolution protocol)**


# **Web Cache Nedir?**

Cache'in tanımını yapacak olursak cache(önbellek), veri depolayan bir yazılım veya donanım bileşenidir. Web cache ise, HTML sayfası ve resimleri gibi belgeleri geçici 
olarak depolamak, bant genişliği kullanımını, sunucu yükünü ve algılanan gecikmeyi azaltmak için oluşturulmuş bir mekanizmadır. Web cache mekanizmasını şablon üzerinden
inceleyelim.

Bir kullanıcının bir web server'ına /home adresi üzerinden erişeceğini varsayalım. Başka bir kullanıcı da /home adresi üzerinden bu server'a erişmek istediğinde aynı 
işlem iki defa tekrarlanmış olur. 

<img width="1230" height="684" alt="image" src="https://github.com/user-attachments/assets/9d2b0bdc-9535-4503-81c6-14f899e24aeb" />

İşte bunun önüne geçmek için web cache var. Web cache'in olduğu senaryoda kullanıcı, /home adresine gitmek istediğinde önce web cache'e gelecek, ondan sonra web server'a gidilecek, ondan sonra da web server bu request için içerik üretecek ve içerik kullanıcıya verilecek. Bu olayların hepsi http tarafında gerçekleşir ve başka kullanıcılar bu işlemleri gerçekleştireceği zaman web cache, web sunucusunu bu yükten kurtarmış olacak.

<img width="1200" height="676" alt="image" src="https://github.com/user-attachments/assets/7625a920-704d-40c0-9326-bb00d85ec228" />

Eğer ki bir şey cache'leniyor ise bu şeyin bir key'i ve value'su olmalıdır. Value zaten content'tir, peki key ne? Key; web cache'in protokol, domain, path ve query string ile ürettiği string'tir.

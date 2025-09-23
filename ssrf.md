# **0x16 | SSRF**

# **SSRF(server side request forgery) nedir?**

Eskiden web uygulamaları başka bir kaynağa ihtiyaç duymuyordu. Ama artık teknolojinin ilerlemesi ve web'in de gelişmesinden dolayı uygulamaları daha kompleks bir yapıya
sahip olmaya başladı. Mesela artık web uygulaması dediğimiz şey aslında şuan bir application proxy olmaya başladı. Çünkü mikroservislerle iletişim kurması gerekiyor. O 
yüzden modern uygulamaları anlamak için kullandığımız her web sitesinin nasıl çalıştığını kafamızda canlandırmalıyız.

SSRF'e gelinecek olursa CSRF ile isim benzerliğine sahiptir ama bu iki zafiyet birbirinden farklıdır. SSRF'in açılımı "Server Side Request Forgery"dir yani sunucu 
taraflı istek sahteciliği. SSRF deyince aklımıza backend'in client gibi davrandığı noktalar aklımıza gelmelidir. Bir uygulama düşünelim. Bu uygulama, sunucular 
bölgesinin dışına çıkıp bazı harici kaynaklara istek atmaktadır. Peki biz ssrf ile bu uygulamanın sunucular bölgesinde bulunan dahili bir kaynağa istek atmasını 
sağlarsak ne olur? 

1-Dahili Ağa Erişim
2-Bulut Metadata Servislerine Erişim
3-Port Tarama ve Servis Keşfi
4-Hassas Verilerin Sızdırılması
5-Diğer Saldırılara Zemin Hazırlama

Uygulama, bizden aldığı url üzerinde bir takım kontroller gerçekleştiriyor. Bu url ile hangi kaynağa erişilebileceği veya hangi formatta olabileceği gibi. Yani uygulamalar validation(tanımlama) yapmakta. Validation'ın da iki yaklaşımı var. Bu yaklaşımlar da black list ve white list. 

SSRF zafiyetini sonuçlarına göre ikiye ayırabiliriz. Basic SSRF ve Blind SSRF olarak ikiye ayırabiliriz bunların arasındaki temel fark birinde response size geri gelirken öbüründe gelmez. 

# **SSRF ile XXE benzerliği**

SSRF (Server-Side Request Forgery) ve XXE (XML External Entity) arasında bazı önemli benzerlikler vardır. Bu iki zafiyet farklı katmanlarda ortaya çıksa da temel etki mantığı açısından benzerlik taşır:

## **Sunucu Tarafından İstek Gönderilmesi**

İkisi de hedef sunucunun kendi adına istek göndermesine neden olur.

SSRF: Siz URL gibi bir girdi verirsiniz, sunucu sizin belirttiğiniz adrese HTTP isteği atar.

XXE: XML dosyasına eklediğiniz <!ENTITY> tanımı, sunucunun sizin gösterdiğiniz dosya veya URL’yi okumasına sebep olur.

## **Internal (Dahili) Kaynaklara Erişim**

Her iki saldırıda da saldırgan normalde dışarıya kapalı olan localhost (127.0.0.1), internal IP’ler (10.x.x.x) veya bulut metadata servisleri gibi kaynaklara erişebilir.

SSRF’de doğrudan URL vererek yapılır;

XXE’de ise ENTITY üzerinden file://, http://, ftp:// gibi protokollerle.

## **Dosya Okuma / Veri Sızdırma**

SSRF: Yanıt döndürülüyorsa, iç sistemlerden alınan hassas veriler sızdırılabilir.

XXE: file:// ile sistemdeki dosyaları (ör. /etc/passwd) okuyabilirsiniz.

## **Pivoting / Ağ Keşfi**

İkisi de iç ağa erişim sağlayarak diğer servislerin keşfine olanak tanır.

Örneğin metadata endpoint'leri, dahili API'ler veya admin panelleri.

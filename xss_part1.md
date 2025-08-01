# **0x08 | XSS hakkında herşey part 1**

## **XSS nedir?**

Açılımı cross site scripting'tir ve siteler arası betik çalıştırma olarak çevrilebilir. Xss, bir çeşit js enjeksiyonu gibi düşünülebilir. Bir web uygulamasının frontend 
kısmına yönelik bir saldırıdır.

## **XSS nerede bulunur?**

Browsera dönen response'un içeriğinde html, css ve js bulunur. Bir siber güvenlikçi perspektifiyle bakacak olursak bizim için js her zaman daha önemlidir. Çünkü js 
uygulamanın hareket kabiliyetini belirler. XSS, %99 ihtimalle browsera dönen response'un body'sinde bulunur. 

<img width="1153" height="581" alt="image" src="https://github.com/user-attachments/assets/06e5ce8c-241f-4345-b66c-14ce0efbed57" />

Arama kısmında "mehmet" şeklinde arama yapıldığında sayfada input görülebilmektedir. 

<img width="474" height="60" alt="image" src="https://github.com/user-attachments/assets/379af923-fee4-483c-a367-ac33d679c1bf" />

Sayfanın html'i incelendiğinde de aynı şekilde görülebilmektedir.

<img width="400" height="46" alt="image" src="https://github.com/user-attachments/assets/be6e3446-0980-4420-bf05-561b748b7e65" />

Browserdan web sunucusuna bir post reqesti gider ve requestte bulunan query string ya da post parametreleri ile arama kısmına yazılan içerik sunucuya ulaşır. Kullanıcının kontrol edebildiği bu mekanizma xss açığı barındırabilir. 

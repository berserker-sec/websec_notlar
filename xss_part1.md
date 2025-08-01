# **0x08 | XSS hakkında herşey part 1**

## **XSS nedir?**

Açılımı cross site scripting'tir ve siteler arası betik çalıştırma olarak çevrilebilir. Xss, bir çeşit js enjeksiyonu gibi düşünülebilir. Bir web uygulamasının frontend 
kısmına yönelik bir saldırıdır.

## **XSS nerede bulunur?**

Browsera dönen response'un içeriğinde html, css ve js bulunur. Bir siber güvenlikçi perspektifiyle bakacak olursak bizim için js her zaman daha önemlidir. Çünkü js 
uygulamanın hareket kabiliyetini belirler. XSS, %99 ihtimalle browsera dönen response'un body'sinde bulunur. 

<img width="1153" height="581" alt="image" src="https://github.com/user-attachments/assets/06e5ce8c-241f-4345-b66c-14ce0efbed57" />

## **XSS tipleri**

# **0x07 XML Parserlar ve XXE**

# **XML nedir?**

Günümüzde programlama dillerini öğrenenler bunun akabininde veri tabanlarını da öğreniyor ve temelde ilişkisel veri tabanıyla öğrenmeye başlıyor. Relational Database 
Management System yani ilişkisel veri tabanlarına örnek olarak MySQL, PostgreSQL, MSSQL, Oracle, Sqlite verilebilir. 

Biraz daha geriye gidildiğinde html'e benzeyen bir işaretleme dili olan xml ile karşılaşırız. XML(Extensible Markup Language), hem insanlar hem bilgi işlem sistemleri 
tarafından kolayca okunabilecek dokümanlar oluşturmaya yarayan bir işaretleme dilidir. Xml verileri bir formatta tutabilmemizi sağlar. 

```
<xml>
 <users>
   <user>
    <name>Mehmet</name>
   </user>
   <user>
    <name>Samed</name>
   </user>
</users>
</xml>
```

Xml günümüzde aktif değildir ama bazı servislerde kullanılmaktadır. 

## **Programlama Dillerinde Xml parsing**

Python'da Xml parse etme örneğinde xml olarak tanımlanan veri dictionary'e çevrilmiştir.

<img width="678" height="636" alt="image" src="https://github.com/user-attachments/assets/3c0655aa-c4f6-4d4d-9715-a4a0ee452c64" />

## **XML Gerçek hayatta kullanımı**

App server'ın verilerini sakladığı yer rdbms'tir. Diğer bir app server ile paylaşmak istediği zaman programlama dillerinde farklılık olduğu görülmektedir. İkisinin de aynı protokolü ve formatı kullanması gerekir. Bunlar da sırasıyla http ve xml'dir.

<img width="1170" height="710" alt="image" src="https://github.com/user-attachments/assets/f40bc64b-d639-450c-b64d-d197121a0a74" />

Input tracing, appsec'te çok önemli olan kısımdır. Uygulamanın aldığı inputlar takip edilerek uygulamaya dış dünyadan gelen parametreler tespit edilebilir. Bu parametreler ile hackerlar, uygulamaya bir takım alengirli işler yaptırabilir. Uygulamaya dış dünyadan gelen şey inputsa başka bir servisten alınan data da inputtur. Bu açıdan xml günümüzde hâlâ kullanılmaktadır ve önemlidir. 

Bir web uygulaması başka bir yerden xml aldığında bunu parse etmesi gerekir. Input validation yapılması gerekirse bile bunun öncesinde parsing yapılmalıdır. Bir hacker'ın hedeflediği yer ise parsing anıdır.

## **XML DTD**

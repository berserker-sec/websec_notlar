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

XML DTD (Document Type Definition), bir XML belgesinin yapısını ve kurallarını tanımlamak için kullanılan bir belgelendirme sistemidir. 

Burada dahili DTD (XML dosyası içinde tanımlanmış)

<img width="603" height="454" alt="image" src="https://github.com/user-attachments/assets/4bed4cd2-ef96-4903-97e1-a77a4c357fc1" />

Harici DTD (Başka bir dosyada tanımlanmış)

<img width="329" height="398" alt="image" src="https://github.com/user-attachments/assets/b500da82-4393-4f71-82b1-f00c1e87fde5" />

## **XML parser davranışları**

Xml parser ilk olarak kodda da görüldüğü üzere bir "DOCTYPE" tanımını görüyor. DTD'nin adı "note" ve "SYSTEM" operandı kullanılmış. SYSTEM'in yaptığı şey de external dtd. Yani xml parser ilk olarak external dtd'yi(Note.dtd) görür ve ondan xml'i parse eder. Note.dtd içindeki verilere göre note adındaki objeyi oluşturur. Bu durumda SYSTEM operandı sayesinde xml parser'a localdaki bir dosyayı okutabilme imkanı vardır ve bir hacker bu özelliği istismar edebilir. 

<img width="898" height="489" alt="image" src="https://github.com/user-attachments/assets/ac3bfed0-50b2-4c4d-b3fb-308ea438ae49" />

## **XXE**

DTD'nin içerisinde sadece dosya değil aynı zamanda string ifadeleri de bulunabilir veya aşağıdaki örnekteki gibi bir element tanımlanabilir mesela.

<img width="413" height="157" alt="image" src="https://github.com/user-attachments/assets/9eadd00d-e175-49bf-9fda-d66b76236f25" />

Entity kullanarak da string ifadeler de yazdırılabilir.

<img width="461" height="237" alt="image" src="https://github.com/user-attachments/assets/e3ab9dcf-3846-4ca4-b5ea-afe098dcf3a6" />

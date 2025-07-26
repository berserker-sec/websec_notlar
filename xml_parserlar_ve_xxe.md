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

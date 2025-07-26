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

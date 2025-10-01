# **0x18 | Directory Trevarsal**

Directory traversal açığı, bir web uygulamasının kullanıcıdan aldığı input doğrultusunda yereldeki bir dosyaya erişim sağladığı noktalarda karşımıza çıkar. Kullanıcıdan alınan input, bir kaynağa erişimde kullanıldığı zaman neye erişilebileceğinin değişmesine olanak tanır. Mesela inputta file adında bir parametre uygulamaya gitsin. Bu parametreye x değeri atansın. Saldırgan x yerine başka bir şey girdiğinde uygulama, başka bir kaynağı okuyup; silme, değiştirme veya ekleme gibi işlemlerini saldırgana sunabilir. 

# **Sözde Kod Örneği**

Aşağıdaki kodda web isteğinden filename isimli parametre alınıyor. Kullanıcı bu parametreye istediği değeri gönderebilir. BASE_PATH değişkeni önceden tanımlı bir ana dizini temsil ediyor (örneğin /var/www/hackerconf.stream/). Kullanıcının gönderdiği dosya adı bu dizine eklenerek tam yol oluşturuluyor. Eğerki kullanıcıdan `../../../../../../../../../../` gibi bir input alınırsa kök dizine gidilir ve daha sonrasında kullanıcı istediği dosyalarda gezinebilir.

```
filename = request.get('filename') # asdasd

path = BASE_PATH + 'static' + '/' + filename

content = file.read(filename)

/var/www/hackerconf.stream/static/../../../../../../etc/passwd

return content

../../../../../../../../../../
```

# **Null Byte ve Directory Traversal**

Uygulamanın kullanıcıdan dosya adı yerine klasör adı aldığını varsayalım. `filename` parametresi yerine `folder` isminde bir parametremiz olsun. Bu parametreye girilecek değerden sonra `selam/liste.txt` adında bir uzantı gelsin. Bu durumda biz ne yaparsak yapalım. Bizden alınan değerden sonra mutlaka `selam/liste.txt` ifadesi gelecektir. 

```
filename = request.get('folder') # asdasd

path = BASE_PATH + 'static' + '/' + folder + 'selam/liste.txt'

content = file.read(folder)

/var/www/hackerconf.stream/static/../../../../../../folder/selam/liste.txt

return content
```

Php'nin bazı eski versiyonlarında %00(null byte) ifadesini aynı sqli'daki yorum satırı gibi kullanarak kendisinden sonraki ifadeyle path'in birleşmemesi sağlanabiliyordu. 

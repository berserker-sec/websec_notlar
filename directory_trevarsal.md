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

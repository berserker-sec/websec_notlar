# **0x18 | Directory Trevarsal**

Directory traversal açığı, bir web uygulamasının kullanıcıdan aldığı input doğrultusunda yereldeki bir dosyaya erişim sağladığı noktalarda karşımıza çıkar. 
Kullanıcıdan alınan input, bir kaynağa erişimde kullanıldığı zaman neye erişilebileceğinin değişmesine olanak tanır. Mesela inputta file adında bir parametre uygulamaya
gitsin. Bu parametreye x değeri atansın. Saldırgan x yerine başka bir şey girdiğinde uygulama, başka bir kaynağı okuyup; silme, değiştirme veya ekleme gibi işlemlerini 
saldırgana sunabilir. 

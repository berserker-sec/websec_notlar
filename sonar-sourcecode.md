# **0x19 | Sonar Source Code İnceleme**

Bu c# kodunda bazı güvenlik açıkları bulunmaktadır.

<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/16fcf988-4b6d-4e5b-9b6f-95254eb37e30" />

Mesela bu kod satırında bir command execution vardır. Bu satırda packageId direkt olarak komut satırına ekleniyor. Eğer kullanıcı zararlı bir şey girerse, uygulama 
sadece paket yüklemekle kalmaz, onun yazdığı sistem komutlarını da çalıştırır. Bu da Command Execution (komut çalıştırma) zafiyetidir.

```
nuget.StartInfo.Arguments = "install " + packageId + " -NonInteractive";
```


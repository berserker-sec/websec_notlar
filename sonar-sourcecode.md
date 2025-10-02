# **0x19 | Sonar Source Code İnceleme**

Bu c# kodunda güvenlik açığı bulunmaktadır.

<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/16fcf988-4b6d-4e5b-9b6f-95254eb37e30" />

Mesela bu kod satırında bir command execution vardır. Bu satırda packageId direkt olarak komut satırına ekleniyor. Eğer kullanıcı zararlı bir şey girerse, uygulama 
sadece paket yüklemekle kalmaz, onun yazdığı sistem komutlarını da çalıştırır. Bu da Command Execution (komut çalıştırma) zafiyetidir.

```
nuget.StartInfo.Arguments = "install " + packageId + " -NonInteractive";
```

Bu php kodunda güvenlik açığı bulunmaktadır.

<img width="900" height="506" alt="image" src="https://github.com/user-attachments/assets/501da294-a75c-4d30-b18e-dd2d5c14e378" />

Bu kodda code evaluation zafiyeti vardır. `return eval("return ($expression);");` satırı kritik noktadır. Kullanıcı kontrollü veri (POST ile gelen şablon) doğrudan eval içine konulup çalıştırılıyor. Yani saldırgan, sunucu üzerinde istediği PHP ifadesini/komutunu çalıştıracak şekilde ifade gönderebilir.

Bu java kodunda güvenlik açığı bulunmaktadır.

<img width="680" height="383" alt="image" src="https://github.com/user-attachments/assets/1ddb94af-4833-48e1-8389-8b125bee2918" />

Sql injection güvenlik açığı içeren kod parcası.

```
final String nodeParameterName = ("snmp" + nodeParm).toLowerCase();
criteria.add(Restrictions.sqlRestriction(nodeParameterName + " = ?)",
    nodeParmValue, new StringType());
```

sqlRestriction'a verilen SQL fragmenti SQL derleyicisine doğrudan gidiyor. Sorgudaki kolon ismi/identifier (yani snmp...) kullanıcı kontrollü olduğunda saldırgan burada SQL sözdizimi bozacak veya yeni koşullar ekleyecek karakterler sokarak sorguyu manipüle edebilir. Değer (?) parametrelenmiş olsa da kolon adı parametrelenemez — bu yüzden parametrelenmiş kısım korunuyor olsa bile sorgu yapısını değiştirebilecek içerik hala mümkün.

Bu python kodunda güvenlik açığı bulunmaktadır.

<img width="680" height="383" alt="image" src="https://github.com/user-attachments/assets/800fec42-de24-4ed9-8656-91795bfb20bc" />



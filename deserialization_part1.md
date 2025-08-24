# **0x0C Deserialization Zafiyeti**

Deserialization, kelime anlamı olarak "seriden çıkarma" demektir. Deserialization işlemi serialization'ın(serileştirmenin) tam tersidir. Serialization bir nesneyi ham 
veriye çevirirken deserialization ham veriyi nesneye çevirir. Deserialization işlemleri sırasında kaynaktan veri okuma ve kullanıcı girdisini doğrulamadan geçirme gibi 
durumlar zafiyete sebep olur. 

## **Web Uygulamalarında Neler Oluyor?**

Bilindiği üzere bir http request-response cycle'ında session bilgisi bulunmaktadır. Session bilgisinin her döngüde eklenmesi gerekir çünkü bu bilgi http protokolünde 
tutulmaz. İşte serialization devreye girer. Örneğin bir programlama dilinde bir sınıf üzerindeki verileri temsil eden bir string oluşturulur daha sonra bu string, nesneye
çevrilebilir hale getirilebilir.

### **Kod Örneği**

# ** 0x0E | SSL temelde nedir? HSTS**

Kullanıcı ile hedef site arasında bir ortadaki adamın olduğunu düşünün. Ortadaki adam kullanıcı ile hedef site arasındaki tüm trafiği izlemektedir. Bir sunucuya HTTP ile
istek atıldığında, çoğu zaman sunucu size bir 301 (Moved Permanently) döndürerek aynı kaynağın HTTPS versiyonuna yönlendirir. Bunu gören ortadaki adam https versiyonuyla
istek atar. Https'e karşılık bir cevap döner ve ortadaki adam da bu cevabı kurbana gönderir. Hedef sitede ssl olsa bile kullanıcı böyle bir durumdayken ssl bağlantısı 
kuramaz.

<img width="1469" height="479" alt="image" src="https://github.com/user-attachments/assets/0e34c670-6312-43bb-a147-c7b314d8af64" />

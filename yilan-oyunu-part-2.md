# REACT NATIVE İLE YILAN OYUNU  \( PART 2 \)

![](/assets/Screen Shot 2017-10-01 at 14.25.18.png)

**1\) **Home ekranımızda, gameStore'dan gelecek en yüksek skor bilgisi var. Bu bilgi telefon hafızasında tutulacak. Bunu sağlamak için **AsyncStorage**'ı kullanabiliriz.  

![](/assets/Screen Shot 2017-10-02 at 01.07.26.png)**AsyncStorage**'da iki temel işlem var. `setItem` ve `getItem`.setItem'dan JSON.stringfy kullanmamızın sebebi, AsyncStorage'da yalnızca string ile çalışabiliyoruz. İki temel method da verilen key'e göre sorgu çekiyor. Burada bizim key'imiz "snakeHighScore". Buradaki algoritamanın mantığına gelince, telefon hafızasında, snakeHighScore key'ine sahip bir veri var mı. Varsa onu dön. Yoksa  sıfır\(0\) olarak yeni bir tane oluştur. Çünkü normal olarak kullanıcı oyunu ilk açtığında herhangi bir skora sahip olmayacaktır. Son olarak bize dönen değeri de, `this.highScore = _highScore` ifadesi ile observable state'imize set ediyoruz.




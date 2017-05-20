# Navigation

React Native'de sayfalar arası geçiş, router için söylenecek çok şey var. Önceki versiyonlarda sunulan direkt react-native kütüphanesinde yer alan Navigator pek sevilmedi. Redux implementasyonu neredeyse imkansızdı. Sonra Navigation-Experimental diye bir şey denendi. Adı üstünde experimental \(deneysel\) bir çalışmaydı ve bana göre başarısız oldu. Daha sonra bu bilgi birikimini toparlayıp, react-native kominitesi tarafından [react-navigation](https://reactnavigation.org/) piyasaya sürüldü. Oldukça kapsamlı ve güzel bir kütüphane oldu. Ancak yine tatmin edici değildi. Normal IOS ve Android uygulamalarda kullanıcı deneyimi sağlamıyordu. Bunun için native bir çözüm daha mantıklı. [wix.com](https://www.wix.com/) da çalışan geliştiricilerde böyle düşünmüş olacaklar ki tamamen native bir kütüphane geliştirmişler, [wix/react-native-navigation](https://github.com/wix/react-native-navigation) .

## wix/react-native-navigation

Gerçekten kullanımı diğer kütüphanelere çok daha kolay. Redux entegrasyonu diğerlerine çok çok daha kolay. Tek problem native bir component olduğu için, yükleme sırasında manuel link yapmak durumunda sizi bırakıyor. Dökümantasyonu da kütüphanenin büyüklüğüne göre biraz basit kaçmış gibi. Discord'da kütüphanenin yaratıcılarıyla konuştuğunuzda aslında kütüphanenin dökümante edilmemiş bir çok özelliğiyle karşılaşıyorsunuz. Örneğin, Android'de header tabbar ve ona stil vermekten henüz bahsedilmemiş durumda. Bunu yaratıcıları biz bu özelliği kendi uygulamamızda kullanmıyoruz bir sonraki versiyonlarda bu kısım değişebilir o yüzden dökümante etmedik diyorlar. Bu sizi korkutmasın ama kütüphane oldukça sağlam, stabil durumda ve arkasında güzel güçlü bir kominitesi var. 

## airbnb/native-navigation








# Navigation

React Native'de sayfalar arası geçiş, router için söylenecek çok şey var. Önceki versiyonlarda sunulan direkt react-native kütüphanesinde yer alan Navigator pek sevilmedi. Redux implementasyonu neredeyse imkansızdı. Sonra Navigation-Experimental diye bir şey denendi. Adı üstünde experimental \(deneysel\) bir çalışmaydı ve bana göre başarısız oldu. Daha sonra bu bilgi birikimini toparlayıp, react-native kominitesi tarafından [react-navigation](https://reactnavigation.org/) piyasaya sürüldü. Oldukça kapsamlı ve güzel bir kütüphane oldu. Ancak yine tatmin edici değildi. Normal IOS ve Android uygulamalarda kullanıcı deneyimi sağlamıyordu. Bunun için native bir çözüm daha mantıklı. [wix.com](https://www.wix.com/) da çalışan geliştiricilerde böyle düşünmüş olacaklar ki tamamen native bir kütüphane geliştirmişler, [wix/react-native-navigation](https://github.com/wix/react-native-navigation) .

Bir kaç,  navigation kütüphanesi

* [react-navigation](https://reactnavigation.org/) \( Javascript çözümler için de en iyisi \)

* [react-native-router-flux](https://github.com/aksonov/react-native-router-flux)

* [react-native-router](https://github.com/t4t5/react-native-router)

* [react-native-router-redux](https://github.com/Qwikly/react-native-router-redux)

* [react-native-redux-router](https://github.com/aksonov/react-native-redux-router)

* [react-router](https://reacttraining.com/react-router/native/guides/quick-start) \(Bu web tarafında kullandığımız react-router. V4 ile native özelliği geldi \)

## [wix/react-native-navigation](https://wix.github.io/react-native-navigation/#/installation-ios)

Gerçekten kullanımı diğer kütüphanelere çok daha kolay. Redux entegrasyonu diğerlerine çok çok daha kolay. Tek problem native bir component olduğu için, yükleme sırasında manuel link yapmak durumunda sizi bırakıyor. Dökümantasyonu da kütüphanenin büyüklüğüne göre biraz basit kaçmış gibi. Discord'da kütüphanenin yaratıcılarıyla konuştuğunuzda aslında kütüphanenin dökümante edilmemiş bir çok özelliğiyle karşılaşıyorsunuz. Örneğin, Android'de header tabbar ve ona stil vermekten henüz bahsedilmemiş durumda. Bunu yaratıcıları biz bu özelliği kendi uygulamamızda kullanmıyoruz bir sonraki versiyonlarda bu kısım değişebilir o yüzden dökümante etmedik diyorlar. Bu sizi korkutmasın, kütüphane oldukça sağlam, stable durumda ve arkasında güzel güçlü bir kominitesi var.

## [airbnb/native-navigation](http://airbnb.io/native-navigation/)

2017 React Konferansında airbnb'den [Leland Richardson](https://twitter.com/intelligibabble) yeni bir native navigation component tanıttı, [airbnb/native-navigation](http://airbnb.io/native-navigation/) .React-Native de ki NavigatorIOS ve wix/react-native-navigation'dan sonra 3. native route kütüphanesi. Yalnız şimdilik beta sürümü yayında, airbnb kendi uygulamasında sorunsuz kullanmaya başladıktan sonra ilk versiyonu yayınlayacağını açıkladı. Sunumu izleyince çoğu geliştiriciyi bu haber heyecanlandırdı. Özellikle sayfa geçişlerinde animasyon eklemek hayli zorlayıcı olabiliyor. Bu tip native ve smooth geçişleri bize vaadeden bir kütüphane olacak. Bu kütüphane hazır olana kadar wix/react-native-navigation'ı kullanmanızı öneririm. Daha fazla bilgi ve navigation sorunsalına geniş bir açıdan bakmak için, aşağıdaki sunumu izleyebilirsiniz

{% video %}https://www.youtube.com/watch?v=tWitQoPgs8w&feature=youtu.be&t=631{% endvideo %}



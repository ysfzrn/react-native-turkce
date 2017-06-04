# ANİMASYON

React JS ile uygulama geliştiren insanlardan en çok şikayet edilen konulardan biri animasyon. Ama react native'de pek öyle değil. React Native'in kendi sunduğu Animated API, gerçekten çok etkileyici. Bunu web tarafında kullanmak isterseniz, [animated](https://github.com/animatedjs/animated) veya [react-native-web](https://github.com/necolas/react-native-web) kütüphanesinin içindeki Animated API gibi çeşitli implementasyonlar mevcut. \(react-native-web, react native codebase'i ile web geliştirmek için yazılmış bir kütüphane, belki denemek istersiniz\)

React Native'de animasyonlar için bize 2 tane API sunuluyor.

* LayoutAnimation
* Animated

### [**LayoutAnimation**](/component-apilarcomponent-apilarmd/animasyon/layoutanimation.md)

`LayoutAnimation`, `Animated` API'a göre çok daha az konfigurasyonu olan ve componentinizde ne özellik etkilenmişse animasyon olarak kullanabileceğiniz ama `Animated` API'a göre de sınırlı olan bir API.

Burada göz önünde bulunduracağınız şey bir sonraki render'da ne etkilenecek. Örneğin `width` ve `height` için oldukça kullanışlı oluyor.

`LayoutAnimation` ayrıca native bir çözüm. JavaScript'in içinde yaratılmış bir API değil. O yüzden smooth yani akıcı animasyonlar için kullanışlı.

### [**Animated**](/component-apilarcomponent-apilarmd/animasyon/animated.md)

`Animated` API, `LayoutAnimation'a` göre oldukça fazla konfigurasyonu olan ve karmaşık animasyonları yazacağınız bir API. Ve maalesef JavaScript tarafında çalışan bir API bu da daha çok animasyonlar da takılmalara sebep olabiliyor. Daha akıcı animasyonlar yapmak için takılmalarda `setNativeProps` ile native dünyada değişiklikler yapabilirsiniz.


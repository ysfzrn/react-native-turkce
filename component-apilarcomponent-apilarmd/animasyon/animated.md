# Animated

Anlat anlat bitmeyecek, kesin bir hiyerarşisi olmayan, resmi dökümanda dahi tüm özellikleriyle anlatılmamış en sevdiğim API. Burası benim kendi yorumum. Şimdi üçüncü ağızdan anlatmaya başlayalım. 

Animated API özellikle syntax itibariyle karışık gelebilir. Bu dökümanda tüm özellikleriyle bahsedebilmem çok zor. Sadece buraya sınırlı kalamayacağınız için kendi dökümanı hariç önereceğim kaynaklar aşağıdaki gibi;

_ \(Bahsedilen tüm kaynaklar _[_browniefed_](https://github.com/browniefed)_ mahlaslı arkadaşa ait\)_

* [İngilizce döküman](http://browniefed.com/react-native-animation-book/api/ANIMATED_SETVALUE.html)
* [Temel anlamda Animated API egghead eğitim videoları](https://egghead.io/lessons/react-animated-timing-and-easing-to-animate-styles-of-a-react-native-view)
* [Gerçek case'lere göre advanced animasyon egghead eğitim videoları](https://egghead.io/lessons/react-create-a-horizontal-parallax-scrollview-in-react-native)

Temel felsefe, bir tane animatedValue\(\) yarat. Bu değişkenin değerini değiştir ve componentin style objesine de değiştirdiğin değeri yaz, animatedValue'ya göre animasyon yarat.

1 - animatedValue\(\) yarat

```js
this._animatedValue = new Animated.Value(0);
```

2 - animatedValue\(\) değerini değiştir

```js
this._animatedValue.setValue(150);
```

3 - style objesine set et

```jsx
<Animated.View style={{left: this._animatedValue}} />
```

---

Şimdi [şuradaki](/component-apilarcomponent-apilarmd/animasyon/layoutanimation.md) layoutAnimation örneğini bu kez Animated API ile yapalım.

ilk önce constructor'da bir animatedValue yaratalım

```js
constructor() {
    super();
    this.state = {
      animValue: new Animated.Value(250)
    };
}
```

Sonra bir tane dikdörtgen bir obje yaratalım.Ve bu objemizin height değeri, yarattığımız `animatedValue()`'a göre değişsin. Burada dikkat etmeniz gereken şey, `Animated.View` . Her component animated API'ı kullanabilir. Bunu `createAnimatedComponent()` fonksiyonuyla  sağlayabilirsiniz. Bunun yanında react native sık kullanılan componentler için bazılarını bize hazır olarak Animated API içinde veriyor. Bunlar; **Animated.View**, **Animated.Image**, **Animated.ScrollView** ve **Animated.Text**.

```jsx
renderRectangle = () => {
    const customStyle = {
      height: this.state.animValue,
    };

    return (
      <Animated.View style={[styles.rectangle, customStyle]}>
        <TouchableWithoutFeedback onPress={() => this.handleSelect()}>
          <View style={{ flex: 1 }} />
        </TouchableWithoutFeedback>
      </Animated.View>
    );
  };
```

Dikdörtgen objemize dokununca, height değeri 250 ise 450, 450 ise 250 olsun.

```js
handleSelect = () => {
    this.state.animValue._value > 250
      ? this.state.animValue.setValue(250)
      : this.state.animValue.setValue(450);
};
```

![](/assets/rnn-animated-1.gif)Oley, setState olmadan tekrar tekrar render ettik. Ama bir saniye hiç animasyona benzer bir tarafı yok bunun :\(

Animasyonu sağlamak için bize 3 tane animasyon tipi sunuluyor ve bir çok ihtiyacı karşılıyor daha doğrusu ben karşılamadığı bir case görmedim.Bunlar;

* [`Animated.decay()`](https://facebook.github.io/react-native/docs/animated.html#decay) : Bir başlangıç hız değeri ile başlar, kademe kademe yavaşlayan animasyonlara yarar

* [`Animated.spring()`](https://facebook.github.io/react-native/docs/animated.html#spring): Fizik kurallarına göre animasyonlar hazırlamaya yarar

* [`Animated.timing()`](https://facebook.github.io/react-native/docs/animated.html#timing):Zamana göre animasyon hazırlamaya yarar

Şimdi bu örneğimiz de handleSelect fonksyionunu Animated.timing\(\) yardımı ile değiştirelim.

```js
 handleSelect = () => {
    this.state.animValue._value > 250
      ? Animated.timing(this.state.animValue, {
          toValue: 250,
          duration: 500
        }).start()
      : Animated.timing(this.state.animValue, {
          toValue: 450,
          duration: 500
        }).start();
  };
```

![](/assets/rnn-animated-2.gif)burada kullandığımız this.state.animValue'ya göre sadece height özelliğini animasyon yaptık. Bununla beraber rengini değiştirip biraz da döndürelim.

Bunun için **interPolation** yapacaz. İşin burasını anlamak çok önemli. Mesela aşağıdaki kodu inceleyerek anlamaya çalışalım;

```js
 let rotateAnimation = this.state.animValue.interpolate({
        inputRange: [250, 450],
        outputRange: ['0deg', '360deg']
 });
```

animationValue olan `this.state.animValue` içinden çağırdığımız `interpolate` fonksiyonu içine `inputRange` ve `outputRange` keylerine sahip bir obje alıyor. `inputRange` **250**'den **--&gt;** **450**'ye doğru giderken,  `outputRange`'de **0deg**'den ---&gt; **360deg**'e doğru gidiyor. Bu arada mesela `inputRange` **350** civarrındayken tahminim `outputRange`'de **180deg**'ye yakın oluyordur. Biz araya girip _\(interpolate'in kelime anlamlarından biri araya girmek\)_ `inputRange` **350** iken **720deg** ol sonra **350** ve **450** arasında, kendini ayarla ve **360deg**'e doğru hızla ilerle diyebiliriz.

_Bu kadar basit bir mevzuyu anlatamadıysam lütfen beni uyarın._

| inputRange | outputRange |
| :--- | :--- |
| 250 | 0deg |
| ... | ... |
| 350 | 180deg |
| ... | ... |
| 450 | 360deg |

şimdi yarattığımız rotateAnimation'ı style objesine atayalım.

```js
const customStyle = {
      height: this.state.animValue,
      transform:[{rotate:rotateAnimation}]
};
```

![](/assets/rnn-animated-3.gif)Maalesef gif yapmak için kullandığım tool pek animasyonları doğru yansıtamadı. Kodun tamamını canlı olarak [şuradan ](https://snack.expo.io/rk0oekzGb)izleyebilirsiniz. \(  [https://snack.expo.io/rk0oekzGb](https://snack.expo.io/rk0oekzGb) \)


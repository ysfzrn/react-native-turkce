# Animated

Anlat anlat bitmeyecek, kesin bir hiyerarşisi olmayan, resmi dökümanda dahi tüm özellikleriyle anlatılmamış en sevdiğim API. Burası benim kendi yorumum. Şimdi üçüncü ağızdan anlatmaya başlayalım.

Animated API özellikle syntax itibariyle karışık gelebilir. Bu dökümanda da tüm özellikleriyle bahsedebilmem çok zor. Sadece buraya sınırlı kalamayacağınız için kendi dökümanı hariç önereceğim kaynaklar aşağıdaki gibi; 

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

Sonra bir tane dikdörtgen bir obje yaratalım.Ve bu objemizin height değeri, yarattığımız `animatedValue()`'a göre değişsin. Burada dikkat etmeniz gereken şey, `Animated.View` . Her component animated API'ı kullanabilir. Bunu **`createAnimatedComponent()`** fonksiyonuyla  sağlayabilirsiniz. Bunun yanında react native sık kullanılan componentler için bazılarını bize hazır olarak Animated API içinde veriyor. Bunlar; **Animated.View**, **Animated.Image**, **Animated.ScrollView** ve **Animated.Text**.

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




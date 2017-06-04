# LayoutAnimation

Herkesin basit layoutAnimation'ı anlatırken yaptığı basit bir örnekle başlayalım. Örneğimizde bir tane karemiz var. Üzerine basınca state'e göre height değeri 250 veya 450 oluyor, o kadar.

```jsx
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      selected: false
    };
  }

  handleSelect = () => {
    this.setState({ selected: !this.state.selected });
  };

  renderRectangle = () => {
    const customStyle = {
      height: this.state.selected ? 450 : 250
    };
    return (
      <View style={[styles.rectangleStyle, customStyle]}>
        <TouchableWithoutFeedback onPress={() => this.handleSelect()}>
          <View>
            <Text style={{ color: "white" }}>
              {" "}{this.state.selected ? "Seçildi" : "Seçilmedi"}{" "}
            </Text>
          </View>
        </TouchableWithoutFeedback>
      </View>
    );
  };

  render() {
    const { selected } = this.state;
    return (
      <View style={styles.container}>
        {this.renderRectangle()}
      </View>
    );
  }
}

// define your styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center"
  },
  rectangleStyle: {
    width: 250,
    backgroundColor: "#2c3e50",
    justifyContent: "center",
    alignItems: "center"
  }
});

export default App;
```

![](/assets/rnn-layoutanimation-1.gif)

Şimdi LayoutAnimation'ın magic kısmına gelelim.

Tek yapmamız gereken LayoutAnimation'ı import edip, state'i değiştirdiğimiz yerde LayoutAnimation konfigurasyonunu yapmak.Baştaki kodda handleSelect fonksiyonuna aşağıdaki kodu eklemeniz yeterli.

```js
...
handleSelect = () => {
    LayoutAnimation.configureNext(LayoutAnimation.Presets.spring);
    this.setState({ selected: !this.state.selected });
  };
...
```

![](/assets/rnn-layoutanimation-2.gif)

_\(yukarıdaki gif sizi aldatmasın gayet smooth bir animasyon sadece gif de takılma varmış gibi gözüküyor\)_

Burada `LayoutAnimation.spring();` olarak da ekleyebilirdik. Ama illâ hazır animasyonları kullanmak yerine custom animasyonları da eklemek isterseniz, `configureNext'i` kullanmalısınız. `duration`, `create`, `update` methodlarını kullanabilirsiniz. Hazır fonksiyonlardan `spring`  dışında `easeInEaseOut` ve `linear` var.

`configureNext`'e ikinci parametre olarak bir callback fonksiyonu da geçirebilirsiniz fakat bu durum sadece ios için geçerli.

> Önemli Not: Android'de LayoutAnimation kullanırken mutlaka yapmanız gereken, 2 şey var.
>
> ```jsx
> var UIManager = require('UIManager'); //UIManager'ı import edin
>
> //componentDidMount aşağıdaki configurasyonu yapın
> componentDidMount() {
>       UIManager.setLayoutAnimationEnabledExperimental && 
>       UIManager.setLayoutAnimationEnabledExperimental(true);
> }
> ```

Şimdi şuradaki örneğe bir göz atalım.[^1]

![](/assets/rnn-layoutanimation-3.gif)

Biz de benzer bir örnek yaratalım.

![](/assets/rnn-layoutanimation-4.gif)

Burada tek yaptığımız baştaki koddaki Rectangle'ı component haline getirip, ScrollView içinde map edip, container componentte ki item'ları flexBox kullanarak wrap etmek  Animasyonu sağlamak için de LayoutAnimation satırının commentini açıp son haline bakalım.

![](/assets/rnn-layoutanimation-5.gif)

Tabi ki benim yaptığım gif sizi yanıltmasın aslında gayet smooth bir animasyon elde ettik burada. Zaten performans monitorunde UI ve JS tarafınd 60 fps nin altına düşmediğini görebilirsiniz.

Kodun tamamı için şuraya bakabilirsiniz. 

[https://gist.github.com/ysfzrn/e540a072e639656269e22e53f9b7da2d](https://gist.github.com/ysfzrn/e540a072e639656269e22e53f9b7da2d)

---

Kaynak

[https://dribbble.com/shots/1279908-UI-Animation-experiment-1](https://dribbble.com/shots/1279908-UI-Animation-experiment-1)[^1]


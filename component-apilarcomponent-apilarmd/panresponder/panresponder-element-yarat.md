### 2. PanResponder Element Yaratalım

Toplamda bizim 9 tane topumuz olacak. Bunların kendine özgü renkleri olacak. Ve sürüklenip bırakılan top bir daha sürüklenmeyecek. Bu verilere dayanarak kendimize yine state'te tutacağımız bir array yaratalım.

```js
balls:[
        { id: 1, value: "R", color: SharedStyle.color.red, selected: false },
        { id: 2, value: "R", color: SharedStyle.color.red, selected: false },
        { id: 3, value: "R", color: SharedStyle.color.red, selected: false },
        { id: 4, value: "Y", color: SharedStyle.color.yellow, selected: false },
        { id: 5, value: "Y", color: SharedStyle.color.yellow, selected: false },
        { id: 6, value: "Y", color: SharedStyle.color.yellow, selected: false },
        { id: 7, value: "B", color: SharedStyle.color.blue, selected: false },
        { id: 8, value: "B", color: SharedStyle.color.blue, selected: false },
        { id: 9, value: "B", color: SharedStyle.color.blue, selected: false }
]
```

Bu array'i görselleştireceğimiz bir ball.js isimli bir component yaratalım sonra da ona drag&drop özellikleri ekleyelim.

```jsx
// ./src/components/Ball.js
import React, { Component } from "react";
import { View, Text, StyleSheet,PanResponder } from "react-native";

// create a component
class Ball extends Component {

  componentWillMount() {
    this.panResponder = PanResponder.create({
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
    })
  }


  render() {
    return <View style={styles.container}  {...this.panResponder.panHandlers}/>;
  }
}
```

Başlangıçta componentimiz touch eventlere tepki versin ve touch eventlerle hareket edebilmesi için `onStartShouldSetPanResponder` ve `onMoveShouldSetPanResponder` fonksiyonlarının true döndürmesini sağladık. Bu fonksiyonlar tetiklendikten sonra iki tane daha fonksiyon tetiklenecektir. Bunlardan birincisi başlangıç değerlerini set edebileceğimiz, `onPanResponderGrant`;  parmağımız componenti sürüklemeye başlayınca ekranda dokunduğumuz yerlerin coordinatlarını almamızı sağlayan `onPanResponderMove`.

> Önemli: Biz PanResponer API'ı kullanırken elementin değerlerini filan almıyoruz. Biz ekranda dokunduğumuz yerlerin koordinat değerlerini alıp, elementin style'ını değiştiriyoruz.

Şimdi başlangıç değerlerimizi set edelim. Burada Animated API kullanacağız. bunun için ilk önce constructor'da  bir Animated state yaratalım.

```jsx
// ./src/components/Ball.js
...
class Ball extends Component {
   constructor(props) {
     super(props);
     this.state = {
       pan: new Animated.ValueXY()
     };
 } 
 ...
```

Şimdi onResponderGrant'da bu state'imize değer verelim.

```jsx
// ./src/components/Ball.js
...
 componentWillMount() {
    this.panResponder = PanResponder.create({
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
      onPanResponderGrant: (e, gestureState) => {
        this.state.pan.setValue({ x: 0, y: 0 });
      },
    })
  }
 ...
```

Component'i tutup elimizle sürükleyince tetiklenecek olan `onPanResponderMove`için bir fonksiyon atayalım.

```jsx
// ./src/components/Ball.js
...
  componentWillMount() {
    this.panResponder = PanResponder.create({
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
      onPanResponderGrant: (e, gestureState) => {
        this.state.pan.setValue({ x: 0, y: 0 });
      },
      onPanResponderMove: this.handleResponderMove,
    })
  }

  handleResponderMove = (evt, gestureState) => {
    this.state.pan.setValue({ x: gestureState.dx, y: gestureState.dy });
  };}
 ...
```

this.state.pan değiştikçe değişecek olan style yapıp, View'den oluşan ball componentini Animated.View'e çevirelim.

```jsx
// ./src/components/Ball.js
...  render() {
    const animatedStyle = {
      transform: this.state.pan.getTranslateTransform(),
      backgroundColor: "red"
    };

    return (
      <Animated.View
        style={[styles.container, animatedStyle]}
        {...this.panResponder.panHandlers}
      />
    );
  }
 ...
```

> getTranslateTranform, tranform:\[{translateX: value }, {translateY:value}  \] işini yapan Animated value ile kullanılabilen hazır bir fonksiyon

Şimdi örnek tek bir Ball componentini app.js' e import edip neler oluyor bir bakalım.

![](/assets/digdagdoe2.gif)

Yine bir şeyler ters gidiyor. İlk defa sürüklerken bir sorun olmuyor fakat ikinci kez topa dokunduğumuzda topun pozisyonu uzaklara kaçıveriyor. Bunu çözmek için 2 tane global değişken yaratacağız ve bu iki değişkeni dinleyen listenerlarımız olacak. Topu son bıraktığımız yerin değerini kendilerinde tutup,  ikinci kez dokunduğumuzda tekrar tetiklenen, **onPanResponderGrant** methodunda bunlarda tuttuğumuz değerleri **this.state.pan** state'ine atacağız. O da dolayısıyla elementin style'ını update edecek.

_Listenerları componentWillUnmount'da remove etmeyi unutmayın_

![](/assets/digdagdoe3.gif)


# REACT NATIVE İLE YILAN OYUNU  \( PART 3 \)

Üçüncü bölümün source kodlarına şuradaki branch'den erişebilirsiniz:[ ](https://github.com/ysfzrn/crazysnake/tree/firstpart)[https://github.com/ysfzrn/crazysnake/tree/thirdpart](https://github.com/ysfzrn/crazysnake/tree/thirdpart)![](/assets/Screen Shot 2017-10-01 at 14.25.18.png)

İlk olarak gameScreen ekranını registerScreen.js içinde register edelim.

```js
import { Navigation } from "react-native-navigation";
import HomeScreen from './screens/home';
import GameScreen from './screens/gameScreen';
import Store from './stores'
import Provider from './Provider'

export function registerScreens() {
  Navigation.registerComponent("crazySnake.HomeScreen", () => HomeScreen,Store,Provider);
  Navigation.registerComponent("crazySnake.GameScreen", () => GameScreen,Store,Provider);
}
```

Daha sonra root.js içinde gameScreen için durum yazalım.

```js
import { Navigation } from "react-native-navigation";
import { reaction } from "mobx";
import { registerScreens } from "./registerScreens";
import { observer } from "mobx-react/native";
import Store from "./stores";

registerScreens();

export default class App {
  constructor() {
    reaction(() => Store.nav.route, () => this.startApp(Store.nav.route));
    Store.nav.appInitialized();
  }

  startApp(root) {
    switch (root) {
      case "root":
        Navigation.startSingleScreenApp({
          screen: {
            screen: "crazySnake.HomeScreen",
            title: "HOME"
          }
        });
        return;
      case "gameScreen":
        Navigation.startSingleScreenApp({
          screen: {
            screen: "snakeGame.GameScreen",
            title: "GameScreen"
          }
        });
        return;
    }
  }
}
```

Aşağıdaki gibi board'u oluşuralım.

![](/assets/Screen Shot 2017-10-02 at 03.00.13.png)gameStore'a dönelim ve oyunun nasıl state'lere ihtiyacı olduğuna bakalım.

```js
"use strict";
import mobx, { observable, action } from "mobx";
import { AsyncStorage } from "react-native";

const segmentRate = 10; // yılanın her hareketinde katedeceği mesafe

class GameStore {
  @observable highScore = 0;  //yapılan en yüksek skoru tutar
  @observable score = 0;        // yiyilen elma sayısı
  @observable intervalRate = 15; //yılanın hızı
  @observable currentDirection = "right"; // Yılanın yönü, alacağı değerler: left / right / up / down 
  @observable lastSegment = 10; // her segmentin kendinden önce takip edeceği, segment
  @observable                   // yılanı oluşturan array ve başlangıç koordinatları
  snake = [
    { id: 1, x: 20, y: 0 },
    { id: 2, x: 10, y: 0 },
    { id: 3, x: 0, y: 0 }
  ];


  ...
}

export default new GameStore();
```

Yılan her bir parçası Segment.js olarak isimlendirdiğim, component'ten oluşacak. Segment component'ine bakalım. Aşağıda görüldüğü bir absolute position'a sahip bir component. Ve dışarıdan left ve top, propslarını alıyor. Yani üstte verdiğimiz yılan başlangıç koordinatlarına göre yılanımız şekil alıyor. Herbir segment sharedStyle'dan gelen 10px lik, yükseklik ve genişliğe sahip. Böylece gameStore'da bulunan segmentRate'e göre uygun bir hareket sergileyecek.

```jsx
//src/components/segment.js
import React, { Component } from "react";
import { View, Text, StyleSheet } from "react-native";
import SharedStyle from "../utils/sharedStyle";

// create a component
class Segment extends Component {
  render() {
    const customStyle = {
      left: this.props.x,
      top: this.props.y
    };
    return <View style={[styles.container, customStyle]} />;
  }
}

// define your styles
const styles = StyleSheet.create({
  container: {
    position: "absolute",
    width: SharedStyle.segment.width,
    height: SharedStyle.segment.height,
    backgroundColor: SharedStyle.color.snake,
    borderWidth: SharedStyle.segment.borderWidth,
    borderColor: SharedStyle.segment.borderColor
  }
});

//make this component available to the app
export default Segment;
```

Şimdi yılanımızı, gameStore'daki snake array'ini map ederek board'umuzun içine koyalım.

![](/assets/Screen Shot 2017-10-02 at 03.19.41.png)

### Peki bu yılan nasıl hareket edecek ?

React Native'de oyun yazmak gerçek anlamda bir challenge olabilir. Çünkü burada biz durmadan render edilecek bir yılanın segmentlerinden bahsediyoruz. Bu yılan büyüdüğünde mesela 100 segment'lik bir boya sahip olduğunda her bir milisaniyede her bir segmentin bir birim hareket etmesi zaten 100 defa render edilmesi demek. Bunu durmaksızın tüm board boyunca sonsuz kere tekrar edebilmesi, react native'in bridge yapısına ters gelebilir. Bu soru benim de aklımı bir hayli meşgul etti ve ilk denemem olan yılanı setInterval ile yapmam ve yılan hareket ettikçe yavaşlaması uygulamanın performansının yerlere düşmesine sebep oldu. Sonra başka hangi timer'ı kullanırım diye düşünürken, **requestAnimationFrame** imdadıma yetişti.  requestAnimationFrame, saniyede 1000 kez bir kodu baştan aşağı çalıştırabiliyor.

Yılanımız başlangıç olarak sağa doğru hareket ediyor. Yani snake array'in içindeki ilk elemanın x koordinat +10 olarak artacak ve bir sonraki segment kendinden önceki segmentin koordinat değerlerine sahip olacak. Algoritmanın özeti bu. Şimdi gameStore'da ilgili action methodunu oluşturalım.

```js
//src/stores/gameStore.js

const segmentRate = 10; // yılanın her hareketinde katedeceği mesafe
const boardWidth = SharedStyle.board.width;
const BoardHeight = SharedStyle.board.height - 10;
let globalID;

...
 @action("Snake is moving")
  handleMoveSnake =() => {
    let temp = _.cloneDeep(this.snake.slice());  
      this.lastSegment = temp[0];
      for (let i = 0; i < temp.length; i++) {
        if (i !== 0) {
          this.lastSegment = temp[i - 1];
        }

        if (this.currentDirection === "right") {
          if (i === 0) {
            if (this.snake[i].x + segmentRate >= boardWidth ) {
              this.snake[i].x = 0;
            }else{
              this.snake[i].x = this.snake[i].x + segmentRate;
            }
          } else {
            this.snake[i].x = this.lastSegment.x;
            this.snake[i].y = this.lastSegment.y;
          }
        } 
      }

      globalID = setTimeout(()=>{
        requestAnimationFrame(this.handleMoveSnake);
       }, 1000 / this.intervalRate);
  }
  ...
```

Buradaki kodu üste yazdığımız paragrafa göre inceleyelim.

İlk önce var olan snake array'inin bir kopyasını oluşturdum. Javascript'in , çoklu levelli  array'lerde eşitlemede referans verip, asıl array'i değiştirmemesi için lodash'ın cloneDeep methodunu kullandım.

```js
let temp = _.cloneDeep(this.snake.slice());
```

for döngüsü ile tüm array'i dönüyoruz. Eğer array'in ilk elemanı, yani yılanımızın başı değilse segment, yılanın yönü farketmeksizin kendisinden bir önceki segmentin değerlerini almasını istiyoruz. lastSegment değerini bu yüzden tutuyoruz.

```js
if (i !== 0) { 
  this.lastSegment = temp[i - 1];
}
```

Yılanın yönü, sağa veya sola doğru ise sadece x koordinat düzleminde işlem yapacağız demek. Sağa giderken yılanın başı, segmentRate kadar ilerleyecek. Eğer board'un sonuna geldiyse, board'un tekrar başına gidecek. Ve diğer segmentler de kendinden önceki segmentin değerlerini alacak.

```js
if (this.currentDirection === "right") {
          if (i === 0) {
            if (this.snake[i].x + segmentRate >= boardWidth ) {
              this.snake[i].x = 0;
            }else{
              this.snake[i].x = this.snake[i].x + segmentRate;
            }
          } else {
            this.snake[i].x = this.lastSegment.x;
            this.snake[i].y = this.lastSegment.y;
          }
}
```

Bu işlemleri sürekli tekrar etmemizi sağlayacak kod parçacığına gelelim. requestAnimationFrame, saniyede 1000 kez frame geçişi yapabiliyor. Yılanımızın hızını kontrol etmek için requestAnimationFrame'i setTimeout ile beraber kullanıyoruz. Böylece frame geçiş hızını ayarlayabiliyoruz.

```js
...
globalID = setTimeout(()=>{
        requestAnimationFrame(this.handleMoveSnake);
}, 1000 / this.intervalRate);
```

Yılanın yönü sağa doğru iken yaptığımız işlemleri, sola, yukarı, aşağı olarak da aynı mantıkla if koşulları ekleyip çoğaltıyoruz. Kalabalık etmesin diye tekrar eden işlemleri buraya yazmak istemiyorum.

Daha sonra gameScreen ekranının componentDidMount methodua aşağıdaki kod parçacığını ekleyelim. Yılanın hareket ettiğini görelim. \( Tabi burada .gif almak için kullandığım tool'da tam bir smooth animation göremeyebilirsiniz, ama nasıl performanslı çalıştığını performans monitor'de görebilirsiniz. \)

```js
...
componentDidMount() {
    const { gameStore } = this.props;
    gameStore.handleMoveSnake();
}
...
```

![](/assets/snaketest2.gif)

Şimdi yön kontrollerini ekleyelim. Yön kontrollerinin mantığı, ilk bölümde belirttiğimiz gibi eski Nokia telefonlarda 3 ve 7  tuşlarıyla oynar gibi olacak. Sağdaki yön tuşu sağa ve yukarı götürecek, soldaki yön tuşu sola ve aşağı götürecek. Bunun için gameStore'a 2 tane daha action ekleyelim, handleRightButton ve handleLeftButton.![](/assets/Screen Shot 2017-10-02 at 04.28.18.png)Daha sonra gameScreen ekranına da butonları ekleyelim.

![](/assets/snaketest3.gif)

Yılan, özgürce hareket etmeye başladı. Şimdi bu yılanı doyurmak için board'da random şekilde belirecek olan elma componentini yaratalım. Bunun için gameStore'da aşağıdaki işlemleri yapalım. elma board içinde segmentRate'in katları \( yani 10'nun katları \) koordinatlarda belirmesi lazım ki, yılanımız elmayı yiyebilsin.

```js
// src/stores/gameStore.js
class GameStore {
  ...
  @observable food = { x: 50, y: 50 }; //elmanın başlangıçta belireceği yer.
  ...
  @action("make food")
  handleMakeFood() {
    const frameX = (boardWidth -10) / segmentRate;
    const frameY = BoardHeight / segmentRate;

    this.food.x = this.getRandomInt(0, frameX) * segmentRate;
    this.food.y = this.getRandomInt(0, frameY) * segmentRate;
  }

  getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }
```

Elma componentini de gameScreen ekranında board içine koyalım.

```jsx
       ...
       <Board>
         {snake.map((segment, i) => {
            return <Segment key={segment.id} id={segment.id} x={segment.x} y={segment.y} />;
          })}
          <Food x={ gameStore.food.x } y={ gameStore.food.y }/>
        </Board>  
       ...
```

Yılan elmayı yedi mi kontrolü ekleyelim. Bu methodu yılan her bir segment ilerleyişinde çağırıp kontrol edelim. Yılan her elmayı yediğinde handleMakeFood methodunu çağırıp, yeni bir elma oluşturalım ve score state'ini +1 arttıralım ve snake array'ini bir tane daha eleman ekleyelim. Son olarak da yılan her 3 elma yediğinde hızını arttıralım. Aşağıdaki method bunları yapacak.

```js
@action("Did the snake eat the food ?")
  handleEatFood() {
    if (this.snake[0].x === this.food.x && this.snake[0].y === this.food.y) {
      this.score = this.score + 1;
      this.snake.push({
        id: this.snake[this.snake.length - 1].id + 1,
        x: this.snake[this.snake.length - 1].x,
        y: this.snake[this.snake.length - 1].y
      });
      if( this.score % 3 === 0 ){
       this.intervalRate = this.intervalRate + 5;
      }

      this.handleMakeFood();
    }
  }
```

![](/assets/snaketest4.gif)

Şimdi yılan kendi kuyruğuna çarptığında oyunun bitmesini sağlayalım. Bunun için handleMoveSnake action methoduna bir kontrol daha ekleyelim. Burada da yapacağımız yılanın başı yani ilk segment, yılanın başka herhangi segmentiyle aynı koordinat değerlerine sahip olup olmadığını kontrol etmek. Eğer kuyruğuna çarpmışsa,  requestAnimationFrame'i, cancelAnimationFrame ile sonlandıracağız. Elde edilen score, var olan en yüksek skordan daha büyük ise telefon hafızasına AsyncStorage ile yeni skoru set edeceğiz. Ve `NavigationStore.handleChangeRoute('gameOverScreen');` ile de gameOver ekranına yönlendirme yapacağız.

> Burada dikkat etmenizi istediğim 2 tane husus var.
>
> 1. cancelAnimationFrame, requestAnimationFrame'i durdurması için mutlaka sonunda return yapmanız gerekmektedir. Eğer yapmazsanız kod aşağı doğru derlenmeye devam edecektir.
> 2. MobX'de store'ları birbiri içinde import edip , birbirlerinin methodlarını çağırmasını sağlayabilirsiniz.

```js
//src/stores/gameStore.js
...
class GameStore{
   ...
  @action("Snake is moving")
  handleMoveSnake =() => {
      ...
     //isGameOver control
      for(let i = 1; i < this.snake.slice().length; i++){
      if(this.snake[0].x === this.snake[i].x && this.snake[0].y === this.snake[i].y ){
          if( this.score > this.highScore ){
            AsyncStorage.setItem('snakeHighScore', JSON.stringify(this.score));
          }
          cancelAnimationFrame(this.handleMoveSnake);
          NavigationStore.handleChangeRoute('gameOverScreen');
          clearTimeout(globalID);
          return;
      }
    }

  globalID = setTimeout(()=>{
        requestAnimationFrame(this.handleMoveSnake);
       }, 1000 / this.intervalRate);
  }   

}
```

En son ekranımızın üzerinden de kısa bir şekilde geçelim. Sayfanın başındaki tasarıma bakarsanız, gameOver ekranında oyunu yeniden başlatacak bir tane buton var. Burada yapacağımız tek şey, NavigationStore.handleChangeRoute\('gameScreen'\); ile ekranı tekrar gameScreen'e çekmek ve başlangıçtaki state'lere geri dönmek. Aşağıdaki gibi;

```js
//src/stores/gameStore.js
...
class GameStore{
   ...
  @action("restart handler")
  handleRestart(){
    NavigationStore.handleChangeRoute('gameScreen');
    this.intervalRate = 5;
    this.currentDirection = "right";
    this.lastSegment = 10;
    this.rightButtonText = "up";
    this.leftButtonText = "down";
    this.score = 0;
    this.highScore = 0;
    this.food = { x: 50, y: 50 };
    this.snake = [
      { id: 1, x: 20, y: 0 },
      { id: 2, x: 10, y: 0 },
      { id: 3, x: 0, y: 0 }
    ];
  }}   

}
```

Hepsi bu kadar. Bir sonraki bölümde bu oyunun Google Play Store'a ve AppStore'a deploy edilmesinden ve aşamalarından bahsedeceğiz.


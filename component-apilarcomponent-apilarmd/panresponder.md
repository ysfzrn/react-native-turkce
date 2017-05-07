# PANRESPONDER

PanResponder API ile React Native'de touch\(dokunma\) eventleri, PanResponder'a ait lifecycle methodlar ile düzenleyip, drag and drop işlemlerini gerçekleştirebiliriz.

```js
  componentWillMount() {
    this._panResponder = PanResponder.create({
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,

      onPanResponderGrant: (evt, gestureState) => this.handleResponderGrant,
      onPanResponderMove: this.handleResponderMove,
      onPanResponderRelease: this.handleResponderRelease
    });
  },

  render: function() {
    return (
      <View {...this._panResponder.panHandlers} />
    );
  },
```

Bir elemente drag-drop özelliği eklemek istediğimizde,  yukarıdaki kod parçacığı sabit bir yaklaşım direkt bunu kullanabiliriz. Hatta kullanığınız editörde bir snippet kısayolu ayarlamanız size pratiklik sağlayacaktır. \( Bu arada react-native'in gesture sisteminde sadece bu methodlar yok. Ben buraya en çok kullanılanlarını ve aşağıdaki örnekte kullanacaklarımızı yazdım\)

Şimdi bu API'ı bir örnekle açıklamak istiyorum. Bizim bir görevimiz olsun. 3x3 kare board ve altında 3 kırmızı, 3 mavi, 3 sarı topumuz olsun. Aşağıdaki topları sürükleyip, board'a bırakalım.

1. Sürükleyip bıraktığımız kutu boş ise , topun rengini alsın.
2. Sürükleyip bıraktığımız kutu dolu ise, top tekrar yerine gitsin
3. Bütün board kareleri dolduğunda yan yana olan tüm renkler aynı ise Kazandınız, değil ise Kaybettiniz diye bir Alert çıkarsın.

![](/assets/digdagdoe.gif)

Görevimizi anladıysak şimdi yapılacakları adım adım sıralayalım.

1. Board çizelim. Board'ın koordinatlarını ve özelliklerini state'te tutalım.
2. Bir PanResponder Element Yaratım. 
3. Hover effect ekleyelim
4. Sürüklediğimiz componenti drop ettikten sonraki kontrolleri ekleyelim.

### 1.BOARD

Bizden istenen 3x3 bir board. Bu board'daki her bir karede sayfada kendine ait koordinatlarını, içinin boş olup olmadığını, üstünde bir componentin sürüklenip sürüklenmediğini bilecek. Bunun için bir başlangıç objesi set edelim.

```js
   const initObj = {
      x: 0,
      y: 0,
      hovering: false,
      filled: false,
      color: MyStyle.color.white,
      value: null
    };
```

Uygulamamız açılır açılmaz board'ı çizmek için componentDidMount methodunda 3x3 lük board bilgisini tutacak bir array oluşturalım. Bu array'ın her bir elemanı az önceki yarattığımız, **initObj** objesinden oluşsun. Oluşturduğumuz geçici array'ı state'e set edelim. Bunu aşağıda **digHole **isimli fonksiyonda topladım. Bunu componentDidMount methodunda çağıralım.

```js
//  ./src/app.js

digHole = () => {
    const initObj = {
      x: 0,
      y: 0,
      hovering: false,
      filled: false,
      color: '#FFFFFF'
      value: null
    };
    let _holes = [];

    for (let i = 0; i < 9; i++) {
      _holes.push(initObj);
    }
    this.setState({ holes: _holes });
  };
```

Güzel artık elimde görselleştirebileceğim bir data'm var. Ama ondan önce bizim componentlerimiz arası width, height, konum ilişkisi oldukça fazla olacak. Bu yüzden bir tane componentlerin bu değerlerini bir araya toplamak adına bir kendimize ait customStyle objesini bir javascript dosyasında tutalım ve kullanacağımız yerlerde import edelim. Bunun için **src **klasörümüzde bir tane util klasörü yaratalım içine de **SharedStyle.js **isimli dosya ekleyelim. \(initObj objesindeki color değerini de bu style'a göre değiştirelim\)

```js
// ./src/util/SharedStyle.js
const SharedStyle ={
  text:{
      small:12,
      regular:18,
      large:24,
      xlarge:32
  },
  color:{
      primary:'#673AB7',
      primaryBlack:'#311B92',
      secondary:'#009688',
      secondaryBlack:'#004D40',
      white:'#FFFFFF',
      disable:'gray',
      red:'#f44336',
      yellow:'#2196F3',
      blue:'#ffeb3b'
  },
  hole:{
      width:80,
      height:80,
      borderWidth:1
  },
  header:{
      height:64,
      marginBottom:5
  }
}

export default SharedStyle
```

Tekrar app.js' e dönelim. Bir adet header componenti ve Board'umuz için bir boardContainer yaratalım.boardContainer style'a dikkat edin. SharedStyle'da verdiğimiz **width** ve **borderWidth** değerlerin 3 katının toplamını verdik ki boardContainer'ımız tam anlamıyla karelerimiz için kapsayıcı olsun.

![](/assets/Screen Shot 2017-05-07 at 02.32.12.png)

Önce bir tane **Hole.js** isimli bir component yapıp holes state'inde tuttuğumuz datayı görselleştirmeye başlayalım.

```jsx
// ./src/components/hole.js

import React, { Component } from "react";
import { View, Text, StyleSheet } from "react-native";
import SharedStyle from "../util/SharedStyle";

// create a component
class Hole extends Component {

  render() {
    const { hole } = this.props;

    return (
      <View style={styles.container}  />
    );
  }
}

// define your styles
const styles = StyleSheet.create({
  container: {
    alignItems: "center",
    justifyContent: "center",
    width: SharedStyle.hole.width,
    height: SharedStyle.hole.height,
    backgroundColor: SharedStyle.color.white,
    borderWidth: SharedStyle.hole.borderWidth,
    borderColor: SharedStyle.color.primaryBlack
  }
});

//make this component available to the app
export default Hole;
```

![](/assets/Screen Shot 2017-05-07 at 02.44.57.png)

**boardContainer tam bir kapsayıcı olmuşa benzemiyor. Bunun için boardContainer'a, flexWrap i eklememiz gerekiyor.**

![](/assets/Screen Shot 2017-05-07 at 02.46.45.png)

Sıra geldi karelerimizin sayfadaki pozisyonlarını bulmaya. Bunun için react-native'de birkaç yöntem var. Ama en pratiği her element yaratıldığında tetiklenen **onLayout** methodu.

![](/assets/Screen Shot 2017-05-07 at 02.52.52.png)

Bir şeyler ters gidiyor. onLayout methodunu çağırdık koordinatları aldık ama bu koordinatlar hole componentin kendi üstündeki View'e göre  boardContainer'a göre değerleri. Bize sayfaya göre olanı lazım. Bunun için bizim boardContainer'ın koordinatlarına da ihtiyacımız var. app.js'e dönüp boardContainer'ın koordinatlarını alıp, hole componentin her birine bunları gönderelim. onLayout merhodu component mount edildikten sonra çağrıldığı için bir render koşulu ekleyelim. mainx ve mainy belli değil ise ekranda bir loader gösterelim.

```jsx
//..src/app.js
...
  handleLayout = e => {
    const { x, y } = e.nativeEvent.layout;
    this.setState({ mainx: x, mainy: y });
  };

  render() {
    const { holes, mainx, mainy } = this.state;
    return (
      <View style={styles.container}>
        <Header />
        <View style={styles.boardContainer} onLayout={this.handleLayout}>
          {mainx !== null && mainy !== null
            ? holes.map((hole, i) => {
                return <Hole key={i} mainx={mainx} mainy={mainy} />;
              })
            : <ActivityIndicator />}
        </View>
      </View>
    );
  }
  ...
```

Şimdi hole.js componentimizde koordinat bulma işlemine mainx ve mainy props'larını da katalım. Aşağıdaki koordinatlar gayet doğru gözüküyor.

![](/assets/Screen Shot 2017-05-07 at 03.50.29.png)

Yukarıda karelere ait array'i yaratırken bir başlangıç objesi set etmiştik ve onda pozisyon değereri {x:0, y:0} şeklindeydi. Şimdi o state'deki array'i değiştirelim.Bunun için hole.js içinde handleLayout methodun da üstte kullanılmak üzere bir function props fırlatalım.

```jsx
 // ./src/components/hole.js

 handleLayout = e => {
    const {mainx, mainy,index } = this.props
    const { x, y } = e.nativeEvent.layout;
    this.setState({ x: mainx+x, y: mainy+y });
    this.props.onHoleLayout(mainx + x, mainy + y, index);
  };
```

Object spread yardımı ile holeLayout methodunda state'imizi güncelleyelim.![](/assets/Screen Shot 2017-05-07 at 04.00.26.png)

### 

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

Yine bir şeyler ters gidiyor. İlk defa sürüklerken bir sorun olmuyor fakat ikinci kez topa dokunduğumuzda topun pozisyonu uzaklara kaçıveriyor. Bunu çözmek için 2 tane global değişken yaratacağız **\(this.\_animatedValueX, this.animatedValueY\) ** ve bu iki değişkeni dinleyen listenerlarımız olacak. Topu son bıraktığımız yerin değerini kendilerinde tutup,  ikinci kez dokunduğumuzda tekrar tetiklenen, **onPanResponderGrant** methodunda bunlarda tuttuğumuz değerleri **this.state.pan** state'ine atacağız. O da dolayısıyla elementin style'ını update edecek.

_Listenerları componentWillUnmount'da remove etmeyi unutmayın_

![](/assets/digdagdoe3.gif)

Şimdi tüm toplarımızı map edelim

```jsx
// ./src/app.js
...
{balls.map((item, i) => {
            return <Ball key={i} ball={item} />;
})}
...
```

![](/assets/Screen Shot 2017-05-07 at 05.23.26.png)

### 3-Hover Effect Ekleme

PanResponder sisteminde `onPanResponderMove` ile ekranın neresinde sürüklenme işlemi oluyor anlayabiliyorduk. Hover effect yaparken de bunu kullanacağız. `onPanResponderMove` aktif iken gerekli değerleri alıp, üst component'e function props fırlatıp, effect oluşma işlemini de üst component'e halledeceğiz.

```jsx
   // ./src/components/ball.js 
  ...
 handleResponderMove = (evt, gestureState) => {
    this.state.pan.setValue({ x: gestureState.dx, y: gestureState.dy });
    this.props.onPositionChanging(evt.nativeEvent.pageX, evt.nativeEvent.pageY);
 };
 ...
```

```jsx
// ./src/app.js
...
{balls.map((item, i) => {
            return (
              <Ball
                key={i}
                ball={item}
                onPositionChanging={this.handlePositionChanging}
              />
            );
})}
...
```

nativeEvent ve gestureState kavramları için;  Panresponder sisteminde herbir method bu iki değişkeni bize otomatik verir. Bu iki değişken \(evt.nativeEvent ve gestureState\) aslında bir obje ve biz burada dokunduğumuz yerin nativeEvent içerisinde sayfaya göre konumunu pageX ve pageY değişkenlerinden alıyoruz. Daha fazla ayrıntı için resmi dökümanda [şuraya](https://facebook.github.io/react-native/docs/panresponder.html) bakabilirsiniz

Şimdi bir kaç util fonksiyonu yazalım. Bunlardan biri ben bir karenin üstünde miyim değil miyim onu söyleyecek olan isDropZone fonksiyonu diğeri üstündeyken holes state'ini değiştirecek olan hoverDropZone fonksiyonu. İşin logic tarafına girmiyorum. Çünkü gayet koddan okunabilir diye düşünüyorum.

```js
// ./src/util/index.js

export const isDropZone = (hole, itemX, itemY, width, height, headerHeight) => {
  if (
    !hole.filled &&
    (hole.x <= itemX && hole.x + width >= itemX) &&
    (hole.y+headerHeight < itemY && hole.y + height+headerHeight > itemY)
  ) {
    return true;
  } else {
    return false;
  }
};

export const hoverDropZone = (holes, itemX, itemY, width, height,headerHeight) => {
  for (let i = 0; i < holes.length; i++) {
    if (isDropZone(holes[i], itemX, itemY, width, height,headerHeight)) {
        holes[i].hovering = true;
    } else {
        holes[i].hovering = false;
    }
  }

  return holes;
};
```

Burada ayrıntı belki de algoritmayı kurarken yaptığım bir hata, sayfadaki header boyutuna da ihtiyacım var. İşin açıkçası karelerin koordinatlarını hesaplarken onu da içine katıp bu parametreye burada ihtiyaç duymayabilirdik. Ama sorun değil.

Şimdi app.js'de handlePositionChanging methodunda hoverDropZone'u çağıralım. Ordan dönen holes array'i ile state'i güncelleyelim.

```js
// ./src/app.js
...
 handlePositionChanging = (itemX, itemY) => {
    let _holes = this.state.holes;

    _holes = hoverDropZone(
      _holes,
      itemX,
      itemY,
      SharedStyle.hole.width,
      SharedStyle.hole.height,
      SharedStyle.header.height
    );
    this.setState({ holes: _holes });
  };
 ...
```

Şimdi this.state.holes state'i sorumlu olduğu kareye kendi üzerinde neler döndüğünü anlatabilir. Kareye ona göre style ekleyelim.

![](/assets/digdagdoe4.gif)

### 4-Drop - Release - Bırakma İşlemi

Tekrar PanResponder componentimize dönüp yeni bir method ekliyoruz, `onPanResponderRelease:this.handleRelease` methodu.  Bu method ne yapacak ? Topun bırakıldığı koordinatları ve hangi topun bırakıldığını üst componente haber verecek.

```jsx
// ./src/components/ball.js
...
  handleRelease = (evt, gestureState) => {
    const {ball } = this.props;
    this.state.pan.flattenOffset();
    this.props.onDrop(ball, evt.nativeEvent.pageX, evt.nativeEvent.pageY);
  };
...  
```

Üst component'te onDrop props'unu karşılayalım

```jsx
// ./src/app.js
  ...
{balls.map((item, i) => {
            return (
              <Ball
                key={i}
                ball={item}
                onPositionChanging={this.handlePositionChanging}
                onDrop={this.handleDrop}
              />
            );
  })}
  ...
```

Burada bir util fonksiyon daha yazalım, selectDropZone diye. Buda bizim balls ve holes array'imiz değiştirsin. Ve bizde o değiştirdiği array'ler ile state'lerimizi güncelleyelim.

```js
// ./src/util.js

export const selectDropZone = (holes, itemX, itemY, width, height,balls, ball,headerHeight) => {
  for (let i = 0; i < holes.length; i++) {
    if (isDropZone(holes[i], itemX, itemY, width, height,headerHeight)) {
      
      holes[i].hovering = false;
      holes[i].filled = true;
      holes[i].color = ball.color;
      holes[i].value = ball.value;
    
      balls = selectBall(balls, ball);
    }
  }

  return {holes, balls};
};


export const selectBall=(balls ,ball)=>{
  for (let i = 0; i < balls.length; i++) {
          if (balls[i].id === ball.id) {
               balls[i].selected = true;
          }
  }

  return balls
}
```

Yukarıdaki algoritma çok basit. for döngüsü ile ilk önce hangi karenin üstünde olduğuna karar veriyor. Sonra o karenin renk ve ilgili değerlerini değiştiriyor sonra da, hangi topun seçildiğine karar verip, o topun bir daha seçilmemesi adına selected özelliğini false'a çekiyor.

Şimdi bu util fonksiyonlarımızı handleDrop methodunda çağıralım.

```jsx
// ./src/app.js
...
  handleDrop = (ball, itemX, itemY) => {
    let _holes = this.state.holes;
    let _balls = this.state.balls;

    const result = selectDropZone(
      _holes,
      itemX,
      itemY,
      SharedStyle.hole.width,
      SharedStyle.hole.height,
      _balls,
      ball,
      SharedStyle.header.height
    );
    _holes = result.holes;
    _balls = result.balls;

    this.setState({ holes: _holes, balls: _balls });
  };
...
```

hole ve ball componentlerimizin style'larının yeni state'e göre şekil alması lazım. Onları ekleyelim. İlk önce hole componentine bakalım.  Hovering ise hoverColor, filled ise toptan aldığı renk, hiçbiri değilse beyaz gözükecek. \( Ne dersiniz bu yöntemi biraz değiştirerek basit bir kalemle çizim uygulaması yapılabilir mi ? \)

![](/assets/digdagdoe5.gif)Şimdi ball.js componentine sen seçildinse ortadan kaybol diyelim.

![](/assets/digdagdoe6.gif)

Şimdi top bırakıldığında boardContainer'da değilse yada dolu bir karenin üzerindeyse yerine geriye dönmesini isteyelim. İşin burası çok basit sadece release methoduna Animated fonksiyonu ekleyeceğiz. 

```jsx
// ./src/components/ball.js
...
handleRelease = (evt, gestureState) => {
    const {ball } = this.props;
    this.state.pan.flattenOffset();
    this.props.onDrop(ball, evt.nativeEvent.pageX, evt.nativeEvent.pageY);
    Animated.spring(this.state.pan, {
              toValue: {x:0 , y:0},
              friction:4,    //canlılık değeri diyeceğim ama çok bir şey anlatmıyor 
              tension:10     //hız kontrolü diyelim buna da
           }).start()
  };
...
```

Animated.spring'de diyoruz ki; top bırakıldığında this.state.pan başlangıç değerini {x:0 , y:0} değerine çek. Bunu da kendine özel animasyonunla yap.

![](/assets/digdagdoe7.gif)






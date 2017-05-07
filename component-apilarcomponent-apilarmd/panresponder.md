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
3. Toplar için state'te array tutalım. Bu array'ı map ederek 2. adımda yarattığımız componenti çoğaltalım.
4. Hover effect ekleyelim
5. Sürüklediğimiz componenti drop ettikten sonraki kontrolleri ekleyelim.

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




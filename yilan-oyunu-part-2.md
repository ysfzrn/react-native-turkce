# REACT NATIVE İLE YILAN OYUNU  \( PART 2 \)

![](/assets/Screen Shot 2017-10-01 at 14.25.18.png)

**1\) **Home ekranımızda, gameStore'dan gelecek en yüksek skor bilgisi var. Bu bilgi telefon hafızasında tutulacak. Bunu sağlamak için **AsyncStorage**'ı kullanabiliriz.

![](/assets/Screen Shot 2017-10-02 at 01.07.26.png)**AsyncStorage**'da iki temel işlem var. `setItem` ve `getItem`.setItem'dan JSON.stringfy kullanmamızın sebebi, AsyncStorage'da yalnızca string ile çalışabiliyoruz. İki temel method da verilen key'e göre sorgu çekiyor. Burada bizim key'imiz "snakeHighScore". Buradaki algoritamanın mantığına gelince, telefon hafızasında, snakeHighScore key'ine sahip bir veri var mı. Varsa onu dön. Yoksa  sıfır\(0\) olarak yeni bir tane oluştur. Çünkü normal olarak kullanıcı oyunu ilk açtığında herhangi bir skora sahip olmayacaktır. Son olarak bize dönen değeri de, `this.highScore = _highScore` ifadesi ile observable state'imize set ediyoruz.

Şimdi ekranımızın tasarımına geçelim. Ekranlarda ortak ilişkili tutacağım stilleri, bir style dosyasında tutmayı tercih ediyorum. İşimi daha kolaylaştırıyor. Ve daha çok tema opsiyonu nasıl olurmuş diye deneme şansım oluyor. Burada kullanacağımız stil dosyası aşağıdaki olacak.

Burada önemli olarak yılanımızın her bir parçası yani segment'i 10px değerin de olacak. Ve yılanın hareket edeceği board'da herhangi bir uyumsuzluk yaşamaması için, board'ın genişliği, telefonun ekran genişliğine göre şekillenmesi ve width değerinin 10'nun katlarından bir değere sahip olması gerekiyor. Bunun için sharedStyle.js içinde definiteWidth adlı bir method yarattım.

```js
//src/util/SharedStyle.js
import { Dimensions, Platform } from "react-native";

const { width, height } = Dimensions.get("window");

function definiteWidth(width){
    const reminder = width % 10;
    if( reminder !== 0 ){
        return width - 5;
    }else{
        return width;
    }
}

const SharedStyle ={
  color:{
      primary:'#122210',
      primaryBlack:'#223D1D',
      secondary:'#F9FF1C',
      snake:'#4CAF50',
      scoreColor: 'yellow',
      buttonBackground: '#000000'
  },
  board:{
      height: Platform.OS === 'ios' ? 500 : 400,
      width: definiteWidth(width),
  },
  segment:{
      width:10,
      height:10,
      backgroundColor: '#4CAF50',
      borderWidth:1,
      borderColor: '#285A2A',
  },
  food: {
      width: 10,
      height: 10,
      backgroundColor: 'red',
      borderWidth:1,
      borderColor: 'black',
  },
  scoreBoard: {
      height: 34,
  }
}

export default SharedStyle
```

Home ekranımız da şöyle olacak. Bu yazı genel olarak MobX üzerine yazıldığı için kullanılan componentlerin ve ekranların tasarımı üzerinde çok durmayacağım. Ama şunu söylemek zorundayım. Buradaki **inject** ve **observer** decoratorlerine dikkat edin.

**@observer** decoratorü ile bu ekranımızın observable değerlerine göre render edilmesini sağlıyoruz. \( İsimlendirme gayet basit, observable, takip edilen değerler, observer, bu değerleri takip eden demek. \)

**@inject** decoratorü ise MobX'de store elemanlarını componentlerde kullanabilmemizi sağlayan bir diğer yöntem. İlgili store'u burada import edip de kullanabilirdik. Ama ben @inject decoratorünü kullanarak, store elemanlarına direkt erişmek yerine, componentin props'larından erişmeyi tercih ediyorum. Hangi yöntem daha doğru ben de bilmiyorum sadece belki gerektiği zaman lifecycle methodlarını \( componentWillReceiveProps gibi \) kullanmaya ihtiyacım olur diye @inject ile props'dan almak daha mantıklı geliyor.

Artık props üzerinde storelara erişebiliyorum. `const { gameStore } = this.props`; ile `gameStore.highScore`'u `ScoreText`componentine gönderebilirim.

```jsx
//import liraries
import React, { Component } from 'react';
import { View, Text, StyleSheet, StatusBar } from 'react-native';
import { inject, observer } from "mobx-react/native";
import SharedStyle from "../utils/sharedStyle";
import { Button, ScoreText} from "../components";

// create a component
@inject("nav", "gameStore")
@observer
class Home extends Component {
  static navigatorStyle = {
    navBarHidden: true  // default olarak gelen navigationBar'ın görünmemesi için
  };

  componentDidMount() {
    const { gameStore } = this.props;
    gameStore.getHighScore();  //highScore değerini al
  }

  handlePlay=()=>{
    const { nav } = this.props;
    nav.handleChangeRoute("gameScreen");   //gameScreen ekranına git
  }

  render() {
    const { gameStore } = this.props; 
    return (
      <View style={styles.container}>
        <StatusBar barStyle="light-content"/>
        <View style={{flex:1,  justifyContent:'center'}} >
          <Logo />
        </View>  
        <View style={{flex:1, flexDirection:'column',   }}>
           <Button text="PLAY" onPress={ this.handlePlay } />
           <ScoreText label="High Score" score={gameStore.highScore} style={{justifyContent:'center', marginTop:42}} />
        </View>  
      </View>
    );
  }
}

const Logo = (props) => {
  return(
   <View style={{alignItems:'center'}}> 
    <Text style={styles.logoText}>CRAZY SNAKE</Text>
    <Text style={styles.logoText}>GAME</Text>
   </View> 
  )
}

// define your styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: SharedStyle.color.primary,
  },
  logoText: {
    fontSize: 52,
    color: SharedStyle.color.snake,
  }
});

//make this component available to the app
export default Home;
```

Gelelim PLAY butonuna bastığımızda MobX üzerinden ekranı değiştirmeye. Bu işi yukarıdaki component de `handlePlay` methodunda yapıyoruz. navigationStore'da yazdığımız,  handleChangeRoute methodunu çağırıyoruz. Sadece yaptığı iş navigationStore'da route observable değerini değiştirip, değerini "gameScreen" olarak güncellemek. O da ilk bölümde anlattığımız, root.js içindeki reaction'ı tetikleyip, this.startApp methodunu çağıracak.

Şimdi oyunun ana kısmı olan gameScreen ekranımıza geçelim. Bu bölüm oldukça uzun olacağından bunu 3. bölümde anlatalım.


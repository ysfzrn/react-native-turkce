# Style ve FlexBox

CSS'de kullandığımız, `display:flex` ile benzer.

|  | Options \(Default:Bold\) | Description |
| :---: | :---: | :--- |
| `flexDirection` | `row,column` | \(**column**\) seçerseniz elementleri dikey, \(**row**\) seçerseniz elementleri yatay sıralar.    Bu seçim çok önemli çünkü bizim **primary axis** 'imiz ne olacak onu belirler. Diğeri **secondary axis** olur |
| `justifyContent` | `flex-start, center, flex-end,   space-around, space-between` | Child elementlerimiz **primary axis** boyunca Container içinde nasıl dağılsın. |
| `alignItems` | `flex-start, center, flex-end,stretch` | Child elementlerimiz **secondary axis** boyunca Container içinde nasıl dağılsın. |

```javascript
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
    container:{
        flex:1   
    }
});
```

React Native de style tanımlamalarını, kendi kütüphanesinin içinde bulunan **StyleSheet** ile yapıyoruz. Bilindik bir obje tanımlaması yapıyoruz aslında. Yukarıdaki örnekte tanımladığımız **container** objesini aşağıdaki gibi kullanıyoruz.

```javascript
<View style={styles.container} />
```

**flex:1** demek içinde bulunduğu componenti enine boyuna %100 kapla demek. En üsteki componente bu değeri verirsek ekranımızın tamamını kaplamış oluruz. Aşağıdaki örneği inceleyelim

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-11-at-22.04.44.png)

Container kartımız **flex:1** ve tüm ekranı kaplıyor. Ayrıca **flexDirection** olarak **column** almış **primary axis** dikey.. Böylece child elementler dikey olarak sıralanıyor.

Mavi Kart **flex:1**, Beyaz Kart **flex:3**, Kırmızı Kart **flex:2** değerlerini almış.

Burada Container componenti primary axis boyunca oransal olarak 6x parçaya bölmüşler. Mavi'ye 1x, Beyaz'a 3x, Kırmızı'ya 2x oranında pay düşmüş. Resimde de bu oranı rahatlıkla görebilirsiniz.

Aynı oranı **primary axis**'miz **row** olsaydı bu kez şöyle bir layout elde etmiş olacaktık.![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-11-at-22.13.06.png)

Şimdi yapmak istediğimiz uygulamaya bir göz atalım.

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-11-at-23.14.28.png)

Burada temel container elementimiz ekranı kaplasın, 2 tane de container elementimiz olsun ve telefonun ekranını 1 e 5 oranında paylaşsınlar. Button componentinin pozisyonu fixed olduğundan onu bu durumdan ben ayrı tutuyorum. Aşağıdaki resimde bunu nasıl gerçekleştirdik inceleyebilirsiniz.

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-11-at-23.30.01.png)

TouchableOpacity componentinin style'ı button objesine yazılmış. Pozisyonu fixed olacak demiştik. React Native'de `positin:fixed`methodu yok. Onun yerine position özelliğine `'relative'` veya `'absolute'` değerlerini verebilirsiniz. Bilindik `left, right, bottom` değerleriyle de ekranın en altına çakılmış durumda. TouchableOpacity gördüğünüz gibi props olarak children kabul ediyor. HTML de ki `<button>Ekle</button>` yerine biz burada `<Text />` componentini kullanmak zorundayız. Ayrıca TouchableOpacity elementine de flexBox stilleri yazdığımıza dikkat edin. \(`alignItems` ve `justifyContent` \)

Şimdi inputContainer'a bir TextInput ekleyelim. TextInput, container'ın en sonunda dursun diye, justifyContent'e 'flex-end' diyelim.

> justifyContent primary axis'e etki ediyordu. primary axis, default olarak `flexDirection:'column'` yani dikey. Burada`justifyContent:'flex-end'`demek, child elementi dikey olarak sona at demek.

```javascript
 ...
 <View style={styles.inputContainer}>
      <TextInput />
 </View>
 ...

 inputContainer:{
        flex:1,
        justifyContent:'flex-end',
        paddingLeft:16,
        paddingRight:16
 },
 ....
```

Şimdi bir de listemize eklemeler yapılınca çoğaltacağımız, ListItem componenti yapalım.

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/listitem.png)

Bu componentte de flexbox'ın nimetlerinden faydalanalım. Flex bir container ımız olsun. Bunun içindeki child componentler merkezi olarak hizalansın. Ancak sağa doğru yaslı duran bir butonumuz olsun.Bu yazılanların kodu şöyle oluşuyor.

```javascript
const ListItem=(props)=>{
    return(    
        <View style={styles.listItemContainer} >
          <Text> Todo List 1 </Text>
          <TouchableOpacity style={ styles.listItemButton } />
        </View>
    )
}

....
listItemContainer:{
        height:48,
        borderRadius:12,
        borderWidth:1,
        borderColor:'#979797',
        margin:14,
        position:'relative',

        flexDirection:'row',  //yatay hizalansın
        alignItems:'center',  //child componentler merkezi hizalansın
        paddingLeft:31,
    },
listItemButton:{
    position:'absolute',        
    right:16,                 //sağa 16px lik uzaklıkta yaslı olsun
    width:26,
    height:26,
    borderRadius:13,
    backgroundColor:'green',
}

....
```

Sonra da main componentimiz de aşağıdaki gibi ListItem componentimizi çağıralım

```javascript
 ...
 <View style={styles.listContainer}>
            <ListItem />
 </View>
 ...
```

Şimdi ListItem da buttonumuzun props alarak rengini belirleyelim.

```javascript
<TouchableOpacity style={ styles.listItemButton } />
```

Hali hazırda listItemButton isimli bir style'ı var. bu listItemButton objesinde bir backgroundColor tanımı yapılmış. O tanım kalsa bir sorun yaratmaz ama kaldıralım.

```javascript
const ListItem=(props)=>{
    ...
          <TouchableOpacity style={[styles.listItemButton,{backgroundColor:props.statusColor } ] } />
   ...
}
```

Yukarıdaki koda dikkat edin. style propsu bir obje alıyor { }. Bu objenin içine biz array tanımlıyoruz { \[ \] }. bu array içine biz istediğimiz kadar style objesi yerleştirebiliriz.{ \[ { }, { } \] }

Son olarak şekillediriğimiz ekran bu şekilde olmuş oldu,

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-12-at-03.20.55.png)

Şimdi IOS da da istediğimiz gibi görünüyor mu ona bakalım. ![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-12-at-03.35.55.png)Maalesef görünmüyor :\( . %90 oranında aynısı da olsa, TextInput ortalıkta yok. Çünkü IOS da height alanı zorunlu. Ve IOS tasarımda genelde materialden farklı olarak, text girişlerinde bir border oluyor. Şimdi textInput'a ios da ayrı, android de ayrı çalışacak şekilde bir style ekleyelim. Bunun için React Native'de **Platform** isimli bir API var. Bu bizim hangi işletim sistemi kodu çalıştırıyor söyleyecek.

```javascript
import { StyleSheet,View, Text,TouchableOpacity,
     TextInput, Platform  } from 'react-native';
...
     <View style={styles.inputContainer}>
             <TextInput style={ styles.textStyle}  />
     </View>
...
textStyle:{
      ...Platform.select({
        ios: {
          height:40, 
          paddingLeft:12, 
          borderWidth:1, 
          borderRadius:12
        },
        android:{

        }
      })
},
....
```

Şimdi tam istediğimiz gibi oldu. Kodun tamamı için [https://gist.github.com/ysfzrn/a825488f65fabcf6e41ede93a678d83c](https://gist.github.com/ysfzrn/a825488f65fabcf6e41ede93a678d83c)

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-12-at-03.48.19.png)


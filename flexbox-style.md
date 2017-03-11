# Style ve FlexBox

CSS'de kullandığımız, `display:flex` ile benzer.

|  | Options \(Default:Bold\) | Description |
| :---: | :---: | :--- |
| `flexDirection` | `row,column` | \(**column**\) seçerseniz elementleri dikey, \(**row**\) seçerseniz elementleri yatay sıralar.    Bu seçim çok önemli çünkü bizim **primary axis** 'imiz ne olacak onu belirler. Diğeri **secondary axis** olur |
| `justifyContent` | `flex-start, center, flex-end,   space-around, space-between` | Child elementlerimiz **primary axis** boyunca Container içinde nasıl dağılsın. |
| `alignItems` | `flex-start, center, flex-end,stretch` | Child elementlerimiz **secondary axis** boyunca Container içinde nasıl dağılsın. |

```js
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
    container:{
        flex:1   
    }
});
```

React Native de style tanımlamalarını, kendi kütüphanesinin içinde bulunan **StyleSheet** ile yapıyoruz. Bilindik bir obje tanımlaması yapıyoruz aslında. Yukarıdaki örnekte tanımladığımız **container** objesini aşağıdaki gibi kullanıyoruz.

```js
<View style={styles.container} />
```

**flex:1 **demek içinde bulunduğu componenti enine boyuna %100 kapla demek. En üsteki componente bu değeri verirsek ekranımızın tamamını kaplamış oluruz.  Aşağıdaki örneği inceleyelim

![](/assets/Screen Shot 2017-03-11 at 22.04.44.png)

Container kartımız **flex:1** ve tüm ekranı kaplıyor. Ayrıca **flexDirection** olarak **column** almış **primary axis** dikey.. Böylece child elementler dikey olarak sıralanıyor.

Mavi Kart **flex:1**, Beyaz Kart **flex:3**, Kırmızı Kart **flex:2** değerlerini almış.

Burada Container componenti primary axis boyunca oransal olarak 6x parçaya bölmüşler. Mavi'ye 1x, Beyaz'a 3x, Kırmızı'ya 2x oranında pay düşmüş. Resimde de bu oranı rahatlıkla görebilirsiniz.

Aynı oranı **primary axis**'miz  **row** olsaydı bu kez şöyle bir layout elde etmiş olacaktık.![](/assets/Screen Shot 2017-03-11 at 22.13.06.png)

Şimdi yapmak istediğimiz uygulamaya bir göz atalım.

![](/assets/Screen Shot 2017-03-11 at 23.14.28.png)

Burada temel container elementimiz ekranı kaplasın, 2 tane de container elementimiz olsun ve telefonun ekranını 1 e 5 oranında paylaşsınlar. Button componentinin pozisyonu fixed olduğundan onu bu durumdan ben ayrı tutuyorum. Aşağıdaki resimde bunu nasıl gerçekleştirdik inceleyebilirsiniz.

![](/assets/Screen Shot 2017-03-11 at 23.30.01.png)

TouchableOpacity componentinin style'ı button objesine yazılmış. Pozisyonu fixed olacak demiştik. React Native'de `positin:fixed`methodu yok. Onun yerine position özelliğine `'relative'` veya `'absolute'` değerlerini verebilirsiniz. Bilindik `left, right, bottom` değerleriyle de ekranın en altına çakılmış durumda. TouchableOpacity gördüğünüz gibi props olarak children kabul ediyor. HTML de ki `<button>Ekle</button>` yerine biz burada `<Text />` componentini kullanmak zorundayız. Ayrıca TouchableOpacity elementine de flexBox stilleri yazdığımıza dikkat edin. \(`alignItems` ve `justifyContent` \)

Şimdi inputContainer'a bir TextInput ekleyelim. TextInput, container'ın en sonunda dursun diye, justifyContent'e 'flex-end' diyelim.

> justifyContent primary axis'e etki ediyordu. primary axis, default olarak `flexDirection:'column'` yani dikey. Burada`justifyContent:'flex-end'`demek, child elementi dikey olarak sona at demek.

```js
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


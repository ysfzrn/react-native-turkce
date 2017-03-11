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

Mavi kart **flex:1**, Beyaz kart **flex:3**, Kırmızı kart **flex:2** değerlerini almış.

Burada Container componenti primary axis boyunca oransal olarak 6x parçaya bölmüşler. Mavi'ye 1x, Beyaz'a 3x, Kırmızı'ya 2x oranında pay düşmüş. Resimde de bu oranı rahatlıkla görebilirsiniz.

Aynı oranı **primary axis**'miz **row** olsaydı bu kez şöyle bir layout elde etmiş olacaktık.![](/assets/Screen Shot 2017-03-11 at 22.13.06.png)






# REACT NATIVE İLE YILAN OYUNU  \( PART 1 \)

İlk bölümün source kodlarına şuradaki branch'den erişebilirsiniz:[ ](https://github.com/ysfzrn/crazysnake/tree/firstpart)[https://github.com/ysfzrn/crazysnake/tree/firstpart](https://github.com/ysfzrn/crazysnake/tree/firstpart)

React Native, MobX ve react-native-navigation kullanarak, basit bir yılan oyunu yapalım.

![](/assets/Screen Shot 2017-10-01 at 14.25.18.png)

Oyunumuz 3 ekrandan oluşacak.

* İlk ekranda kullanıcının telefonunda yapılan en yüksek puanı gösterecek
* Play butonuna basılınca direkt oyun başlayacak.
* Oyun ekranında 2 tane buton olacak. \( Eski Nokia telefonlardan hatırlarsanız, sadece 3 ve 7 tuşlarıyla, yılanımızı 4 farklı yöne doğru gitmesini yönetebiliyorduk.\)
* Yılanımız her bir elmasını \( evet o kırmızı şey çok belli olmasa da bir elma \) yediğinde hızı artarak devam edecek.
* Yılan kendi kuyruğuna çarpınca da oyun bitecek, yeni bir yüksek skor yapılmışsa, yeni skor, telefona kaydedilecek.

Hadi başlayalım :\)

**1\) **Projemizi react-native-cli ile yaratalım.

> `react-native init crazysnake`

**2\) **Projemizin klasörüne girip \(cd crazysnake\), kullanacağımız dependency paketlerini indirelim.

> `yarn add lodash mobx mobx-react react-native-navigation`
>
> `yarn add --dev babel-plugin-transform-decorators-legacy`

**3\) ** babelrc dosyamızı düzenleyip, decorator'lerin çalışmasını sağlayalım.

> `{`
>
> `'presets': ['react-native'],`
>
> `'plugins': ['transform-decorators-legacy']`
>
> `}`

**4\) **Mobx için yaptıklarımızdan sonra react-native-navigation paketine geçelim. Bu paket wix ekibinin yazdığı native bir kütüphane olduğundan entegrasyonu android ve ios için ayrı ayrı yapmak gerekiyor. Bu paketin kurulumunu daha önce yazdığımız [şu makalede](https://ysfzrn.gitbooks.io/react-native-turkce/navigation/wixreact-native-navigation.html) bulabilirsiniz. Buradaki adımları eksiksiz yaptığınıza emin olun.

**5\)** Geliştirme ortamı kurulumumuzu bitirdik. Şimdi kod yazmaya başlayalım. Bir src isimli source klasör oluşturalım. Ve bunun için de root.js isimli bir dosya oluşturalım. Proje path'inde yer alan index.android.js ve index.ios.js içinde de bu root js i çağıralım.

![](/assets/Screen Shot 2017-10-01 at 15.20.48.png)

**6\) **react-native-navigation paketi redux ile kullanılmak üzere tasarlanmış. Ama korkmayın, mobx ile de kullanmak için tek yapmamız gereken bir tane fazladan Provider.js isimli dosya yaratıp aşağıdaki kodu değiştirmeden kopyalamanız yeterli.

```js
import { Provider } from "mobx-react/native";

const SPECIAL_REACT_KEYS = { children: true, key: true, ref: true };

export default class MobxRnnProvider extends Provider {
  props: {
    store: Object
  };

  context: {
    mobxStores: Object
  };

  getChildContext() {
    const stores = {};

    // inherit stores
    const baseStores = this.context.mobxStores;
    if (baseStores) {
      for (let key in baseStores) {
        stores[key] = baseStores[key];
      }
    }

    // add own stores
    for (let key in this.props.store) {
      if (!SPECIAL_REACT_KEYS[key]) {
        stores[key] = this.props.store[key];
      }
    }

    return {
      mobxStores: stores
    };
  }
}
```

**7\) **Şimdi store'ları oluşturmaya başlayalım. 2 tane store olacak. 1 tanesi navigasyonu mobx üzerinden de yönetebileceğimiz, navigationStore. Diğeri, oyunun state'lerini yöneteceğimiz gameStore olacak. stores, klasörünün altında dabu iki store'u  export edeceğiz. \( Bu arada bu projede navigationStore kullanmak zorunda değildik. Sadece mobx ile nasıl kullanıldığına örnek olması bakımından yazma gereği duydum. \)

![](/assets/Screen Shot 2017-10-01 at 15.31.42.png)

**8\) **Ana** **ekranlarımız için **screens** klasörü, componentler için **components** klasörü, resim ve ikonlar için **assets** klasörü, uygulamanın genel kullanılabilecek fonksiyonları için bir tane de **util** fonksiyonu oluşturalım. Ve screens klasörüne home.js isimli ilk ekranımızı koyalım.

**9\) **Şimdi ekranlarımızı navigation paketine register edelim. Store ve Provider isimli dosyalarımızı da ekranlara parametre olarak geçirelim. Bunun için source klasöründe **registerScreens.js** isimli bir dosya yaratıp aşağıdaki gibi içini düzenliyoruz.

![](/assets/Screen Shot 2017-10-01 at 15.42.03.png)

**10\) **Şimdi başta yarattığımız root.js isimli dosyamıza geçelim. Ve aşağıdaki gibi düzenleyelim. Burada root.js bir react componenti değil bir javascript class'ı. Ve bu class'ın içinde başta çalışacak constructor methodunda, mobx'in reaction fonksiyonunu kullanıyoruz. reaction, computed methodlara benzese de, observable state değiştiğinde yeni bir değer üretmek yerine, side effect oluşturmamızı sağlar. Aşağıdaki kodda yapılan işlem, Store.nav.route değeri değiştiğinde, startApp methodunu çalıştır.

![](/assets/Screen Shot 2017-10-01 at 15.58.41.png)

Son olarak react-native run-ios ve react-native run-android komutlarıyla aşağıdaki gibi sonuç aldıysanız, oyunumuzun ikinci bölümüne geçebiliriz.

![](/assets/Screen Shot 2017-10-01 at 16.12.24.png)


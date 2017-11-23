# react navigation

react-native core ekibi navigation, sayfa yönlendirme de herkesi memnun edecek bir navigation kütüphanesini yazmakta ve bunu react-native'in kendisinin içine koymakta zorlanınca, bunu açık kaynağa gönül vermiş insanların halledebileceğine inandığını söyledi. Hal böyle olunca, yüzlerce react-native için router kütüphanesi çıkmaya başladı. Kimileri native çözüm denedi, kimileri javascript ile halledebileceğini düşündü. Bunlardan başarılı olduğuna inandığımız react-native-navigation kütüphanesini bir önceki bölümde anlatmaya çalışmıştık. Şimdi javascript çözümlerin içinde çoğu insana göre en iyisi, en popüler olan, github'da react-community grubunun oluşturduğu react-navigation'a bakalım.

> react-navigation, sayfa yönlendirme işine yapılan native bir çözüm değil. O yüzden performansına uygulamanızı yazarken her zaman dikkat etmeniz gerekir. Uygulamanızda mobx veya redux gibi bir state yönetim aracı kullanıyorsanız, sayfalar arası çok yüklü propslar geçirmiyorsanız çok performans sıkıntısı yaşamazsınız.



### Kurulum

Aslında karşımızda bir javascript kütüphanesi var. O yüzden tek yapmamız gereken, npm veya yarn ile uygulamamıza dependency olarak eklemek.

```js
//npm kullanıyorsanız
npm install --save react-navigation

//yarn kullanıyorsanız
yarn add react-navigation
```

### Başlangıç

react-navigation'da, mutlaka kendisinin içinde olan 3 navigator'den en az birini yaratmanız gerekmektedir. \(Birbiri içinde de bu navigator'leri kullanabilirsiniz, nasıl olduğunu örneklerde açıklamaya çalışacağız. \)

* `StackNavigator` : Tüm sayfayı kullanmak istiyorsanız kullanmanız gereken navigator. 
* `TabNavigator` : Tab'lardan oluşan bir uygulamanız varsa kullanmanız gereken navigator.
* `DrawerNavigator` : Yandan açılmalı bir sidebar ile sayfa yönlendirmesi yapmak istiyorsanız kullanmanız gereken navigator.

### StackNavigator

Tüm navigator'lerde olduğu gibi StackNavigator, parametre olarak bir obje alıyor. Bu objenin key'leri bizim sayfa route'larımız. Sayfayı yönlendirirken bu key'lerden faydalanacağız. 

Aşağıdaki örnekten Home ve Details key'leri de bir objeyi gösteriyor. Objenin içinde sabit key'lerimiz var. En önemlisi screen key'lerine, container componentlerimiz olan HomeScreen ve DetailsScreen componentlerini atadığımıza dikkat edin.

```jsx
import { StackNavigator } from 'react-navigation';
import HomeScreen from './screens/home'
import DetailsScreen from './screens/details'

const RootNavigator = StackNavigator({
  Home: {
    screen: HomeScreen,
  },
  Details: {
    screen: DetailsScreen,
  },
});

export default RootNavigator;
```

### TabNavigator

StackNavigator örneğini, tab'ları olan bir uygulamaya dönüştürmek istersek yapacağımız şey yukarıdaki ile aynı. Sadece StackNavigator yerine TabNavigator'ı çağırıyoruz.

```jsx
import React from 'react';
import { TabNavigator } from 'react-navigation';
import HomeScreen from './screens/home'
import DetailsScreen from './screens/details'


const RootTabs = TabNavigator({
  Home: {
    screen: HomeScreen,
  },
  Profile: {
    screen: DetailsScreen,
  },
});

export default RootTabs;
```

### DrawerNavigator

DrawerNavigator kullanırken de temelde farklı bir şey yapmıyoruz.

```jsx
import React from 'react';
import { DrawerNavigator } from 'react-navigation';
import HomeScreen from './screens/home'
import DetailsScreen from './screens/details'


const RootDrawer = DrawerNavigator({
  Home: {
    screen: HomeScreen,
  },
  Profile: {
    screen: DetailsScreen,
  },
});

export default RootDrawer;
```




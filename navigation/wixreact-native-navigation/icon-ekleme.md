# Icon Ekleme - TabBar

React Native'de icon deyince akla gelen ilk kütüphane, [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons). Gerçekten harikulade bir kütüphane. react-native-navigation ile nasıl kullanılıyor bir bakalım.

Aşağıdaki komut ile indirelim.

```
npm install react-native-vector-icons --save
```

Sonra react-native link ile linkleme yapalım.

```
react-native link react-native-vector-icons
```

**android/app/src/main/java/com/projenizin\_ismi/MainApplication.java **dosyasına gidelim ve VectorIconsPackage'i aşağıdaki set edelim.

```java
...
import com.oblador.vectoricons.VectorIconsPackage;
...
public List<ReactPackage> createAdditionalReactPackages() {
return Arrays.<ReactPackage>asList(
  new VectorIconsPackage()
);
}
...
```

Bizim arkadaş listesini göreceğimiz bir FriendList.js isimli bir ekranımız ve mesajları görebieceğimiz ProfileScreen.js isimli bir componentimiz olsun.Ve aşağıdaki gibi register edelim.

```jsx
// ./src/screen.js

import { Navigation } from 'react-native-navigation';
import FirstScreen from './FirstScreen';
import SecondScreen from './SecondScreen';
import FriendListScreen from './FriendListScreen';
import ProfileScreen from './ProfileScreen';

export function registerScreens() {
  Navigation.registerComponent('chatapp.FirstScreen', () => FirstScreen);
  Navigation.registerComponent('chatapp.SecondScreen', () => SecondScreen);
  Navigation.registerComponent('chatapp.FriendListScreen', () => FriendListScreen);
  Navigation.registerComponent('chatapp.ProfileScreen', () => ProfileScreen);
}
```

./src/index.ios.js dosyasına dönelim ve uygulamamızın başlangıç sayfasını bir tabBar'a dönüştürelim. Bunun için react-native-navigation'da `Navigation.startTabBasedApp` api'sini kullanacağız. Ama daha önce icon belirleyelim.FriendList için, IonicIcons'daki ios-people ve profil sayfası için ios-person iconlarını kullanalım. ./src/index.ios.js 'de App class'ımıza bir method ekleyelim adı populateIcons olsun.Bu bize Promise dönsün, Iconların yüklemesi tamamlandığında uygulamamız belirlediğimiz ilk ekrandan çalışmaya başlasın. Bunları yaptığımız aşğaıdaki kodu dikkatle lütfen inceleyin.

```jsx
// ./src/index.ios.js

import { Navigation } from "react-native-navigation";
import { registerScreens } from "./screen";
import Icon from "react-native-vector-icons/Ionicons";

registerScreens();

export default class App {
  constructor() {
    this.populateIcons().then(() => {
      this.startApp();
    }).catch((error) => {
      console.error(error);
    });
  }

  populateIcons = function() {
    return new Promise(function(resolve, reject) {
      Promise.all([
        Icon.getImageSource("ios-people", 30),
        Icon.getImageSource("ios-people-outline", 30),
        Icon.getImageSource("ios-person", 30),
        Icon.getImageSource("ios-person-outline", 30)
      ])
        .then(values => {
          friendListIcon = values[0];
          friendListOutlineIcon = values[1];
          profileIcon = values[2];
          profileOutlineIcon = values[3];
          resolve(true);
        })
        .catch(error => {
          console.log(error);
          reject(error);
        })
        .done();
    });
  };

  startApp() {
    Navigation.startSingleScreenApp({
      screen: {
        screen: "chatapp.FirstScreen",
        title: "Chat App"
      }
    });
  }
}
```

Şimdi bir tek ekran olarak çalışan uygulamamızı, aşağıdaki gibi  tabBar'a dönüştürelim.

```jsx
// ./src/index.ios.js
...
  startApp() {
    Navigation.startTabBasedApp({
        tabs: [
          {
            label: 'Arkadaşlar',
            screen: 'chatapp.FriendListScreen',
            icon: friendListIcon,
            selectedIcon: friendListOutlineIcon,
            title: 'Arkadaşlarım'
          },
          {
            label: 'Profil',
            screen: 'chatapp.ProfileScreen',
            icon: profileIcon,
            selectedIcon: profileOutlineIcon,
            title: 'Profilim'
          }
        ]
      });;
  }
  ...
```

index.ios.js'de yazdığımız kodu index.android.js'e de kopyalayalım ve uygulamayı tekrardan iki emulatorde de çalıştıralım.

![](/assets/rnn-ios-9.gif)

Android kullanıcıları çok ekranın altındaki tabBar'a alışkın değil o yüzden bunu TopTabBar yapalım. Bunun için index.android.js startApp fonksiyonunun aşağıdaki değiştirelim.

```jsx
 // ./src/index.android.js
 
 startApp() {
    Navigation.startSingleScreenApp({
    screen: {
    screen: 'chatapp.FirstScreen',
    title:'Chat App',
    topTabs: [
      {
        screenId: 'chatapp.FriendListScreen',
        icon: friendListIcon,
      },
      {
        screenId: 'chatapp.ProfileScreen',
        icon: profileIcon,
      }
    ]
   }});
  }
```

Burada react-native-navigation yaratıcılarının hoşuna gitmeyen durum, topTabs kullanırken aslında chatapp.FirstScreen çok işe yaramaz halde fazladan yer alması. Bunu değiştirmeyi planlıyorlar ama şimdilik bun componenti biz tabBar'a style vermek için kullanabiliriz. O yüzden FirstScreen componentine aşağıdaki static style'ı tanımlayarak son hale bir bakalım.

```jsx
// ./src/FirstScreen.js
import React, { Component } from "react";
import { View, Text, StyleSheet, TouchableOpacity,PixelRatio } from "react-native";

class FirstScreen extends Component {
  static navigatorStyle = {
    statusBarColor: '#1d84d6',

    navBarBackgroundColor: '#2196f3', // Top color
    navigationBarColor: '#1d84d6', // Bottom color
    navBarTextColor: '#ffffff', // Title color

    // Text
    topTabTextColor: 'rgba(255, 255, 255, 0.7)',
    selectedTopTabTextColor: '#ffffff',

    // Icons
    selectedTopTabIconColor: '#ffffff',
    topTabIconColor: 'rgba(255, 255, 255, 0.7)',

    selectedTopTabIndicatorColor: 'orange',
    selectedTopTabIndicatorHeight: PixelRatio.get() * 2,

    topTabScrollable: true,
  }
...
```

![](/assets/rnn-ios-10.gif)


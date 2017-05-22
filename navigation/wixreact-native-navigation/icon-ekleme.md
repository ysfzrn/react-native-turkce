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

./src/index.ios.js dosyasına dönelim ve uygulamamızın başlangıç sayfasını bir tabBar'a dönüştürelim. Bunun için react-native-navigation'da `Navigation.startTabBasedApp` api'sini kullanacağız. Ama daha önce icon belirleyelim.FriendList için, IonicIcons'daki ios-people ve profil sayfası için ios-person iconlarını kullanalım.


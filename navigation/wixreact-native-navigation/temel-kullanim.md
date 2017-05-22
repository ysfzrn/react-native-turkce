# Temel Kullanım

**1**- İlk yapmamız gereken her zaman olduğu gibi bir **src **isimli source klasörü oluşturmak. Ve onun içine bir registerScreen.js adlı dosyayı oluşturmak. Onun içinde de **FirstScreen.js** isimli ilk ekranımızı ekleyelim.

**2**- Aynı yerde bir tane tüm ekranlarımızı yönetebileceğimiz, **screen.js** isimli bir dosya daha ekleyelim.

**3**- **screen.js**'den export ettiğimiz fonksiyonu kullanacağımız, projemizin ana çıkış kaynağı olan index.ios.js ve index.android.js isimli iki dosya oluşturalım.

**4** - FirstScreen.js bildiğimiz bir react-native component onu screen.js de import edip, Navigasyon'da kullanacağımız bir ekran olduğunu aşağıdaki gibi belirtelim.

```jsx
//  ./src/screen.js

import { Navigation } from 'react-native-navigation';
import FirstScreen from './FirstScreen';

export function registerScreens() {
  Navigation.registerComponent('chatapp.FirstScreen', () => FirstScreen);
}
```

**5**- index.ios.js ve index.android.js ' de ekranlarımızı App isimli class'da set edelim.

```jsx
//  ./src/index.ios.js veya ./src/index.android.js

import { Navigation } from "react-native-navigation";

import { registerScreens } from "./screen";

registerScreens();

export default class App {
  constructor() {
    this.startApp();
  }

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

**6**- App class'ını proje root path'inde yer alan index.ios.js ve index.android.js'de çalıştıralım. Tabi bu bir class olduğu için bunu normal fonksiyon olarak çağıramayız o yüzden bir değişkene set edip çağırmalıyız.

```jsx
// index.ios.js veya index.android.js

import App from './src'

const app = new App();
```

**7**- Şimdi react-native run-ios ile \( Android kurulumunu yaptıysanız react-native run-android ile \) uygulamamızı çalıştaralım. Aşağıdaki gibi bir ekran görmeliyiz.

![](/assets/Screen Shot 2017-05-22 at 04.05.30.png)


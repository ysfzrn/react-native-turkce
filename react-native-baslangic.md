# React Native Başlangıç

Şimdi bir tane işin içine Redux, Navigation, rest karıştırmadan klasik olarak basit bir Todo List uygulamasını React Native'de adım adım yapalım.

Uygulamamızın görseli şöyle olsun.

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/todolist.png)

Global olarak RN cli'yi yüklemediyseniz

> `npm install -g react-native-cli`

```text
Projenizde bir üst versiyona geçmek için `react-native upgrade` komutunu çalıştırabilirsiniz.
```

Workspace klasörümüzde projemizi başlatalım

> `react-native init mytodolist`

Projemizin içine girelim \(bunu yazıyorum çünkü olduğu yerde run etmeye çalışıp hata alıyor diyenler oluyor \)

> `cd mytodolist`

Projemizi run ederken, aşağıdaki komutları kullanıyoruz.

Android için \( Genymotion ı veya emulatorü açtın mı, java adresini doğru verdin mi ? \)

> `react-native run-android`

IOS için \( Xcode yüklü ise sadece aşağıdaki kodu çalıştır \)

> `react-native run-ios`

Şimdi ilk yapmamız gereken Hot ve Live Reloading' leri telefonun seçenekler menusunden açmak. IOS'da cmd+R ile de açabilirsiniz.

React Native CLI , aşağıdaki gibi bir proje verecek bize.

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/todo1.png)

Burada android ve ios klasörlerimiz var. Olaya native deneyimi yaşatan kısım burası. Başta bahsettiğimiz gibi bu klasörler içinde bize native componentleri React Native, bir köprü ile bize sunuyor. Bu klasörlere nasıl dokunacağımıza sonra bakacağız.

> Burada `index.android.js ve index.ios.js` isimli iki dosya görüyoruz. React Native aynı isimli iki dosyayı uzantısı **.ios.js ise** IOS da, **.android.js** ise Android de derler. Bu ne işimize yarar derseniz. Diyelimki siz bir tane **navigationBar** isimli bir component yaptınız. IOS ve Android de farklı davranmasını bekliyorsunuz. Normalde **navigationBar.js** diye bir componente bu farklı davranışları tanımladığınızda ortaya çok karmaşık bir yapı çıkıyorsa **navigationBar.ios.js** ve **navigationBar.android.js** olarak ikiye bölün ve rahat rahat `import NavigationBar from './navigationBar'` olarak çağırıp kullanın.

Hem android hem de IOS da çalışmasını beklediğimiz kod bloğunu bir klasörde toplayalım. index.android.js ve index.ios.js de de sadece bu en üst componenti import edelim.

**index.android.js** \|\| **index.ios.js**

```javascript
import React, { Component } from 'react';
import {
  AppRegistry,
} from 'react-native';

import Main from './src'

AppRegistry.registerComponent('mytodolist', () => Main);
```

Yeni klasör yapımız aşağıdaki gibi oldu, ![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-11-at-20.39.53.png)

Bir sonraki adımımızda Main componentimizi aşağıdaki gibi düzenlemeye başlayabiliriz. En yukarıdaki görseli elde etmek için biraz flexbox olayına el atalım.

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/screen-shot-2017-03-11-at-20.54.41.png)


# macOS Installation

MacOS yüklü bir bilgisayarda react-native uygulaması yazmak için yapmamız gerekenler;

* Bilgisayarınızda güncel bir [Homebrew](https://brew.sh/) yüklü mü kontrol edin. Güncel değil ise [buradan](https://brew.sh/) yazılan komutları takip ederek yükleyin.
* Daha sonra Homebrew kullanarak, [Node](https://nodejs.org/en/) ve [Watchman](https://facebook.github.io/watchman/docs/install.html) yükleyin.

```text
brew install node
brew install watchman
```

* Eğer bilgisayarınızda zaten node yüklü ise, node versiyonunuzun 4 veya daha üstü olması gerekmektedir. Bunun için komut ekranına `node` yazarak girin ve çıkan node ekranında `process.version` kodunu çalıştırarak node'un versiyonunu kontrol edin. 
* React Native projelerini baştan yaratmada kullanacağımız, react-native-cli aracını global olarak bilgisayarınıza, aşağıdaki komutu kullanarak indirin.

```text
npm install -g react-native-cli

//eğer yarn kullanıyorsanız
yarn global add react-native-cli
```

* Daha sonra [Mac App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)'a girerek Xcode'u bilgisayarınıza indirin ve kurun. Xcode'u kurduğunuzda, otomatik olarak IOS simulator'u ve ihtiyacınız olan native tüm araçları zaten kurmuş olacaksınız.
* Eğer bilgisayarınızda zaten Xcode yüklü ise versiyonunun 8 veya daha üzerinde olduğuna emin olun.

## **Hepsi bu kadar :\)**

Şimdi komut satırına, `react-native init merhabaReactNative` yazarak ilk projenizi yaratabilirsiniz.

Daha sonra `cd merhabaReactNative` yazarak projenize girip

`react-native run-ios` komutu ile projenizi simulatorde çalıştırabilirsiniz.


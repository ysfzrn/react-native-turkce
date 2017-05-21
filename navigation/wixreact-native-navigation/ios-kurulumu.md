# IOS Kurulumu

**1**- react native versiyonu olarak 0.43 ve üstünü kullanıyor olduğunuzdan emin olun.

**2**- npm kullanıyorsanız 3 ve üstü versiyon kullandığınızdan emin olun.

```
   npm install react-native-navigation@latest
```

**3**- yarn kullanıyorsanız

```
   yarn add react-native-navigation@latest
```

**4**-react-native init ile kurduğunuz projede ios bölümünü **XCode** ile açın. **Libraries** kısmına sağ tıklayıp **Add Files** seçeneğini seçin. **node\_modules** klasörüne kaydedilen, **react-native-navigation/ios/ReactNativeNavigation.xcodeproj**  projesini **Libraries** kısmına ekleyin.

![](/assets/rnn-ios-1.gif)

**5**- XCode'da navigator'de projenizi seçin ve **Build Phases** kısmına gelin. **Link Binary with Libraries** kısmında **libReactNativeNavigation.a** seçip ekleyin.

![](/assets/rnn-ios-2.gif)


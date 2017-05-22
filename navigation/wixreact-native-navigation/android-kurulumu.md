# Android Kurulumu

Android kurulumu ve kullanımı IOS implemantasyonuna göre daha çetrefilli. Kullanım açısından, Android, material design ile tasarlandığından sahne geçişleri daha çok animasyon içeren daha komplike bir özelliğe sahip ama estetik açısından da daha güzel. Hem android kullanıcılarının aşina olduğu özellikleri zor diye de atlamamak lâzım. Kullanıcı deneyimi uygulamalarınızda asla göz ardı edemeyeceğiniz bir özellik. Tabi burada hiçbir kütüphaneden, react-native'in kendisinden bile tam anlamıyla bahsetmek, örnekler hazırlamak oldukça zor. react-native-navigation'ın anroid'e özel kullanımları için lütfen [resmi dökümantasyonuna](https://wix.github.io/react-native-navigation/#/android-specific-use-cases) göz atın.

**1**- react native versiyonu olarak 0.43 ve üstünü kullanıyor olduğunuzdan emin olun.

**2**- npm kullanıyorsanız 3 ve üstü versiyon kullandığınızdan emin olun.

```
   npm install react-native-navigation@latest
```

**3**- yarn kullanıyorsanız

```
   yarn add react-native-navigation@latest
```

**4**- **android/settings.gradle** içine aşağıdaki kod parçacığını ekleyin.

```
include ':react-native-navigation'
project(':react-native-navigation').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-navigation/android/app/')
```

**5**- **android/app/build.gradle **içinde Android sdk versiyonunu 25 olarak set edin ve project dependency kısmına react-native-navigation'ı ekleyin.

```
 android {
     compileSdkVersion 25
     buildToolsVersion "25.0.1"
     ...
 }

 dependencies {
     compile fileTree(dir: "libs", include: ["*.jar"])
     compile "com.android.support:appcompat-v7:23.0.1"
     compile "com.facebook.react:react-native:+"
     compile project(':react-native-navigation')
 } 
```

![](/assets/rnn-ios-5.gif)




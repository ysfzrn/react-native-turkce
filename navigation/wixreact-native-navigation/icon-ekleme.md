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

./src/index.ios.js dosyasına dönelim.




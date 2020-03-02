# Babel, ES2015, ES6, ES7 Nedir ?

## Babel

React Native yazarken biz JavaScript yazıyoruz ama eski tarzda değil. Bizim yazdığımız bazı özel fonksiyonlarımızın \(Map\(\), async, vb\) çalışabilmesi için **Babel** derleyicisine ihtiyacımız var. Babel, JavaScript kodunu derleyen, onu Native platformun anlayacağı dile çeviren temel araç.

### Babel Ayarları _\*\*_

Babel ayarlarımızı, **.babelrc** konfigurasyon dosyamıza yazdığımız obje tanımlamasıyla yapıyoruz. Bu dosyaya ihtiyacımız olan plugini ekleyip, bu pluginin sunduğu sentaksı, platformun anlayacağı dile çevir diye Babel'e komut veriyoruz.

Örneğin, React Native yazarken biz [metro-react-native-babel-preset](https://www.npmjs.com/package/metro-react-native-babel-preset) pluginini kullanıyoruz. Bu bizim React Native kodumuzu, IOS ve Android simulatörlerimizin anlayacağı dile çeviriyor. Ekstra plugin kullanmak istiyorsak .babelrc dosyamıza eklemeler yapabiliriz.

Gerçi react native projesine başlarken .babelrc dosyasını otomatik olarak bize veriyor. Ve çoğu durumda bu dosyayı açıp da değiştirmeye ihtiyacımız olmayacak.

React Native'in default olarak kullandığı, yukarıda bahsi geçen, babel-preset plugini bizim **es2015, es2016, es2017 ve react** pluginlerini kullanabilmemize imkân sağlıyor.

Eski tarz JavaScript yazmıyoruz demiştik. Kullandığımız yeni tarzımızın adı da ECMAScript \(ES6, ES7\)

\(Spoiler: Zamanla ECMAScript'in sentaksını görüp nasıl ya, bu fonksiyon mu şimdi gibisinden tepkiler vereceğinize eminim ve bu çok normal ama alıştıkça seviyorsunuz. Eğer sevmezseniz, kasmayın ve siz eski tarzda yazmaya devam edin. Zaten mevcut uygulamaların production hallerinin, hep eski tarzda yazıldığı söyleniyor. Babel kullanmalısınız ama ES6 ve ES7 ile yazmak zorunda değilsiniz. \)

Gelecek bölümde, **ES6, ES7** ve **JSX** in daha da içine girelim.

Babel'in kendi plugin dökümantasyonu: [https://babeljs.io/docs/plugins/\#transform-plugins](https://babeljs.io/docs/plugins/#transform-plugins)


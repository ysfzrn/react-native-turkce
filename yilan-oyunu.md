# YILAN OYUNU

React Native, MobX ve react-native-navigation kullanarak, basit bir yılan oyunu yapalım.

![](/assets/Screen Shot 2017-10-01 at 14.25.18.png)

Oyunumuz 3 ekrandan oluşacak.

* İlk ekranda kullanıcının telefonunda yapılan en yüksek puanı gösterecek
* Play butonuna basılınca direkt oyun başlayacak.
* Oyun ekranında 2 tane buton olacak. \( Eski Nokia telefonlardan hatırlarsanız, sadece 3 ve 7 tuşlarıyla, yılanımızı 4 farklı yöne doğru gitmesini yönetebiliyorduk.\)
* Yılanımız her bir elmasını \( evet o kırmızı şey çok belli olmasa da bir elma \) yediğinde hızı artarak devam edecek.
* Yılan kendi kuyruğuna çarpınca da oyun bitecek, yeni bir yüksek skor yapılmışsa, yeni skor, telefona kaydedilecek.

Hadi başlayalım :\)

**1\) **Projemizi react-native-cli ile yaratalım.

> `react-native init crazysnake `

**2\) **Projemizin klasörüne girip \(cd crazysnake\), kullanacağımız dependency paketlerini indirelim. 

> `yarn add lodash mobx mobx-react react-native-navigation`
>
> `yarn add --devbabel-plugin-transform-decorators-legacy`

**3\) ** babelrc dosyamızı düzenleyip, decorator'lerin çalışmasını sağlayalım.

> `{`
>
> `'presets': ['react-native'],`
>
> `'plugins': ['transform-decorators-legacy']`
>
> `}`

**4\) **Mobx için yaptıklarımızdan sonra react-native-navigation paketine geçelim. Bu pkaet wix ekibinin yazdığı native bir kütüphane olduğundan entegrasyonu anroid ve ios için ayrı ayrı yapmak gerekiyor. Bu paketin kurulumunu daha önce yazdığımız [şu makalede](https://ysfzrn.gitbooks.io/react-native-turkce/navigation/wixreact-native-navigation.html) bulabilirsiniz. Buradaki adımları eksiksiz yaptığınıza emin olun.




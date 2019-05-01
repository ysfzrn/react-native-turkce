# Android Apk Oluşturma

Aslında apk oluşturma aşamasına geldiyseniz, bu kısmın react-native ile bir alakası yok. Android geliştiricilerin yaptıklarını yapacağız burada. Bunun 2 yöntemi var, Android Studio yardımı ile yapabilirsiniz ya da direkt komut satırını da kullanabilirsiniz. Biz burada, daha kolay geldiği için komut satırıyla adım adım uygulamamızın apk sını oluşturacağız.

> Android'in kendi [resmi sayfasından](https://developer.android.com/studio/publish/app-signing.html) daha detaylı bakabilirsiniz.

Diyelim ki bizim **myapp** isimli bir uygulamamız olsun.

1. Uygulamamızın klasörüne `cd` komutu ile girelim ve `react-native run-android` komutu ile react-native packager'ı çalıştıralım.
2. .../workspace/myapp/android/app/src/main\` klasörünün altına eğer yoksa \`assets\` isimli bir klasör oluşturalım.
3. Uygulamamızın bundle ını assets klasörüne indirmek için şu kodu komut satırında çalıştıralım.

   **curl "**[http://localhost:8081/index.android.bundle?platform=android](http://localhost:8081/index.android.bundle?platform=android)**" -o "android/app/src/main/assets/index.android.bundle"**

4. Şimdi komut satırında android klasörüne girip \\( cd \`.../workspace/myapp/android\` \\) aşağıdaki kod ile unsigned edilmemiş apk mızı oluşturalım.

   `./gradlew assembleRelease`

5. Şimdi oluşturduğumuz apk'yı signed edebilmek için kendimize bir keytool üretelim ve \*\*bu keytool'u her zaman bulabileceğimiz, şifresini asla unutmayacağımız biçimde\*\* bir klasörde muhafaza edelim. Nerde hangi klasörde yaratacağınız tamamen size kalmış.Keytool üretmek için aşağıdaki kodu komut satırında çalıştıralım.

   `keytool -genkey -v -keystore my-keystore.keystore -alias name_alias -keyalg RSA -validity 10000`

   > my-keystore: keytool'unuz klasör yapısında göreceğiniz ismi, başka isim verebilirsiniz. name\_alias: bu çok spesifik bir şey olmalı. Bir sonraki adımda ihtiyacımız olacak.

6. Şimdi uygulamamızı ürettiğimiz keytool ile signed edelim. \(_&lt;keytool-alias&gt; 'ı bir önceki adımdan hatırlayın\)_

   `jarsigner -verbose -keystore <keystore_path.keystore> <...workspace/myapp/android/app/build/outputs/apk/app-release-unsigned.apk> <keytool-alias>`

7. Şimdi sıra uygulamamızın apk'sına isim vermeye. Bunun için de sadece yapmanız gereken tabi ki yine aşağıdaki kodu çalıştırmanız. \(Bizim uygulamamızın ismi myapp olduğu için ben myapp verdim\)

   `cd ...app/build/outputs/apk`

   `zipalign -f -v 4 app-release-unsigned.apk myapp.apk`

8. Bu kadar, artık ürettiğiniz apk'yı ister telefonunuza yükler kullanırsınız isterseniz bir [Google Play Developer](https://play.google.com/apps/publish/signup/) hesabı açıp store'a koyabilirsiniz.


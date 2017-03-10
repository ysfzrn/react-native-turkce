# REACT NATİVE BAŞLANGIÇ

Kitabın öncesinde React JS için bahsettiğimiz ne varsa, Lifecycle methodlar, state , props mantığı,  statefull veya stateless componentler ve daha bir sürü şey React Native için de geçerli. Tabi React Native'de bir DOM yapımız yok. Mobil işletim sistemlerinde bunu çözmek için React Native takımı, şöyle bir yol geliştirmiş.

Android ve IOS da hali hazırda olan native componentler için araya bir köprü koymuş. Bu köprü sayesinde, bu componentleri Javascriptin algılayabileceği hale getirmiş. Şimdi o bazı componentlerin html tagleri karşılıklarına bakalım.

\(React Native 0.42 için geçerlidir\)

| **HTML** | **React Native** |
| :---: | :---: |
| **div** | **View** |
| **button** | **Button** |
| **img** | **Image** |
| **input** | **TextInput** |
| **label** | Text |

React JS yazarken, kullandığımız temel html taglerini nerdeyse aynı özellikleriyle React Native'de de kullanabiliyoruz.

\( button ve label ile ilgili bazı söylenmesi gereken şeyler var fakat Temel Componentler kısmında oraya değinelim \)

Şimdi bir tane  işin içine Redux, Navigation, rest karıştırmadan klasik olarak basit bir Todo List uygulamasını React Native'de adım adım yapalım.

Global olarak RN cli'yi yüklemediyseniz ya da nerdeyse 2 haftada bir versiyonu çıkıyor güncel versiyon için ne olur ne olmaz diye global olarak cli'yi tekrar yükleyelim.



> npm install -g react-native-cli

    Projenizde bir üst versiyona geçmek için `react-native upgrade` komutunu çalıştırabilirsiniz.



Workspace klasörümüzde projemizi başlatalım

> react-native init mytodolist



Projemizin içine girelim \(bunu yazıyorum çünkü olduğu yerde run etmeye çalışıp hata alıyor diyenler oluyor \)

> cd mytodolist



Projemizi run ederken, aşağıdaki komutları kullanıyoruz.

Android için  \( Genymotion ı veya emulatorü açtın mı,  java adresini doğru verdin mi ? \)

> react-native run-android

IOS için       \( Xcode yüklü ise sadece aşağıdaki kodu çalıştır \)

> react-native run-ios
















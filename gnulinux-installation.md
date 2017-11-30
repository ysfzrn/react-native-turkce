# Gnu/Linux Installation

Bu kurulum anlatılımı, popüler Linux dağıtımı olan Ubuntu işletim sistemine göre uyarlanmıştır. 

## 1 - Nodejs ve NPM kurulumu
Linux için kurulum yapmak için öncelikle terminal'i (Türkçe sistemlerde uçbirim) açıyoruz. Ardından Ubuntu repositorylerinin (Yazılımların adreslerinin bulunduğu adres depoları) güncellemek için şu komutu terminale giriyoruz. 

'sudo apt-get update'

Şimdi repolarımızı güncellediğimize göre nodejs ve npm (nodejs package manager) kurulumuna başlayabiliriz. Aşağıdaki komuta birde resimde bulunmayan yüklemeyi otomatik onaylamak için -y komutu ekledim. Kurulum için şu komutu terminalde çalışıtırın. 

'sudo apt-get install -y nodejs npm'

![NPM Kurulumu](/assets/npmvenodejskurulum.png)

Daha sonra npm paketlerimizi yüklerken nodejs'in çağrılırken sorun çıkarmaması için şu komutu terminalde çalıştıralım. Bu komut node ve nodejs ile hard link adı verilen bir ilişki (Windows ortamındaki kısa yol  gibi düşünün) oluşturarak iki komut çağrıldığındada aynı şekilde çalışmasını sağlar.

'ln -s /usr/bin/nodejs /usr/bin/node'

### - React Native CLI Kurulumu
React Native komutlarımınızı çalıştığımız paketi yüklemek  için aşağıdaki komutu terminalden çalıştırın. 

'sudo npm install -g react-native-cli'

![](/assets/LinuxReactNativeCLI.png)

## 2 - JDK Kurulumu
Android emülatörümüzün çalışabilmesi için JDK'nın (java developer kit)bilgisayarımızda kurulu olması gerekiyor. Yalnız öncelikle uyarmak isterim ki React Native JDK olarak OpenJDK değil OracleJDK kurulmasını istiyor. Bu yazının yazıldığı tarihte en güncel Java sürümünün Java 9 ve React Native'in resmi sitesinde Java 8 ve üzeri JDKları desteklediğini söylemesine karşın, Java 9 kurulumundan sonra gradle(ilerde nedir öğrenceksiniz) tabanlı sorunlar olması nedeniyle burada Java 8 JDK anlatılacaktır. 

Aşağıdaki kodu terminelden çalıştığımızda oracle java reposunu sitemimize eklemiş oluyoruz.

'sudo add-apt-repository ppa:webupd8team/java -y'

Yeni bir repo eklediğimizden ötürü sistemimizi şu komutla güncellememiz gerekiyor. Bu işlem biraz zaman alabilir.

'sudo apt-get update'

Sonunda Java kurulumu yapacağımız komutu çalıştırıyoruz. Bu işlem biraz zaman alabilir.
 
'sudo apt-get install oracle-java8-installer' 

Şimdi yüklemiş olduğumuz java sürünü sistemin ortam değişkenlerine kaydedeceğiz. Bunun için şu komutu çalıştırın.

'sudo apt-get install oracle-java8-set-default'

## 3 - Android Studio Kurulumu
Android SDK'leri (Software development kit / Yazılım geliştirme kiti) ve AVD (Android virtual machine) içerdiğinden dolayı Android Studio'yu kurmamız gerekmektedir. Öncelikle [Buradan](https://developer.android.com/studio/index.html) güncel Android studio sürmünü masaüstümüze indirelim. Sonra masaüstümğze gelip sağ --> Open Terminal sonra aşağıdaki komutu çalıştıralım. 

'unzip indirdiğimiz dosyanınn adı -d $HOME' 

Örnek olarak,

'unzip android-studio-ide-171.4443003-linux.zip -d $HOME'

Bu komut indirdiğimiz zip paketinin içesindekileri Home klasörüne çıkartıyor. Şimdi kurulum scriptinin olduğu klasöre gidelim. Bunun için şu kodu çalıştıralım. 

'cd $HOME/android-studio/bin'

Sıra geldi Scriptimizi çalıştırmaya...

'./studio.sh'

Şimdi karşımıza çıkan ekranlarda üzerini sarı ile çizdiğim yerleri seçerek devam ediyoruz.
![](/assets/LinuxAndroidStudioSettingLocation.png)

![](/assets/LinuxAndroidStudioSetup1.png)

![](/assets/LinuxAndroidStudioSetup2.png)

![](/assets/LinuxAndroidStudioSetup3.png)

![](/assets/LinuxAndroidStudioSetup4.png)

![](/assets/LinuxAndroidStudioSetup5.png)

![](/assets/LinuxAndroidStudioSetup6.png)

Bu adım internetten veri indirdiği için uzun sürebilir lütfen sabırlı olun. 

![](/assets/LinuxAndroidStudioSetup7.png)

Şimdi çıkan ekranda Configure --> Create Desktop Entry yoluna tıklayınız. Daha sonra şifrenizi soran pencereye şifrenizi girip ok butonuna basınız. 

![](/assets/LinuxAndroidStudioSetup8.png)

Artık Android Studio'yu kullanmaya başlayabiliriz. Uygulamar arasında "Android Studio" diye aratın ve Android Studio logosuna çift tıklayın. 


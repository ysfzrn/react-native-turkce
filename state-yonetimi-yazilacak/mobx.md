# MOBX \( State yönetmenin en kolay yolu \)

Mobx, reactive programlama felsefesiyle yazılmış rakiplerine göre öğrenme süresi çok kısa olan, basitliğiyle dikkat çeken bir state yönetim kütüphanesi.

> Reactive Programlama nedir ?  \( [ekşi sözlükte](https://eksisozluk.com/reactive-programlama--5205874) okuduğum tanım, bayağı sade olduğu gibi aşağıya yazıyorum \)
>
> Bir degisikligin veri akışında yayılımı üzerine kurulmuş programlama paradigması.örneğin normalde a = b+c ile b ile c nin toplamı a'ya atanır ve b veya c degistiginde a degismez. Reactive programlamada ise b veya c degistiginde bu değişiklik a'ya da yansıtılır ve toplam değişmiş olur.

Mobx'in arkasında yatan mantık da aynı. Uygulamanın state'inde herhangi bir şey değişmişse, değişime göre otomatik olarak yeni state üretilir. React içinde de state değiştiğinde önyüz tekrar render edilir. Hal böyle olunca da, Mobx ve React harika bir ikili oluveriyorlar. Şimdi mobx'de ki temel kavramları basit bir TODO List örneğiyle açıklayalım. 

> Biliyorum, todo list örneği görmekten belki bir kısmınıza kusma geliyor olabilir ama söz bundan sonraki örneğimizi bir react-native, mobx ve react-native-navigation kullanarak bir oyun geliştirerek yapacağız.




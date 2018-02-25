# FONKSİYONEL PROGRAMLAMA NEDİR

React Native ile ilgili bir içerikte bu konunun ne işi var demeyin sakın. Çünkü yeni javascript dünyasında son 3-4 senedir yazılan kodlar buradaki prensiplere dayanarak yazılmakta. Yazılan kodları anlayabilmek adına, bu prensipler nedir hem okurken, hem de yazarken göz ardı etmemiz gereken bilgiler. Ayrıca Redux ile ilgili bir yazı yazmak istiyordum fakat fonksiyonel programlamaya değinmeden Redux'ı öğrenmeye kalkmak biraz bir şeyler çalışıyor ama nasıl çalışıyor anlamıyorum moduna itiyor insanı ve Redux kullanmanın avantajlarını ve dezavantajlarını tam olarak göremiyoruz. Bu yüzden Redux öğrenmekten çok daha kıymetli olacağını düşündüğüm fonksiyonel programlama ile ilgili bir şeyler karalamak istedim.

Bu seride yazılacak bilgiler [şuradaki](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0) makalenin birebir değil ama kısmen çevirisi sayılabilir.

**Fonksiyonel Programlama**, side effect’lerden, mutable data kullanmaktan, ortak state paylaşımı yapmaktan kaçınarak, pure fonksiyonları birleştirerek \( composition \), uygulama yapma silsilesidir.  Fonksiyonel programlama, imperative değil, declarative'dir ve uygulamanın state'i pure fonksiyonlar boyunca akar. Object oriented programlamanın tersine, uygulamanın state'i objelerin içinde olur ve fonksiyonlar yardımıyla paylaşılır. _\(Burada kullanılan, İngilizce terimleri yazı boyunca açıklamaya çalışacağım\)_

Fonksiyonel programlama, object oriented programlama ve procedural programlama gibi bir yaklaşım, paradigmadır.

###### Fonksiyonel programlamanın temel prensipleri

* Pure fonksiyonlar
* Composition
* Ortak state kullanımından kaçınma
* State mutate etmekten kaçınma
* Side effect'lerden kaçınma

#### Pure Fonksiyonlar

Pure fonksiyonların tanımına bakarsanız şöyle bir şey görürsünüz, \_verilen bir inputa göre, her zaman aynı output'u dönen fonksiyonlar, pure fonksiyonlardır.  \_Bu tanım ilk bakışta çok anlamsız gelebilir ki bana öyle geliyordu.

Mesela şöyle bir fonskiyonumuz olsun;

```js
//es5
function double(x){
    return x * 2
}

//es6
const double = x => x * 2  
```




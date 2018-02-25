# FONKSİYONEL PROGRAMLAMA NEDİR

React Native ile ilgili bir içerikte bu konunun ne işi var demeyin sakın. Çünkü yeni javascript dünyasında son 3-4 senedir yazılan kodlar buradaki prensiplere dayanarak yazılmakta. Yazılan kodları anlayabilmek adına, bu prensipler nedir hem okurken, hem de yazarken göz ardı etmemiz gereken bilgiler. Ayrıca Redux ile ilgili bir yazı yazmak istiyordum fakat fonksiyonel programlamaya değinmeden Redux'ı öğrenmeye kalkmak biraz bir şeyler çalışıyor ama nasıl çalışıyor anlamıyorum moduna itiyor insanı ve Redux kullanmanın avantajlarını ve dezavantajlarını tam olarak göremiyoruz. Bu yüzden Redux öğrenmekten çok daha kıymetli olacağını düşündüğüm fonksiyonel programlama ile ilgili bir şeyler karalamak istedim.

Bu seride yazılacak bilgiler [şuradaki](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0) makalenin birebir değil ama kısmen çevirisi sayılabilir.

**Fonksiyonel Programlama**, side effect’lerden, mutable data kullanmaktan, ortak state paylaşımı yapmaktan kaçınarak, pure fonksiyonları birleştirerek \( composition \), uygulama yapma silsilesidir.  Fonksiyonel programlama, imperative değil, declarative'dir ve uygulamanın state'i pure fonksiyonlar boyunca akar. Object oriented programlamanın tersine, uygulamanın state'i objelerin içinde olur ve fonksiyonlar yardımıyla erişilir. _\(Burada kullanılan, İngilizce terimleri yazı boyunca açıklamaya çalışacağım\)_

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

`double` fonksiyonu nereden çağrırılırsa çağırılsın, herhangi bir şeye bağımlı olmadan, aynı input'ta aynı sonucu verecektir. `double(5)` her yerde 10 sonucunu verecektir. 10 gördüğünüz yere `double(5)` yazmanız hiçbir şey farkettirmeyecektir. Çünkü double\(5\) bir pure fonksiyondur. Karşı örnek olarak en basitinden `math.random( )` fonksiyonu çağrıldığı her yerde, zaten input almıyor, farklı sonuç verecektir. Dolayısıyla `math.random( )` ' a pure fonksiyon diyemeyiz.

Pure fonksiyonların bir diğer özelliği, **hiçbir side effect\( yan etkileri \)'leri yoktur. **Yani dışarıdan bir müdahele ile değiştirilemezler

Javascript'te bir objenin argümanları, özellikleri, o objenin referanslarıdır. Eğer bir fonksiyon dışarıdan o objenin referanslarını değiştirirse bu objeyi mutasyona uğratmak yani mutate etmek yani değiştirmek artık ne derseniz deyin, bu durum pure fonksiyonlarda asla olmaması gereken bir durumdur. \( Redux'la beraber çalışırsanız göreceğiniz en keskin kural Redux'ta kullanacağınız reducer'ları pure fonksiyonlardan oluşturmanız gerekmektedir \).

Örneğin; aşağıda kullanılan push methodu mutate bir fonksiyondur. Ve objenin referanslarını değiştirir. Bu istenmeyen bir durum yaratabilir. Kodun tahmin edilebilirliğini ve güvenilirliğini kötü etkileyebilir. 

```js
const addToCart=(cart, item)=>{
   cart.push( item );  // push mutate bir fonksiyon
   return cart;
}
```

Bu fonksiyonu immutate yapıp pure fonksiyona dönüştürmek için, push yerine Object Assign veya array spread operator kullanılabilir.

```js
//es5
const addToCart=(cart, item)=>{
   Object.assign(card, { item })  //Object Assign, immutate bir fonksiyondur.push yerine kullanılabilir
   return cart;
}
```

veya

```js
//es6
const addToCart=(cart, item)=>{
   cart = [...cart, item ]  //array spread operator ile de pure fonksiyon yaratılabilir.
   return cart;
}
```

Örnek olarak [şuradaki](https://codepen.io/ericelliott/pen/MyojLq) impure fonksiyonu, pure yapmaya çalışabilirsiniz. 

Bir sonraki yazıda React'ın temellerini oluşturan, composition mantığından yani fonksiyon birleştirmekten bahsedeceğiz.


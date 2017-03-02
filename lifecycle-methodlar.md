# Lifecycle Methodlar

`React.Component bizim`en temel class'ımız. Her componentin kendine özel lifecycle methodları var. Bu methodları 3 grupta topluyoruz.

> **Bir componentin mount edilmesi ne demek ?**
>
> Browserların bir DOM \(Document Object Model \) yapısı var. Bu document &lt;html&gt; tag i başlayıp sırayla, &lt;head&gt;, &lt;body&gt; diğer html taglariyle devam ediyor. Bir component mount edildi demek de  componentin DOM'a yerleştirilmesi, browser da görünmesi demek.

### Mounting Methodlar

Bir component DOM'da yaratılınca, insert edilince çağrılan methodlardır

* constructor
* componentWillMount
* render\( \)
* componentDidMount

### Updating Methodlar

Bir componentin state ya da propsunun değişmesiyle, tekrardan render edilmesi sonucu çağrılan methodlardır.

* componentWillReceiveProps\( \)
* shouldComponentUpdate\( \)
* componentWillUpdate\( \)
* render\( \)
* componentDidUpdate\( \)

### Unmounting Methodlar

Bir component DOM'dan çıkarılmak üzereyken çağrılan fonksiyondur.

* componentWillMount



##### **constructor\( \)**

```js
 constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
} 
```

constructor methodu içinde componentin propslarını kullanmak isterseniz, `super(props) '`u  çağırmalısınız. Aksi takdirde props'ları görmeyecektir. Diğer methodlardaki gibi `this.props`'u bu method için kullanamazsınız. Onun yerinde yukarıda örnekte olduğu gibi `props.initialColor` gibi kullanabilirsiniz.

Başlangıç state'i tanımlamak ve bir fonksiyonu componente bind etmek için en uygun yer constructor. Bu iki tanımlamaya ihtiyacınız yoksa constructor methodunu tanımlamayabilirsiniz.



##### **componentWillMount\( \)**

Component mount edilmeden hemen önce çağrılır. Dolayısıyla render\(\) methodundan da önce çağrılır. Bu yüzden bu method içinde state değiştirip, render\(\) methodunu tekrardan çalıştırmamalıyız. Önerileni, bu methodu sadece server rendering yaparken kullanmamız. render\(\) 'dan önce çalışmasını istediğiniz bir fonksiyon varsa constructor\(\) buradan daha uygun bir yer



##### **render\( \)**

React.Component class'ı için olmazsa olmaz, çağrılmazsa sorun çıkaracak tek method `render( )`

Bu method çağrılınca karşılığında size, ona verdiğiniz props ve state'e göre  &lt;div /&gt;, &lt;img /&gt; gibi sizin tanımladığınız bir DOM componentinin görünümü verecektir.

render\( \) methodunda, eğer siz hiçbir şey mount etmek istemiyorsanız  dönüş değeri olarak,  **null**, **false **değerlerini de verebilirsiniz. Higher order component'lerde veya koşullu bir componenti render ederken çok işinize yarayacaktır.

render\( \) methodu pure olmalı. React ile uğraşırken bu cümleyi çok görürsünüz. state' e göre size dönüş üreten bir fonksiyon içinde state değiştirmek, sizi ve browser'ınızı sonsuz döngüye sokacaktır.

render\( \) methodu DOM elementlerinin bir görüntüsünü, yani asıl tabiriyle virtual, sanal DOM size üretir. Eğer DOM yapısına direkt müdahele etmek istiyorsanız, render\( \) methodu yerine componentDidMount\( \) methodu kullanmanız önerilir.     ** **


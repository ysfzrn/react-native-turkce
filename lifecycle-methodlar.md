# Lifecycle Methodlar

`React.Component bizim `en temel class'ımız. Her componentin kendine özel lifecycle methodları var. Bu methodları 3 grupta topluyoruz.

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




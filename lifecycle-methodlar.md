# Lifecycle Methodlar

<code>React.Component</code> bizim en temel class'ımız. Her componentin kendine özel lifecycle methodları var. Bu methodları 3 grupta topluyoruz.

> **Bir componentin mount edilmesi ne demek ?**
>
> Browserların bir DOM (Document Object Model ) yapısı var. Bu document &lt;html&gt; tag i başlayıp sırayla, &lt;head&gt;, &lt;body&gt; diğer html taglariyle devam ediyor. Bir component mount edildi demek de  componentin DOM'a yerleştirilmesi, browser da görünmesi demek.

### Mounting Methodlar

Bir component DOM'da yaratılınca, insert edilince çağrılan methodlardır

* constructor( )
* componentWillMount( )
* render( )
* componentDidMount( )

### Updating Methodlar

Bir componentin state ya da propsunun değişmesiyle, tekrardan render edilmesi sonucu çağrılan methodlardır.

* componentWillReceiveProps( )
* shouldComponentUpdate( )
* componentWillUpdate( )
* render( )
* componentDidUpdate( )

### Unmounting Methodlar

Bir component DOM'dan çıkarılmak üzereyken çağrılan fonksiyondur.

* componentWillMount

-----------------------------

##### constructor( )

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

##### componentWillMount( )

Component mount edilmeden hemen önce çağrılır. Dolayısıyla <code>render()</code> methodundan da önce çağrılır. Bu yüzden bu method içinde state değiştirip, <code>render()</code> methodunu tekrardan çalıştırmamalıyız. Önerileni, bu methodu sadece server rendering yaparken kullanmamız. <code>render()</code> 'dan önce çalışmasını istediğiniz bir fonksiyon varsa <code>constructor()</code> buradan daha uygun bir yer

##### render( )

`React.Component` class'ı için olmazsa olmaz, çağrılmazsa sorun çıkaracak tek method `render()`

Bu method çağrılınca karşılığında size, ona verdiğiniz props ve state'e göre  &lt;div /&gt;, &lt;img /&gt; gibi sizin tanımladığınız bir DOM componentinin görünümü verecektir.

`render( )` methodunda, eğer siz hiçbir şey mount etmek istemiyorsanız  dönüş değeri olarak,  **null**, **false **değerlerini de verebilirsiniz. Higher order component'lerde veya koşullu bir componenti render ederken çok işinize yarayacaktır.

`render( )` methodu **pure** olmalı. React ile uğraşırken bu cümleyi çok görürsünüz. state' e göre size dönüş üreten bir fonksiyon içinde state değiştirmek, sizi ve browser'ınızı sonsuz döngüye sokacaktır.

`render( )` methodu DOM elementlerinin bir görüntüsünü, yani asıl tabiriyle virtual, sanal DOM size üretir. Eğer DOM yapısına direkt müdahele etmek istiyorsanız, <code>render()</code> methodu yerine <b>componentDidMount()</b> methodu kullanmanız önerilir.

##### componentDidMount( )

Component mount edildiğinde apar topar çağrılan methodumuz. DOM'a etki eden bir şeyin tanımlanması gereken yer burası. Atıyorum <b>D3.js</b> gibi third party bir library kullanacaksanız, başlangıç değerlerini burada atayın. Bir API çağıracaksanız, bir şeyi fetch edecekseniz en müsait yer bu method.

##### componentWillReceiveProps( nextProps )

Component yeni, değişmiş props'unu alıp mount edilmeden önce çağrılır. Eğer props'a göre state değiştirmeye ihtiyaç duyarsanız, aşağıda örnekteki gibi kullanabilirsiniz;

```js
componentWillReceiveProps( nextProps ){
    if(this.props.open !== nextProps.open){
        this.setState({ reset : true })
    }
}
```

React, bu methodu component yeni props almasa da çağırabilir, bunun bir garantisi yok. O yüzden nextProps ve this.props karşılaştırmasına göre işlem yapmanız avantajınıza olacaktır.

React bu methodu başlangıçta çağırmaz. Sadece component mount edildikten sonraki süreçte update olursa bu method çalışır.

##### shouldComponentUpdate( nextProps, nextState )

Yeni props ve state alınıp, render edilmeden önce bu method çağrılır. React ' da herhangi bir state değişince otomatik olarak render methodu çalışır. Bu da aslında kendisiyle alakası olmayan state değişiminden, başka bir componentin etkilenmesi ve boşuna render edilip,  zaten hali hazırda olan aynı görseli oluşturması demek. Bu yüzden props veya state değişikliği, ilgili componentinizi etkilemiyorsa,  bu methodu kullanıp, ciddi bir performans kazanabilirsiniz.

```js
shouldComponentUpdate( nextProps, nextState ){
    if(nextState.open === this.state.open){   
           return false    //"open" state i değişmediyse componenti tekrar render etme
    }
}
```

> shouldComponentUpdate false dönerse, componentWillUpdate\(\), render\(\) ve componentDidUpdate\(\) methodları çalışmaz

##### componentWillUpdate( nextProps, nextState )

Bu method, yeni props ve state alındığında, <code>render()</code> dan önce çalışır. React bu methodu başlangıçta çağırmaz.  Bu method içinde state set edilmez. Bunun yerine önceden bahsettiğimiz <code>componentWillReceiveProps()</code> kullanılabilir.

##### componentDidUpdate( prevProps, prevState )

shouldComponent false dönerse bu method çağrılmaz.

render methodundan sonra hemen çağrılan methoddur. Hali hazırdaki props değeri ile bir önceki propsu karşılaştırmak için idealdir. Props değişikliği varsa network istekleri de buradan yapılabilir.

##### componentWillUnmount( )

Şahsen benim abi bu method çok iyi ya dediğim ama nadiren kullandığım bir fonksiyon. Componentimiz artık yok edilecekken, yok edilmeden hemen önce çağrılan method. Componentin başında yaptığımız bir listener'ı kaldırmak için, önceden yapılmış bir network requestini iptal etmek için, özetle bir şeyleri sıfırlamak için çok ideal bir yer. Component'e gitmeden önce hesap sorduğumuz yer diyelim.

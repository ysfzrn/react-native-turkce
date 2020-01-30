# Lifecycle Methodlar

`React.Component` veya `React.PureComponent`bizim en temel class'ımız. Her componentin kendine özel lifecycle methodları var. Bu methodları 3 grupta topluyoruz.

> **Bir componentin mount edilmesi ne demek ?**
>
> Browserların bir DOM \(Document Object Model \) yapısı var. Bu document &lt;html&gt; tag i başlayıp sırayla, &lt;head&gt;, &lt;body&gt; diğer html taglariyle devam ediyor. Bir component mount edildi demek de componentin DOM'a yerleştirilmesi, browser da görünmesi demek. Tabi react native'de browser DOM yapısı yok ama biz yine de component'in ekranda görünmesini React.Js den gelen terminolojiyle DOM'da göründü DOM'dan kayboldu diyerek tanımlayacağız.

## Mounting Methodlar

Bir component DOM'da yaratılınca, insert edilince çağrılan methodlardır

* constructor\( \)
* render\( \)
* componentDidMount\( \)
* componentDidUpdate\( \)

## Updating Methodlar

Bir componentin state ya da propsunun değişmesiyle, tekrardan render edilmesi sonucu çağrılan methodlardır.

* static getDerivedStateFromProps\(\)
* shouldComponentUpdate\( \)
* getSnapshotBeforeUpdate\(\)
* render\( \)
* componentDidUpdate\( \)
* componentWillUnmount

## Unmounting Methodlar

Bir component DOM'dan çıkarılmak üzereyken çağrılan fonksiyondur. Render fonksiyonundan önce çağrılırlar. componentWillMount'un deprecated olmasından sonra böyle bir method olarak sadece elimizde constructor kaldı

### constructor\( \)

```javascript
 constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
```

constructor methodu içinde componentin propslarını kullanmak isterseniz, `super(props) '`u çağırmalısınız. Aksi takdirde props'ları görmeyecektir. Diğer methodlardaki gibi `this.props`'u bu method için kullanamazsınız. Onun yerinde yukarıda örnekte olduğu gibi `props.initialColor` gibi kullanabilirsiniz.

Başlangıç state'i tanımlamak ve bir fonksiyonu componente bind etmek için en uygun yer constructor. Bu iki tanımlamaya ihtiyacınız yoksa constructor methodunu tanımlamayabilirsiniz.

### render\( \)

`React.Component` class'ı için olmazsa olmaz, çağrılmazsa sorun çıkaracak tek method `render()`

Bu method çağrılınca karşılığında size, ona verdiğiniz props ve state'e göre &lt;div /&gt;, &lt;img /&gt; gibi sizin tanımladığınız bir DOM componentinin görünümü verecektir.

`render( )` methodunda, eğer siz hiçbir şey mount etmek istemiyorsanız dönüş değeri olarak, **null**, **false** değerlerini de verebilirsiniz. Higher order component'lerde veya koşullu bir componenti render ederken çok işinize yarayacaktır.

`render( )` methodu **pure** olmalı. React ile uğraşırken bu cümleyi çok görürsünüz. state' e göre size dönüş üreten bir fonksiyon içinde state değiştirmek, sizi ve browser'ınızı sonsuz döngüye sokacaktır.

`render( )` methodu DOM elementlerinin bir görüntüsünü, yani asıl tabiriyle virtual, sanal DOM size üretir. Eğer DOM yapısına direkt müdahele etmek istiyorsanız, `render()` methodu yerine **componentDidMount\(\)** methodu kullanmanız önerilir.

### componentDidMount\( \)

Component mount edildiğinde apar topar çağrılan methodumuz. DOM'a etki eden bir şeyin tanımlanması gereken yer burası. Atıyorum **D3.js** gibi third party bir library kullanacaksanız, başlangıç değerlerini burada atayın. Bir API çağıracaksanız, bir şeyi fetch edecekseniz en müsait yer bu method.

### static getDerivedStateFromProps\( props, state \)

Bu methodumuz, component her yeni props aldığında çağrılır. \( render methodundan önce \) static bir fonksiyon olduğu için component'in diğer objelerine erişemez fakat props ve state diye iki parametreye sahip olduğu için, component'in geçmiş propslarını constructor methodunda state'e atıp, yeni gelen props'ları, props parametresi ile karşılaştırıp, kendi mantığınıza göre state'i güncelleyip, component'in tekrar render edilmesini ya da edilmemesini sağlayabilirsiniz. \( Diğer methodlara göre biraz karışık, bir örnekle açıklayalım \)

```javascript
static getDerivedStateFromProps(props, state){
    if( props.clickCount % 2 === 0 ){
      return {
        ...state,
        clickCount: props.clickCount
      }
    }else{
      return null
    }
  }
```

Yukarıdaki örnekte; component yeni props aldığında getDerivedStateFromProps tetiklenir. Yeni gelen props 2 ile bölünebiliyorsa, yeni gelen **clickCount** props'u **clickCount** state'ine atanır. setState methodu kullanmadan state'i değiştirmiş oluruz. State değiştiği için de render methodu tekrar tetiklenir.

**!!!** Burada unutmamanız gereken nokta, bu methodu içinde mutlaka return ifadesi olması gerektiğidir. Eğer state'inizde hiçbir değişiklik yapmayacaksanız, return null ifadesini yazmalısınız.

### shouldComponentUpdate\( nextProps, nextState \)

Yeni props ve state alınıp, render edilmeden önce bu method çağrılır. React ' da herhangi bir state değişince otomatik olarak render methodu çalışır. Bu da aslında kendisiyle alakası olmayan state değişiminden, başka bir componentin etkilenmesi ve boşuna render edilip, zaten hali hazırda olan aynı görseli oluşturması demek. Bu yüzden props veya state değişikliği, ilgili componentinizi etkilemiyorsa, bu methodu kullanıp, ciddi bir performans kazanabilirsiniz.

```javascript
shouldComponentUpdate( nextProps, nextState ){
    if(nextState.open === this.state.open){   
           return false    //"open" state i değişmediyse componenti tekrar render etme
    }
}
```

> shouldComponentUpdate false dönerse, componentWillUpdate\(\), render\(\) ve componentDidUpdate\(\) methodları çalışmaz

### getSnapshotBeforeUpdate\(prevProps, prevState\)

Her props alındığında bu method çağrılır \(render methodundan sonra\). Bu method componentDidUpdate ile kullanılmalıdır ve boolean değer dönmelidir. Bu methodun döndüğü değer componentDidUpdate'in 3. parametresi olacaktır.

Aşağıdaki örnekte, getSnapshotBeforeUpdate methodundaki koşula göre true veya false değeri dönüyor. ComponentDidUpdate methodu da getSnapshotBeforeUpdate methodundan gelen snapshot parametresi yardımıyla gerekli aksiyonu alıyor. Özellikle bir api'nin sonucuna göre animasyonu harekete geçirme için çok kullanışlı ve performanslı bir yöntem

```javascript
getSnapshotBeforeUpdate = (prevProps) => {
    const { value } = this.props;
    if (prevProps.value !== value) {
      return true;
    }
    return false;
  };

  componentDidUpdate = (prevProps, prevState, snapshot) => {
    const { animatedValue } = this.state;
    if (snapshot) {
      Animated.timing(animatedValue, {
        toValue: prevProps.value ? 0 : 100,
        useNativeDriver: true,
      }).start();
    }
  };
```

### componentDidUpdate\( prevProps, prevState, snapShot \)

shouldComponent false dönerse bu method çağrılmaz.

render methodundan sonra hemen çağrılan methoddur. Hali hazırdaki props değeri ile bir önceki propsu karşılaştırmak için idealdir. Props değişikliği varsa network istekleri de buradan yapılabilir. Ama bunu son çare olarak değerlendirin. Daha çok kullanacaksanız animasyonlar için getSnapshotBeforeUpdate methodu ile birlikte kullanın. Hele bu methodu kullanıyorsanız, state yönetimine dikkat edip, componentinizin PureComponent olmasına veya shouldComponentUpdate methodunu kullanarak doğru koşullar verdiğinize emin olun.

### componentWillUnmount\( \)

Şahsen benim abi bu method çok iyi ya dediğim ama nadiren kullandığım bir fonksiyon. Componentimiz artık yok edilecekken, yok edilmeden hemen önce çağrılan method. Componentin başında yaptığımız bir listener'ı kaldırmak için, önceden yapılmış bir network requestini iptal etmek için, özetle bir şeyleri sıfırlamak için çok ideal bir yer. Component'e gitmeden önce hesap sorduğumuz yer diyelim.


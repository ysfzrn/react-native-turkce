# State Mantığı

State tanımı,  componentimizin her sıfırdan çağrıldığında çalışacak olan constructor fonksiyonunda yapılır.

_\( Sıfırdan çağrılmasından kastım daha mount,render edilmemiş bir componentin çağrılması. Yoksa hali hazırda ekranımızda olan mount edilmiş bir componentte constructor tekrar tekrar çağrılmaz \)_

```js
class MyComponent extends React.Component {
    constructor(props) {
      super(props);

      this.state = {
          open:false
      };
    }
  ...
}
```

State ne yapar;  statefull component denilen, kendi içinde state tanımı mevcut olan componentin her state'i değiştiğinde componentin **render** fonksiyonunun tekrardan çalışmasını sağlar.

**Stateless Component Örneği**

```js
const MyComponent = (props)=>{
  return( 
     <div>...</div>   //state'in değiştireceği bir render methodu yok
  )
} 
```

**Statefull Component Örneği**

```js
class LinkButton extends React.Component {
    render() {            //her state değiştiğinde render() tekrar tekrar çalışacak
        return (
            <div>...</div>
        );
    }
}
```

![](/assets/Desktop22.png)

Bir önceki sayfadaki ekranımızı,  propslar yardımı ile jsbin de composition yaparak oluşturalım.  
[source code](http://jsbin.com/mebesol/12/edit?js,output)

Burada üç tane stateless \(bazıları **dumb component **diyor\) component, **FlexItem, FlexColumn, FlexRow** var.

Bir tane de state almaya müsait statefull \( bazıları **container component **diyor \) component, **MyComponent** var.

Burada herhangi bir asenkron state değişimi sorunu ile karşılaşmamanız adına en dıştaki, hiyerarşide en üstte bulunan componentte state i belirleyip değiştirip, alttaki componentlere dağıtmanız. Aksi takdirde aşağıdan yukarı bir state yönetimi Redux gibi state yönetimini kullanmadan neredeyse imkansız.

Şimdi bir tane state tanımlayalım ve bir timer yardımıyla saniyede bir arttıralım. Alttaki componentlere dağıtalım her componentte kendi içinde state'i nasıl değerlendirir, nasıl şekil alır o componentin davranışına kalsın.




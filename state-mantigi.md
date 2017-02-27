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

Bir önceki sayfadaki ekranımızı propslar yardımı ile jsbin de composition yaparak oluşturalım.


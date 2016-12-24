### ES6 Örnekler

**Block Scoped Declaration**

```javascript
const a = 1
let b = 'foo'

// Not allowed!
// a = 2

// Ok!
b = 'bar'

if (true) {
  const a = 3
}
```

**const** ve **let **ifadelerine bakalım. Normal JavaScript'te bu ifadeler yerine değişkenleri tanımlamak için **var** kullanılır. Aralarındaki temel fark **const **ve** let **ifadeleri yalnızca bulundukları blok için geçerlidir.

**const **bildiğimiz "CONSTANT" kelimesinin kısaltması, sabit yani tanımlandığı blokta kendisine yalnızca bir kere atama yapılabilir

**let ** ifadesi değişkenler için uygundur.  Yukarıdaki örnekte üst blokta tanımlanmış **const a **ifadesine bir kere atama yapılmış daha sonra başka bir değişiklik yapılınca buna izin vermemiş. ama farklı bir blokta tekrar atama yapılabilmiştir.   ** **

Naber

[source code](http://jsbin.com/xezun/1/embed?js,console)


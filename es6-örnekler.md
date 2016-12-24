# ES6 Örnekler

\(**UYARI:** Bu sayfaya JS Bin'den kod gömülmüştür. Kullandığınız browser açmıyorsa lütfen adres çubuğundan **load unsafe script **seçeneğini seçiniz. \)

### **Block Scoped Declaration**

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

### **Arrow Function \(=&gt;\)**

Arrow ya da fat arrow function denilen \(=&gt;\) bu fonksiyon başta kafa karıştırıcı olabilir. Normal bir fonksiyondan temel anlamda bir kaç tane farkı vardır.

Birincisi; ES5 ile yazılmış bir JavaScript kodunda, bir fonksiyonu bind etmek için kullandığımız, `this` kodu eğer biz arrow function kullanıyorsak gerekli değildir. Çünkü arrow function için fonksiyonun kendi bloğu ya da dış blok aynı kapsamdadır.Bir örnekle bakalım;

```javascript
//ES5:
 function merhaba(){
    this.msj = 'Merhaba';
    this.selamver = function(adi){
       return this.msj + adi
    }.bind(this)
 }

 //ES6:
 function merhaba(){
    this.msj = 'Merhaba';
    this.selamver = (adi) =>{
       return this.msj + adi
    }
 }
```

ES5 ile yazılan örnekte `selamver` fonksiyonu kendi üst bloğundaki `msj` 'a ulaşmak için `bind` edilmesi gerekirken; ES6 ile yazılan halinde, arrow function kullanılmış ve `bind` edilmesine gerek kalmadan `msj` değişkenine ulaşılmıştır

İkincisi; Eğer fonksiyonunuz bir parametre alıyorsa süslü parantez `{}` ve `return` kullanmanıza ihtiyaç yok. Kullanırsanız da bir şey değişmez. Örneğin;

```javascript
var half = function(x){
  return x/2
}

var halfES6 = (x)=>{
  return x/2
}

var halfES6_2 = ( x => x/2 )


half(6);     //3
halfES6(8);  //4
halfES6_2(10); //5
```

[sourcecode]('http://jsbin.com/docuhuy/2/edit?html,js,console')



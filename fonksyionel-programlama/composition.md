# FONKSİYONEL PROGRAMLAMA VE COMPOSITION

Composition, iki ya da daha fazla fonksiyonu birleştirerek yeni bir fonksiyon yaratma işlemi.

Matematikten, f\(\) ve g\(\) fonksiyonlarını düşünelim.f\(g\(x\)\) , bir composition işlemidir. Ve işlem sırası aşağıdaki gibidir

* x
* g
* f

Esasında, ortaokul matematiğinde gördüğümüz fog yani bileşke fonksiyonlar.

c\(x\) = f\(g\(x\)\)

Fonksiyonları birleştirirken kullanılan diğer temel fonksiyon türlerine göz atalım.

### Curry, Partial ve Pipe Fonksiyonlar

#### Curry Fonksiyon

Bir fonksiyon, input olarak bir fonksyion ve çoklu parametre alıp, output olarak **tek parametreli bir fonksiyon** dönüyorsa, bu bir **curry fonksiyondur**.

```js
const curry = fn => (...args) => fn.bind(null, ...args);
```

#### **Partial Fonksiyon**

Bir fonksiyon, input olarak, bir fonksiyon ve çoklu parametreler alır, output olarak **daha az parametreyle bir fonksiyon** dönüyorsa, bu bir **partial fonksiyondur**.

```js
const  partial=(fn, …args)=>{ fn.bind(null, …args)  }
```

#### Pipe Fonksiyon

İki ya da daha fazla fonksiyonu input olarak alıp, parametre olan bu fonksiyonları, bir önceki fonksiyonunun outputu ile sırayla çalıştırabilecek şekilde ayarlayan ve output olarak bize tek fonksiyon dönen fonksiyonlar, **pipe fonksiyonlardır.**

```js
const _pipe = (f, g) => (...args) =>  g(f(...args))
const pipe = (...fns) =>  fns.reduce(_pipe );
```

Şimdi terimsel tanımlamaları geçip, bu fonksiyonları nasıl kullanacağız ona bakalım.

İlk önce kullanacağımız standart, fonksiyonlarımızı yazalım.

```js
    const _pipe = (f, g) => (...args) => g(f(...args))
    const pipe = (...fns) => fns.reduce(_pipe);
    const partial = (fn, ...args) => fn.bind(null, ...args);
```

Şimdi de kullanacağımız pure fonksiyonlarımıza bakalım.

```js
    const add = (a, b, c = 0) => a + b + c;
    const multiply = (a, b) => a * b;
    const dec = (a) => a - 1;
```

Bunları hep beraber, comeTogether adlı fonksiyonda pipe yardımı ile  birleştirelim yani compose edelim.

```js
    const comeTogether = pipe(multiply, dec, partial(add, 10, 3));
```

Artık bir sürü iş yapan fonksiyonumuzu aşağıdaki şekilde çağırabiliriz.

```js
    const result = comeTogether(2, 4);  // 20
```

**Sahne arkasında neler olup bitti ?**

* İlk önce `multiply` fonksiyonu, 2 ve 4 ü çarpttı ve 8 buldu.
* `multiply`'dan dönen 8 değerini `dec` aldı ve 1 eksiltti, 7 buldu.
* `dec`'den dönen 7 değerini `add` fonksiyonu direkt alamadı. Çünkü o çoklu parametreyle çalışan bir fonksiyon olduğu için `partial` fonksiyonunun yardımına ihtiyaç duydu. `partial` fonksiyonu yardımı ile `add` fonksiyonu 7 yi aldı, 10 ve 3 ile toplayıp 20 değerini döndü. 

[source code](http://jsbin.com/fitilob/1/edit?js,output)


_Not: Burada bahsedilen, curry, partial ve pipe fonksiyonlarının içeriğini daha da ayrıntılı biçimde analiz edeceğimiz bir yazı yazmayı planlıyorum. Fakat bu kısıtlı zamanda şimdilik hem kendim için hem de ufak önbilgi edinmek isteyenler için bu kadarını paylaşmak istedim. Yoksa bu adı geçen fonksiyonlar yoktan var olmadı. Bilindik kısaltılmış es6 ile yazılan javascript fonksiyonları  ama ilk bakışta ne olduğu ve anlaşılması zor fakat kullanımı itibari ile standart fonksiyonlar._

Kaynak: [Master the JavaScript Interview: What is Function Composition](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-function-composition-20dfb109a1a0)


# FONKSİYONEL PROGRAMLAMA VE COMPOSITION

Composition, iki ya da daha fazla fonksiyonu birleştirerek yeni bir fonksiyon yaratma işlemi.

Matematikten, f\(\) ve g\(\) fonksiyonlarını düşünelim.f\(g\(x\)\) , bir composition işlemidir. Ve işlem sırası aşağıdaki gibidir

* x
* g
* f

Esasında, ortaokul matematiğinde gördüğümüz fog yani bileşke fonksiyonlar.

c\(x\) = f\(g\(x\)\)

Composition'ın derinlerine dalmadan esas temelleri olan, curry ve partial fonksiyonlar nedir onlara bir göz atalım.

### Curry, Partial ve Pipe Fonksiyonlar

Bir fonksiyon, input olarak çoklu parametreli bir fonksiyonu alıp, output olarak **tek parametreli bir fonksiyon** dönüyorsa, bu bir **curry fonksiyondur**.

Bir fonksiyon, input olarak, bir fonksiyon ve çoklu parametreler alır, output olarak **daha az parametreyle bir fonksiyon** dönüyorsa, bu bir partial fonksiyondur.

İki ya da daha fazla fonksiyonu input olarak alıp, parametre olan bu fonksiyonları, bir önceki fonksiyonunun outputu ile sırayla çalıştıran fonksiyonlar da pipe fonksiyonlardır.

**Partial Fonksiyon**

```js
const  partial=(fn, …args)=>{ fn.bind(null, …args)  }
```

* İlk …args \( rest operator \) gelen parametreleri bundled yapıp array’e dönüştürür
* İkinci …args \( spread operator \) , array’ı sırayla dağıtır

* İlk parametreyi null geçiyoruz, çünkü gelen context’i değiştirmek istemiyoruz. Function dönmesini sağlıyoruz.






# ListView

ListView vertical olarak sırf performans gözeterek hazırlanmış bir component. Sadece ekranda görünen componentler render, edilir, diğer componentler cihazın performansını zorlamaz. Bir update işlemi geldiyse tüm liste tekrardan render edilmez, sadece ekranda görünenler render edilir. Bu da ciddi bir akıcılık kazandırır. Ama kabul edelim, biraz syntax ı karışık gelebilir. Tek tek nasıl yazarız ona bakalım.

`ListView`, `ScrollView` gibi kendi altındaki elementleri yani children elementleri render etmez. Onun yerine `renderRow` propsununu kullanır. `renderRow` aslında `dataSource` ' dan dönen elementleri derleyen bir çıktı üreten bir fonksiyon. Örneğin, `dataSource` bir array dönüyorsa, rowData'yı aşağıdaki gibi kullanırız.

```markup
(rowData) => <Text>{rowData}</Text>
```

## DataSource

```javascript
 const ds = new ListView.DataSource({
      rowHasChanged: (r1, r2) => r1 !== r2
 });
```

ListView elementleri render ederken DataSource API'sini kullanır. DataSource input olarak bir array alır. Bu array her türlü veriyi içerebilir. rowHasChanged bizim verdiğimiz koşula göre verileri karşılaştırır ve hangilerinin render edileceğine karar verir. Yukarıdaki örnekte `(r1, r2) => r1 !== r2` olarak verilmiş ama biz `(r1, r2) => r1.id !== r2.id` olarakta tanımlayabilirdik.

Son olarak datayı `DataSource.cloneWithRows(inputArray)` olarak çağırmalısınız.

Şimdi her şeyi baştan olarak yazalım.

**1-Bizim bir elimizde array olsun.**

```javascript
import React, { Component } from "react";
import { View, Text, ListView, StyleSheet } from "react-native";

const rows = [
  { id: 1, text: "row1" },
  { id: 2, text: "row2" },
  { id: 3, text: "row3" },
  { id: 4, text: "row4" },
  { id: 5, text: "row5" },
  { id: 6, text: "row6" }
];
```

**2-Şimdi constructor ' da DataSource' u yaratalım**

```javascript
  constructor() {
    super();
    const ds = new ListView.DataSource({
      rowHasChanged: (r1, r2) => r1 !== r2
    });  
   }
```

**3-Data yı** `DataSource.cloneWithRows(inputArray)` **olarak çağıralım**

```javascript
 constructor() {
    super();
    const ds = new ListView.DataSource({
      rowHasChanged: (r1, r2) => r1 !== r2
    });
    this.state = {
      list: ds.cloneWithRows(rows)
    };
  }
```

**4- ListView kullanıma hazır**

```javascript
<ListView
       dataSource={this.state.list}
       renderRow={rowData =><Text>{rowData.text}</Text>}
 />
```

Örneğin tamamı için burayı inceleyebilirsiniz

[https://sketch.expo.io/SJxhqQRoe](https://sketch.expo.io/SJxhqQRoe)


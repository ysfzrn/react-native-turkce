# Animated.Event\(\)

Ekranı kaydırırken, scroll yaparken, Animated.Event\( \) kullanılarak animated değişkeni direkt olarak değiştirebiliriz.

Örnek olarak, biz ekranı dikey olarak scroll yaparken animated değişken olan, animatedValue'yu aşağıdaki gibi değiştirebiliriz

```jsx
<ScrollView
        style={styles.container}
        scrollEventThrottle={16}
        onScroll={Animated.event([
          { nativeEvent: { 
                contentOffset: { 
                    y: animatedValue  //yatay scroll için, y yerine x yazabilirsiniz
                } 
            } 
          }
        ])}
>
...
```

Animated.Event\( \), bizim yerimize animated değişkeni, kendi içinde setValue\(\) kullanarak değiştirmektedir. 





PanResponder ile nasıl kullandığına bakalım,

```jsx
onPanResponderMove: Animated.event([
   null,                // raw event'i null geçiyoruz
   {dx: this._pan.x, dy: this._pan.y},    // gestureState 
 ]), 
```

Burada,  ikinci parametre `gestureState` , yukarıdaki örnekte state'ler dx ve dy değerleri.  Kullanıcı ekrana ilk dokunuşundan, parmağını ne kadar kaydırırsa, ardışık olarak her harekette, state'in değerini güncelleyecektir.


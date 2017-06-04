# Animated

Anlat anlat bitmeyecek, kesin bir hiyerarşisi olmayan, resmi dökümanda dahi tüm özellikleriyle anlatılmamış en sevdiğim API. Burası benim kendi yorumum. Şimdi üçüncü ağızdan anlatmaya başlayalım.

Temel felsefe, bir tane animatedValue\(\) yarat. Bu değişkenin değerini değiştir ve componentin style objesinede değiştirdiğin değeri yaz. animatedValue'ya göre animasyon yarat.

1 - animatedValue\(\) yarat

```js
this._animatedValue = new Animated.Value(0);
```

2 - animatedValue\(\) değerini değiştir

```js
this._animatedValue.setValue(150);
```

3 - style objesine set et

```jsx
<Animated.View style={{left: this._animatedValue}} />
```




# Props Mantığı

React Js ' de her bir component aslında bir fonksiyon. Nasıl görünceklerini, ekranda nasıl bir yer kaplayacaklarını da bu fonksiyonlara verdiğimiz, input değerleriyle belirliyoruz. İşte bu input değerlere **props ** diyoruz.

```js
const MyButton = (props)=>{
  return( 
     <button> { props.text } </button>
  )
}
```

 Üzerindeki yazıyı dışarıdan props aracılığıyla alan basit bir button componenti. Bu componenti tekrar tekrar farklı props'lar ile kullanabiliriz.  Aşağıdaki örneği inceleyebilirsiniz.

[source code](http://jsbin.com/mebesol/9/edit?html,js,output)



# ![](/assets/Desktop22.png)

Yukarıda gördüğünüz bir önyüz. Ve bu UI **Top, Left, Right **ve** Bottom **componentleriyle  4 e bölünmüş durumda.


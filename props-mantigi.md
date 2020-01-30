# Props Mantığı

## Props Mantığı

React Js ' de her bir component aslında bir fonksiyon. Nasıl görünceklerini, ekranda nasıl bir yer kaplayacaklarını da bu fonksiyonlara verdiğimiz, input değerleriyle belirliyoruz. İşte bu input değerlere **props** diyoruz.

```javascript
const MyButton = (props)=>{
  return( 
     <button> { props.text } </button>
  )
}
```

Üzerindeki yazıyı dışarıdan props aracılığıyla alan basit bir button componenti. Bu componenti tekrar tekrar farklı props'lar ile kullanabiliriz. Aşağıdaki örneği inceleyebilirsiniz.

[source code](http://jsbin.com/mebesol/9/edit?html,js,output)

## ![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/desktop22%20%281%29.png)

Yukarıda gördüğünüz bir önyüz. Ve bu UI **Top, Left, Right** ve **Bottom** componentleriyle 4 e bölünmüş durumda. Bu componentler aldıkları propslar ile kendi içlerinde bağımsız şekil alabiliyorlar. Peki farklı componentler arasındaki olası ilişkiyi nasıl sağlayacağız ? İşte bu anda **state mantığı** devreye giriyor


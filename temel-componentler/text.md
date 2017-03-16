# Text

Normalde HTML de `<div> Merhaba Dünya </div>`  tag'ler arasına yazı yazıp o element tag'in fontuna style verebiliyorduk. Mobile'de durum biraz farklı. Bunun için &lt;Text&gt; componentini kullanıyoruz. font stillerimizi bu elemente tanımlıyoruz.

Güzel özelliklerinden birisi iç içe &lt;Text&gt; elementlerini kullanabiliyoruz. 

```html
<Text style={{...}}>
    Merhaba <Text style={{...}} > Dünya </Tex>
</Tex> 
```

Font stillerini Text componentinde tanımlıyoruz dedik,tabi durum böyle olunca varsayılanın dışında belli bir font kullanmaya karar verdiysek, o zaman kendimize bir tane özel bir Text componenti yapıp, mesela ismi MyText olsun, bunun stillerinde fontFamily özelliğini verip, diğer Text componentleri de bu componentimizin child'ı olarak çağırabiliriz

```html
<MyText>
   Merhaba <Text style={{...}} > Dünya </Tex>
</MyTex> 
```

&lt;Text&gt; componenti de View gibi onLayout eventini desteklemektedir.






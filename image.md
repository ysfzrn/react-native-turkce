# Image

Resimlerimizi derlemek için kullandığımı component. İster bir network bağlantısından, isterse local'den resimleri render edebiliriz.

Bu örnekte server'dan dinamik olarak resimleri fetch eden bir componenti görüyorsunuz.

```javascript
<Image  resizeMode = "contain"
        style={styles.image}
        source={{uri:`${API_PUBLIC}${category.name}.png`  }} />
```

Mesela diyelim ki sizin assets klasörünüzde boyutuna göre **test.png**, **test@2x.png** ve **test@test3.png** adında 3 tane resminiz olsun. Siz bunları **require\('./test.png'\)** yazarak çağırdığınızda, cihazın boyutuna göre resmi seçilip, render edilecektir.

React native'in 0.39 ve alt sürümlerinde Image componenti için mutlaka width ve height değerleri verilmek zorundaydı. Üst ve şimdiki sürümde \(0.42\) default olarak Image componentinin boyutu `flex:1` olarak derlenmektedir.


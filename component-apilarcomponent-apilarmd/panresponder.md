# PANRESPONDER

PanResponder API ile React Native'de touch\(dokunma\) eventleri, PanResponder'a ait lifecycle methodlar ile düzenleyip, drag and drop işlemlerini gerçekleştirebiliriz.

```js
  componentWillMount() {
    this._panResponder = PanResponder.create({
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,

      onPanResponderGrant: (evt, gestureState) => this.handleResponderGrant,
      onPanResponderMove: this.handleResponderMove,
      onPanResponderRelease: this.handleResponderRelease
    });
  },

  render: function() {
    return (
      <View {...this._panResponder.panHandlers} />
    );
  },
```

Bir elemente drag-drop özelliği eklemek istediğimizde,  yukarıdaki kod parçacığı sabit bir yaklaşım direkt bunu kullanabiliriz. Hatta kullanığınız editörde bir snippet kısayolu ayarlamanız size pratiklik sağlayacaktır. \( Bu arada react-native'in gesture sisteminde sadece bu methodlar yok. Ben buraya en çok kullanılanlarını ve aşağıdaki örnekte kullanacaklarımızı yazdım\)

Şimdi bu API'ı bir örnekle açıklamak istiyorum. Bizim bir görevimiz olsun. 3x3 kare board ve altında 3 kırmızı, 3 mavi, 3 sarı topumuz olsun. Aşağıdaki topları sürükleyip, board'a bırakalım.

1. Sürükleyip bıraktığımız kutu boş ise , topun rengini alsın.
2. Sürükleyip bıraktığımız kutu dolu ise, top tekrar yerine gitsin
3. Bütün board kareleri dolduğunda yan yana olan tüm renkler aynı ise Kazandınız, değil ise Kaybettiniz diye bir Alert çıkarsın.



![](/assets/digdagdoe.gif)

Görevimiz anladıysak şimdi yapılacakları adım adım parçalara bölelim.

1. Board çizelim. Board'ın koordinatlarını ve özelliklerini state'te tutalım.
2. Bir PanResponder Element Yaratım. 
3. Toplar için state'te array tutalım. Bu array'ı map ederek 2. adımda yarattığımız componenti çoğaltalım.
4. Hover effect ekleyelim
5. Sürüklediğimiz componenti drop ettikten sonraki kontrolleri ekleyelim.




# JSX Nedir ?

\(Bu sayfada jsbin den gömülü kod kullanılmaktadır. Eğer kodu göremiyorsanız, browserınızdali **load unsafe script **seçeneğini seçmelisiniz\)

[source code](http://jsbin.com/mebesol/2/edit?,js,output)

Yukarıdaki basit bir React componenti. React ve ReactDOM kütüphanelerini script tag'i ile HTML sayfamızın başına yazdık.

Sonra Javascript derleyicimizi, JSX seçtik \( ES6/Babel de seçebilirdik \)

Burada dikkatinizi çeken bir şey oldu mu ? Biz javascript kodumuza, HTML kodu yazdık :\)

Önceden bahsettiğimiz, React ın bize sunduğu lego tasarlar gibi uygulama geliştirmek tam olarak böyle, parça parça HTML kodlarını component olarak yazıp hepsini belli bir hiyerarşiyle bir araya getirmek.

[source code](http://jsbin.com/mebesol/4/edit?,js,output)

Aslında en yukarıda yaptığımız JavaScript içine HTLM kod yazarak yaptığımız şey tam olarak React.createElement yerine HTML tagleri kullanmak. Bana sorarsanız HTML yazmak daha kolay. 


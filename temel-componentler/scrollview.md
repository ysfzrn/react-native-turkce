# ScrollView

Kaydırılabilir içerikler kullanabileceğimiz en basit component. Eğer sizin 30 ve altında bir içeriğiniz varsa ScrollView çok kullanışlı. Hem yatay hem de dikey olarak içerikleri kaydırabiliyoruz. 

```jsx
      <ScrollView style={styles.container}>
        <View style={styles.boxLarge} />
        <ScrollView horizontal>      //default olarak vertical seçilidir.
          <View style={styles.boxSmall} />
          <View style={styles.boxSmall} />
          <View style={styles.boxSmall} />
        </ScrollView>
        <View style={styles.boxLarge} />
        <View style={styles.boxSmall} />
        <View style={styles.boxLarge} />
      </ScrollView>
```

30 ve üzerinde bir içeriği sıralamak istiyorsanız, performans açısından bunun için bize önerilen, ScrollView yerine, ListView 'i kullanmamız.




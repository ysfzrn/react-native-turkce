# MOBX \( State yönetmenin en kolay yolu \)

Mobx, reactive programlama felsefesiyle yazılmış rakiplerine göre öğrenme süresi çok kısa olan, basitliğiyle dikkat çeken bir state yönetim kütüphanesi.

> Reactive Programlama nedir ?  \( [ekşi sözlükte](https://eksisozluk.com/reactive-programlama--5205874) okuduğum tanım, bayağı sade ve açıklayıcı olduğu gibi aşağıya yazıyorum \)
>
> Bir değişikliğin veri akışında yayılımı üzerine kurulmuş programlama paradigması.Örneğin normalde `a = b+c` ile `b` ile `c` nin toplamı `a`'ya atanır ve `b` veya `c` degistiginde `a` degismez. Reactive programlamada ise `b` veya `c` degistiginde bu değişiklik `a`'ya da yansıtılır ve toplam değişmiş olur.

Mobx'in arkasında yatan mantık da aynı. Uygulamanın state'inde herhangi bir şey değişmişse, değişime göre otomatik olarak yeni state üretilir. React içinde de state değiştiğinde önyüz tekrar render edilir. Hal böyle olunca da, Mobx ve React harika bir ikili oluveriyorlar.

Şimdi mobx'de ki temel kavramları basit bir TODO List örneğiyle açıklayalım.Belki TODO List örneği görmekten bir kısmınıza kusma geliyor olabilir ama söz bundan sonraki örneğimizi bir react-native, mobx ve react-native-navigation kullanarak bir oyun geliştirerek yapacağız.

Flux ve redux da olduğu gibi, MobX'de de action'lar var olan state'i değiştirmek için varlar. Ama redux'da olduğu gibi state değişikliklerini reducer üzerinden değil, **direkt action fonksiyonunun içinde  yapıyoruz.  **![](/assets/mobxDiagram.png)

### React Native - Mobx Entegrasyonu

**1\)**  İlk yapmamız gereken mobx ve mobx-react paketlerini projemizin dependency paketlerine eklemek

> `yarn add mobx mobx-react`

**2\)** MobX, javascript'e ES7 ile gelen decoratorler ile genellikle yazılır. \( decorator kullanmadan da yazabilirsiniz ama okunabilirlik açısında tavsiye etmiyorum. \). Bu decoratorleri etkin kılmak için babel-plugin-transform-decorators-legacy paketini projemizin devDependencies'e  ekliyoruz

> `yarn add --dev babel-plugin-transform-decorators-legacy`

Paketimizi ekledikten sonra, .**babelrc** dosyasının içini aşağıdaki gibi değiştiriyoruz

> `{`
>
> `'presets': ['react-native'],`
>
> `'plugins': ['transform-decorators-legacy']`
>
> `}`

**3\) **Eğer VsCode kullanıyorsanız, editörünüzün decoratorlere kızmaması için projenizde **tsconfig.json** isimli bir dosya oluşturup, aşağıdaki satırları kopyalaıp kaydedin.

> `{`
>
> `"compilerOptions": {`
>
> `"experimentalDecorators": true,`
>
> `"allowJs": true`
>
> `}`
>
> `}`

**4\) **Şimdi todoStore diye isimlendireceğimiz sınıfımızı yaratalım. Başlangıç state'leri değişken olarak tanımlayıp, başlarına observable decorator koyduk. Böylece bu state'lerde değişiklik olduğu zaman, observable componentlerimiz bunları algılayıp tekrar render edilecekler. 

```js
import mobx, { observable } from "mobx";

class Store {
    @observable todos=[];
    @observable selectedStatus="all";
}

const todoStore = new Store()
export default todoStore;
```

**5\) **State'leri değiştirecek olan action'larımızı, @action decorator'u yardımı ile tanımlayalım. Observable değişkenlere redux'da olduğu gibi bir reducer aracılığıyla değil, zaten class içinde tanımlı olduklarından method içinde müdahele edebiliyoruz.

```js
import mobx, { observable, action } from "mobx";

class Store {
    @observable todos=[];
    @observable selectedStatus="all";

    @action('adding todo item')
    addTodo(todo){
        this.todos.push({
            id: this.todos[this.todos.length - 1].id +1,
            status: false,
            text: todo
        })
    }

    @action('Selected status changed')
    statusChange(status){
        this.selectedStatus = status;
    }

}

const todoStore = new Store()
export default todoStore;
```

**6\)** MobX'de bir diğer temel kavram @computed decorator'ü. Computed methodları, birden fazla observable değerinin değişikliğinden etkilenen yeni bir değer türeteceğimiz zaman kullanıyoruz. Aşağıdaki örneğe bakalım. Status'u _all_ olan, ya da _done_ olan todo list'de olan elemanları _filteredTodos_ değişkeninde tutmak ya da önyüz'de _todos_ listesini filtrelemek yerine, çoğu yazılım dilinde hali hazırda olan getter, setter methodları yapıyoruz.    

```js
import mobx, { observable, action, computed } from "mobx";

class Store {
    @observable todos=[];
    @observable selectedStatus="all";

    @action('adding todo item')
    addTodo(todo){
        this.todos.push({
            id: this.todos[this.todos.length - 1].id +1,
            status: false,
            text: todo
        })
    }

    @action('Selected status changed')
    statusChange(status){
        this.selectedStatus = status;
    }
    
    @computed get filterTodos() {
        if(this.selectedStatus === 'all'){
            return this.todos;
        } else if( this.selectedStatus === 'done' ){
    	return this.todos.filter(
			todo => todo.status === true);
        } else if( this.selectedStatus === 'todo' ){
    	return this.todos.filter(
			todo => todo.status === false);
        }
    }

}

const todoStore = new Store()
export default todoStore;
```




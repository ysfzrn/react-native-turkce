# İç İçe Navigator Kullanımı

![](https://github.com/ysfzrn/react-native-turkce/tree/37853d6e5cb460c3118cb5ab0091ea8bf845ba4d/.gitbook/assets/nestingnavigators.gif)

Navigator'ler iç içe yazılabilir \( composable \) özelliğine sahiptir.Yukarıdaki örnekte TabNavigator ve StackNavigator ün nasıl kullanıldığını inceleyebilirsiniz.

```jsx
import { StackNavigator, TabNavigator } from "react-navigation";
import HomeScreen from "./src/screens/home";
import DetailsScreen from "./src/screens/details";
import ProfileScreen from "./src/screens/profile";

const MainNavigator = TabNavigator({
  Home: {
    screen: HomeScreen
  },
  Details: {
    screen: DetailsScreen
  }
});

const RootNavigator = StackNavigator({
    Main:{
        screen:MainNavigator  // Yukarıdaki TabNavigator 
    },
    Profile:{
        screen:ProfileScreen
    }
})

export default RootNavigator;
```


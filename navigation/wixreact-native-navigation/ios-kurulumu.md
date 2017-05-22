# IOS Kurulumu

**1**- react native versiyonu olarak 0.43 ve üstünü kullanıyor olduğunuzdan emin olun.

**2**- npm kullanıyorsanız 3 ve üstü versiyon kullandığınızdan emin olun.

```
   npm install react-native-navigation@latest
```

**3**- yarn kullanıyorsanız

```
   yarn add react-native-navigation@latest
```

**4**-react-native init ile kurduğunuz projede ios bölümünü **XCode** ile açın. **Libraries** kısmına sağ tıklayıp **Add Files** seçeneğini seçin. **node\_modules** klasörüne kaydedilen, **react-native-navigation/ios/ReactNativeNavigation.xcodeproj**  projesini **Libraries** kısmına ekleyin.

![](/assets/rnn-ios-1.gif)

**5**- XCode'da navigator'de projenizi seçin ve **Build Phases** kısmına gelin. **Link Binary with Libraries** kısmında **libReactNativeNavigation.a** seçip ekleyin.

![](/assets/rnn-ios-2.gif)

**6**- Yine Project Navigator'de bu kez **Build Settings**'i seçin. Search kısmına **Header Search Paths** yazın. ve aşağıdaki kodu kopyalayıp bu kısıma ekleyin ve mutlaka **recursive** seçeneğini seçin.

```
$(SRCROOT)/../node_modules/react-native-navigation/ios
```

![](/assets/rnn-ios-3.gif)

**7**- Son olarak aşağıdaki kodu projenizin **AppDelegate.m** dosyasının tamamını ezerek kopyalayın.\( Evet tamamını ezerek \)

```js
#import "AppDelegate.h"
#import <React/RCTBundleURLProvider.h>

// **********************************************
// *** DON'T MISS: THE NEXT LINE IS IMPORTANT ***
// **********************************************
#import "RCCManager.h"

// IMPORTANT: if you're getting an Xcode error that RCCManager.h isn't found, you've probably ran "npm install"
// with npm ver 2. You'll need to "npm install" with npm 3 (see https://github.com/wix/react-native-navigation/issues/1)

#import <React/RCTRootView.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  NSURL *jsCodeLocation;
#ifdef DEBUG
//  jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.ios.bundle?platform=ios&dev=true"];
  jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index.ios" fallbackResource:nil];
#else
   jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif


  // **********************************************
  // *** DON'T MISS: THIS IS HOW WE BOOTSTRAP *****
  // **********************************************
  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  self.window.backgroundColor = [UIColor whiteColor];
  [[RCCManager sharedInstance] initBridgeWithBundleURL:jsCodeLocation];

  /*
  // original RN bootstrap - remove this part
  RCTRootView *rootView = [[RCTRootView alloc] initWithBundleURL:jsCodeLocation
                                                      moduleName:@"example"
                                               initialProperties:nil
                                                   launchOptions:launchOptions];
  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  UIViewController *rootViewController = [UIViewController new];
  rootViewController.view = rootView;
  self.window.rootViewController = rootViewController;
  [self.window makeKeyAndVisible];
  */
  

  return YES;
}

@end
```

![](/assets/rnn-ios-4.gif)


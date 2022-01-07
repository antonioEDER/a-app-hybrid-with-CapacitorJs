# app hibrido com CapacitorJs

```
Esse é um projeto desenvolvido com CapacitorJs e VueJs.

Nele vamos explorar os plugins de:
- Android
- Câmera
- Toast
- Vibration
- Login with Google
- Splash Screens and Icons
```


## Create Project
```
npx cap init
```

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Add platform [Android|Ios]
```
npx cap add [android|Ios]
```

### Open platform [Android|Ios]
```
npx cap open [android|Ios]
```

### Generation plugins
```
yarn @capacitor/cli plugin:generate
```

### Run APP Android
```
yarn android
```

### Generator icons and Splash for Android
```
yarn global add cordova-res
cordova-res
cordova-res android --skip-config --copy
```

### Configuration for Android with capacitojs
```
- /android/app/src/main/res/values/strings.xml
    - <string name="server_client_id">id.apps.googleusercontent.com</string>
- index.html
    - <meta name="google-signin-client_id" content="id.apps.googleusercontent.com">
- android/app/src/main/java/br/com/eucatur/MainActivity.java
    - import com.codetrixstudio.capacitor.GoogleAuth.GoogleAuth;
    - this.init { add(GoogleAuth.class); }
    
```

### Customize configuration
See [Configuration Reference](https://capacitorjs.com/).

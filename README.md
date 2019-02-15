# Ionic 3 Android Back Button Handler

[![NPM version][npm-image]][npm-url]

##Installation
```
npm i ionic3-android-backbutton
```

##Usage
most cases where this plugin is useful is to prevent the user from exiting the app before a certain condition is fulfilled, this is where the plugin gets useful especially when using the `registerBeforeExit()` method. See the example below.

these methods act globally and will affect the whole app so you need to unregister them manually after you don't need them.

###Example: make sure user sees an alert before he leaves the page
```typescript
import {Component} from '@angular/core';
import {AlertController, Platform} from 'ionic-angular';
import {StatusBar} from '@ionic-native/status-bar';
import {SplashScreen} from '@ionic-native/splash-screen';

import {HomePage} from '../pages/home/home';
import {Backbutton} from "../providers/backbutton";

@Component({
    templateUrl: 'app.html'
})
export class MyApp {
    rootPage: any = HomePage;

    constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen, public bb: Backbutton, public alertCtrl: AlertController) {
        platform.ready().then(() => {
            statusBar.styleDefault();
            splashScreen.hide();
            this.bb.registerBeforeExit(() => {
                    let alert = this.alertCtrl.create({
                        title: 'You are now going to exit the app',
                        buttons: ['OK']
                    });
                    alert.present();
                    alert.onDidDismiss(() => {
                        this.bb.exit()
                    });
                    return false;
            })
        });
    }
}
```
### Methods
#### registerDefaultAction(f: Function)
register the function to be called whenever the back button is pressed. If your function returns true, the default back button behavior with be triggered, if your function returns false the default behavior will be canceled.
#### registerBeforeExit(f: Function)
register the function to be called whenever the app is about to exit upon hardware back button press. If your function returns true, the default back button behavior with be triggered and your app will exit, if your function returns false the default behavior will be canceled.
####unregisterAction(type: string){
| type           | Description                           |
| -------------- | --------------------------------------|
| `default`      | unregister `registerDefaultAction()`  |
| `default`      | unregister `registerBeforeExit()`     |
####unregisterAll()
reverts back button default behavior
####back()
emulate android back button (including potentially exiting the app)
####exit()
exit the app

[npm-url]: https://npmjs.org/package/ionic3-android-backbutton
[npm-image]: https://img.shields.io/badge/npm-0.0.5-green.svg
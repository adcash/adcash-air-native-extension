The **Adcash native extension** provides the following features:
* **Banner** Ads and setting their position(top/bottom)
* **Interstitial** Ads
* **Ad Events** that you can receive if subscribed
* **Conversion Tracking** that is easy to use and would serve advertisers
* **Sample project** to demonstrate the native extension integration

Applications that integrate the native extension can run on:
* **Android** (both actual devices and emulators; ARM and x86 CPU's are both supported)

## Prerequisites
* Adobe AIR SDK **18.0**
* Apache Flex SDK **4.14.1**
* **Zone ID(s)**. You create these at [Adcash website](https://www.adcash.com/console/scripts.php)
* Android SDK **23** (target)

> Note: To integrate **Adcash native extension** into your project you follow the same procedure as for other native extensions. We have included instructions for **Adobe Flash Builder 4.7** IDE below. However other IDEs should work just as well.

## Integrate the Native Extension
Download the `Adcash.ane` file [here](https://github.com/adcash/adcash-air-native-extension) and add it to your application as a **native extension** by following these steps:

1. Right-click on your project and select **Properties**. 
    
    Navigate to **Flex Build Path > Native Extensions**, select **Add ANE...** and browse to the `Adcash.ane` to add it:

    ![alt tag](http://developer.adca.sh/wp-content/uploads/2015/08/air1.png)

2. If your app will run on Android, go to **Flex Build Packaging > Google Android > Native Extensions** and make sure the **Package** check box for the `Adcash.ane` is checked:

    ![alt tag](http://developer.adca.sh/wp-content/uploads/2015/08/air2.png)

## Configure the Native Extension
Before you can use our native extension in your code, you need the following import declaration:

```javascript
import com.adcash.*;
```

Also, declare the **Adcash** object variable as well as variables for the **Zone ID(s)**:

```javascript
protected var _adcash:Adcash = new Adcash();
public var _interstitialZoneId:String;
public var _bannerZoneId:String;
```

Make sure to set the **Zone ID(s)** for the ad type(s) that you would use. Make sure your **Zone ID(s)** are specifically for Android.   
Go ahead and set the previously defined variables:

```javascript
// Set the zone id-s:
_interstitialZoneId = "<YOUR_ANDROID_INTERSTITIAL_ZONE_ID_HERE>";
_bannerZoneId = "<YOUR_ANDROID_BANNER_ZONE_ID_HERE>";
```

#### Banner
To show a banner in your application do the following:

```javascript
_adcash.loadBanner(_bannerZoneId, AdPosition.POSITION_BOTTOM);
```

After the banner ad is loaded, it will show automatically. Keep in mind that currently the instance of the banner is only one thus if you load again this would override the banner you previously had.

You can set the **refresh rate** of the re-loadings for the banner in your web portal and use the value you have there. But now you can also do this inside the application you are developing by calling:

```javascript
_adcash.setAutoRefreshRate(<INTEGER_RATE_IN_SECONDS>);
```

Finally, the available **events** that you can listen for are:

```javascript
_adcash.addEventListener(AdEvent.BANNER_LOAD_OK, myEventHandler);
_adcash.addEventListener(AdEvent.BANNER_LOAD_FAIL, myEventHandler);
_adcash.addEventListener(AdEvent.BANNER_LEFT_APP, myEventHandler);
_adcash.addEventListener(AdEvent.BANNER_LEFT_OPEN, myEventHandler);
_adcash.addEventListener(AdEvent.BANNER_LEFT_CLOSED, myEventHandler);
```

#### Interstitial
To show an interstitial add the following:

```javascript
_adcash.loadInterstitial(_interstitialZoneId);
```

After an interstitial ad is loaded, it will not show automatically.
At convenient points in the execution of your application you can check if an interstitial has been loaded and is ready to be displayed by calling the `isInterstitialReady()` method.  
Also, keep in mind that currently the instance of the interstitial is only one thus if you load again this would override the interstitial you previously had.

When the interstitial is ready you call:

```javascript
_adcash.showInterstitial();
```

The available **events** that you can listen for are:

```javascript	
_adcash.addEventListener(AdEvent.INTERSTITIAL_LOAD_OK, myEventHandler);
_adcash.addEventListener(AdEvent.INTERSTITIAL_LOAD_FAIL, myEventHandler);
_adcash.addEventListener(AdEvent.INTERSTITIAL_LEFT_APP, myEventHandler);
_adcash.addEventListener(AdEvent.INTERSTITIAL_OPEN, myEventHandler);
_adcash.addEventListener(AdEvent.INTERSTITIAL_CLOSED, myEventHandler);
```

## Conversion Tracking
You can now easily report conversion from the plugin to us. You simply need to report it with your **payout_id** which you can get familiar with in the general documentation for [Android Adcash SDK 2.0.0](http://developer.adca.sh/article/android/2-0-0/get-started-2-0-0/adcash-android-sdk-3/).  
You can also include **additional parameters** with the report conversion call to our servers if you would just add them beforehand.  
And before all actions make sure to tell the plugin to **prepare** for the conversion.

Here is a snippet showing the flow of calls:

```javascript
// Before the conversion, always prepare:
_adcash.prepareForConversion();

// Optional :
// To include additional params to the URL, 
// use the following method:
_adcash.addConversionParam("key1","value1");
_adcash.addConversionParam("key2","value2");
_adcash.addConversionParam("key3","value3");

// And when all params are added, report:
_adcash.reportConversion(<PAYOUT_ID_HERE>);
```

## Sample Project
You can find an example of how to integrate and use the native extension [here](https://github.com/adcash/adcash-air-native-extension).

If you use Adobe Flash Builder, you can open the sample project by selecting **File > Import Flash Builder Project...** from the menu and choosing the `Demo.fxp` file.

## Special Considerations
The **Adcash native extension** can conflict with other third-party native extensions. If a conflict occurs it will be detected at the application packaging phase. 

If you stumble upon such a conflict please notify us and we will work on resolving it.

## Support
If you need any support or assistance you can contact us by sending email to <mobile@adcash.com>.

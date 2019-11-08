# Tools and Debugging
>The Yumi Mediation Test Suite allows you to test whether you have correctly configured your application and ad units to be able to display ads from third-party networks via Yumi mediation.

This guide outlines how to use the Yumi Mediation Test Suite in your iOS app. The first step is integrating the tool into your app.

# Call Debug Mode

```C#
using YumiMediationSDK.Api;
using YumiMediationSDK.Common;

public class YumiSDKDemo : MonoBehaviour
{
   private YumiDebugCenter debugCenter;
  
   private void CallDebugCenter(){
        if (this.debugCenter == null)
        {
            this.debugCenter = new YumiDebugCenter();
        }
        // Note: Fill in the ad slot information to distinguish between iOS and Android
        this.debugCenter.PresentYumiMediationDebugCenter("YOUR_BANNER_PLACEMENT_ID", "YOUR_INTERSTITIAL_PLACEMENT_ID", "YOUR_REWARDVIDEO_PLACEMENT_ID", "YOUR_NATIVE_PLACEMENT_ID","YOUR_SPLASH_PLACEMENT_ID","YOUR_CHANNEL_ID", "YOUR_VERSION_ID");
    }
}
```

# Sample

Take the iOS platform as an example (the Android platform has the same logic but different UI).

<div align="center"><img width="160" height="282" src="resources/debug-1.png"/></div>

*<p align="center" size=1>Select platform integration category</p>*

<div align="center"><img width="160" height="282" src="resources/debug-2.png"/></div>

*<p align="center" size=1>Select single platform<br>If the platform you want is not in the list, it is not added to the project<br>The green platform is added to the project and configured<br>The grey platform is added to the project but not configured</p>*

<div align="center"><img width="160" height="282" src="resources/debug-3.png"/></div>

*<p align="center" size=1>Select the AD type and debug the single platform</p>*

# TEST ID

| OS      | Formats        | Slot(Placement) ID | Note                                                                                                                                          |
| ------- | -------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Android | Banner         | uz852t89           | Using this test ID, you are able to get test ads which are from YUMI, AdMob, AppLovin, Baidu, IQzone                                                         |
| Android | Interstitial   | 56ubk22h           | Using this test ID, you are able to get test ads which are form YUMI, AdMob, AppLovin, Baidu, IronSource, InMobi, IQzone, Unity Ads, Vungle, ZPLAYAds          |
| Android | Rewarded Video | ew9hyvl4           | Using this test ID, you can get test ads which are from YUMI, AdMob, AppLovin, GDTMob, IronSource, InMobi, IQzone, Unity Ads, Vungle, ZPLAYAds          |
| Android | Native         | dt62rndy           | You can get test ads which are from YUMI, AdMob, Baidu, GDTMob, Facebook                                                         |
| iOS     | Banner         | l6ibkpae           | You can get test ads which are from YUMI, AdMob, AppLovin, Baidu, Facebook, GDTMob                                                |
| iOS     | Interstitial   | onkkeg5i           | Using this test ID, you can get test ads which are from YUMI, AdMob, Baidu, Chartboost, GDTMob, IronSource, InMobi, IQzone, Unity Ads, Vungle, ZPLAYAds |
| iOS     | Rewarded Video | 5xmpgti4           | Using this test ID, you can get test ads which are from YUMI, AdMob, AdColony, AppLovin, IronSource, InMobi, Mintegral, Unity Ads, Vungle, ZPLAYAds   |
| iOS     | Native         | atb3ke1i           | Using this test ID, you can get test ads which are from YUMI, AdMob, Baidu, GDTMob, Facebook                                                        |


# Android build failed
## Failed to find Build Tools...
```
* What went wrong:
A problem occurred configuring root project 'gradleOut'.
> Failed to find Build Tools revision 29.0.0
```
**How to fix**

Remove `buildToolsVersion '**BUILDTOOLS**'` in [mainTemplet](../../Assets/Plugins/Android/mainTemplate.gradle).

## No toolchains found...
```
* What went wrong:
A problem occurred configuring root project 'gradleOut'.
> No toolchains found in the NDK toolchains folder for ABI with prefix: mips64el-linux-android
```
**How to fix**

Change the version of gradle plugin in [mainTemplet](../../Assets/Plugins/Android/mainTemplate.gradle), for example, change `classpath 'com.android.tools.build:gradle:3.0.1'` to `classpath 'com.android.tools.build:gradle:3.2.1'`.

## Failed to apply plugin...
```
* What went wrong:
A problem occurred evaluating root project 'gradleOut'.
> Failed to apply plugin [id 'com.android.application']
   > Minimum supported Gradle version is 4.6. Current version is 4.2.1. If using the gradle wrapper, try editing the distributionUrl in
```
**How to fix(pick one of the follows)**

1. upgrade gradle version to 4.6
2. degrade gradle plugin to match gradle 4.2.1 version. you can check [Update Gradle](https://developer.android.com/studio/releases/gradle-plugin#updating-gradle) to change the gradle plugin version in [mainTemplet](../../Assets/Plugins/Android/mainTemplate.gradle), for example, change `classpath 'com.android.tools.build:gradle:x.x.x'` to `classpath 'com.android.tools.build:gradle:3.0.0+'`.

## Resolving Android Dependencies
It maybe spend some time to resolving android dependencies when clicked Assets -> Play Services Resolver -> Android Resolver -> Resolve / Force Resolve. More androidPackages added and more time will be taken. When resolving conflicts, try not to use the Unity IDE, otherwise the Unity IDE may become stuck.

## the 64K reference limit
You can use one of the following solutions to avoid the 64K reference limit:

Solution-A: Modify AndroidManifest.xml and mainTemplate.gradle which located Unity project's Assets/Plugins/Android/, if there are no such files then copy from [AndroidManifest](https://github.com/yumimobi/YumiMediationSDK-Unity/blob/master/Assets/Plugins/Android/AndroidManifest.xml) and [mainTemplate](https://github.com/yumimobi/YumiMediationSDK-Unity/blob/master/Assets/Plugins/Android/mainTemplate.gradle).

AndroidManifest.xml
```xml
<manifest>
  ...
  <application
      android:name="android.support.multidex.MultiDexApplication"
      ...
      >
      ...
  </application>
  ...
</manifest>
```
mainTemplate.gradle
```groovy
allprojects {
  repositories {
    google()
    jcenter()
    ...
  }
}
dependencies {
  ...
  implementation 'com.android.support:multidex:1.0.3'
  ...
**DEPS**}
```

Solution-B: Export Unity project to Android Studio project, then to [Avoid the 64K limit](https://developer.android.com/studio/build/multidex#avoid).

## Clicking the Android Resolver/Force Resolve option Android dependencies failed
Clicking Assets/Play Services Resolver/Android Resolver/Force Resolve option error log：
```
stderr:
Exception in thread "main" java.lang.RuntimeException: Timeout of 120000 reached waiting for exclusive access to file: /.gradle/wrapper/dists/gradle-5.1.1-bin/90y9l8txxfw1s2o6ctiqeruwn/gradle-5.1.1-bin.zip
	at org.gradle.wrapper.ExclusiveFileAccessManager.access(ExclusiveFileAccessManager.java:61)
	at org.gradle.wrapper.Install.createDist(Install.java:48)
	at org.gradle.wrapper.WrapperExecutor.execute(WrapperExecutor.java:128)
	at org.gradle.wrapper.GradleWrapperMain.main(GradleWrapperMain.java:61)
```
Please check if the Assets/Plugin/Android/mainTemplate.gradle file in your Unity project exists. If it does not exist, please add mainTemplate.gradle fil.

Generate the mainTemplate.gradle file using the Unity tool：

<div align="center"><img height="352" src="resources/mainTemplate.png"/></div>

## Android 9.0 compatibility considerations
At present, Mintegral platform the Android SDK does not support Android9.0 or above. If the app crashes above Android9.0, you can solve by the ways below.

- Set targaetSDKveriosn to 27 or less

## Set your AdMob app MANAGER (If you don't set, will meet a crash)
- iOS update your info.plist file.[Admob document](https://developers.google.com/admob/ios/quick-start?hl=zh-cn) 
- Android update your AndroidManifest.xml。[Admob document](https://developers.google.com/admob/android/quick-start?hl=zh-cn)

## Gdt(广点通) platform FAQ：
### Gdt(广点通) platform Native ad con‘t show media Native Ad probleam：

**How to fix**

Make sure that the package: "xxx.xxx.xxx" in the Assets/Plugins/Android/AndroidManifest.xml of your Unity project is consistent with the package name "xxx.xxx.xxx" of your Unity project. E.g：</span></p>
<img src="resources\gdt1.png" alt="gdt1">

### Gdt(广点通) platform ad No Fill probleam：

**How to fix**

Make sure that the Gdt(广点通) platform ad required permissions added
```
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />  
<uses-permission android:name="android.permission.ACCESS_COARSE_UPDATES"/>
```

## Baidu platform FAQ：

### Baidu platform ad No Fill probleam：

**How to fix**

Make sure that the Baidu platform ad required permissions added
```
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```
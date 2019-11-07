# Get Started
Integrating the YumiMediationSDK Mobile Ads Unity plugin into an app, which you will do here, is the first step toward displaying AdMob ads and earning revenue. Once the integration is complete, you can choose an ad format (such as interstitial or rewarded video) to get detailed implementation steps.
# Prerequisites
- Unity 5.6 and above

   - To deploy to iOS

     Xcode 7.0 or higher

     iOS 8.0 and above

     [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)

   - To deploy to Android

     Android SDK： > 4.1 (API level 16)

     [Demo ](https://github.com/yumimobi/YumiMediationSDK-Unity)

# Download the YumiMediationSDK Unity plugin
The YumiMediationSDK Unity plugin enables Unity developers to easily serve Yumimobi Ads on Android and iOS apps without having to write Java or Objective-C code. The plugin provides a C# interface for requesting ads that is used by C# scripts in your Unity project. Use the links below to download the Unity package for the plugin or to take a look at its code on GitHub.

[Download the YumiMediationSDK Unity plugin](https://github.com/yumimobi/YumiMediationSDK-Unity/raw/master/YumiMediationSDKPlugin.unitypackage)

[VIEW SOURCE](https://github.com/yumimobi/YumiMediationSDK-Unity)

# Import the YumiMediationSDK Unity plugin
## First import
Open your project in the Unity editor. Select **Assets> Import Package> Custom Package** and find the YumiMediationSDKPlugin.unitypackage file that you downloaded.

![img](resources/01.png)

Make sure all of the files are selected and click **Import**.

![img](resources/02.png)

## Update plugin

Delete the Assets/YumiMediationSDK directory, then reimport the plugin as the 3.1 section discussed.

Delete the Assets/PlayServicesResolver directory, then reimport the plugin as the 3.1 section discussed.

YumiMediationSDK Unity plugin has moved bridge files - Assets/Plugins/Android/unity-plugin-library.jar and Assets/Plugins/iOS/* - to Assets/YumiMediationSDK/../ . So if you imported those files before, you need delete them to avoid compilation error.

# Include the Mobile Ads SDK

The YumiMediationSDK Unity plugin is distributed with the [Unity Play Services Resolver library](https://github.com/googlesamples/unity-jar-resolver). This library is intended for use by any Unity plugin that requires access to Android specific libraries (e.g., AARs) or iOS CocoaPods. It provides Unity plugins the ability to declare dependencies, which are then automatically resolved and copied into your Unity project.

Follow the steps listed below to ensure your project includes the YumiMediationSDK Unity
## Deploy iOS 

No procedure are required to integrate the YumiMediationSDK into a Unity project.

The YumiMediationSDK Ads Unity plugin dependencies are listed in **Assets/YumiMediationSDK/Editor/YumiMobileAdsDependencies.xml**  .
iOS dependencies：

```xml
    <iosPods>
        <iosPod name="YumiMediationSDK" version="4.3.3" minTargetSdk="8.0">
            <sources>
                <source>https://github.com/CocoaPods/Specs</source>
            </sources>
        </iosPod>
        <!-- adapters -->
        <iosPod name="YumiMediationAdapters/AdColony" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/AdMob" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/AppLovin" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/Baidu" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/Chartboost" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/Domob" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/Facebook" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/GDT" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/InMobi" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/IronSource" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/Unity" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/Vungle" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/Mintegral" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/OneWay" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/ZplayAds" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/TapjoySDK" version="4.3.2">
        </iosPod>
         <iosPod name="YumiMediationAdapters/BytedanceAds" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/InneractiveAdSDK" version="4.3.2">
        </iosPod>
        <iosPod name="YumiMediationAdapters/PubNative" version="4.3.2">
        </iosPod>
        <!-- debugCenter -->
        <iosPod name="YumiMediationDebugCenter-iOS" version="4.3.2">
        </iosPod>
    </iosPods>
```

e.g., Delete `AdMob`, Delete `<iosPod name="YumiMediationAdapters/AdMob" version="4.3.2"></iosPod>`  

Complete the above procedure, Open **xcworkspace** project.

**Note: Use CocoaPods to identify iOS dependencies. CocoaPods runs as a post-build process step.**
**Note: CocoaPods will auto download the thirdparty network's SDK, you don't need add it by manual.**

## Deploy Android 

In the Unity editor, select **Assets> Play Services Resolver> Android Resolver>Force Resolve**. The Unity Play Services Resolver library will copy the declared dependencies into the  **Assets/Plugins/Android** directory of your Unity app.

![img](resources/03.png)

The YumiMediationSDK Ads Unity plugin dependencies are listed in **Assets/YumiMediationSDK/Editor/YumiMobileAdsDependencies.xml**.

Android dependencies:

```xml
<androidPackages>
  <androidPackage spec="com.yumimobi.ads:mediation:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:adcolony:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:applovin:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:playableads:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:pubnative:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:admob:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:baidu:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:bytedance:4.3.0"/>
  <androidPackage spec="com.yumimobi.ads.mediation:chartboost:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:facebook:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:gdt:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:inmobi:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:inneractive:4.3.0"/>
  <androidPackage spec="com.yumimobi.ads.mediation:oneway:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:vungle:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:ironsource:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:ksyun:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:mintegral:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:tapjoy:4.3.0" />
  <androidPackage spec="com.yumimobi.ads.mediation:unity:4.3.0" />
  <repositories>
      <repository>https://dl.bintray.com/yumimobi/thirdparty/</repository>
      <repository>https://dl.bintray.com/yumimobi/ads/</repository>
      <repository>https://dl.bintray.com/pubnative/maven</repository>
      <repository>https://tapjoy.bintray.com/maven</repository>
      <repository>https://jcenter.bintray.com/</repository>
      <repository>https://maven.google.com/</repository>
  </repositories>
</androidPackages>
```
e.g., Delete  `admob`, Delete `<androidPackage spec="com.yumimobi.ads.mediation:admob:4.3.0" />`.

**Note: Unity plugin will auto download the thirdparty network's SDK, you don't need add it by manual.**
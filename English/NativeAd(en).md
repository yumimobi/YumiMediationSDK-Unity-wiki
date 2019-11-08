# Native Ads
> Native ads match both the form and function of the user experience in which they're placed. They also match the visual design of the app they live within. 

This guide shows you how to use the YumiMediationSDK Ads Unity plugin to implement YumiMediationSDK Native Ads in a Unity app
# Prerequisites
Complete [Get Started](https://github.com/yumimobi/YumiMediationSDK-Unity/wiki/GetStarted). Your Unity app should already have the YumiMediationSDK Unity plugin imported.

# Create a Native Ad
The first step toward displaying a nativeAd is to create an `YumiNativeAd` object in a script attached to a GameObject.

```C#
using UnityEngine;
using UnityEngine.UI;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using YumiMediationSDK.Api;
using YumiMediationSDK.Common;

public class YumiNativeScene : MonoBehaviour
{
    private YumiNativeAd nativeAd;
    private YumiNativeData yumiNativeData;
    // UI elements in scene
    [Header("Text:")]
    public Text title;
    public Text body;
    [Header("Images:")]
    public GameObject mediaView;
    public GameObject iconImage;
    [Header("Buttons:")]
    // This doesn't be a button - it can also be an image
    public Button callToActionButton;

    // ad panel
    public GameObject adPanel;
  
    void Start()
    {
        this.InitNativeAd();
    }
    private void InitNativeAd()
    {
        string gameVersionId = "YOUR_VERSION_ID";
        string channelId = "YOUR_CHANNEL_ID";
        #if UNITY_ANDROID
          string nativePlacementId = "YOUR_NATIVE_PLACEMENT_ID_ANDROID";
        #elif UNITY_IOS
          string nativePlacementId = "YOUR_NATIVE_PLACEMENT_ID_IOS";
        #else
          string nativePlacementId = "unexpected_platform";
        #endif
       // you must set native  express ad view  transform if you want to support native express ad
        NativeAdOptionsBuilder builder = new NativeAdOptionsBuilder();
        builder.setExpressAdViewTransform(adPanel.transform);

        YumiNativeAdOptions options = new YumiNativeAdOptions(builder);
        // YumiNativeAdOptions options = new NativeAdOptionsBuilder().Build(); // only native ad
        nativeAd = new YumiNativeAd(NativePlacementId, ChannelId, GameVersionId, gameObject,options);
        // call back
        nativeAd.OnNativeAdLoaded += HandleNativeAdLoaded;
        nativeAd.OnAdFailedToLoad += HandleNativeAdFailedToLoad;
        nativeAd.OnAdClick += HandleNativeAdClicked;
        /// ------only available in ExpressAdView------
        nativeAd.OnExpressAdRenderSuccess += HandleNativeExpressAdRenderSuccess;
        nativeAd.OnExpressAdRenderFail += HandleNativeExpressAdRenderFail;
        nativeAd.OnExpressAdClickCloseButton += HandleNativeExpressAdClickCloseButton;
    }
    #region native call back handles
    public void HandleNativeAdLoaded(object sender, YumiNativeToLoadEventArgs args)
    {
        Logger.Log("HandleNativeAdLoaded event opened");
        if (nativeAd == null)
        {
            Logger.Log("nativeAd is null");
            return;
        }

        if (args == null || args.nativeData == null || args.nativeData.Count == 0)
        {
            Logger.Log("nativeAd data not found.");
            return;
        }
        // args.nativeData is nativeAd data
      	yumiNativeData = args.nativeData[0];
    }
    public void HandleNativeAdFailedToLoad(object sender, YumiAdFailedToLoadEventArgs args)
    {
        Logger.Log("HandleNativeAdFailedToLoad event received with message: " + args.Message);
    }
    public void HandleNativeAdClicked(object sender, EventArgs args)
    {
        Logger.Log("HandleNativeAdClicked");
    }
     /// ------only available in ExpressAdView------
     public void HandleNativeExpressAdRenderSuccess(object sender , YumiNativeDataEventArgs args)
    {
        Logger.Log("HandleNativeExpressAdRenderSuccess");
    }
    public void HandleNativeExpressAdRenderFail(object sender, YumiAdFailedToRenderEventArgs args)
    {
        Logger.Log("HandleNativeExpressAdRenderFail" + args.Message + "data id is " + args.nativeData.uniqueId);
    }
    public void HandleNativeExpressAdClickCloseButton(object sender, YumiNativeDataEventArgs args)
    {
        Logger.Log("HandleNativeExpressAdClickCloseButton" + args.nativeData.uniqueId);
    }
    #endregion
}
```
# YumiNativeAdOptions
`YumiNativeAdOptions` is the last parameter to init the `YumiNativeAd`, you can set the ad style by this.

```C#
// AdOptionViewPosition: TOP_LEFT,TOP_RIGHT,BOTTOM_LEFT,BOTTOM_RIGHT
public AdOptionViewPosition adChoiseViewPosition { get; private set; }
// AdAttribution: AdOptionsPosition、text、textColor、backgroundColor、textSize、hide
public AdAttribution adAttribution { get; private set; }
// TextOptions: textSize，textColor，backgroundColor
public TextOptions titleTextOptions { get; private set; }
public TextOptions descTextOptions { get; private set; }
public TextOptions callToActionTextOptions { get; private set; }
// ScaleType: SCALE_TO_FILL、SCALE_ASPECT_FIT、SCALE_ASPECT_FILL
public ScaleType iconScaleType { get; private set; }
public ScaleType coverImageScaleType { get; private set; }
// native express ad view  transform
public Transform expressAdViewTransform { get; private set; }
```

The default create `YumiNativeAdOptions` instance code:
```C#
YumiNativeAdOptions options = new NativeAdOptionsBuilder().Build();
```
The custom create `YumiNativeAdOptions` instance code:
```C#
 NativeAdOptionsBuilder builder = new NativeAdOptionsBuilder();
 builder.setExpressAdViewTransform(adPanel.transform);

 YumiNativeAdOptions options = new YumiNativeAdOptions(builder);
```
**If you want to support native express ad you must use `builder.setExpressAdViewTransform(adPanel.transform);` to create options objects**

# Request Native

```C#
int adCount = 1;// adCount: you can load more than one ad
this.nativeAd.LoadAd(adCount);
```

# Create Your Native Ad Layout

```C#
public class YumiNativeScene : MonoBehaviour
  {
    private YumiNativeAd nativeAd;
    // UI elements in scene
    [Header("Text:")]
    public Text title;
    public Text body;
    [Header("Images:")]
    public GameObject mediaView;
    public GameObject iconImage;
    [Header("Buttons:")]
    // This doesn't be a button - it can also be an image
    public Button callToActionButton;
    /// ...
  }
```

Here is how they can be associated with the views in the editor:

![image](./resources/nativeAd.png)

# Populating Your Layout Using the Ad's Metadata

```C#
public class YumiNativeScene : MonoBehaviour
{
  private YumiNativeAd nativeAd;
  private YumiNativeData yumiNativeData;
  private void RegisterNativeViews()
    {
        Dictionary<NativeElemetType, Transform> elementsDictionary = new Dictionary<NativeElemetType, Transform>();
        elementsDictionary.Add(NativeElemetType.PANEL, adPanel.transform);
        elementsDictionary.Add(NativeElemetType.TITLE, title.transform);
        elementsDictionary.Add(NativeElemetType.DESCRIPTION, body.transform);
        elementsDictionary.Add(NativeElemetType.ICON, iconImage.transform);
        elementsDictionary.Add(NativeElemetType.COVER_IMAGE, mediaView.transform);
        elementsDictionary.Add(NativeElemetType.CALL_TO_ACTION, callToActionButton.transform);
        // This is a method to associate a YumiNativeData with the ad assets gameobject you will use to display the native ads.
        nativeAd.RegisterNativeDataForInteraction(yumiNativeData, elementsDictionary);

    }
}
```

# Show Native Ad View
1. Native ad

```C#
// Determines whether nativeAd data is invalidated, if invalidated please reload
if (this.nativeAd.IsAdInvalidated(yumiNativeData))
  {
      Logger.Log("Native Data is invalidated");
      return;
  }
// the ad is native ad
if (!yumiNativeData.isExpressAdView)
  {
    this.nativeAd.ShowView(yumiNativeData);
  }
```
2. Native express ad 
```C#
  // if the ad is native express view please show ad in HandleNativeExpressAdRenderSuccess
  if (yumiNativeData.isExpressAdView)
  {
    // ...
  }
```

**You should check whether the ad has been invalidated before displaying it.**

# Hide Native Ad View

```C#
this.nativeAd.HideView(yumiNativeData);// Hide nativeAd data associate view
```

# Remove Native Ad View

Remove current native ad view from screen, and disconnect the native data from the view.
If you want to display a new view by this layout, call this function first.

```C#
this.nativeAd.UnregisterView(yumiNativeData);
```

# Destroy Native Ad

```C#
this.nativeAd.Destroy();
```
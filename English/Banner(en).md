# Banner Ads
> Banner ads are rectangular image or text ads that occupy a spot on screen. They stay on screen while users are interacting with the app, and can refresh automatically after a certain period of time.

This guide shows you how to integrate banner ads from YumiMediationSDK into a Unity app. 

# Prerequisites
Complete [Get Started](https://github.com/yumimobi/YumiMediationSDK-Unity/wiki/GetStarted(en)). Your Unity app should already have the YumiMediationSDK Unity plugin imported.

# Create a BannerView
The first step toward displaying a banner is to create a `YumiBannerView` object in a C# script attached to a GameObject.
```c#
using YumiMediationSDK.Api;
using YumiMediationSDK.Common;

public class YumiSDKDemo : MonoBehaviour
{
  private YumiBannerView bannerView;

  void Start()
  {
    this.InitBanner();
  }

  private void InitBanner()
  {
    string  gameVersionId = "YOUR_VERSION_ID";
    string  channelId = "YOUR_CHANNEL_ID";

    #if UNITY_ANDROID
      string bannerPlacementId = "YOUR_BANNER_PLACEMENT_ID_ANDROID";
    #elif UNITY_IOS
      string bannerPlacementId = "YOUR_BANNER_PLACEMENT_ID_IOS";
    #else
      string bannerPlacementId = "unexpected_platform";
    #endif

    // You can set the banner size & banner position & autoRefresh & IsSmart in YumiBannerViewOptions
    // This file is described below.
    YumiBannerViewOptions bannerOptions = new YumiBannerViewOptionsBuilder().Build();
    this.bannerView = new YumiBannerView(BannerPlacementId, ChannelId, GameVersionId, bannerOptions);

    /* banner add ad event */
    this.bannerView.OnAdLoaded    += HandleAdLoaded;
    this.bannerView.OnAdFailedToLoad  += HandleAdFailedToLoad;
    this.bannerView.OnAdClick   += HandleAdClicked;
  }

  #region Banner callback handlers

  public void HandleAdLoaded( object sender, EventArgs args )
  {
    Logger.Log( "HandleAdLoaded event received" );
  }

  public void HandleAdFailedToLoad( object sender, YumiAdFailedToLoadEventArgs args )
  {
    Logger.Log( "HandleFailedToReceiveAd event received with message: " + args.Message );
  }

  public void HandleAdClicked( object sender, EventArgs args )
  {
    Logger.Log( "Handle Ad Clicked" );
  }

  #endregion
}
```
# Request Banner

```C#
this.bannerView.LoadAd();
```

# Hide Banner

```C#
this.bannerView.Hide();
```

# Show Banner

```C#
this.bannerView.Show();
```

# Destroy Banner

```C#
this.bannerView.Destroy();
```

# (Optional) YumiBannerViewOptions

`YumiBannerViewOptions` is the last parameter to init `YumiBannerView`, you can get it in `YumiBannerViewOptions` file.

- `adPosition`

  Set the position of the banner in the superview.

  default is `BOTTOM`.
  
- `bannerSize`
  
  Set the banner size.

  default:
  - iPhone and iPod Touch ad size. Typically 320x50.
  - Leaderboard size for the iPad. Typically 728x90.

- `isSmart`

  Set the banner to automatically adapter the screen width.

  default is `true`.

- `disableAutoRefresh`

  default is `false`. banner will request next ad automatically.

  if you set it to `true`, then you should call `this.bannerView.LoadAd();` by manual.

The default create `YumiBannerViewOptions` instance code:
```C#
YumiBannerViewOptions bannerOptions = new YumiBannerViewOptionsBuilder().Build();
```

The custom create `YumiBannerViewOptions` instance code:
```C#
YumiBannerViewOptionsBuilder builder = new YumiBannerViewOptionsBuilder();
builder.setAdPosition(YumiAdPosition.TOP);
builder.setSmartState(false);
builder.setDisableAutoRefreshState(true);
builder.setBannerSize(YumiBannerAdSize.YUMI_BANNER_AD_SIZE_320x50);

YumiBannerViewOptions bannerOptions = new YumiBannerViewOptions(builder);
```

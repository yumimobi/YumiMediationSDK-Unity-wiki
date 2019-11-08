# Interstitial Ads
> Interstitial ads are full-screen ads that cover the interface of their host app.

This guide explains how to integrate interstitial ads into a Unity app.

# Prerequisites
Complete [Get Started](https://github.com/yumimobi/YumiMediationSDK-Unity/wiki/GetStarted(en)). Your Unity app should already have the YumiMediationSDK Unity plugin imported.

# Create a interstitial ad
The first step toward displaying a interstitial is to create an `YumiInterstitialAd` object in a script attached to a `GameObject`.

**The interstitial ad will auto cached when create an interstitial ad**

```C#
using YumiMediationSDK.Api;
using YumiMediationSDK.Common;
public class YumiSDKDemo : MonoBehaviour 
{
  private YumiInterstitialAd interstitialAd;
  void Start() 
  {
    this.RequestInterstitial();
  }
  private void RequestInterstitial() 
  {
    string gameVersionId = "YOUR_VERSION_ID";
    string channelId = "YOUR_CHANNEL_ID";
    #if UNITY_ANDROID
      string interstitialPlacementId = "YOUR_INTERSTITIAL_PLACEMENT_ID_ANDROID";
    #elif UNITY_IOS
      string interstitialPlacementId = "YOUR_INTERSTITIAL_PLACEMENT_ID_IOS";
    # else
      string interstitialPlacementId = "unexpected_platform";
    #endif
    this.interstitialAd = new YumiInterstitialAd(interstitialPlacementId, channelId, gameVersionId);

    // add interstitial event
    this.interstitialAd.OnAdLoaded += HandleInterstitialAdLoaded;
    this.interstitialAd.OnAdFailedToLoad += HandleInterstitialAdFailedToLoad;
    this.interstitialAd.OnAdClicked += HandleInterstitialAdClicked;
    this.interstitialAd.OnAdClosed += HandleInterstitialAdClosed;
    this.interstitialAd.OnAdFailedToShow += HandleInterstitialAdFailedToShow;
    this.interstitialAd.OnAdOpening += HandleInterstitialAdOpened;
    this.interstitialAd.OnAdStartPlaying += HandleInterstitialAdStartPlaying;
  }
  
  #region interstitial callback handlers
  public void HandleInterstitialAdLoaded(object sender, EventArgs args)
  {
      Logger.Log("HandleInterstitialAdLoaded event received");
  }

  public void HandleInterstitialAdFailedToLoad(object sender, YumiAdFailedToLoadEventArgs args)
  {
      Logger.Log("HandleInterstitialAdFailedToLoad event received with message: " + args.Message);
  }

  public void HandleInterstitialAdClicked(object sender, EventArgs args)
  {
      Logger.Log("HandleInterstitialAdClicked Clicked");
  }
  public void HandleInterstitialAdClosed(object sender, EventArgs args)
  {
      Logger.Log("HandleInterstitialAdClosed Ad closed");
  }

  public void HandleInterstitialAdFailedToShow(object sender, YumiAdFailedToShowEventArgs args)
  {
      Logger.Log("HandleInterstitialAdFailedToShow event received with message: " + args.Message);
  }
  public void HandleInterstitialAdOpened(object sender, EventArgs args)
  {
      Logger.Log("HandleInterstitialAdOpened  ad opened ");
  }
  public void HandleInterstitialAdStartPlaying(object sender, EventArgs args)
  {
      Logger.Log("HandleInterstitialAdStartPlaying event StartPlaying ");
  }
  #endregion
}
```
**The interstitialAd object will auto load another ad when interstitialAd had closed. You don't need to create a new YumiInterstitialAd object**

# Show Interstitial
It is recommended to call `this.interstitialAd.IsReady()` to determine if the screen is ready.

```C#
if(this.interstitialAd.IsReady())
{
  this.interstitialAd.Show();
}
```
# Destroy Interstitial

```c#
this.interstitialAd.Destroy();
```
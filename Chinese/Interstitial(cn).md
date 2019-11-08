# 插屏广告
> 插屏广告是全屏广告，它会覆盖其托管应用的整个界面

本文档介绍如何将插屏广告集成到您的 Unity 应用中。

# 前提条件
完成[入门指南](https://github.com/yumimobi/YumiMediationSDK-Unity-wiki/wiki/GetStarted(cn))的介绍。您的 Unity 应用应该已经导入了 YumiMediationSDK Unity 插件。

# 创建插屏广告
要展示插屏广告，首先要在附加到 `GameObject` 的脚本中创建 `YumiInterstitialAd` 对象。

**当 interstitial 对象被创建时，它会自动缓存广告**

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
**当前 interstitialAd 关闭后，这个 interstitialAd 对象将自动加载下一条广告。 您无需创建新的 `YumiInterstitialAd`对象**

# 展示插屏广告
建议先调用 `this.interstitialAd.IsReady()` 判断插屏是否准备好

```C#
if(this.interstitialAd.IsReady())
{
  this.interstitialAd.Show();
}
```
# 销毁插屏广告

```c#
this.interstitialAd.Destroy();
```
# RewardVideo Ads
> Rewarded ads are ads that users have the option of interacting with in exchange for in-app rewards. 

This guide shows you how to integrate rewarded video ads from YumiMediationSDK into a Unity app.

# Prerequisites
Complete [Get Started](https://github.com/yumimobi/YumiMediationSDK-Unity/wiki/GetStarted(en)). Your Unity app should already have the YumiMediationSDK Unity plugin imported.

# Get reference to singleton instance
The first step toward displaying a reward video ad is to get a reference to the singleton `YumiRewardVideoAd` instance. This reference can be retrieved by calling `YumiRewardVideoAd.Instance`.

```C#
using YumiMediationSDK.Api;
using YumiMediationSDK.Common;
public class YumiSDKDemo : MonoBehaviour 
{
  private YumiRewardVideoAd rewardVideoAd;
  void Start() 
  {
    this.RequestRewardVideo();
  }
  private void RequestRewardVideo() 
  {
    string gameVersionId = "YOUR_VERSION_ID";
    string channelId = "YOUR_CHANNEL_ID";
    #if UNITY_ANDROID
      string rewardVideoPlacementId = "YOUR_REWARDVIDEO_PLACEMENT_ID_ANDROID";
    #elif UNITY_IOS
      string rewardVideoPlacementId = "YOUR_REWARDVIDEO_PLACEMENT_ID_IOS";
    # else
      string rewardVideoPlacementId = "unexpected_platform";
    #endif
    this.rewardVideoAd = YumiRewardVideoAd.Instance;

    this.rewardVideoAd.OnAdOpening += HandleRewardVideoAdOpened;
    this.rewardVideoAd.OnAdStartPlaying += HandleRewardVideoAdStartPlaying;
    this.rewardVideoAd.OnAdRewarded += HandleRewardVideoAdReward;
    this.rewardVideoAd.OnRewardVideoAdClosed += HandleRewardVideoAdClosed;
    this.rewardVideoAd.OnAdLoaded += HandleRewardVideoAdLoaded;
    this.rewardVideoAd.OnAdFailedToLoad += HandleRewardVideoAdFailedToLoad;
    this.rewardVideoAd.OnAdFailedToShow += HandleRewardVideoAdFailedToShow;
    this.rewardVideoAd.OnAdClicked += HandleRewardVideoAdClicked;

    // Initiates the ad request, should only be called once as early as possible.
    this.rewardVideoAd.LoadAd(rewardVideoPlacementId, channelId, gameVersionId);
  }
  
  #region reward video callback handlers
  public void HandleRewardVideoAdOpened(object sender, EventArgs args)
  {
      Logger.Log("HandleRewardVideoAdOpened event opened");
  }

  public void HandleRewardVideoAdStartPlaying(object sender, EventArgs args)
  {
      Logger.Log("HandleRewardVideoAdStartPlaying event start playing ");
  }

  public void HandleRewardVideoAdReward(object sender, EventArgs args)
  {
      Logger.Log("HandleRewardVideoAdReward reward");
  }
  public void HandleRewardVideoAdClosed(object sender, YumiAdCloseEventArgs args)
  {
      Logger.Log("HandleRewardVideoAdClosed Ad closed result is  " + args.IsRewarded);
  }
  public void HandleRewardVideoAdLoaded(object sender, EventArgs args)
  {
      Logger.Log("HandleRewardVideoAdLoaded event received");
  }

  public void HandleRewardVideoAdFailedToLoad(object sender, YumiAdFailedToLoadEventArgs args)
  {
      Logger.Log("HandleRewardVideoAdFailedToLoad event received with message: " + args.Message);
  }

  public void HandleRewardVideoAdFailedToShow(object sender, YumiAdFailedToShowEventArgs args)
  {
      Logger.Log("HandleRewardVideoAdFailedToShow event with message: " + args.Message);
  }
  public void HandleRewardVideoAdClicked(object sender, EventArgs args)
  {
      Logger.Log("HandleRewardVideoAdClicked Clicked");
  }
  #endregion
}
```
# Load Reward Video
**Initiates the ad request, should only be called once as early as possible.**
```C#
  this.rewardVideoAd.LoadAd(rewardVideoPlacementId, channelId, gameVersionId);
```

# Show Reward Video
To show a reward video ad, check the `IsReady()` method to verify that it's finished loading, then call `Play()`. Here's an example of how to do this:
```c#
if(this.rewardVideoAd.IsReady())
{
  this.rewardVideoAd.Play();
}
```


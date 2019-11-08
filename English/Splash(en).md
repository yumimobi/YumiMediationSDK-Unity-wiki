**Splash Ads**
>Splash ads is a special ad format that lets you monetize your app load screens. The ads are shown when users cold launch your app or bring it to the foreground. While the ad shows for a few seconds, it can be closed by your users at any time.

This guide shows you how to integrate Splash ads from YumiMediationSDK into a Unity app.

# Prerequisites
Complete [Get Started](https://github.com/yumimobi/YumiMediationSDK-Unity/wiki/GetStarted). Your Unity app should already have the YumiMediationSDK Unity plugin imported.

# Import YumiSplashScene
Fllow the steps as the image showing:
![image](./resources/splashScene.png)

**Recommend:**
Set your app's launchImage as `YumiSplashScene`'s background image.

# Set Ad Placement
Set your ad information in the `void Start()` method in the **YumiMediationSDK/Api/YumiSplashScript** file.
```C#
void Start()
    {
      #if UNITY_ANDROID
        SplashPlacementId = "YOUR_SPLASH_PLACEMENT_ID_ANDROID";
      #elif UNITY_IOS
        SplashPlacementId = "YOUR_SPLASH_PLACEMENT_ID_IOS";
      #else
        SplashPlacementId = "unexpected_platform";
      #endif
      GameVersionId = "YOUR_GAME_VERSION";
      ChannelId = "YOUR_CHANNEL_ID";
      // ...
    }
```
# Handle Delegate
Show your logic when the splash close or fail to show.
Replace `YOUR_MAIN_SCENE` with your main scene name.
- Set `YOUR_MAIN_SCENE` is your main scene
```C#
    private void InputMainSence()
    {
        SceneManager.LoadScene("YOUR_MAIN_SCENE");
    }
```

# YumiSplashOptions
`YumiSplashOptions` is the last parameter to init `YumiSplashAd`, you can get it in `YumiSplashOptions` file.
- `adFetchTime`

  Fetch ad timeout duration , default 3s. 
  Over the deadline, sdk will return the `HandleSplashAdFailToShow` delegate, otherwise will return the `HandleSplashAdSuccssToShow` delegate,and display the splash ad.

- `adOrientation`
  Splash ad orientation. only Admob support this function.

- `adBottomViewHeight`
  The height of the ad bottom view.
  Bottom view's height should not exceed 15% of the screen height.

  Create default instance of `YumiSplashOptions`:
  ```C#
  YumiSplashOptions splashOptions = new YumiSplashOptionsBuilder().Build();
  ```
  Create custom instance of `YumiSplashOptions`:
  ```C#
  YumiSplashOptionsBuilder builder = new YumiSplashOptionsBuilder();
  builder.setAdBottomViewHeight(100);
  builder.setAdFetchTime(3);
  builder.setAdOrientation(YumiSplashOrientation.YUMISPLASHORIENTATION_PORTRAIT);

  YumiSplashOptions splashOptions = new YumiSplashOptions(builder);
  ```

# Show splash with bottom custom view
You can set your logo view in bottomView's location.
```C#
/// bottom view's height should not exceed 15% of the screen height.
YumiSplashOptionsBuilder builder = new YumiSplashOptionsBuilder().setAdBottomViewHeight(100);
YumiSplashOptions splashOptions = new YumiSplashOptions(builder);

YumiSplashAd splashAd = new YumiSplashAd(SplashPlacementId, ChannelId, GameVersionId, splashOptions);

// ...

```
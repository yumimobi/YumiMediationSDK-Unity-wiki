# GDPR
>This documentation is provided for compliance with the European Union's General Data Protection Regulation (GDPR).       
 If you are collecting consent from your users, you can make use of APIs discussed below to inform YumiMediationSDK and some downstream consumers of this information. 
 Get more information, please visit our official website.

 # Set GDPR

```C#
public enum YumiConsentStatus
    {
		/// <summary>
		/// The user has granted consent for personalized ads.
		/// </summary>
		PERSONALIZED,

		/// <summary>
		/// The user has granted consent for non-personalized ads.
		/// </summary>
		NONPERSONALIZED,
		/// <summary>
		///  The user has neither granted nor declined consent for personalized or non-personalized ads.
		/// </summary>
		UNKNOWN

	}
```

```C#
// Your user's consent. In this case, the user has given consent to store and process personal information.
YumiGDPRManager.Instance.UpdateNetworksConsentStatus(YumiConsentStatus.PERSONALIZED);
```

# Networks informations
Statistics start at YumiMediationSDK 4.1.0.
Get more informationm, please visit our official website

| Ad Network | GDPR Support | Note |
| :----: | :--------:| :--: |
| Unity  | yes |   |
| Admob  | yes |   |
| Mintegral | yes |   |
| Adcolony  | yes |   |
| IronSource  | yes |   |
| Inneractive | yes |   |
| Chartboost | yes |   |
| InMobi | yes |   |
| IQzone | yes |   |
| Yumi | yes |   |
| AppLovin  | yes |   |
| Baidu  | no |   |
| Facebook | no | Get more information, please visit Facebook website. |
| Domob  | no |   |
| GDT | no |   |
| Vungle | no | setting in Vungle dashboard |
| OneWay | no |   |
| BytedanceAds | no |   |
| ZplayAds  | no |   |

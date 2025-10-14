---
title: 裝置上決策支援哪些功能？
description: 瞭解如何使用即時伺服器呼叫，透過機器學習來提供最相關且最吸引人的個人化內容。
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 13%

---

# 支援的功能概述

[!DNL Adobe Target]的伺服器端SDK可讓開發人員在效能與資料新鮮度之間做出選擇，進而做出決策。 換言之，如果透過機器學習提供最相關且最吸引人的個人化內容對您而言至關重要，則應進行即時伺服器呼叫。 但是，當效能較重要時，則應該進行裝置上決策。 若要讓[!UICONTROL on-device decisioning]運作，請參閱下列支援功能清單：

* 活動類型
* 對象目標定位
* 配置方法

## 活動類型

下表指出[!UICONTROL on-device decisioning]支援或不支援使用[表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=zh-Hant&)建立的[活動型別](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=zh-Hant)。

| 活動類型 | 支援 |
| --- | --- |
| [A/B 測試](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=zh-Hant) | 是 |
| [自動分配](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=zh-Hant) | 無 |
| [自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=zh-Hant) | 無 |
| [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=zh-Hant) (A4T) | 是 |
| [多變數測試](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=zh-Hant) (MVT) | 否 |
| [體驗鎖定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=zh-Hant) (XT) | 是 |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=zh-Hant) (AP) | 無 |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=zh-Hant) | 無 |


## 對象目標定位

下表指出[!UICONTROL on-device decisioning]支援或不支援的對象規則。

| 對象規則 | 裝置上決策 |
| --- | --- |
| [地理](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=zh-Hant) | 是 |
| [網路](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=zh-Hant) | 否 |
| [行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=zh-Hant) | 否 |
| [自訂引數](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=zh-Hant) | 是 |
| [作業系統 &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=zh-Hant) | 是 |
| [網頁](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=zh-Hant) | 是 |
| [瀏覽器](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=zh-Hant) | 是 |
| [訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=zh-Hant) | 否 |
| [流量來源](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=zh-Hant) | 否 |
| [時間範圍](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=zh-Hant) | 是 |
| [Experience Cloud對象](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=zh-Hant) (來自Adobe Audience Manager、Adobe Analytics和Adobe Experience Manager的對象 | 否 |

### [!UICONTROL on-device decisioning]的地理定位

為了維持具有地理型對象的[!UICONTROL on-device decisioning]活動近乎零延遲，Adobe建議您在`getOffers`的呼叫中自行提供地理值。 若要這麼做，請在要求的`Context`中設定`Geo`物件。 這表示您的伺服器需要一種方式來確定每個一般使用者的位置。 例如，您的伺服器可能會使用您設定的服務，執行IP對地理位置的查詢。 某些託管提供者(例如Google Cloud)會透過每個`HttpServletRequest`中的自訂標頭提供此功能。

>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        context: {
            geo: {
                city: "SAN FRANCISCO",
                countryCode: "US",
                stateCode: "CA",
                latitude: 37.75,
                longitude: -122.4
            }
        },
        execute: {
            pageLoad: {}
        }
    }
})
```

>[!TAB Java]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(ipToGeoLookup(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

    public static Geo ipToGeoLookup(String ip) {
        GeoResult geoResult = geoLookupService.lookup(ip);
        return new Geo()
            .city(geoResult.getCity())
            .stateCode(geoResult.getStateCode())
            .countryCode(geoResult.getCountryCode());
    }

}
```

>[!ENDTABS]

不過，如果您無法在伺服器上執行IP對地理位置的查詢，但您仍想要針對包含地理位置型對象的`getOffers`要求執行[!UICONTROL on-device decisioning]，也支援此功能。 此方法的缺點在於它將使用遠端IP對地理的查閱，這會增加每個`getOffers`呼叫的延遲。 此延遲應低於遠端`getOffers`呼叫，因為它點選了位於伺服器附近的CDN。 您必須在要求的`Context`中的`Geo`物件中僅提供`ipAddress`欄位，SDK才能擷取您使用者IP位址的地理位置。 如果提供除了`ipAddress`之外的任何其他欄位，[!DNL Target] SDK將不會擷取地理位置中繼資料以進行解析。


>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        context: {
            geo: {
                ipAddress: "127.0.0.1"
            }
        },
        execute: {
            pageLoad: {}
        }
    }
})
```

>[!TAB Java]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(new Geo().ipAddress(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

}
```

>[!ENDTABS]

## 配置方法

下表指出[!UICONTROL on-device decisioning]支援或不支援的配置方法。

| 配置方法 | 支援 |
| --- | --- |
| 手動 | 是 |
| [自動分配至最佳體驗](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=zh-Hant) | 否 |
| [針對個人化體驗自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html?lang=zh-Hant) | 否 |

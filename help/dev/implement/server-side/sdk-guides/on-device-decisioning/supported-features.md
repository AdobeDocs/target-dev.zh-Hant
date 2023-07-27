---
title: 裝置上決策支援哪些功能？
description: 瞭解如何使用即時伺服器呼叫，透過機器學習來提供最相關且最吸引人的個人化內容。
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '657'
ht-degree: 10%

---

# 支援的功能概述

[!DNL Adobe Target]的伺服器端SDK讓開發人員可靈活地選擇效能與資料新鮮度來做出決策。 換言之，如果透過機器學習提供最相關且最吸引人的個人化內容對您而言至關重要，則應進行即時伺服器呼叫。 但是，當效能較重要時，則應該進行裝置上決策。 的 [!UICONTROL 裝置上決策] 若要使用，請參閱以下支援功能清單：

* 活動類型
* 對象目標定位
* 配置方法

## 活動類型

下表指出其 [活動型別](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) 使用建立 [表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?) 支援或不支援 [!UICONTROL 裝置上決策].

| 活動類型 | 支援 |
| --- | --- |
| [A/B 測試](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) | 是 |
| [自動分配](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | 無 |
| [自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) | 無 |
| [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) (A4T) | 是 |
| [多變數測試](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html) (MVT) | 否 |
| [體驗鎖定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html) (XT) | 是 |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) (AP) | 無 |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html) | 無 |


## 對象目標定位

下表指出支援或不支援的對象規則 [!UICONTROL 裝置上決策].

| 對象規則 | 裝置上決策 |
| --- | --- |
| [地理](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | 是 |
| [網路](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | 否 |
| [行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | 否 |
| [自訂參數](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | 是 |
| [作業系統 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | 是 |
| [網頁](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | 是 |
| [瀏覽器](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | 是 |
| [訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | 否 |
| [流量來源](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | 否 |
| [時間範圍](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | 是 |
| [Experience Cloud對象](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Adobe Audience Manager、Adobe Analytics和Adobe Experience Manager的受眾 | 否 |

### 的地理目標定位 [!UICONTROL 裝置上決策]

為了維持幾乎零延遲， [!UICONTROL 裝置上決策] 活動包含以地理為基礎的對象，Adobe建議您在呼叫中提供地理值， `getOffers`. 若要這麼做，請設定 `Geo` 中的物件 `Context` 請求的。 這表示您的伺服器需要一種方式來確定每個一般使用者的位置。 例如，您的伺服器可能會使用您設定的服務，執行IP對地理位置的查詢。 有些託管提供者(例如Google Cloud)會透過每個中的自訂標題提供此功能 `HttpServletRequest`.

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

不過，如果您無法在伺服器上執行IP對地理位置的查詢，但您仍想要執行 [!UICONTROL 裝置上決策] 的 `getOffers` 如果請求包含以地理為基礎的對象，這也受到支援。 此方法的缺點在於它將使用遠端IP對地理的查詢，這會為每個查詢增加延遲 `getOffers` 呼叫。 此延遲應低於遠端 `getOffers` 呼叫，因為它點選了位在伺服器附近的CDN。 您必須僅提供 `ipAddress` 中的欄位 `Geo` 中的物件 `Context` ，以便SDK擷取您使用者IP位址的地理位置。 如果除了任何其他欄位 `ipAddress` 「 」提供的 [!DNL Target] SDK不會擷取地理位置中繼資料以進行解析。


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

下表指出支援或不支援的配置方法 [!UICONTROL 裝置上決策].

| 配置方法 | 支援 |
| --- | --- |
| 手動 | 是 |
| [自動分配至最佳體驗](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | 否 |
| [針對個人化體驗自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html) | 否 |

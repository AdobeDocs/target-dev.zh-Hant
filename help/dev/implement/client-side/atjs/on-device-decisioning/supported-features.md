---
keywords: 實作， javascript資料庫， js， atjs，裝置上決策，裝置上決策，支援的功能， $8
description: 瞭解[!UICONTROL on-device decisioning]支援哪些功能。
title: 裝置上決策支援哪些功能
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
source-git-commit: 79ffa3f58d780f587fe1202b82d3860395504dfe
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 12%

---

# [!UICONTROL on-device decisioning]支援的功能

[!DNL Adobe Target] JS SDK讓客戶可靈活選擇資料的效能與最新狀態，以便做出決策。 換言之，如果透過機器學習提供最相關且最吸引人的個人化內容對您而言至關重要，則應進行即時伺服器呼叫。 但是，當效能較為重要時，就應該做出裝置上及記憶體中的決策。 若要讓[!UICONTROL on-device decisioning]運作，請參閱下列區段，其中列出支援的功能。

## 支援的活動類型

下表指出哪些[活動型別](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=zh-Hant)是由[表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=zh-Hant)或[視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=zh-Hant) (VEC)所建立，支援[!UICONTROL on-device decisioning]或不支援。

| 活動類型 | 支援? |
| --- | --- |
| [A/B 測試](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=zh-Hant) | 是 |
| [自動分配](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=zh-Hant) | 否 |
| [自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=zh-Hant) ![進階版](../../../assets/premium.png) | 否 |
| [多變數測試](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=zh-Hant) (MVT) | 否 |
| [體驗鎖定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=zh-Hant) (XT) | 是 |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=zh-Hant) ![進階版](../../../assets/premium.png) | 否 |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=zh-Hant) ![進階版](../../../assets/premium.png) | 否 |
| 使用Analytics for Target[&#128279;](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=zh-Hant&) (A4T)的活動 | 是 |

## 對象目標定位

下表指出[!UICONTROL on-device decisioning]支援或不支援的對象規則。

| 對象規則 | 支援? |
| --- | --- |
| [地理](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=zh-Hant) | 是<P>使用裝置上決策時，支援下列地理屬性：<ul><li>國家/地區</li><li>城市</li><li>緯度</li><li>經度</li></ul> |
| [網路](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=zh-Hant) | 否 |
| [行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=zh-Hant) | 否 |
| [自訂參數](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=zh-Hant) | 是 |
| [作業系統 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=zh-Hant) | 是 |
| [網頁](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=zh-Hant) | 是 |
| [瀏覽器](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=zh-Hant) | 是 |
| [訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=zh-Hant) | 否 |
| [流量來源](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=zh-Hant) | 否 |
| [時間範圍](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=zh-Hant) | 是 |
| Adobe Experience Cloud受眾<P>（[!DNL Audiences from Adobe Analytics]、[!DNL Adobe Audience Manager]和[!DNL Adobe Experience Manager]） | 否 |

### [!UICONTROL on-device decisioning]的地理定位

若要針對具有地理型對象的[!UICONTROL on-device decisioning]活動維持最低延遲度，Adobe建議您在[getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)的呼叫中自行提供地理值。 在請求的內容中設定Geo物件。 這表示透過瀏覽器判斷每位訪客所在位置。 例如，您可以使用您設定的服務執行IP對地理位置的查詢。 某些託管提供者(例如Google Cloud)會透過每個`HttpServletRequest`中的自訂標頭提供此功能。

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
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

不過，如果您無法在伺服器上執行IP對地理的查詢，但您仍想要對包含地理型受眾的[getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)要求執行[!UICONTROL on-device decisioning]，也支援此功能。 此方法的缺點在於它使用遠端IP對地理的查閱，這會增加每個`getOffers`呼叫的延遲。 此延遲應低於具有伺服器端決策的`getOffers`呼叫，因為它會點選位於伺服器附近的CDN。 在要求SDK擷取訪客IP位址之地理位置的內容中，僅提供地理物件的「ipAddress」欄位。 如果提供除了「ipAddress」之外的任何其他欄位，[!DNL Target] SDK將不會擷取地理位置中繼資料以進行解析。

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
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

### 配置方法

下表指出[!UICONTROL on-device decisioning]支援或不支援的配置方法。

| 配置方法 | 支援? |
| --- | --- |
| 手動 | 是 |
| [自動分配至最佳體驗](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=zh-Hant) | 否 |
| [針對個人化體驗自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=zh-Hant) | 否 |

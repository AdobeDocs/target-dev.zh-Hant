---
keywords: 實作， javascript資料庫， js， atjs，裝置上決策，裝置上決策，支援的功能， $8
description: 瞭解支援哪些功能 [!UICONTROL 裝置上決策].
title: 裝置上決策支援哪些功能
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 9%

---

# 支援的功能 [!UICONTROL 裝置上決策]

此 [!DNL Adobe Target] JS SDK讓客戶可靈活選擇資料的效能與最新狀態，以便做出決策。 換言之，如果透過機器學習提供最相關且最吸引人的個人化內容對您而言至關重要，則應進行即時伺服器呼叫。 但是，當效能較為重要時，就應該做出裝置上及記憶體中的決策。 的 [!UICONTROL 裝置上決策] 若要使用，請參閱以下列出所支援功能的章節。

## 支援的活動類型

下表指出其 [活動型別](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) 建立者： [表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) 或 [視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC)支援或不支援 [!UICONTROL 裝置上決策].

| 活動類型 | 支援? |
| --- | --- |
| [A/B 測試](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) | 是 |
| [自動分配](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | 無 |
| [自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) ![Premium](../../../assets/premium.png) | 否 |
| [多變數測試](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html) (MVT) | 否 |
| [體驗鎖定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html) (XT) | 是 |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) ![Premium](../../../assets/premium.png) | 無 |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html) ![Premium](../../../assets/premium.png) | 否 |
| [使用目標分析(Analytics)的活動](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?) (A4T) | 是 |

## 對象目標定位

下表指出支援或不支援的對象規則 [!UICONTROL 裝置上決策].

| 對象規則 | 支援? |
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
| Adobe Experience Cloud受眾<P>([!DNL Audiences from Adobe Analytics], [!DNL Adobe Audience Manager], 和 [!DNL Adobe Experience Manager]) | 否 |

### 的地理目標定位 [!UICONTROL 裝置上決策]

為了將延遲維持在最小 [!UICONTROL 裝置上決策] 活動包含以地理為基礎的對象，Adobe建議您在呼叫中提供地理值， [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md). 在請求的內容中設定Geo物件。 這表示透過瀏覽器判斷每位訪客所在位置。 例如，您可以使用您設定的服務執行IP對地理位置的查詢。 有些託管提供者(例如Google Cloud)會透過每個中的自訂標題提供此功能 `HttpServletRequest`.

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

不過，如果您無法在伺服器上執行IP對地理位置的查詢，但您仍想要執行 [!UICONTROL 裝置上決策] 的 [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) 如果請求包含以地理為基礎的對象，這也受到支援。 此方法的缺點在於它使用遠端IP對地理的查詢，這會為每個查詢增加延遲 `getOffers` 呼叫。 此延遲應低於 `getOffers` 使用伺服器端決策呼叫，因為它點選了位在伺服器附近的CDN。 在要求SDK擷取訪客IP位址之地理位置的內容中，僅提供地理物件的「ipAddress」欄位。 如果提供「ipAddress」以外的任何其他欄位， [!DNL Target] SDK不會擷取地理位置中繼資料以進行解析。

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

下表指出支援或不支援的配置方法 [!UICONTROL 裝置上決策].

| 配置方法 | 支援? |
| --- | --- |
| 手動 | 是 |
| [自動分配至最佳體驗](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | 否 |
| [針對個人化體驗自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) | 否 |

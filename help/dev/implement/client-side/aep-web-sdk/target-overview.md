---
title: 使用 [!DNL Adobe Target] 搭配 [!DNL Web SDK] 進行個人化。
description: 瞭解如何使用 [!DNL Experience Platform Web SDK] 使用 [!DNL Adobe Target]呈現個人化內容。
feature: AEP Web SDK
source-git-commit: 1fe6adc25604612ed9fa090f1f68c18b9c0bdf63
workflow-type: tm+mt
source-wordcount: '1342'
ht-degree: 5%

---

# 使用[!DNL Adobe Target]和[!DNL Web SDK]進行個人化

[!DNL Adobe Experience Platform] [!DNL Web SDK]可以傳送並轉譯在[!DNL Adobe Target]中管理的個人化體驗至Web Channel。 您可以使用稱為[視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=zh-Hant) (VEC)的WYSIWYG編輯器，或是非視覺化介面[表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=zh-Hant)，來建立、啟用及傳遞您的活動和個人化體驗。

>[!IMPORTANT]
>
>瞭解如何使用[!DNL Target]將Target從at.js 2.x移轉至Experience Platform Web SDK[!DNL Experience Platform Web SDK]教學課程，將您的[實作移轉至](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=zh-Hant)。
>
>瞭解如何透過[!DNL Target]使用Web SDK實作Adobe Experience Cloud[教學課程來第一次實作](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=zh-Hant)。 如需[!DNL Target]的特定資訊，請參閱教學課程中標題為[使用Experience Platform Web SDK設定Target ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html?lang=zh-Hant)的部分。

下列功能已經過測試，目前在[!DNL Target]中支援：

* [A/B 測試](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=zh-Hant)
* [A4T曝光和轉換報告](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=zh-Hant)
* [Automated Personalization活動](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=zh-Hant)
* [體驗鎖定目標活動](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=zh-Hant)
* [多變數測試 (MVT)](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=zh-Hant)
* [Recommendations活動](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=zh-Hant)
* [原生目標印象和轉換報告](https://experienceleague.adobe.com/docs/target/using/reports/reports.html?lang=zh-Hant)
* [VEC支援](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=zh-Hant)

## [!DNL Web SDK]系統圖表

下圖可協助您瞭解[!DNL Target]和[!DNL Web SDK]邊緣決策的工作流程。

![使用Experience Platform Web SDK的Adobe Target Edge Decisioning圖表](/help/dev/implement/client-side/aep-web-sdk/assets/target-platform-web-sdk-new.png)

| 呼叫 | 詳細資料 |
| --- | --- |
| 1 | 裝置載入[!DNL Web SDK]。 [!DNL Web SDK]傳送要求至Edge Network並包含XDM資料、資料串流環境ID、傳入引數和客戶ID （選用）。 頁面（或容器）已預先隱藏。 |
| 2 | Edge Network會傳送要求給Edge服務，以使用訪客ID、同意和其他訪客內容資訊（例如地理位置和方便使用的裝置名稱）來擴充要求。 |
| 3 | Edge Network會使用訪客ID和傳入的引數將擴充的個人化要求傳送至[!DNL Target]邊緣。 |
| 4 | 設定檔指令碼執行，然後注入至[!DNL Target]設定檔儲存體。 設定檔儲存體會從[!UICONTROL Audience Library]擷取區段（例如，從[!DNL Adobe Analytics]、[!DNL Adobe Audience Manager]、[!DNL Adobe Experience Platform]共用的區段）。 |
| 5 | 根據URL要求引數和設定檔資料，[!DNL Target]會決定要針對目前頁面檢視和未來預先擷取檢視，為訪客顯示哪些活動和體驗。 [!DNL Target]然後將此資料傳回Edge Network。 |
| 6 | a. Edge Network會將個人化回應傳送回頁面，選擇性地包括其他個人化的設定檔值。 目前頁面上的個人化內容會儘快出現，不會有忽隱忽現的預設內容。<br>b。作為使用者在單頁應用程式(SPA)中的動作結果而針對檢視顯示的個人化內容會進行快取，以便在觸發檢視時立刻套用，不需額外的伺服器呼叫。 <br>c。Edge Network會傳送訪客ID和Cookie中的其他值，例如同意、工作階段ID、身分、Cookie檢查、個人化。 |
| 7 | 網頁SDK會將通知從裝置傳送至Edge Network。 |
| 8 | Edge Network將[!UICONTROL Analytics for Target] (A4T)詳細資料（活動、體驗和轉換中繼資料）轉送到[!DNL Analytics]邊緣。 |

## 正在啟用[!DNL Adobe Target]

若要啟用[!DNL Target]，請執行下列動作：

1. 使用適當的使用者端代碼啟用[!DNL Target]資料流[中的](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/datastreams/overview)。
1. 將`renderDecisions`選項新增至您的事件。

然後，您也可選擇新增下列選項：

* **`decisionScopes`**：將此選項新增至您的事件，以擷取特定活動（適用於使用表單式撰寫器建立的活動）。
* **[預先隱藏程式碼片段](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/personalization/manage-flicker)**：僅隱藏頁面的某些部分。

## 使用[!UICONTROL Adobe Target] VEC

若要搭配[!DNL Web SDK]實作使用VEC，請安裝並啟動[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)或[Chrome](https://experienceleague.adobe.com/zh-hant/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension) VEC Helper擴充功能。

如需詳細資訊，請參閱[Adobe Target指南](https://experienceleague.adobe.com/docs/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html?lang=zh-Hant)中的&#x200B;*視覺化體驗撰寫器Helper擴充功能*。

## 呈現個人化內容

如需詳細資訊，請參閱[呈現個人化內容](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)。

## XDM中的受眾

為透過[!DNL Target]傳遞的[!DNL Web SDK]活動定義對象時，必須定義並使用[XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=zh-Hant)。 定義XDM結構描述、類別和結構描述欄位群組後，您可以建立由XDM資料定義的[!DNL Target]對象規則以用於目標定位。 在[!DNL Target]內，XDM資料會在[!UICONTROL Audience Builder]中顯示為自訂引數。 XDM是使用點標籤法序列化（例如，`web.webPageDetails.name`）。

如果您的[!DNL Target]活動具有使用自訂引數或使用者設定檔的預定義對象，則無法透過SDK正確傳送這些對象。 您必須改用XDM，而不是使用自訂引數或使用者設定檔。 不過，有透過[!DNL Web SDK]支援的現成受眾目標定位欄位不需要XDM。 這些欄位可在[!DNL Target] UI中使用，不需要XDM：

* Target 資料庫
* 地理
* 網路
* 作業系統
* 網頁
* 瀏覽器
* 流量來源
* 時間段

如需詳細資訊，請參閱[Adobe Target指南](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-rules.html?lang=zh-Hant)中的&#x200B;*對象*&#x200B;類別。

### 回應 Token

回應Token可用來將中繼資料傳送給第三方，例如Google或Facebook。 傳回回應Token
在`meta` > `propositions`內的`items`欄位中。

範例如下：

```json
{
  "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI2NzM2IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
  "scope": "__view__",
  "scopeDetails": ...,
  "renderAttempted": true,
  "items": [
    {
      "id": "0",
      "schema": "https://ns.adobe.com/personalization/dom-action",
      "meta": {
        "experience.id": "0",
        "activity.id": "126736",
        "offer.name": "Default Content",
        "offer.id": "0"
      }
    }
  ]
}
```

若要收集回應Token，您必須訂閱`alloy.sendEvent` Promise、透過`propositions`反複運算，並從`items` -> `meta`擷取詳細資料。

每個`proposition`都有一個`renderAttempted`布林值欄位，指出`proposition`是否已轉譯。 請參閱下列範例：

```js
alloy("sendEvent",
  {
    "renderDecisions": true,
    "decisionScopes": [
      "hero-container"
    ]
  }).then(result => {
    const { propositions } = result;

    // filter rendered propositions
    const renderedPropositions = propositions.filter(proposition => proposition.renderAttempted === true);

    // collect the item metadata that represents the response tokens
    const collectMetaData = (items) => {
      return items.filter(item => item.meta !== undefined).map(item => item.meta);
    }

    const pageLoadResponseTokens = renderedPropositions
      .map(proposition => collectMetaData(proposition.items))
      .filter(e => e.length > 0)
      .flatMap(e => e);
  });
  
```

啟用自動轉譯時，主張陣列包含：

#### 在頁面載入時：

* 以`propositions`旗標設為`renderAttempted`的表單式撰寫器式`false`
* 以`renderAttempted`旗標設為`true`的視覺化體驗撰寫器型建議
* 單一頁面應用程式檢視的視覺化體驗撰寫器式主張，其中`renderAttempted`旗標設為`true`

#### 檢視上 — 變更（針對快取檢視）：

* 單一頁面應用程式檢視的視覺化體驗撰寫器式主張，其中`renderAttempted`旗標設為`true`

停用自動轉譯時，主張陣列包含：

#### 在頁面載入時：

* 以[!DNL Form-based Composer]為基礎的`propositions`，`renderAttempted`旗標設為`false`
* [!DNL Visual Experience Composer]個以`renderAttempted`旗標設為`false`的主張
* 單一頁面應用程式檢視以[!DNL Visual Experience Composer]為基礎的主張，其中`renderAttempted`旗標設為`false`

#### 檢視上 — 變更（針對快取檢視）：

* 單一頁面應用程式檢視的視覺化體驗撰寫器式主張，其中`renderAttempted`旗標設為`false`

### 單一設定檔更新

[!DNL Web SDK]可讓您將設定檔更新為[!DNL Target]設定檔，並更新為[!DNL Web SDK]做為體驗事件。

若要更新[!DNL Target]設定檔，請確定設定檔資料是以下列方式傳遞：

* 在`"data {"`下
* 在`"__adobe.target"`下
* 前置詞`"profile."`

| 索引鍵 | 類型 | 說明 |
| --- | --- | --- |
| `renderDecisions` | 布林值 | 指示個人化元件是否應解譯DOM動作 |
| `decisionScopes` | 陣列`<String>` | 要擷取決定的範圍清單 |
| `xdm` | 物件 | 在XDM中格式化的資料，登陸到網頁SDK做為體驗事件 |
| `data` | 物件 | 任意索引鍵/值配對會傳送至target類別下的[!DNL Target]個解決方案。 |

<!--Typical [!DNL Web SDK] code using this command looks like the following:-->

**延遲儲存設定檔或實體引數，直到內容顯示給一般使用者**

若要將設定檔中的錄製屬性延遲到內容顯示為止，請在您的要求中設定`data.adobe.target._save=false`。

例如，您的網站包含三個決定範圍，分別對應至網站上的三個類別連結（「男性、女性和兒童」），而您想要追蹤使用者最後造訪的類別。 傳送這些請求，並將`__save`旗標設為`false`，以避免在請求內容時保留類別。 內容視覺化之後，針對要記錄的對應屬性，傳送適當的裝載（包括`eventToken`和`stateToken`）。

以下範例會傳送trackEvent樣式訊息、執行設定檔指令碼、儲存屬性，並立即記錄事件。

```js
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": { /* Experience Event XDM data */ },
    "data": {
        "__adobe": {
            "target": {
                " __save": true|false,
                //defaults to true if omitted
                "profile.gender": "female",
                "profile.age": 30,
                "entity.name": "T-shirt",
                "entity.id": "1234"
            }
        }
    }
})
```

>[!NOTE]
>
>如果省略`__save`指示詞，則會立即儲存設定檔和實體屬性。 `__save`指示詞僅與設定檔屬性和實體詳細資料相關。

## 要求建議

下表列出[!DNL Recommendations]屬性，以及是否透過[!DNL Web SDK]支援每個屬性：

| 類別 | 屬性 | 支援狀態 |
| --- | --- | --- |
| Recommendations — 預設實體屬性 | entity.id | 支援 |
|  | entity.name | 支援 |
|  | entity.categoryId | 支援 |
|  | entity.pageUrl | 支援 |
|  | entity.thumbnailUrl | 支援 |
|  | entity.message | 支援 |
|  | entity.value | 支援 |
|  | entity.inventory | 支援 |
|  | entity.brand | 支援 |
|  | entity.margin | 支援 |
|  | entity.event.detailsOnly | 支援 |
| Recommendations — 自訂實體屬性 | entity.yourCustomAttributeName | 支援 |
| Recommendations — 保留的mbox/頁面引數 | excludedIds | 支援 |
|  | cartIds | 支援 |
|  | productPurchasedId | 支援 |
| 類別相關性的頁面或料號類別 | user.categoryId | 支援 |

**如何傳送Recommendations屬性至[!DNL Target]：**

```js
alloy("sendEvent", {
  "renderDecisions": true,
  "data": {
    "__adobe": {
      "target": {
        "entity.id": "123",
        "entity.genre": "Drama"
      }
    }
  }
});
```

## 顯示mbox轉換量度 {#display-mbox-conversion-metrics}

以下範例顯示如何追蹤顯示mbox轉換並傳送設定檔引數給[!DNL Target]，而不需要符合任何內容或活動的資格。

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [{
                    "scope": "conversion-step-1" //example scope name
                }],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    }
});
```


| 屬性 | 說明 |
|---------|----------|
| `xdm._experience.decisioning.propositions[x].scope` | 與成功量度關聯的範圍（將歸因於Target端的特定活動）。 |
| `xdm._experience.decisioning.propositions[x].eventType` | 說明預期事件型別的字串。 針對此使用案例將此專案設定為`"decisioning.propositionDisplay"`。 |

## 除錯

已棄用mboxTrace和mboxDebug。 請改用[Web SDK偵錯](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/use-cases/debugging)的方法。

## 術語

**主張**：在[!DNL Target]中，主張與從活動選取的體驗相關。

**結構描述**：決定的結構描述是[!DNL Target]中的優惠型別。

**範圍**：決定的範圍。 在[!DNL Target]中，範圍是mbox。 全域mbox是`__view__`範圍。

**XDM**： XDM已序列化為點標籤法，然後作為mbox引數放入[!DNL Target]中。

---
title: Experience Platform Web SDK中A4T資料的使用者端記錄
description: 瞭解如何使用Experience Platform Web SDK為Adobe Analytics for Target (A4T)啟用使用者端記錄。
seo-title: Client-side logging for A4T data in the Experience Platform Web SDK
seo-description: Learn how to enable client-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: target；a4t；記錄；Web SDK；體驗；平台；
feature: Implementation
source-git-commit: 4d4ca7dcffdbaee5770084bd85c3109df0d6a8d4
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# [!DNL Experience Platform Web SDK]中A4T資料的使用者端記錄

[!DNL Adobe Experience Platform Web SDK]可讓您在網頁應用程式的使用者端上收集[Target (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=zh-Hant)的Adobe Analytics資料。

使用者端記錄表示使用者端會傳回相關的[!DNL Target]資料，讓您收集資料並與[!DNL Analytics]共用。 如果您打算使用[資料插入API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html?lang=zh-Hant)手動將資料傳送至Analytics，則應啟用此選項。

>[!NOTE]
>
>使用[AppMeasurement.js](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=zh-Hant)執行此工作的方法目前正在開發中，將於不久的將來提供。

本文介紹為[!DNL Platform Web SDK]設定使用者端A4T記錄的步驟，並提供常見使用案例的實施範例。

## 先決條件 {#prerequisites}

本教學課程假設您熟悉有關使用[!DNL Platform Web SDK]進行個人化目的的基本概念和程式。 如果您需要簡介，請檢閱下列檔案：

* [設定網頁SDK](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/commands/configure/overview)
* [正在傳送事件](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/commands/sendevent/overview)
* [呈現個人化內容](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## 設定[!DNL Analytics]使用者端記錄 {#set-up-client-side-logging}

下列子節概述如何為您的[!DNL Analytics]實作啟用[!DNL Platform Web SDK]使用者端記錄。

### 啟用[!DNL Analytics]使用者端記錄 {#enable-analytics-client-side-logging}

若要考慮為您的實作啟用[!DNL Analytics]使用者端記錄，您必須停用[!DNL Adobe Analytics]資料流[中的](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/datastreams/overview)設定。

![已停用Analytics資料流設定](/help/dev/implement/a4t/assets/disable-analytics-datastream.png)

### 從SDK擷取[!DNL A4T]資料並將其傳送到[!DNL Analytics] {#a4t-to-analytics}

為了讓此報告方法正常運作，您必須傳送從[!DNL A4T]點選中的[`sendEvent`](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/commands/sendevent/overview)命令擷取的[!DNL Analytics]相關資料。

當[!DNL Target] Edge計算主張回應時，它會檢查是否已啟用[!DNL Analytics]使用者端記錄（例如，在您的資料流中是否已停用[!DNL Analytics]）。 如果啟用使用者端記錄，則系統會在回應中為每個主張新增[!DNL Analytics]權杖。

流量看起來類似這樣：

![使用者端記錄流程](/help/dev/implement/a4t/assets/analytics-client-side-logging.png)

下列是啟用`interact`使用者端記錄時[!DNL Analytics]回應的範例。 如果主張是針對具有[!DNL Analytics]報告的活動，它將具有`scopeDetails.characteristics.analyticsToken`屬性。

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "experience": {
              "id": "0"
            },
            "strategies": [
              {
                "algorithmID": "0",
                "trafficType": "0"
              }
            ],
            "characteristics": {
              "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
              "analyticsToken": "434689:0:0|2,434689:0:0|1"
            }
          },
          "items": [
            {
              "id": "1184844",
              "schema": "https://ns.adobe.com/personalization/html-content-item",
              "meta": {
                "geo.state": "bucuresti",
                "activity.id": "434689",
                "experience.id": "0",
                "activity.name": "a4t test form based activity",
                "offer.id": "1184844",
                "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
              },
              "data": {
                "id": "1184844",
                "format": "text/html",
                "content": "<div> analytics impressions </div>"
              }
            }
          ]
        },
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "characteristics": {
              "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
              "analyticsToken": "434689:0:0|32767"
            }
          },
          "items": [
            {
              "id": "434689",
              "schema": "https://ns.adobe.com/personalization/measurement",
              "data": {
                "type": "click",
                "format": "application/vnd.adobe.target.metric"
              }
            }
          ]
        }
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

[!UICONTROL Form-based Experience Composer]活動的主張可以同時包含相同主張下的內容和點按量度專案。 因此，不是在`scopeDetails.characteristics.analyticsToken`屬性中顯示內容的單一分析權杖，這些屬性可以相應地在`scopeDetails.characteristics.analyticsDisplayToken`和`scopeDetails.characteristics.analyticsClickToken`屬性中同時指定顯示和點按分析權杖。

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "experience": {
              "id": "0"
            },
            "strategies": [
              {
                "algorithmID": "0",
                "trafficType": "0"
              }
            ],
            "characteristics": {
               "displayToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
               "clickToken": "E0gb6q1+WyFW3FMbbQJmrg==",
               "analyticsDisplayToken": "434689:0:0|2,434689:0:0|1", 
               "analyticsClickToken": "434689:0:0|32767"
            }
          },
          "items": [
            {
              "id": "1184844",
              "schema": "https://ns.adobe.com/personalization/html-content-item",
              "meta": {
                "geo.state": "bucuresti",
                "activity.id": "434689",
                "experience.id": "0",
                "activity.name": "a4t test form based activity",
                "offer.id": "1184844",
                "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
              },
              "data": {
                "id": "1184844",
                "format": "text/html",
                "content": "<div> analytics impressions </div>"
              }
            },
            {
              "id": "434689",
              "schema": "https://ns.adobe.com/personalization/measurement",
              "data": {
                "type": "click",
                "format": "application/vnd.adobe.target.metric"
              }
            }
          ]
        }
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

來自`scopeDetails.characteristics.analyticsToken`、以及`scopeDetails.characteristics.analyticsDisplayToken` （適用於顯示的內容）和`scopeDetails.characteristics.analyticsClickToken` （適用於點選量度）的所有值都是需要收集的A4T裝載，並包含在`tnta`資料插入API[呼叫中的](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)標籤。

>[!IMPORTANT]
>
>`analyticsToken`、`analyticsDisplayToken`、`analyticsClickToken`屬性可包含多個權杖，以單一逗號分隔的字串串串連。
>
>在下節提供的實作範例中，正在反複收集多個[!DNL Analytics]權杖。 若要串連[!DNL Analytics]權杖的陣列，請使用類似下列的函式：
>
>```javascript
>var concatenateAnalyticsPayloads = function concatenateAnalyticsPayloads(analyticsPayloads) {
>   if (analyticsPayloads.size > 1) {
>       return [].concat(analyticsPayloads).join(',');
>   }
>   return [].concat(analyticsPayloads).join();
>};
>```

## 實施範例 {#implementation-examples}

下列小節示範如何針對常見使用案例實作[!DNL Analytics]使用者端記錄。

### [!UICONTROL Form-Based Experience Composer]個活動 {#form-based-composer}

您可以使用[!DNL Platform Web SDK]控制來自[Adobe Target表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=zh-Hant)活動的建議執行。

當您請求特定決定範圍的建議時，傳回的建議包含其適當的[!DNL Analytics]權杖。 最佳實務是鏈結[!DNL Experience Platform Web SDK] `sendEvent`命令並重複處理傳回的主張，以便在同時收集[!DNL Analytics]權杖時執行這些建議。

您可以為`sendEvent`活動範圍觸發[!UICONTROL Form-Based Experience Composer]命令，如下所示：

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  for (var i = 0; i < results.propositions.length; i++) {
    //Execute the propositions and collect the Analytics payload
  }
});
```

從這裡，您必須實作程式碼以執行主張並建構最終將傳送給[!DNL Analytics]的裝載。 這是`results.propositions`可能包含的範例：

```json
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "experience": {
        "id": "0"
      },
      "strategies": [
        {
          "algorithmID": "0",
          "trafficType": "0"
        }
      ],
      "characteristics": {
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
        "analyticsToken": "434689:0:0|2,434689:0:0|1"
      }
    },
    "items": [
      {
        "id": "1184844",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434689",
          "experience.id": "0",
          "activity.name": "a4t test form based activity",
          "offer.id": "1184844",
          "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
        },
        "data": {
          "id": "1184844",
          "format": "text/html",
          "content": "<div> analytics impressions </div>"
        }
      }
    ]
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
        "analyticsToken": "434689:0:0|32767"
      }
    },
    "items": [
      {
        "id": "434689",
        "schema": "https://ns.adobe.com/personalization/measurement",
        "data": {
          "type": "click",
          "format": "application/vnd.adobe.target.metric"
        }
      }
    ]
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434688"
      },
      "experience": {
        "id": "0"
      },
      "strategies": [
        {
          "algorithmID": "0",
          "trafficType": "0"
        }
      ],
      "characteristics": {
          "displayToken": "91TS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqgEt==",
          "clickToken": "Tagb6q1+WyFW3FMbbQJrtg==",
          "analyticsDisplayTokens": "434688:0:0|2,434688:0:0|1",
          "analyticsClickTokens": "434688:0:0|32767"
        }
      }
    },
    "items": [
      {
        "id": "1184845",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434688",
          "experience.id": "0",
          "activity.name": "a4t test form based activity 1",
          "offer.id": "1184845"
        },
        "data": {
          "id": "1184845",
          "format": "text/html",
          "content": "<div> analytics impressions 1</div>"
        }
      },
      {
        "id": "434688",
        "schema": "https://ns.adobe.com/personalization/measurement",
        "data": {
          "type": "click",
          "format": "application/vnd.adobe.target.metric"
        }
      }
    ]
  }
]
```

若要從包含內容專案的主張中擷取[!DNL Analytics]權杖，您可以實作類似下列的函式：

```javascript
function getDisplayAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsDisplayToken) {
    return characteristics.analyticsDisplayToken;
  }
  return characteristics.analyticsToken;
}
```

主張可以有不同型別的專案，如相關專案的`schema`屬性所指示。 [!UICONTROL Form-Based Experience Composer]活動支援四個主張專案結構描述：

```javascript
var HTML_SCHEMA = "https://ns.adobe.com/personalization/html-content-item";
var MEASUREMENT_SCHEMA = "https://ns.adobe.com/personalization/measurement";
var JSON_SCHEMA = "https://ns.adobe.com/personalization/json-content-item";
var REDIRECT_SCHEMA = "https://ns.adobe.com/personalization/redirect-item";
```

`HTML_SCHEMA`和`JSON_SCHEMA`是反映選件型別的結構描述，而`MEASUREMENT_SCHEMA`則反映應附加至DOM元素的量度。

當訪客實際點按先前顯示的內容時，點選量度的[!DNL Analytics]個裝載應從內容專案個別收集並傳送至[!DNL Analytics]。

在此情況下，以下協助程式函式有助於取得點選量度A4T裝載：

```javascript
function getClickAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsClickToken) {
    return characteristics.analyticsClickToken;
  }
  return characteristics.analyticsToken;
}
```

#### 實作摘要 {#implementation-summary}

總而言之，使用[!UICONTROL Form-Based Experience Composer]套用[!DNL Experience Platform Web SDK]活動時，必須執行下列步驟：

1. 傳送擷取[!UICONTROL Form-Based Experience Composer]活動選件的事件；
1. 將內容變更套用至頁面；
1. 傳送`decisioning.propositionDisplay`通知事件；
1. 從SDK回應中收集[!DNL Analytics]顯示權杖，並建構點選[!DNL Analytics]的裝載；
1. 使用[!DNL Analytics]資料插入API[將裝載傳送至](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)；
1. 若已傳遞的建議中有任何點選量度，則應設定點選接聽程式，以便執行點選時傳送`decisioning.propositionInteract`通知事件。 應設定`onBeforeEventSend`處理常式，以便在攔截`decisioning.propositionInteract`事件時會發生下列動作：
   1. 正在從[!DNL Analytics]收集點選`xdm._experience.decisioning.propositions`權杖
   1. 透過[!DNL Analytics]資料插入API[!DNL Analytics]，以收集的[裝載傳送點選](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)點選；

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  var analyticsPayload = new Set();
  results.propositions.forEach(function (proposition) {
    proposition.items.forEach(function (item) {
      if (item.schema === HTML_SCHEMA) {
        // 1. Apply offer
        // 2. Collect executed propositions and send the decisioning.propositionDisplay notification event
        // 3. Collect the display Analytics tokens
      }
      if (item.schema === MEASUREMENT_SCHEMA) {
        // Setup click listener, so that when clicked:
        // 1. Collect clicked propositions and send the decisioning.propositionInteract notification event
        // Note: onBeforeEventSend handler should be configured, so that when intercepting decisioning.propositionInteract events:
        //   1. Collect the click Analytics tokens from xdm._experience.decisioning.propositions
        //   2. Send the click Analytics hit with the collected Analytics payload via Data Insertion API
      }
    });
  });
  // Send the page view Analytics hit with the collected display Analytics payload via Data Insertion API
});
```

### [!UICONTROL Visual Experience Composer] (VEC)活動 {#visual-experience-composer-acitivties}

[!DNL Platform Web SDK]可讓您處理使用[視覺化體驗撰寫器(VEC)](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=zh-Hant)編寫的選件。

>[!NOTE]
>
>實作此使用案例的步驟與[表單式體驗撰寫器活動](#form-based-composer)的步驟非常類似。 如需詳細資訊，請參閱上一節。

啟用自動轉譯時，您可以從頁面上執行的主張中收集[!DNL Analytics]權杖。 最佳實務是鏈結[!DNL Experience Platform Web SDK] `sendEvent`命令並逐一檢視傳回的主張，以篩選Web SDK嘗試呈現的主張。

**範例**

```javascript
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function (results) {
  var analyticsPayloads = new Set();
  
  for (var i = 0; i < results.propositions.length; i++) {
  
    var proposition = propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getDisplayAnalyticsPayload(proposition);
      
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // Send the page view Analytics hit with collected Analytics payload via Data Insertion API
});
```

### 使用`onBeforeEventSend`處理頁面量度 {#using-onbeforeeventsend}

您可以使用[!DNL Adobe Target]個活動，在頁面上設定不同的量度(手動附加至DOM或自動附加至DOM （VEC編寫的活動）。 這兩種型別都是網頁上延遲的一般使用者互動。

若要解決此問題，最佳實務是使用[!DNL Analytics] `onBeforeEventSend`鉤點收集[!DNL Adobe Experience Platform Web SDK]裝載。 `onBeforeEventSend`連結應使用`configure`命令設定，並反映在透過資料流傳送的所有事件中。

以下是如何設定`onBeforeEventSent`以觸發[!DNL Analytics]點選的範例：

```javascript
alloy("configure", {
  datastreamId: "datastream configuration ID",
  orgId: "adobe ORG ID",
  onBeforeEventSend: function(options) {
    const xdm = options.xdm;
    const eventType = xdm.eventType;
    if (eventType === "decisioning.propositionInteract") {
      const analyticsPayloads = new Set();
      const propositions = xdm._experience.decisioning.propositions;

      for (var i = 0; i < propositions.length; i++) {
        var proposition = propositions[i];
        analyticsPayloads.add(getClickAnalyticsPayload(proposition));
      }
      // Trigger the Analytics hit
    }
  }
});
```

## 下一步 {#next-steps}

本指南涵蓋[!DNL Platform Web SDK]中A4T資料的使用者端記錄。 如需如何在Edge Network上處理A4T資料的詳細資訊，請參閱[伺服器端記錄](/help/dev/implement/a4t/server-side-a4t.md)的指南。
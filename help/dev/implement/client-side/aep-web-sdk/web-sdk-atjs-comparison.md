---
title: 比較at.js與Experience Platform Web SDK
description: 瞭解at.js功能與 [!DNL Experience Platform Web SDK]的比較。
keywords: target；adobe target；activity.id；experience.id；renderDecisions；decisionScopes；預先隱藏程式碼片段；vec；表單式體驗撰寫器；xdm；對象；決定；範圍；結構；系統圖表；圖表
feature: AEP Web SDK
exl-id: 31c9722b-5d92-4653-aa20-4183d166c097
source-git-commit: 158c45b824df8d3bd565ac7c654b65f1fd631e2c
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 5%

---

# 比較at.js程式庫與[!DNL Adobe Experience Platform Web SDK]

## 概觀

本文概述`at.js`資料庫與Experience Platform Web SDK之間的差異。

## 安裝程式庫

### 安裝at.js

[!DNL Adobe]可讓客戶直接從[!DNL Adobe Experience Cloud]，[!UICONTROL Implementation]索引標籤下載程式庫。 at.js程式庫已根據客戶的下列設定加以自訂： clientCode、imsOrgId等。

### 安裝Web SDK

預先建立的版本可在CDN上取得。 您可以直接在頁面上在CDN上參考程式庫，或將其下載並託管在您自己的基礎架構上。 它提供縮制和未縮制的格式。 未縮制的版本對於除錯而言相當實用。

如需詳細資訊，請參閱[使用JavaScript資料庫](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/install/library)安裝Web SDK。

## 設定程式庫

### 設定at.js

在每個at.js檔案的結尾，您會看到[!DNL Adobe]例項化並傳遞設定物件的區段。 此區段可自訂，下載時，Adobe會將該區段填入目前的客戶設定。

```javascript
window.adobe.target.init(window, document, {
  "clientCode": "demo",
  "imsOrgId": "",
  "serverDomain": "localhost:5000",
  "timeout": 2000,
  "globalMboxName": "target-global-mbox",
  "version": "2.0.0",
  "defaultContentHiddenStyle": "visibility: hidden;",
  "defaultContentVisibleStyle": "visibility: visible;",
  "bodyHiddenStyle": "body {opacity: 0 !important}",
  "bodyHidingEnabled": true,
  "deviceIdLifetime": 63244800000,
  "sessionIdLifetime": 1860000,
  "selectorsPollingTimeout": 5000,
  "visitorApiTimeout": 2000,
  "overrideMboxEdgeServer": false,
  "overrideMboxEdgeServerTimeout": 1860000,
  "optoutEnabled": false,
  "optinEnabled": false,
  "secureOnly": false,
  "supplementalDataIdParamTimeout": 30,
  "authoringScriptUrl": "//cdn.tt.omtrdc.net/cdn/target-vec.js",
  "urlSizeLimit": 2048,
  "endpoint": "/rest/v1/delivery",
  "pageLoadEnabled": true,
  "viewsEnabled": true,
  "analyticsLogging": "server_side",
  "serverState": {},
  "decisioningMethod": "server-side",
  "legacyBrowserSupport":  false
});
```

[了解更多](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)

### 設定Platform Web SDK

使用[`configure`](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/commands/configure/overview)命令完成SDK的設定。 `configure`命令是&#x200B;*一律*&#x200B;先呼叫。

## 如何要求並自動轉譯頁面載入[!DNL Target]選件

### 使用at.js

使用at.js 2.x時，如果您啟用設定`pageLoadEnabled,`，程式庫會透過[!DNL Target]觸發對`execute -> pageLoad` Edge的呼叫。 如果所有設定都設定為預設值，則不需要自訂編碼。 將at.js新增至頁面並由瀏覽器載入後，就會執行[!DNL Target] Edge呼叫。

### 使用[!DNL PLatform Web SDK]

在[!DNL Target] [視覺化體驗撰寫器](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/visual-experience-composer)中建立的內容可由SDK自動擷取及轉譯。

若要要求並自動轉譯[!DNL Target]選件，請使用`sendEvent`命令並將`renderDecisions`選項設為`true.`如此會強制SDK自動轉譯任何符合自動轉譯條件的個人化內容。

範例:  

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

[!DNL Experience Platform Web SDK]會自動傳送包含[!DNL Platform WEB SDK]所執行之優惠的通知。 以下範例說明通知要求裝載看起來如何：

```json
{
  "events": [{
      "xdm": {
        "_experience": {
          "decisioning": {
            "propositions": [
              {
                "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
                "scope": "cart",
                "scopeDetails": {
                  "decisionProvider": "TGT",
                  "activity": {
                    "id": "127019"
                  },
                  "experience": {
                    "id": "0"
                  },
                  "strategies": [
                    {
                      "step": "entry",
                      "algorithmID": "0",
                      "trafficType": "0"
                    },
                    {
                      "step": "display",
                      "algorithmID": "0",
                      "trafficType": "0"
                    }
                  ],
                  "characteristics": {
                    "eventToken": "bKMxJ8dCR1XlPfDCx+2vSGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                  }
                }
              }
            ]
          }
        },
        "eventType": "display",
        "web": {
          "webPageDetails": {
            "viewName": "cart",
            "URL": "https://alloyio.com/personalizationSpa/cart"
          },
          "webReferrer": {
            "URL": ""
          }
        },
        "device": {
          "screenHeight": 800,
          "screenWidth": 1280,
          "screenOrientation": "landscape"
        },
        "environment": {
          "type": "browser",
          "browserDetails": {
            "viewportWidth": 1280,
            "viewportHeight": 284
          }
        },
        "placeContext": {
          "localTime": "2021-12-10T15:50:34.467+02:00",
          "localTimezoneOffset": -120
        },
        "timestamp": "2021-12-10T13:50:34.467Z",
        "implementationDetails": {
          "name": "https://ns.adobe.com/experience/alloy",
          "version": "2.6.2",
          "environment": "browser"
        }
      }
    }
  ]
}
```

[了解更多](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## 如何要求和&#x200B;*NOT*&#x200B;自動轉譯頁面載入目標選件

### 使用at.js

有兩種方式可觸發對[!DNL Target]個Edge的呼叫，該呼叫會擷取選件以載入頁面。

範例1：

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox", 
   success: console.log,
   error: console.error
});
```

範例2：

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

[了解更多](https://experienceleague.adobe.com/en/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions)

### 使用[!DNL Platform Web SDK]

在`sendEvent`下執行具有特殊領域的`decisionScopes`命令： `__view__`。 [!DNL Adobe]使用此範圍作為訊號，從[!DNL Target]擷取所有頁面載入活動，並預先擷取所有檢視。 [!DNL Platform Web SDK]也會嘗試評估所有以VEC檢視為基礎的活動。 [!DNL Platform Web SDK]目前不支援停用檢視預先擷取。

若要存取任何個人化內容，您可以提供回呼函式，SDK從伺服器收到成功回應後，就會呼叫此函式。 系統會為您的回呼提供結果物件，其中可能包含包含任何傳回之個人化內容的建議屬性。

範例:  

```javascript
alloy("sendEvent", {
    xdm: {...},
    decisionScopes: ["__view__"]
  }).then(function(result) {
    if (result.propositions) {
      result.propositions.forEach(proposition => {
        proposition.items.forEach(item => {
          if (item.schema === HTML_SCHEMA) {
            // manually apply offer
            document.getElementById("form-based-offer-container").innerHTML =
              item.data.content;
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
          // manually send the display notification event, so that Target/Analytics impressions aare increased
            alloy("sendEvent",{
              "xdm": {
                "eventType": "decisioning.propositionDisplay",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          }
        });
      });
    }
  });
```

[了解更多](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## 如何請求特定的表單式Target mbox

### 使用at.js

您可以使用`getOffer`函式擷取活動：

範例1：

```javascript
adobe.target.getOffer({
   mbox: "hero-banner", 
   success: console.log,
   error: console.error
});
```

範例2：

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        mboxes: [
        {
          index: 0,
          name: "hero-banner"
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

[了解更多](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions.html)

### 使用[!DNL Platform Web SDK]

您可以使用[!UICONTROL Form-Based Composer]命令，並在`sendEvent`選項下傳遞mbox名稱，擷取`decisionScopes`型活動。 `sendEvent`命令會傳回Promise，此承諾會以包含請求之活動/主張的物件來解析：

此程式碼片段是`propositions`陣列的外觀：

```javascript
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
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
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw=="
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
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg=="
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
]
```

範例:  

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items; j++) {
        var item = proposition.items[j];
        if (item.schema === HTML_SCHEMA) {
          // apply offer
          document.getElementById("form-based-offer-container").innerHTML =
            item.data.content;
          const executedPropositions = [
            {
              id: proposition.id,
              scope: proposition.scope,
              scopeDetails: proposition.scopeDetails
            }
          ];

          alloy("sendEvent", {
            "xdm": {
              "eventType": "decisioning.propositionDisplay",
              "_experience": {
                "decisioning": {
                  "propositions": executedPropositions
                }
              }
            }
          });
        }
      }
    }
  }
});
```

[了解更多](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## 如何套用[!DNL Target]活動

### 使用at.js

您可以使用[!DNL Target]函式套用`applyOffers`活動： `adobe.target.applyOffer(options).`

範例:  

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

從`applyOffers`專屬檔案[進一步瞭解](https://experienceleague.adobe.com/en/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2)命令。

### 使用[!DNL Platform Web SDK]

您可以使用[!DNL Target]命令套用`applyPropositions`活動。

範例:  

```javascript
alloy("applyPropositions", {
    propositions: [...]
});
```

從`applyPropositions`專屬檔案[進一步瞭解](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)命令。

## 如何追蹤事件

### 使用at.js

您可以使用`trackEvent`函式或使用`sendNotifications.`來追蹤事件

此函數會觸發要求以報告使用者動作，例如點擊和轉換。此函式不會在回應中傳遞活動。

**範例 1**

```javascript
adobe.target.trackEvent({ 
    "type": "click",
    "mbox": "some-mbox"
});
```

**範例 2**

```javascript
adobe.target.sendNotifications({ 
    request: {
       notifications: [{
          ...,
          mbox: {
            name: "some-mbox"
          },
          type: "click",
          ...
       }]
    }
});
```

[了解更多](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-trackevent.html)

### 使用[!DNL Platform Web SDK]

您可以呼叫`sendEvent`命令、填入`_experience.decisioning.propositions` XDM `fieldgroup`，並將`eventType`設定為下列兩個值之一，以追蹤事件和使用者動作：

* `decisioning.propositionDisplay`：代表[!DNL Target]活動的轉譯。
* `decisioning.propositionInteract`：代表使用者與活動的互動，例如滑鼠點按。

`_experience.decisioning.propositions` XDM `fieldgroup`是物件陣列。 每個物件的屬性衍生自`result.propositions`命令中傳回的`sendEvent`： `{ id, scope, scopeDetails }.`

**範例1 — 呈現活動後追蹤`decisioning.propositionDisplay`事件**

```javascript
alloy("sendEvent", {
  xdm: {},
  decisionScopes: ['discount']
}).then(function(result) {
  var propositions = result.propositions;

  var discountProposition;
  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      if (proposition.scope === "discount") {
        discountProposition = proposition;
        break;
      }
    }
  }

  if (discountProposition) {
    // Find the item from proposition that should be rendered.
    // Rather than assuming there a single item that has HTML
    // content, find the first item whose schema indicates
    // it contains HTML content.
    for (var j = 0; j < discountProposition.items.length; j++) {
      var discountPropositionItem = discountProposition.items[i];
      if (discountPropositionItem.schema === "https://ns.adobe.com/personalization/html-content-item") {
        var discountHtml = discountPropositionItem.data.content;
        // Render the content
        var dailySpecialElement = document.getElementById("daily-special");
        dailySpecialElement.innerHTML = discountHtml;
        
        // For this example, we assume there is only a single place to update in the HTML.
        break;  
      }
    }
    // Send a "decisioning.propositionDisplay" event signaling that the proposition has been rendered.
    alloy("sendEvent", {
      "xdm": {
        "eventType": "decisioning.propositionDisplay",
        "_experience": {
          "decisioning": {
            "propositions": [{
              "id": id,
              "scope": scope,
              "scopeDetails": scopeDetails
            }],
            "propositionEventType": {
              "display": 1
            }
          }
        }
      }
    });
  }
});
```

**範例2 — 發生點選量度後追蹤`decisioning.propositionInteract`事件**

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items.length; j++) {
        var item = proposition.items[j];

        if (item.schema === "https://ns.adobe.com/personalization/measurement") {
          // add metric to the DOM element
          const button = document.getElementById("form-based-click-metric");

          button.addEventListener("click", event => {
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
            // send the click track event
            alloy("sendEvent", {
              "xdm": {
                "eventType": "decisioning.propositionInteract",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          });
        }
      }
    }
  }
});
```

[了解更多](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content#manual)

**範例3 — 追蹤執行動作後引發的事件**

此範例會追蹤在執行特定動作（例如按一下按鈕）後引發的事件。
您可以透過`__adobe.target`資料物件新增任何其他自訂引數。

您也可以新增`commerce` XDM物件。

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [
                    {
                        "scope": "orderConfirm" //example scope name
                    }
                ],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    },
    "commerce": {
        "order": {
            "purchaseID": "a8g784hjq1mnp3",
            "purchaseOrderNumber": "VAU3123",
            "currencyCode": "USD",
            "priceTotal": 999.98
        }
    },
    "data": {
        "__adobe": {
            "target": {
                "pageType": "Order Confirmation",
                "user.categoryId": "Insurance"
            }
        }
    }
})
```

## 如何在單頁應用程式中觸發檢視變更

### 使用at.js

使用`adobe.target.triggerView`函式。 每當新頁面載入或頁面上的元件重新呈現時，就可呼叫此函數。`adobe.target.triggerView()`函式應為單頁應用程式(SPA)實作，以使用[!UICONTROL Visual Experience Composer] (VEC)來建立[!UICONTROL A/B Test]和[!UICONTROL Experience Targeting] (XT)活動。 如果未在網站上實作`adobe.target.triggerView()`，VEC就無法用於SPA。

**範例**

```javascript
adobe.target.triggerView("homeView")
```

[了解更多](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)

### 使用[!DNL Platform Web SDK]

若要觸發或訊號單頁應用程式[!UICONTROL View Change]，請在`web.webPageDetails.viewName`命令的`xdm`選項下設定`sendEvent`屬性。 [!DNL Platform Web SDK]會檢查檢視快取，如果`viewName`中指定的`sendEvent`有選件，則會執行它們並傳送顯示通知事件。

**範例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  xdm:{
    web:{
      webPageDetails:{
        viewName: "homeView"
      }
    }
  }
});
```

[了解更多](/help/dev/implement/client-side/aep-web-sdk/spa-implementation.md)

## 如何善用[!UICONTROL Response Tokens]

從[!DNL Target]傳回的Personalization內容包含[回應Token](https://experienceleague.adobe.com/en/docs/target/using/administer/response-tokens)。 回應Token包括有關活動、選件、體驗、使用者設定檔、地理資訊等的詳細資訊。 這些詳細資料可與協力廠商工具共用或用於偵錯。 回應權杖可在[!DNL Target]使用者介面中設定。

### 使用at.js

使用at.js自訂事件接聽[!DNL Target]回應並讀取回應Token。

**範例**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[了解更多](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)

### 使用[!DNL Platform Web SDK]

>[!IMPORTANT]
>
>確保您使用[!DNL Experience Platform Web SDK] 2.6.0版或更新版本。

回應Token是作為`propositions`的一部分傳回的，在`sendEvent`命令的結果中公開。 每個主張包含`items,`陣列，且每個專案都填入了回應Token （若已在`meta`管理UI中啟用） [!DNL Target]物件。 [了解更多](https://experienceleague.adobe.com/en/docs/target/using/administer/response-tokens)

**範例**

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Format of result.propositions:
      /*
        [
            {
                "id": "",
                "scope": "",
                "items": [
                    {
                        "id": "",
                        "schema": "",
                        "data": {},
                        "meta": { // RESPONSE TOKENS
                            "activity.name": ...,
                            "offer.id": ...,
                            "profile.activeActivities": ...
                        }
                    }
                ],
                "scopeDetails": {}
                "renderAttempted": false
            }
        ]
      */
    }
  });
```

[了解更多](/help/dev/implement/client-side/aep-web-sdk/accessing-response-tokens.md)

## 如何管理忽隱忽現情形

### 使用at.js

使用at.js時，您可以設定`bodyHidingEnabled: true`來管理忽隱忽現的情形，讓at.js妥善處理
在擷取及套用DOM變更之前，預先隱藏個人化容器。

可以覆寫at.js `bodyHiddenStyle.`，預先隱藏包含個人化內容的頁面區段

依預設，`bodyHiddenStyle`會隱藏整個HTML `body.`

兩個設定都可以使用`window.targetGlobalSettings.`覆寫`window.targetGlobalSettings`應該放在載入at.js之前。

### 使用[!DNL Platform Web SDK]

客戶可以使用[!DNL Platform Web SDK]在configure命令中設定其預先隱藏樣式，如下列範例所示：

```javascript
alloy("configure", {
  datastreamId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

載入[!DNL Platform Web SDK]非同步處理時，[!DNL Adobe]建議在插入[!DNL Platform Web SDK]之前，將下列程式碼片段插入頁面中：

```html
<script>
  !function(e,a,n,t){
  if (a) return;
  var i=e.head;if(i){
  var o=e.createElement("style");
  o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
  setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
  (document, document.location.href.indexOf("adobe_authoring_enabled") !== -1, "body { opacity: 0 !important }", 3000);
</script>
```

## 如何處理A4T

### 使用at.js

使用at.js支援兩種A4T記錄：

* Analytics使用者端記錄
* Analytics伺服器端記錄

#### Analytics使用者端記錄

**範例1：使用[!DNL Target]全域設定**

可透過在at.js設定中設定`analyticsLogging: client_side`或覆寫`window.targetglobalSettings`物件來啟用Analytics使用者端記錄。

設定此選項時，傳回的裝載格式如下所示：

```json
{
  "analytics": {
    "payload": {
      "pe": "tnt",
      "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
    }
  }
}
```

接著，裝載可透過[!DNL Analytics]轉送至[!DNL &#x200B; Data Insertion API]。

範例2：在每個`getOffers`函式中進行設定：

```javascript
adobe.target.getOffers({
      request: {
        experienceCloud: {
          analytics: {
            logging: "client_side"
          }
        },
        prefetch: {
          mboxes: [{
            index: 0,
            name: "a1-serverside-xt"
          }]
        }
      }
    })
    .then(console.log)
```

此程式碼片段是回應裝載的外觀：

```json
{
  "prefetch": {
    "mboxes": [{
      "index": 0,
      "name": "a1-serverside-xt",
      "options": [{
        "content": "<img src=\"http://s7d2.scene7.com/is/image/TargetAdobeTargetMobile/L4242-xt-usa?tm=1490025518668&fit=constrain&hei=491&wid=980&fmt=png-alpha\"/>",
        "type": "html",
        "eventToken": "n/K05qdH0MxsiyH4gX05/2qipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
        "responseTokens": {
          "profile.memberlevel": "0",
          "geo.city": "bucharest",
          "activity.id": "167169",
          "experience.name": "USA Experience",
          "geo.country": "romania"
        }
      }],
      "analytics": {
        "payload": {
          "pe": "tnt",
          "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
        }
      }
    }]
  }
}
```

[!DNL Analytics]承載（`tnta`權杖）應包含在使用[!DNL Analytics]資料插入API[的](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)點選中。

#### [!DNL Analytics]伺服器端記錄

可透過在at.js設定中設定[!DNL Analytics]或覆寫`analyticsLogging: server_side`物件來啟用`window.targetglobalSettings`伺服器端記錄。

然後資料會以下列方式流動：

![顯示Analytics伺服器端記錄工作流程的圖表](/help/dev/implement/client-side/aep-web-sdk/assets/a4t-server-side-atjs.png)

[進一步瞭解](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html)

### 使用[!DNL Platform Web SDK]

網頁SDK也支援：

* Analytics使用者端記錄
* Analytics伺服器端記錄

#### [!DNL Analytics]使用者端記錄

當該DataStream組態的[!DNL Analytics]停用時，[!DNL Adobe Analytics]使用者端記錄已啟用。

![顯示Analytics使用者端記錄工作流程的圖表](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-disabled-datastream-config.png)

客戶可以存取需要使用[!DNL Analytics]資料插入API`tnta`與[!DNL Analytics]共用的[權杖(](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md))，方法是鏈結`sendEvent`命令並逐一檢視產生的主張陣列。

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
    var proposition = results.propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getAnalyticsPayload(proposition);
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // send the page view Analytics hit with collected Analytics payload using Data Insertion API
});
```

以下圖表顯示啟用[!DNL Analytics]使用者端時資料如何流動：

![Analytics使用者端記錄中的資料流程圖](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-client-side-logging.png)

#### [!DNL Analytics]伺服器端記錄

為該DataStream組態啟用[!DNL Analytics]時，[!DNL Analytics]伺服器端記錄已啟用。

![顯示Analytics設定的Datastreams UI。](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-enabled-datastream-config.png)

啟用伺服器端[!DNL Analytics]記錄時，A4T裝載需要與[!DNL Analytics]共用，以便[!DNL Analytics]報告顯示正確的曝光次數和轉換，並在Edge Network層級共用，讓客戶不需要執行任何其他處理。

以下是啟用伺服器端Analytics記錄時，資料如何流入系統：

![顯示伺服器端Analytics記錄中的資料流的圖表](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-server-side-logging.png)

## 如何設定[!DNL Target]全域設定

### 使用at.js

您可以使用`window.targetGlobalSettings,`覆寫at.js資料庫中的設定，而非在[!DNL Target] UI中或使用REST API進行設定。

覆寫應在載入at.js之前或在「管理>實作>編輯at.js設定>程式碼設定>資料庫標題」中定義。

範例:  

```javascript
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

[了解更多](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html)

### 使用[!DNL Platform Web SDK]

網頁SDK不支援此功能。

## 如何更新[!DNL Target]設定檔屬性

### 使用at.js

**範例 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "profile.name": "test",
     "profile.gender": "female"
   },
   success: console.log,
   error: console.error
});
```

**範例 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          profileParameters: {
            name: "test",
            gender: "female"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

### 使用[!DNL Platform Web SDK]

若要更新[!DNL Target]設定檔，請使用`sendEvent`命令並設定`data.__adobe.target`屬性，使用`profile.`為金鑰名稱加上前置詞

**範例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "profile.gender": "female",
        "profile.age": 30
      }
    }
  }
});
```

## 如何使用[!DNL Target Recommendations]

### 使用at.js

**範例 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "entity.name": "T-shirt",
     "entity.id": "1234"
   },
   success: console.log,
   error: console.error
});
```

**範例 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          parameters: {
            "entity.name": "T-shirt",
            "entity.id": "1234"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

[了解更多](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-getoffers-atjs-2.html)

### 使用[!DNL Platform Web SDK]

若要傳送[!DNL Recommendations]資料，請使用`sendEvent`命令並設定`data.__adobe.target`屬性，使用`entity.`為索引鍵名稱加上前置詞

**範例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "entity.name": "T-shirt",
        "entity.id": "1234"
      }
    }
  }
});
```

## 如何使用第三方ID

### 使用at.js

使用at.js有多種傳送`mbox3rdPartyId`的方式，可使用`getOffer,`或`getOffers`：

**範例 1**

```javascript
adobe.target.getOffer({
  mbox:"test",
  params:{
    "mbox3rdPartyId": "1234"
  },
  success: console.log,
  error: console.error
});
```

**範例 2**

```javascript
adobe.target.getOffers({
    request: {
      id:{
        thirdPartyId: "1234"
      },
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

或者，有方法可在`mbox3rdPartyId`或`targetPageParams`中設定`targetPageParamsAll.`

設定`targetPageParams`時，它會傳送`target-global-mbox` （也稱為`pag-lLoad`）的要求。

建議將使用`targetPageParamsAll`設定，因為它將在每[!DNL Target]個要求中傳送。 使用`targetPageParamsAll`的優點是您可以在頁面上定義一次`mbox3rdPartyId`，以確保所有[!DNL Target]請求都有正確的`mbox3rdPartyId.`

```javascript
window.targetPageParamsAll = function() {
      return {
        "mbox3rdPartyId": "1234"
      };
    };
```

```javascript
window.targetPageParams = function() {
  return {
    "mbox3rdPartyId": "1234"
  };
};
```

[了解更多](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetpageparams.html)

### 使用[!DNL Platform Web SDK]

[!DNL Platform Web SDK]支援[!DNL Target]第三方識別碼。 不過，還需要執行幾個步驟。

身分對應可讓客戶傳送多個身分。 所有身分識別都已設定名稱空間。 每個名稱空間可以有一或多個身分。 特定身分可以標示為主要身分。 有了這些知識，您可以瞭解設定[!DNL Platform Web SDK]使用[!DNL Target]協力廠商ID的必要步驟。

1. 在資料流設定頁面中設定包含[!DNL Target]協力廠商ID的名稱空間：

![顯示Target第三方ID名稱空間欄位的Datastreams UI](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

1. 在每個`sendEvent`命令中傳送該身分名稱空間，如下所示：

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "identityMap": {
      "TGT3PID": [
        {
          "id": "1234",
          "primary": true
        }
      ]
    }
  }
});
```

## 如何設定屬性代號

### 使用at.js

使用at.js有兩種設定屬性權杖的方法，使用`targetPageParams`或`targetPageParamsAll.`使用`targetPageParams`會將屬性權杖新增至`target-global-mbox`呼叫，但使用`targetPageParamsAll`會將權杖新增至所有[!DNL Target]呼叫：

**範例 1**

```javascript
   window.targetPageParamsAll = function() {
      return {
        "at_property": "1234"
      };
    };
```

**範例 2**

```javascript
window.targetPageParams = function() {
      return {
        "at_property": "1234"
      };
    };
```

### 使用[!DNL Platform Web SDK]

使用[!DNL Platform Web SDK]的客戶在設定[!DNL Adobe Target]名稱空間下的資料流設定時，能夠在較高層級設定屬性：

![顯示Adobe Target設定的Datastreams UI。](/help/dev/implement/client-side/aep-web-sdk/assets/at-property-setup.png)

這表示該特定資料流組態的每個[!DNL Target]呼叫都包含該屬性Token。

## 如何預先擷取mbox

### 使用at.js

此功能僅適用於at.js 2.x。 at.js 2.x有一個名為`getOffers`的新函式。 `getOffers`函式可讓客戶預先擷取一或多個mbox的內容。 其範例如下:

```javascript
adobe.target.getOffers({
    request: {
      prefetch: {
        mboxes: [{
          index: 0,
          name: "test-mbox",
          parameters: {
            ...
          },
          profileParameters: {
            ...
          }
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!NOTE]
>
>Adobe建議確保`mbox`陣列中的每個`mboxes`都有自己的索引。 通常第一個mbox有`index=0`個下一個`index=1,`，依此類推。

### 使用[!DNL Platform Web SDK]

[!DNL Platform Web SDK]目前不支援此功能。

## 如何偵錯我的[!DNL Target]實作

### 使用at.js

at.js程式庫會公開這些偵錯功能：

* Mbox停用 — 停用[!DNL Target]的擷取和演算功能，以檢查頁面是否損毀，且沒有[!DNL Target]個互動
* Mbox除錯 — at.js會記錄每個動作
* 目標追蹤 — 追蹤物件中產生mbox追蹤權杖，其中包含參與決策程式的詳細資料，可在`window.___target_trace`物件下使用。

>[!NOTE]
>
>所有這些偵錯功能都可以與[Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)的增強功能搭配使用。

### 使用[!DNL Platform Web SDK]

使用[!DNL Platform Web SDK]時，您有多重偵錯功能：

* 使用[Assurance](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/assurance/home)
* [已啟用網頁SDK偵錯](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/assurance/home)
* 使用[Web SDK監視鉤點](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)
* 使用[Adobe Experience Platform Debugger](https://experienceleague.adobe.com/en/docs/experience-platform/debugger/home)
* 目標追蹤

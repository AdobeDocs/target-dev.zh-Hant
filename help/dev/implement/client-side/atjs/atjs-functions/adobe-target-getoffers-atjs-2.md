---
keywords: adobe.target.getOffers， getOffers， getoffers， get offers， at.js，函式，函式， $8
description: 使用 [!UICONTROL adobe.target.getOffers()] 函式及其選項 [!DNL Adobe Target] at.js程式庫引發要求以取得多個 [!DNL Target] 選件。 (at.js 2.x)
title: 如何使用 [!UICONTROL adobe.target.getOffers()] 功能？
feature: at.js
exl-id: b96a3018-93eb-49e7-9aed-b27bd9ae073a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 64%

---

# [!UICONTROL adobe.target.getOffers()] - at.js 2.x

此函數可讓您透過傳入多個 mbox 來擷取多個選件。此外，還可針對使用中活動內的所有檢視擷取多個選件。

>[!NOTE]
>
>此函數於 at.js 2.x 推出。此函數不適用於 at.js 版本 1。*x* 版本不支援此函數。

| 機碼 | 類型 | 必要? | 說明 |
| --- | --- | --- | --- |
| `consumerId` | 字串 | 無 | 如果未提供，預設值為用戶端的全域 mbox。此機碼可用來產生用於A4T整合的增補資料ID (SDID)。<P>使用時 `getOffers()`，每個呼叫都會產生新的SDID。 如果同一頁面有多個mbox要求，且您想要保留SDID (以便符合target-global-mbox中的SDID和 [!DNL Adobe Analytics] SDID)，使用 `consumerId` 引數。<P>如果 `getOffers()` 包含三個mbox （名為「mbox1」、「mbox2」和「mbox3」），包括： `consumerId: "mbox1, mbox2, mbox3"` 在 `getOffers()` 呼叫。 |
| `decisioningMethod` | 字串 | 無 | &quot;server-side&quot;、&quot;on-device&quot;、&quot;hybrid&quot; |
| `request` | 物件 | 是 | 請參閱下方的「要求」表格。 |
| `timeout` | 數字 | 無 | 請求逾時。如果未指定，則會使用預設的 at.js 逾時。 |

## 請求

>[!NOTE]
>
>請參閱 [Delivery API檔案](/help/dev/implement/delivery-api/overview.md) 以取得下列所有欄位之可接受型別的相關資訊。

| 欄位名稱 | 必要? | 限制 | 說明 |
| --- | --- | --- | --- |
| request > id | 無 |  | `tntId`、`thirdPartyId` 或 `marketingCloudVisitorId` 其中一個是必要項目。 |
| request > id > thirdPartyId | 無 | 大小上限 = 128。 |  |  |
| Request > experienceCloud | 無 |  |  |
| Request > experienceCloud > analytics | 無 |  | Adobe Analytics 整合 |
| Request > experienceCloud > analytics > logging | 無 | 必須在頁面上實作下列項目:<ul><li>訪客 ID 服務</li><li>Appmeasurement.js</li></ul> | 支援下列值：<P>**client_side**：指定後，會傳回分析裝載給呼叫者，呼叫者應將其用來傳送至 [!UICONTROL Adobe Analytics] 透過 [!UICONTROL 資料插入API].<P>**server_side**[!DNL Target][!DNL Analytics]: 這是預設值，其中 和 後端會使用 SDID 將多個呼叫拼接在一起以用於報表用途。 |
| request > prefetch | 無 |  |  |
| request > prefetch > views | 無 | 最大計數50。<P>名稱不得空白。<P>名稱長度 `<=` 128.<P>值長度 `<=` 5000。<P>名稱不得以「profile」開頭。<P>不允許的名稱： 「orderId」、「orderTotal」、「productPurchasedId」。 | 傳遞參數以用於擷取使用中活動內的相關檢視。 |
| request > prefetch > views > profileParameters | 無 | 計數上限50。<P>名稱不得空白。<P>名稱長度 `<=` 128.<P>值長度 `<=` 5000。<P>僅接受字串值。<P>名稱不得以「profile」開頭。 | 傳入設定檔參數以用於擷取使用中活動內的相關檢視。 |
| request > prefetch > views > product | 無 |  |  |
| request > prefetch > views > product -> id | 無 | 不得空白。<P>大小上限= 128。 | 傳入產品 ID 以用於擷取使用中活動內的相關檢視。 |
| request > prefetch > views > product > categoryId | 無 | 不得空白。<P>大小上限= 128。 | 傳入產品類別 ID 以用於擷取活動內的相關檢視。 |
| request > prefetch > views > order | 無 |  |  |
| request > prefetch > views > order > id | 無 | 長度上限 = 250。 | 傳入訂單 ID 以用於擷取使用中活動內的相關檢視。 |
| request > prefetch > views > order > total | 無 | 總金額 `>=` 0。 | 傳入訂單總金額以用於擷取使用中活動內的相關檢視。 |
| request > prefetch > views > order > purchasedProductIds | 無 | 沒有空白值。<P>每個值的長度上限為50。<P>串連並以逗號分隔。<P>產品ID總長度 `<=` 250。 | 傳入已購產品 ID 以用於擷取使用中活動內的相關檢視。 |
| request > execute | 無 |  |  |
| request > execute > pageLoad | 無 |  |  |
| request > execute > pageLoad > parameters | 無 | 最大計數50。<P>名稱不得空白。<P>名稱長度 `<=` 128.<P>值長度 `<=` 5000。<P>僅接受字串值。<P>名稱不得以「profile」開頭。<P>不允許的名稱： 「orderId」、「orderTotal」、「productPurchasedId」。 | 頁面載入時使用指定的參數擷取選件。 |
| request > execute > pageLoad > profileParameters | 無 | 最大計數50。<P>名稱不得空白。<P>名稱長度 `<=` 128.<P>值長度 `<=`256.<P>名稱不得以「profile」開頭。<P>僅接受字串值。 | 頁面載入時使用指定的設定檔參數擷取選件。 |
| request > execute > pageLoad > product | 無 |  |  |
| request > execute > pageLoad > product -> id | 無 | 不得空白。<P>大小上限 = 128. | 頁面載入時使用指定的產品 ID 擷取選件。 |
| request > execute > pageLoad > product > categoryId | 無 | 不得空白。<P>大小上限 = 128. | 頁面載入時使用指定的類別 ID 擷取選件。 |
| request > execute > pageLoad > order | 無 |  |  |
| request > execute > pageLoad > order > id | 無 | 長度上限 = 250。 | 頁面載入時使用指定的訂單 ID 擷取選件。 |
| request > execute > pageLoad > order > total | 無 | `>=` 0。 | 頁面載入時使用指定的訂單總金額擷取選件。 |
| request > execute > pageLoad > order > purchasedProductIds | 無 | 沒有空白值。<P>每個值的長度上限為50。<P>串連並以逗號分隔。<P>產品ID總長度 `<=` 250。 | 頁面載入時使用指定的已購產品 ID 擷取選件。 |
| request > execute > mboxes | 無 | 大小上限 = 50.<P>沒有null元素。 |  |
| request > execute > mboxes>mbox | 是 | 不得空白。<P>沒有&#39;-clicked&#39;尾碼。<P>大小上限 = 250.<P>允許的字元： `'-, ._\/=:;&!@#$%^&*()_+|?~[]{}'` | mbox 的名稱。 |
| request > execute > mboxes>mbox>index | 是 | 不是Null。<P>唯一.<P>`>=` 0. | 請注意，索引不代表處理 mbox 的順序。與含有數個區域 mbox 的網頁相同，無法指定處理 mbox 的順序。 |
| request > execute > mboxes > mbox > parameters | 無 | 最大計數= 50。<P>名稱不得空白。<P>名稱長度 `<=` 128.<P>僅接受字串值。<P>值長度 `<=` 5000。<P>名稱不得以「設定檔」開頭。<P>不允許的名稱： 「orderId」、「orderTotal」、「productPurchasedId」。 | 使用指定的參數為特定 mbox 擷取選件。 |
| request > execute > mboxes>mbox>profileParameters | 無 | 最大計數= 50。<P>名稱不得空白。<P>名稱長度 `<=` 128.<P>僅接受字串值。<P>值長度 `<=`256.<P>名稱不得以「設定檔」開頭。 | 使用指定的設定檔參數為特定 mbox 擷取選件。 |
| request > execute > mboxes>mbox > product | 無 |  |  |
| request > execute > mboxes > mbox > product > id | 無 | 不得空白。<P>大小上限 = 128. | 使用指定的產品 ID 為特定 mbox 擷取選件。 |
| request > execute > mboxes > mbox > product > categoryId | 無 | 不得空白。<P>大小上限 = 128. | 使用指定的類別 ID 為特定 mbox 擷取選件。 |
| request > execute > mboxes > mbox > order | 無 |  |  |
| request > execute > mboxes>mbox > order > id | 無 | 長度上限 = 250。 | 使用指定的訂單 ID 為特定 mbox 擷取選件。 |
| request > execute > mboxes > mbox > order > total | 無 | `>=` 0。 | 使用指定的訂單總金額為特定 mbox 擷取選件。 |
| request > execute > mboxes > mbox > order > purchasedProductIds | 無 | 沒有空白值。<P>每個值的長度上限= 50。<P>串連並以逗號分隔。<P>產品ID總長度 `<=` 250。 | 使用指定的已購產品 ID 為特定 mbox 擷取選件。 |

## 呼叫 [!UICONTROL getOffers()] 適用於所有檢視

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
      prefetch: {
        views: [{}]
    }
  }
});
```

## 呼叫 [!UICONTROL getOffers()] 進行裝置上決策

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
}); 
```

## 呼叫 [!UICONTROL getOffers()] 以使用傳入的引數和設定檔引數擷取最新的檢視

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    "prefetch": {
      "views": [
        {
          "parameters": {
            "ad": "1"
          },
          "profileParameters": {
            "age": 23
          }
        }
      ]
    }
  }
});
```

## 呼叫 [!UICONTROL getOffers()] 以使用傳入的引數和設定檔引數擷取mbox。

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    execute: {
      mboxes: [
        {
          index: 0,
          name: "first-mbox"
        },
        {
          index: 1,
          name: "second-mbox",
          parameters: {
            a: 1
          },
          profileParameters: {
            b: 2
          }
        }
      ]
    }
  }
});
```

## 呼叫 [!UICONTROL getOffers()] 從使用者端擷取分析裝載

```javascript {line-numbers="true"}
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

**回應**:

```javascript {line-numbers="true"}
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

接著，裝載可以轉送至 [!DNL Adobe Analytics] 透過 [資料插入API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

## 透過從多個mbox擷取及呈現資料 [!UICONTROL getOffers()] 和 [!UICONTROL applyOffers()]

at.js 2.x 可讓您透過 `[!UICONTROL getOffers()]` API 擷取多個 mbox。您也可以擷取多個 mbox 的資料，然後使用 `[!UICONTROL applyOffers()]` 在 CSS 選取器所識別的不同位置中呈現資料。

下列範例顯示已實作 at.js 2.x 的單一 HTML 頁面:

```html {line-numbers="true"}
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>at.js 2.x, multiple selectors and multiple mboxes</title>
  <script src="at.js"></script>
</head>
<body>
  <div id="container1">Default content 1</div>
  
  <div id="container2">Default content 2</div>

  <div id="container3">Default content 3</div>
</body>
</html>
```

假設您有三個要透過從 [!DNL Target] 收到的內容修改的容器。您可為三個 mbox 建構單一要求，其中每個 mbox 都有要呈現於個別容器中的一些內容。

要求和呈現的程式碼可能如下列範例所示:

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    prefetch: {
      mboxes: [
        {
          index: 0,
          name: "mbox1"
        },
        {
          index: 1,
          name: "mbox2"
        },
        {
          index: 2,
          name: "mbox3"
        }
      ]
    }
  }
})
.then(response => {
  // get all mboxes from response
  const mboxes = response.prefetch.mboxes;
  let count = 1;

  mboxes.forEach(el => {
    adobe.target.applyOffers({
      selector: "#container" + count,
      response: {
        prefetch: {
          mboxes: [el]
        }
      }
    });

    count += 1;
  });
});
```

在 `request > prefetch > mboxes` 區段中，有三個不同的 mbox。如果已成功完成要求，您會從 `response > prefetch > mboxes` 收到每個 mbox 的回應。有了回應和您要用於呈現的位置之後，您可以叫用 `applyOffers()`，呈現從 [!DNL Target] 擷取的內容。在此範例中，我們有下列對應:

* mbox1 > CSS 選取器 #container1
* mbox2 > CSS 選取器 #container2
* mbox3 > CSS 選取器 #container3

此範例使用計數變數來建構 CSS 選取器。在實際情況中，您可以在 CSS 選取器和 mbox 之間使用不同的對應。

請注意，此範例使用 `prefetch > mboxes`，但您也可以使用 `execute > mboxes`。請務必確認，如果您在 `getOffers()` 中使用預先擷取，則在 `applyOffers()` 叫用中也應使用預先擷取。

## 呼叫 [!UICONTROL getOffers()] 執行頁面載入

下列範例說明如何使用執行pageLoad [!UICONTROL getOffers()] 使用at.js 2.*x*

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
        execute: {
            pageLoad: {
                parameters: {}
            }
        }
    }
});
```

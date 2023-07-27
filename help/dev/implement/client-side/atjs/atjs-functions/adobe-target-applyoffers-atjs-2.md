---
keywords: adobe.target.applyOffers， applyOffers， applyoffers，套用選件， at.js，函式，函式，
description: 使用 [!UICONTROL adobe.target.applyOffers()] 的函式 [!DNL Adobe Target] at.js JavaScript程式庫以在回應中套用多個選件。 (at.js 2.x)
title: 如何使用 [!UICONTROL adobe.target.applyOffers()] 功能？
feature: at.js
exl-id: c391e3f4-fdf1-4e33-8dcb-6bf46e390538
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 85%

---

# [!UICONTROL adobe.target.applyOffers(options)] - at.js 2.x

此函數可讓您套用一個以上由 `adobe.target.getOffers()` 擷取的選件。

>[!NOTE]
>
>此函式於at.js 2.*x*.此函式不適用於at.js版本1。*x* 版本不支援此函數。

| 機碼 | 類型 | 必要? | 說明 |
| --- | --- | --- | --- |
| selector | 字串 | 無 | HTML 元素或 CSS 選取器過去會識別 [!DNL Target] 應該放置選件內容的 HTML 元素。如果未提供選取器， [!DNL Target] 假設要使用的HTML元素為HTMLHEAD。 |
| 回應 | 物件 | 是 | 回應來自 `getOffers()` 的物件。<br />請參閱下方的「要求」表格。 |

## 回應

>[!NOTE]
>
>請參閱 [Delivery API檔案](/help/dev/implement/delivery-api/overview.md) 以取得下列所有欄位之可接受型別的相關資訊。

| 欄位名稱 | 說明 |
| --- | --- |
| response > prefetch > views > options > content | 請注意，「選項」的內容並無明確定義，且直接取決於選項類型/範本結構。 |
| response > prefetch > views > options > type | 選項類型。反映「內容」欄位的類型。支援的類型為動作。 |
| response > prefetch > views > state | 不透明的檢視狀態 Token，該 Token 應隨檢視的顯示通知轉送 |
| response > prefetch > views > options > responseTokens | 包含處理目前選項時已收集的 `responseTokens` 地圖。 |
| response > prefetch > views > analytics > payload | [!DNL Analytics]用於用戶端整合的 裝載，套用檢視後應該傳送至 [!DNL Analytics] |
| response > prefetch > views > trace | 包含所有追蹤資料的物件，這些資料為各檢視的預先擷取呼叫之追蹤資料。<br />追蹤物件也包含追蹤的版本。<br />追蹤物件也包含目前檢視的詳細資訊。 |
| response > prefetch > views > options > eventToken | 系統會為每個選項執行事件記錄。應針對每個已套用的選項，將個別事件 Token 新增至通知 Token 清單中。請注意，一個檢視是由多個選項組成。如果所有選項均已套用並顯示，則需要將所有 `eventTokens` 納入通知中。 |
| response > prefetch > views > name | 人類可讀的檢視名稱。 |
| response > prefetch > views > metrics | 應監看報告量度並向 [!DNL Target] 通知。目前僅支援點擊量度。當有使用者點擊元素，就應收集適當的 `eventTokens` 並傳送通知。 |
| response > prefetch > views > key | 用於識別檢視的機碼或指紋。 |
| response > prefetch > views > id | 檢視的 ID。 |
| response > notifications > id | 通知 ID。 |
| response > notifications > events > type | 通知、點擊或顯示的類型。 |
| response > notifications > events > trace | 通知事件的追蹤。 |
| response > notifications > events > token | 隨通知事件傳送的 Token。 |
| response > notifications > events > timestamp | 隨通知事件傳送的時間戳記。 |
| response > notifications > events > errorCode | 如果通知失敗，錯誤碼會指出失敗的原因。 |
| response > notifications > events | 已記錄或無法記錄的目前通知事件。 |
| response > notifications | 指出已記錄或失敗的通知。 |
| response > execute > mboxes > mbox > trace | 包含個別 mbox 要求之所有追蹤資料的物件。 |
| response > execute > mboxes > mbox > responseTokens | 包含特定 mbox 要求執行的 `responseTokens` 地圖。 |
| response > execute > mboxes > mbox > option > content | 請注意，「選項」的內容並無明確定義，且直接取決於選項類型/範本結構。 |
| response > execute > mboxes > mbox > option > type | 選項類型。反映「內容」欄位的類型。支援的類型為: html、重新導向 、JSON 和動態。 |
| response > execute > mboxes > mbox > options | 回應選項。 |
| response > execute > mboxes > mbox > metrics > eventToken | 點擊事件的 Token。 |
| response > execute > mboxes > mbox > metrics > type | &quot;click&quot; |
| response > execute > mboxes > mbox > metrics | 包含 `clickThrough` 量度清單。 |
| response > execute > mboxes > mbox > mbox | mbox 的名稱。 |
| response > execute > mboxes > mbox >index | 指出回應是針對要求中帶有此索引的 mbox。 |
| response > execute > mboxes > mbox > analytics > payload | [!DNL Analytics]用於用戶端整合的 裝載，套用 mbox 後應該傳送至 [!DNL Analytics](請參閱「啟用 A4T 的促銷活動」一節)。 |
| response > execute > mboxes | 已執行 mbox 的清單。 |
| response > execute > pageLoad > options > content | 請注意，「選項」的內容並無明確定義，且直接取決於選項類型/範本結構。 |
| response > execute > pageLoad > options > type | 選項類型。反映「內容」欄位的類型。支援的類型為: html、重新導向、JSON、動態和動作。 |
| response > execute > pageLoad > options | 未依檢視分組的選項 (target-global-mbox + 含有未依檢視分組的檢視之活動選項)。 |
| response > execute > pageLoad > metrics | 未設定為屬於特定檢視的點擊量度。 |
| response > execute > pageLoad > trace | 包含 PageLoad 要求之所有追蹤資料的物件。 |
| response > execute > pageLoad > analytics > payload | [!DNL Analytics]用於用戶端整合的 裝載，套用頁面載入內容後應該傳送至 [!DNL Analytics](請參閱「啟用 A4T 的促銷活動」一節)。 |

## 範例 [!UICONTROL applyOffers()] 呼叫

```javascript {line-numbers="true"}
adobe.target.applyOffers({response:{
  "execute": {
    "pageLoad": {
      "options": [{
        "type": "html",
        "content": "page-load"
      },
      {
        "type": "actions",
        "content": [{
          "type": "setHtml",
          "content": "<h1>Container 1</h1>",
          "selector": "#container1",
          "cssSelector": "#container1"
        },
        {
          "type": "setHtml",
          "content": "<h3>Container 3</h3>",
          "selector": "#container3",
          "cssSelector": "#container3"
        }]
      }],
 
 
      "metrics": [{
        "type": "click",
        "selector": "#container1",
        "eventToken": "page-load-click-metric" 
      }]
    }
  }
}});
```

## 與鏈結的Promise呼叫範例 `[!UICONTROL getOffers()]` 和 `[!UICONTROL applyOffers()]`，因為這些函式是以Promise為基底

```javascript {line-numbers="true"}
adobe.target.getOffers({...})
.then(response => adobe.target.applyOffers({ response: response }))
.then(() => console.log("Success"))
.catch(error => console.log("Error", error));
```

如需如何使用getOffers()的更多範例，請參閱getOffers [檔案](adobe-target-getoffers-atjs-2.md)

### 頁面載入請求範例


```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
        execute: {
            pageLoad: {}
        }
    }
}).
then(response => adobe.target.applyOffers({ response: response }))
.then(() => console.log("Success"))
.catch(error => console.log("Error", error));
```

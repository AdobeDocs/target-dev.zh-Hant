---
keywords: adobe.target.triggerView， triggerView， triggerview， trigger view， at.js，函式，函式， viewName， viewname，檢視名稱， adobe.target.triggerView1
description: 將adobe.target.triggerView()函式用於 [!DNL Adobe Target] at.js JavaScript程式庫，用於單頁應用程式(SPA)。 (at.js 2.x)
title: 如何使用adobe.target.triggerView()函式？
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 37%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

每當新頁面載入或頁面上的元件重新呈現時，就可呼叫此函數。`adobe.target.triggerView()` 應為單頁應用程式(SPA)實作，以使用 [!UICONTROL 視覺化體驗撰寫器] (VEC)以建立 [!UICONTROL A/B測試] 和 [!UICONTROL 體驗鎖定] (XT)活動。 如果 `[!UICONTROL adobe.target.triggerView()]` 未在網站上實作，VEC無法用於SPA。 如需詳細資訊，請參閱[實作單頁應用程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。

>[!NOTE]
>
>此函式於at.js 2.*x*.此函式不適用於at.js版本1。*x* 版本不支援此函數。

| 參數 | 類型 | 必要? | 說明 |
| --- | --- | --- | --- |
| viewName | 字串 | 是 | 傳入任何名稱作為要代表檢視的字串類型。此檢視名稱會出現在 [!UICONTROL 修改] 供行銷人員建立動作並執行其動作的VEC面板 [!UICONTROL A/B測試] 和 [!UICONTROL 體驗鎖定] XT活動。 |
| options | 物件 | 無 |  |
| options > page | 布林值 | 無 | **TRUE:** page 的預設值為 true。當 page=true，會傳送通知至 [!DNL Target] 後端以增加曝光計數。<P>依預設，當 `[!UICONTROL triggerView]` 呼叫，但options > page設為false時除外。<P>**FALSE:** 當 page=false，不會傳送通知以增加曝光計數。只有當您想重新呈現頁面上具有選件的元件時，才應該使用此方法。<P>**注意**：VEC中的自訂程式碼選件不會在以下情況下重新呈現： `[!UICONTROL triggerView()]` 呼叫方式 `{page: false}` 作為選項。 |

## 範例: True

將通知傳送至 後端以增加活動曝光次數和其他量度的 `[!UICONTROL triggerView()]`[!DNL Target] 呼叫。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## 範例: False

不將通知傳送至 後端以增加曝光計數的 `[!UICONTROL triggerView()]`[!DNL Target] 呼叫。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## 範例：Promise鏈結為 `getoffers()` 和 `applyOffers()`

要執行 `triggerView()` 當 `getOffers()` promise已解決，請務必執行 `triggerView()` 在最後一個區塊上，如下面的範例所示。 這是VEC偵測的必要專案 `Views` （在製作模式下）。

```javascript {line-numbers="true"}
adobe.target.getOffers({
    'request': {
        'prefetch': {
            'views': [{
                'parameters': {}
            }]
        }
    }
}).then(function(response) {
    // Apply Offers
    adobe.target.applyOffers({
        response: response
    });
}).catch(function(error) {
    console.log("AT: getOffers failed - Error", error);
}).finally(() => {
    // Trigger View call, assuming pageView is defined elsewhere
    adobe.target.triggerView(pageView, {
        page: true
    });
    console.log('AT: View triggered on : ' + pageView);
});
```

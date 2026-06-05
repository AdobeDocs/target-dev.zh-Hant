---
keywords: adobe.target.triggerView， triggerView， triggerview， trigger view， at.js，函式，函式， viewName， viewname，檢視名稱， adobe.target.triggerView1
description: 針對 [!DNL Adobe Target] at.js JavaScript資料庫使用adobe.target.triggerView()函式，以用於單頁應用程式(SPA)。 (at.js 2.x)
title: 如何使用adobe.target.triggerView()函式？
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
TQID: https://experienceleague.adobe.com/pBC1GRKG0mxeaZ1hfaByKv2tu-XScrSJfm7lUw-3yKw
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 446
ht-degree: 19%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

每當新頁面載入或頁面上的元件重新呈現時，就可呼叫此函數。 應為單頁應用程式(SPA)實作`adobe.target.triggerView()`，以使用[!UICONTROL 視覺化體驗撰寫器] (VEC)來建立[!UICONTROL A/B測試]和[!UICONTROL 體驗鎖定目標] (XT)活動。 如果未在網站上實作`[!UICONTROL adobe.target.triggerView()]`，VEC就無法用於SPA。 如需詳細資訊，請參閱[實作單頁應用程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。

>[!NOTE]
>
>此函式已在at.js 2.*x*&#x200B;中推出。 此函式不適用於at.js 1.*x*&#x200B;版。

| 參數 | 類型 | 必要? | 說明 |
| --- | --- | --- | --- |
| viewName | 字串 | 是 | 傳入任何名稱作為要代表檢視的字串類型。 此檢視名稱會顯示在VEC的[!UICONTROL 修改]面板中，供行銷人員建立動作和執行其[!UICONTROL A/B測試]和[!UICONTROL 體驗鎖定目標] XT活動。 |
| options | 物件 | 無 |  |
| options > page | 布林值 | 無 | **TRUE:** page 的預設值為 true。 當 page=true，會傳送通知至 [!DNL Target] 後端以增加曝光計數。<P>呼叫`[!UICONTROL triggerView]`時，除非options > page設為false，預設一律會傳送通知。<P>**FALSE：**&#x200B;當page=false，不會傳送通知以增加曝光計數。 只有當您想重新呈現頁面上具有選件的元件時，才應該使用此方法。<P>**注意**：呼叫`[!UICONTROL triggerView()]`並將`{page: false}`作為選項時，不會重新轉譯VEC中的自訂程式碼選件。 |

## 範例: True

將通知傳送至[!DNL Target]後端以增加活動曝光次數和其他量度的`[!UICONTROL triggerView()]`呼叫。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## 範例: False

`[!UICONTROL triggerView()]`呼叫，不會傳送通知至[!DNL Target]後端以增加曝光計數。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## 範例：與`getoffers()`和`applyOffers()`鏈結的Promise

若要在解析`getOffers()` Promise時執行`triggerView()`，必須在最終區塊上執行`triggerView()`，如下列範例所示。 這是VEC在編寫模式中偵測`Views`所必需的。

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

## 範例： `triggerView()`與[!UICONTROL Adobe Visual Editing Helper擴充功能]的最佳相容性

使用[Adobe Visual Editing Helper擴充功能](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}時，請考量下列事項：

由於[!DNL Googl]e針對[!DNL Chrome]擴充功能新增了V3資訊清單原則，[!UICONTROL Visual Editing Helper擴充功能]必須等待`DOMContentLoaded`事件，才能在VEC中載入[!DNL Target]資料庫。 此延遲可能會導致網頁在編寫程式庫準備就緒前引發`triggerView()`呼叫，導致檢視未在載入時填入。

若要緩解此問題，請使用頁面`load`事件的接聽程式。

以下是實作範例：

```javascript
function triggerViewIfLoaded() {
    adobe.target.triggerView("homeView");
}

if (document.readyState === "complete") {
    // If the page is already loaded
    triggerViewIfLoaded();
} else {
    // If the page is not yet loaded, set up an event listener
    window.addEventListener("load", triggerViewIfLoaded);
}
```



---
keywords: adobe.target.triggerView， triggerView， triggerview， trigger view， at.js，函式，函式， viewName， viewname，檢視名稱， adobe.target.triggerView1
description: 針對 [!DNL Adobe Target] at.js JavaScript程式庫使用adobe.target.triggerView()函式，以用於單頁應用程式(SPA)。 (at.js 2.x)
title: 如何使用adobe.target.triggerView()函式？
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: fe4e607173c760f782035a10f52936d96e9db300
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 21%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

每當新頁面載入或頁面上的元件重新呈現時，就可呼叫此函數。應為單頁應用程式(SPA)實作`adobe.target.triggerView()`，以使用[!UICONTROL Visual Experience Composer] (VEC)來建立[!UICONTROL A/B Test]和[!UICONTROL Experience Targeting] (XT)活動。 如果未在網站上實作`[!UICONTROL adobe.target.triggerView()]`，則VEC無法用於SPA。 如需詳細資訊，請參閱[實作單頁應用程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。

>[!NOTE]
>
>此函式於at.js 2.*x* 使用供跨網域追蹤功能時。 此函式不適用於at.js版本1。*x* 版本不支援此函數。

| 參數 | 類型 | 必要? | 說明 |
| --- | --- | --- | --- |
| viewName | 字串 | 是 | 傳入任何名稱作為要代表檢視的字串類型。此檢視名稱會顯示在VEC的[!UICONTROL Modifications]面板中，供行銷人員建立動作和執行其[!UICONTROL A/B Test]和[!UICONTROL Experience Targeting] XT活動。 |
| options | 物件 | 無 |  |
| options > page | 布林值 | 無 | **TRUE:** page 的預設值為 true。當 page=true，會傳送通知至 [!DNL Target] 後端以增加曝光計數。<P>呼叫`[!UICONTROL triggerView]`時，除非options > page設為false，預設一律會傳送通知。<P>**FALSE：**&#x200B;當page=false，不會傳送通知以增加曝光計數。 只有當您想重新呈現頁面上具有選件的元件時，才應該使用此方法。<P>**注意**：呼叫`[!UICONTROL triggerView()]`並將`{page: false}`作為選項時，不會重新轉譯VEC中的自訂程式碼選件。 |

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

## 範例： `triggerView()`與[!UICONTROL Adobe Visual Editing Helper extension]的最佳相容性

使用[AdobeVisual Editing Helper擴充功能](https://experienceleague.adobe.com/zh-hant/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}時，請考量下列事項：

由於[!DNL Googl]e針對[!DNL Chrome]擴充功能新增了V3資訊清單原則，[!UICONTROL Visual Editing Helper extension]必須等候`DOMContentLoaded`事件，才能在VEC中載入[!DNL Target]資料庫。 此延遲可能會導致網頁在編寫程式庫準備就緒前引發`triggerView()`呼叫，導致檢視未在載入時填入。

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



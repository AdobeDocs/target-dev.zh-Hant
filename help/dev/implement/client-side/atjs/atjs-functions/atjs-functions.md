---
keywords: at.js，函式， javascript資料庫
description: 檢視可與 [!DNL Adobe Target]中at.js JavaScript程式庫的1.x和2.x版本搭配使用的函式清單。
title: 我可以對at.js使用哪些函式？
feature: at.js
exl-id: 1efed365-8a74-4c85-bdb1-8daaaf53d642
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 58%

---

# at.js 函數

可搭配[!DNL Adobe Target] at.js JavaScript資料庫使用的函式清單。 按一下函數欄中的連結，取得詳細資訊和範例。

| 函數 | 詳細資料 |
| --- | --- | 
| [[!UICONTROL adobe.target.getOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) | 此函式會引發要求以取得[!DNL Target]選件。 搭配使用 `adobe.target.applyOffer()` 來處理回應或使用您自己的成功處理。 |
| [[!UICONTROL adobe.target.getOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)<P>(at.js 2.x) | 此函數可讓您透過傳入多個 mbox 來擷取多個選件。此外，還可針對使用中活動內的所有檢視擷取多個選件。<P>**注意：**&#x200B;此函式已在at.js 2.x中推出。此函式不適用於at.js版本1。*x*。 |
| [[!UICONTROL adobe.target.applyOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) | 此函數用來套用回應內容。 |
| [[!UICONTROL adobe.target.applyOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)<P>(at.js 2.x) | 此函數可讓您套用一個以上由 [!UICONTROL adobe.target.getOffers()] 擷取的選件。<P>**注意：**&#x200B;此函式已在at.js 2.x中推出。此函式不適用於at.js版本1。*x*。 |
| [[!UICONTROL adobe.target.triggerView (viewName, options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)<P>(at.js 2.x) | 每當新頁面載入或頁面上的元件重新呈現時，就可呼叫此函數。<P> 此函式應為單頁應用程式(SPA)實作，以便使用[!UICONTROL Visual Experience Composer] (VEC)來建立[!UICONTROL A/B Test]和[!UICONTROL Experience Targeting] (XT)活動。 |
| [[!UICONTROL adobe.target.trackEvent(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) | 此函數會觸發要求以報告使用者動作，例如點擊和轉換。它不會在回應中傳遞活動。 |
| [[!UICONTROL mboxCreate(mbox,params)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)<P>(at.js 1.x) | 執行要求並將選件套用至具有 mboxDefault 類別名稱的最接近 DIV。<P>**注意：**&#x200B;此函式適用於at.js版本1。*x* 版。自 at.js 2.x 版起已棄用此函數。如果與 at.js 2.x 搭配使用，此函數會傳回預設內容。 |
| [[!UICONTROL mboxDefine(options)]和[!UICONTROL mboxUpdate(options)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)<P>(at.js 1.x) | 定義和更新 mbox。<P>**注意：**&#x200B;此函式適用於at.js版本1。*x* 版。自 at.js 2.x 版起已棄用此函數。如果與 at.js 2.x 搭配使用，此函數會傳回預設內容。 |
| [[!UICONTROL targetGlobalSettings(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) | 您可以使用`[!UICONTROL targetGlobalSettings()]`覆寫at.js資料庫中的設定，而非在[!DNL Target Standard/Premium] UI中或使用REST API進行設定。<ul><li>資料提供者: 此設定可讓客戶收集來自協力廠商資料提供者 (例如 Demandbase、BlueKai 和自訂服務) 的資料，並在全域 mbox 要求中以 mbox 參數的形式傳遞資料至 Target。</li></ul> |
| [[!UICONTROL targetPageParams(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) | 此方法允許您將參數從要求程式碼外部附加至全域 mbox。 |
| [[!UICONTROL targetPageParamsAll(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) | 此方法允許您將參數從要求程式碼外部附加至所有 mbox。 |
| [[!UICONTROL registerExtension(options)]](/help/dev/implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)<P>(at.js 1.x) | 提供標準方式來註冊特定的延伸模組。<P>**注意：**&#x200B;此函式適用於at.js版本1。*x* 版。自 at.js 2.x 版起已棄用此函數。如果與 at.js 2.x 搭配使用，此函數會傳回預設內容。 |
| [[!UICONTROL at.js custom events]](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md) | at.js 自訂事件可讓您知道 mbox 要求或選件失敗或成功。 |
| [[!UICONTROL adobe.target.sendNotifications(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)<P>(at.js 2.1.0) | 此函式會在體驗呈現時傳送通知給[!DNL Target]邊緣，而不需要使用`[!UICONTROL adobe.target.applyOffer()]`或`[!UICONTROL adobe.target.applyOffers()]`。<P>**注意**：此函式已在at.js 2.1.0中推出，且將適用於2.1.0以上的任何版本。 |

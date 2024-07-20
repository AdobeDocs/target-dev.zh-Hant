---
keywords: mboxCreate， mboxcreate， mbox create， at.js，函式，函式
description: 對 [!DNL Adobe Target] at.js JavaScript程式庫使用[!UICONTROL mboxCreate()]函式，將選件套用至具有mboxDefault類別名稱的最接近DIV。 (at.js 1.x)
title: 如何使用[!UICONTROL mboxCreate()]函式？
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 56%

---

# [!UICONTROL mboxCreate(mbox,params)] - at.js 1.x

執行要求並將選件套用至具有 mboxDefault 類別名稱的最接近 DIV。

>[!NOTE]
>
>此函數僅適用於 at.js 1.*x* 版。自 at.js 2.x 版起已棄用此函數。如果與 at.js 2.x 搭配使用，此函數會傳回預設內容。

此函式多半內建在at.js中，以便於從mbox.js （現已被取代）轉換至at.js。 `[!UICONTROL mboxCreate()]` 較新的替代方案是 `[!UICONTROL adobe.target.applyOffer()]`/ `[!UICONTROL adobe.target.getOffer()]` 或 Angular 指令。

## 範例

```javascript {line-numbers="true"}
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  mboxCreate('mboxName','param1=value1','param2=value2'); 
</script>
```

## 附註

`mboxCreate()` 現在使用「json」端點而非「standard」端點，且非同步觸發。因此:

* [偵錯](/help/dev/implement/client-side/target-debugging-atjs/target-debugging-atjs.md)有一些不同。
* 避免選件程式碼需要同步，封鎖呼叫。

  例如，設定後續進入頁面之網站程式碼或其他 mbox 所使用 JavaScript 變數的選件。

* 在叫用`[!UICONTROL mboxCreate()]`之前，請確定您有`<div class="mboxDefault"></div>`，因為at.js不會為您新增此專案。

* 不建議將空白的 top-of-page `[!UICONTROL mboxCreate()]` 函數用作全域 mbox。

  at.js中自動建立的全域mbox是較好的選項，因為它從`<head>`引發，而且可能較早傳回內容。

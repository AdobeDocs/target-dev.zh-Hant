---
keywords: mboxCreate， mboxcreate， mbox create， at.js，函式，函式
description: 對 [!DNL Adobe Target] at.js JavaScript資料庫使用[!UICONTROL mboxCreate()]函式，將選件套用至具有mboxDefault類別名稱的最接近DIV。 (at.js 1.x)
title: 如何使用[!UICONTROL mboxCreate()]函式？
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
TQID: https://experienceleague.adobe.com/hCEKL9RPtqIbMVEouzObjU6dc7TKl1hBtKZ1jEdicRE
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
source-wordcount: 216
ht-degree: 40%

---

# [!UICONTROL mboxCreate(mbox，params)] - at.js 1.x

執行要求並將產品建議套用至具有 mboxDefault 類別名稱的最接近 DIV。

>[!NOTE]
>
>此函式僅適用於at.js 1.*x*&#x200B;版。 此函式已在at.js 2.x版本中棄用。 如果與at.js 2.x搭配使用，此函式會傳回預設內容。

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

`mboxCreate()` 現在使用「json」端點而非「standard」端點，且非同步觸發。 因此:

* [偵錯](/help/dev/implement/client-side/target-debugging-atjs/target-debugging-atjs.md)有一些不同。
* 避免產品建議程式碼需要同步，封鎖呼叫。

  例如，設定後續進入頁面之網站程式碼或其他 mbox 所使用 JavaScript 變數的產品建議。

* 在叫用`[!UICONTROL mboxCreate()]`之前，請確定您有`<div class="mboxDefault"></div>`，因為at.js不會為您新增此專案。

* 不建議將空白的 top-of-page `[!UICONTROL mboxCreate()]` 函數用作全域 mbox。

  at.js中自動建立的全域mbox是較好的選項，因為它從`<head>`引發，而且可能較早傳回內容。

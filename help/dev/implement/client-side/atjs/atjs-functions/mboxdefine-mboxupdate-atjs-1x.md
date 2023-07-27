---
keywords: mboxDefine， mboxdefine， mbox define， mboxUpdate， mboxupdate， mbox更新， at.js，函式，函式， mboxDefine0
description: 使用 [!UICONTROL mboxDefine()] 和 [!UICONTROL mboxUpdate()] 的函式 [!DNL Adobe Target] at.js JavaScript程式庫來定義或更新mbox。 (at.js 1.x)
title: 如何使用 [!UICONTROL mboxDefine()] 與 [!UICONTROL mboxUpdate()] 函式？
feature: at.js
exl-id: 0a7dbea2-1cbd-4a5b-ba68-4c76a88d65c4
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 52%

---

# [!UICONTROL mboxDefine()] 和 [!UICONTROL mboxUpdate()] - at.js 1.x

在中定義和更新mbox [!DNL Adobe Target].

>[!NOTE]
>
>這些函數僅適用於 at.js 1.*x* 版。這些函式已在at.js 2.*x*.如果與at.js 2.*x*.

`[!UICONTROL mboxDefine()]` 和 `[!UICONTROL mboxCreate()]` 已與 HTML DIV 元素綁定，這些元素應會顯示選件。這些 HTML DIV 元素應該具有 `mboxDefault` 類別。如果 HTML 元素沒有附加此類別，您會看到一些顯著的閃爍。

## mboxDefine

在 nodeId 與 mbox 名稱之間建立內部對應，但是不會執行要求。用來結合 `[!UICONTROL mboxUpdate()]`。多半建置在at.js中，以便於從mbox.js （現已被取代）轉換至at.js。

## mboxUpdate

執行要求並套用選件至 `nodeId` () 中的 `mboxDefine()` 所識別的元素。也可用來更新 `[!UICONTROL mboxCreate]` 起始的 mbox。多半建置在at.js中，以便於從mbox.js （現已被取代）轉換至at.js。 可使用選取器選項將 `mboxDefine()`/ `mboxUpdate()` 取代為 [adobe.target.getOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) 和 [adobe.target.applyOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) 

## 範例

```javascript {line-numbers="true"}
<div id="someId" class="mboxDefault"></div> 
<script> 
 mboxDefine('someId','mboxName','param1=value1','param2=value2'); 
 mboxUpdate('mboxName','param3=value3','param4=value4'); 
</script>
```

---
keywords: mboxDefine， mboxdefine， mbox define， mboxUpdate， mboxupdate， mbox更新， at.js，函式，函式， mboxDefine0
description: 使用 [!DNL Adobe Target] at.js JavaScript資料庫的[!UICONTROL mboxDefine()]和[!UICONTROL mboxUpdate()]函式來定義或更新mbox。 (at.js 1.x)
title: 如何使用[!UICONTROL mboxDefine()]和[!UICONTROL mboxUpdate()]函式？
feature: at.js
exl-id: 0a7dbea2-1cbd-4a5b-ba68-4c76a88d65c4
TQID: https://experienceleague.adobe.com/Fn-Ej8jk2AMEn79tOtRoP9GQc36Ugy6FtXyn6x7jkmA
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 202
ht-degree: 47%

---

# [!UICONTROL mboxDefine()]和[!UICONTROL mboxUpdate()] - at.js 1.x

在[!DNL Adobe Target]中定義和更新mbox。

>[!NOTE]
>
>這些函式僅適用於at.js 1.*x*&#x200B;版。 這些函式已在at.js 2.*x*&#x200B;版本中棄用。 如果與at.js 2.*x*&#x200B;搭配使用，這些函式會傳回預設內容。

`[!UICONTROL mboxDefine()]` 和 `[!UICONTROL mboxCreate()]` 已與 HTML DIV 元素綁定，這些元素應會顯示產品建議。 這些 HTML DIV 元素應該具有 `mboxDefault` 類別。 如果 HTML 元素沒有附加此類別，您會看到一些顯著的閃爍。

## mboxDefine

在 nodeId 與 mbox 名稱之間建立內部對應，但是不會執行要求。 用來結合 `[!UICONTROL mboxUpdate()]`。 多半建置在at.js中，以便於從mbox.js （現已被取代）轉換至at.js。

## mboxUpdate

執行要求並套用產品建議至 `nodeId` () 中的 `mboxDefine()` 所識別的元素。 也可用來更新 `[!UICONTROL mboxCreate]` 起始的 mbox。 多半建置在at.js中，以便於從mbox.js （現已被取代）轉換至at.js。 可使用選取器選項將 `mboxDefine()`/ `mboxUpdate()` 取代為 [adobe.target.getOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) 和 [adobe.target.applyOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md)

## 範例

```javascript {line-numbers="true"}
<div id="someId" class="mboxDefault"></div> 
<script> 
 mboxDefine('someId','mboxName','param1=value1','param2=value2'); 
 mboxUpdate('mboxName','param3=value3','param4=value4'); 
</script>
```

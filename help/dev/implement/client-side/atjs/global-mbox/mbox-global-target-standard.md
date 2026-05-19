---
keywords: 全域 mbox, target classic, 使用來自 target classic 的全域 mbox
description: 如果您已在您的頁面上針對舊版實作建立全域mbox，請瞭解如何針對您的 [!DNL Adobe Target] 活動使用舊版全域mbox。
title: 我是否可以使用來自舊版實作的全域mbox？
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
TQID: https://experienceleague.adobe.com/BCubNDwB8gxZ9bpuCNhxcnFnjB1xQK8ZRkLveinPj4w
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
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 285
ht-degree: 20%

---

# 使用來自舊版實作的全域mbox

根據預設，[!DNL Target]會建立名為target-global-mbox的全域mbox，用來執行[!DNL Target]中建立的活動。 不過，如果您已在您的頁面上針對舊版實作建立全域mbox，則可將該mbox用於[!DNL Target]活動。

>[!NOTE]
>
>每個帳戶只能有一個全域 mbox。

若想同時在 [!DNL Target] 和您的舊版實施中使用現有的全域 mbox，您必須設定幾個參數。

1. 前往[!DNL Target]，然後按一下&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]**。

   預設會啟用&#x200B;**[!UICONTROL Page load enabled (Auto-create global mbox]**，而自訂全域mbox的名稱為`target-global-mbox`。

1. 如果您想使用現有的mbox，請停用&#x200B;**[!UICONTROL Page load enabled (Auto-create global mbox]**，並在&#x200B;**[!UICONTROL Global Mbox]**&#x200B;欄位中指定先前建立之全域mbox的名稱。

   全域mbox下拉式清單會列出您帳戶中的所有mbox。 如果您想使用尚未存在的mbox，請建立mbox。

1. 按一下 **[!UICONTROL Save]**。

   您的帳戶設定已更新。

1. 下載新的at.js檔案，並在您的網站上參考該檔案。

   所有現有活動都會更新為使用指定的全域 mbox，包括先前已建立和實施的活動。

## 疑難排解全域mbox實作

下列常見問題集可用來疑難排解全域mbox實作：

### 全域mbox為何不會載入，或為何頁面載入時載入全域mbox發生延遲？

確認at.js參考是頁面上的第一個JavaScript呼叫。 如需此問題的其他解決方案，請參閱[全域mbox常見問題](/help/dev/implement/client-side/atjs/global-mbox/global-mbox-faq.md)。

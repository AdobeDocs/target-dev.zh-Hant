---
keywords: 全域 mbox, target classic, 使用來自 target classic 的全域 mbox
description: 如果您已在您的頁面上針對舊版實作建立全域mbox，請瞭解如何針對您的 [!DNL Adobe Target] 活動使用舊版全域mbox。
title: 我是否可以使用來自舊版實作的全域mbox？
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
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

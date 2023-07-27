---
keywords: 全域 mbox, target classic, 使用來自 target classic 的全域 mbox
description: 瞭解如何為您的使用舊版全域mbox [!DNL Adobe Target] 活動（如果您已在頁面上為舊版實作建立全域mbox）。
title: 我是否可以使用來自舊版實作的全域mbox？
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 36%

---

# 使用來自舊版實作的全域mbox

根據預設， [!DNL Target] 建立稱為target-global-mbox的全域mbox，用來執行在中建立的活動 [!DNL Target]. 不過，若您已為您的舊版實作在頁面上建立全域 mbox，則可將該 mbox 用於您的 [!DNL Target] 活動。

>[!NOTE]
>
>每個帳戶只能有一個全域 mbox。

若想同時在 [!DNL Target] 和您的舊版實施中使用現有的全域 mbox，您必須設定幾個參數。

1. 前往 [!DNL Target]，然後按一下 **[!UICONTROL 管理]** > **[!UICONTROL 實施]**.

   根據預設， **[!UICONTROL 已啟用頁面載入(自動建立全域mbox]** 已啟用，且自訂全域mbox已命名為 `target-global-mbox`.

1. 如果您想使用現有的mbox，請停用 **[!UICONTROL 已啟用頁面載入(自動建立全域mbox]**，並在「 」中指定先前建立之全域mbox的名稱 **[!UICONTROL 全域Mbox]** 欄位。

   全域mbox下拉式清單會列出您帳戶中的所有mbox。 如果您想使用尚未存在的mbox，請建立mbox。

1. 按一下&#x200B;**[!UICONTROL 儲存]**。

   接著會更新您帳戶的 設定。

1. 下載新的at.js檔案，並在您的網站上參考該檔案。

   所有現有活動都會更新為使用指定的全域 mbox，包括先前已建立和實施的活動。

## 疑難排解全域mbox實作

下列常見問題集可用來疑難排解全域mbox實作：

### 全域 mbox 為什麼不會載入，或為什麼頁面載入時載入全域 mbox 發生延遲?

確認at.js參考是頁面上的第一個JavaScript呼叫。 如需此問題的其他解決方案，請參閱 [全域mbox常見問題](/help/dev/implement/client-side/atjs/global-mbox/global-mbox-faq.md).

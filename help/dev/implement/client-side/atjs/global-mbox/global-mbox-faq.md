---
keywords: 疑難排解, 常見問題集, FAQ, 全域, 全域 mbox
description: 閱讀有關Adobe [!DNL Target] 全域mbox的常見問答(FAQ)和回答。
title: 關於全域mbox的常見問題集有哪些？
feature: at.js
exl-id: 7bcd1b67-809a-466a-b648-6e0e44386157
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 39%

---

# 全域 mbox 常見問題

關於全域 mbox 常見問題集 (FAQ) 的清單。

## 如果我的[!DNL Target]帳戶設定為跨多個網域，我可以有多個全域mbox嗎？

整個帳戶僅支援一個全域 mbox。

您可以在活動中新增 URL 規則，以限制執行活動的位置。如需詳細資訊，請參閱[在類似頁面上包含相同體驗](https://experienceleague.adobe.com/docs/target/using/experiences/vec/temtest.html?lang=zh-Hant)。

您也可以使用[targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)傳遞頁面上的引數，然後在[!UICONTROL Visual Experience Composer] (VEC)的「設定URL」區段中選取引數，或在[!UICONTROL Form-Based Experience Composer]中將引數新增為「細分」。

## 我該如何在[!DNL Target]全域mbox上傳遞收入資料？

若要收集有關target-global-mbox的收入和訂單資訊，「mbox引數」必須傳送至[!DNL Target]。 這些引數是用於傳送更多資訊給[!DNL Target]的名稱/值組。 [!DNL Target]會自動尋找這些引數（保留名稱）以填入收入資料。

對於`orderConfirmPage`，您應該傳入`orderTotal`、`orderId`和`productPurchasedId`。

這些引數必須透過`targetPageParams()`傳送至target-global-mbox。 如需詳細資訊，請參閱[將參數傳遞至全域 mbox](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)。

您也會想要將目標定位新增至轉換片段，以便[!DNL Target]只會在已檢視訂單確認頁面時計算target-global-mbox上的轉換，如下所示：

![替代影像](assets/revenue1.png)

上面顯示的「網站頁面」區段包含下列選擇: 目前頁面、URL、包含、orderconfirm。

![替代影像](assets/revenue2.png)

上圖中的選項包含下列設定:

* **您要測量這個活動的什麼?:**&#x200B;收入
* **報告的預設檢視:**&#x200B;每次造訪帶來的收入 (RPV)
* **客群採取什麼動作來指出已達到您的目標?** 已檢視 mbox、target-global-mbox

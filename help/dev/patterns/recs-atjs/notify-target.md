---
title: 通知Target
description: 請確定所有需要由 [!DNL Target] 追蹤的事件都是使用trackEvent方法傳送。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: efccadab-d139-4423-8613-c2743d87b3a0
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 1%

---

# 通知[!DNL Target]

完成此步驟可確保所有必須傳送至[!DNL Adobe Target]的事件都會使用`trackEvent`方法傳送。

任何需要在[!DNL Target]中追蹤的事件都可以是主要轉換事件或成功量度。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

## 通知[!DNL Target]圖表 {#diagram}

下圖中的步驟編號與下節相對應。

![通知目標圖表](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 4.1：引發[!DNL Adobe Target]追蹤API

此步驟可協助您確保必須傳送至[!DNL Target]的所有事件都是使用`trackEvent`方法傳送。

+++查看詳細資料

![引發Adobe Target追蹤API圖表](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram-combined.png){width="400" zoomable="yes"}

您傳送下方&#x200B;*必要條件*&#x200B;一節中所述的訂單轉換屬性。 mbox的名稱並不重要，但轉換是使用`orderConfirmPage`。

您不需要在此呼叫中包含訂單轉換屬性。 這些呼叫最好能記錄成功量度，這些量度可在主要轉換事件之前被視為迷你轉換事件。 `CardIds`必須包含在以`Add to Cart`事件為基礎的購物車型建議中。

**必要條件**

* 與您的業務團隊會面，以識別所有可視為轉換或成功量度的事件。 您也必須識別產生收入的轉換事件，以便將這些詳細資料連同事件資料一起傳送給[!DNL Target]。
* 確定資料層中可以使用下列屬性，以便您在傳送時能夠包含轉換事件。 轉換事件會產生收入，例如產品購買或加入購物車事件。

   * `productPurchaseId`：訂單中已購買的產品ID。 請使用逗號分隔多個產品。
   * `orderTotal`：購買的訂單總計。
   * `orderId`：購買的訂單識別碼。

  下圖顯示 [!DNL Experience Platform][&#128279;](https://experienceleague.adobe.com/docs/tags.html?lang=zh-Hant){target=_blank}中 [!DNL tags] 的規則，此規則僅應在[!UICONTROL Confirmation]頁面上觸發。

  ![動作設定頁面](/help/dev/patterns/recs-atjs/assets/action-configuration.png){width="400" zoomable="yes"}

* 如果您正在追蹤購物車新增的事件，請傳送`cartIds`作為引數。 可為`cardIds`傳遞以逗號分隔的產品識別碼清單。

**讀數**

* [adobe.target.trackEvent()方法](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* 購物車型條件的[cartIds](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=zh-Hant#cart-based){target=_blank}

**動作**

* 使用`adobe.target-trackEvent()`方法傳送所有必須傳送至[!DNL Target]的資料。

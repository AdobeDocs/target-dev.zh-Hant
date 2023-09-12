---
title: 通知Target
description: 確定所有需要追蹤的事件 [!DNL Target] 會使用trackEvent方法傳送。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 6cd78f8e3cbdd97a09b0cb6ca3af55994e85f819
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 1%

---

# 通知 [!DNL Target]

完成此步驟可確保所有必須傳送至的事件 [!DNL Adobe Target] 使用傳送 `trackEvent` 方法。

任何需要在中追蹤的事件 [!DNL Target] 可以是主要轉換事件或成功量度。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

## 通知 [!DNL Target] 圖表 {#diagram}

下圖中的步驟編號與下節相對應。

![通知Target](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 引發 [!DNL Adobe Target] 追蹤API

此步驟可協助您確定所有必須傳送至的事件 [!DNL Target] 使用傳送 `trackEvent` 方法。

+++查看詳細資料

![Fire Adobe Target追蹤API圖表](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram.png){width="400" zoomable="yes"}

您傳送訂單轉換屬性，如 *必要條件* 一節。 mbox的名稱並不重要，但轉換後會使用 `orderConfirmPage`.

您不需要在此呼叫中包含訂單轉換屬性。 這些呼叫最好能記錄成功量度，這些量度可在主要轉換事件之前被視為迷你轉換事件。 `CardIds` 必須包含在購物車型建議中，且必須根據 `Add to Cart` 事件。

**必要條件**

* 與您的業務團隊會面，以識別所有可視為轉換或成功量度的事件。 您也必須識別產生收入的轉換事件，以便將這些詳細資料傳送至 [!DNL Target] 以及事件資料。
* 確定資料層中可以使用下列屬性，以便您在傳送時能夠包含轉換事件。 轉換事件會產生收入，例如產品購買或加入購物車事件。

   * `productPurchaseId`：隨訂單購買的產品ID。 請使用逗號分隔多個產品。
   * `orderTotal`：購買的訂單總計。
   * `orderId`：購買的訂單ID。

* 如果您正在追蹤購物車新增的事件，請傳送 `cartIds` 作為引數。 可以傳遞產品ID的逗號分隔清單 `cardIds`.

**讀數**

* [adobe.target.trackEvent()方法](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [適用於購物車型標準的cartIds](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based){target=_blank}

**動作**

* 使用 `adobe.target-trackEvent()` 傳送所有必須傳送至的資料的方法 [!DNL Target].








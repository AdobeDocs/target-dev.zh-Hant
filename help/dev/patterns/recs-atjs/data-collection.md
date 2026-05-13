---
title: 設定資料彙集
description: 請確定資料收集的所有必要工作都以正確順序執行。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 66e0f18d-c78c-463b-8c47-132ef6332927
TQID: https://experienceleague.adobe.com/fg3xJnwYAVyz-N-xzT5Piu35Ajd2UMEvuTvTQs2wj3c
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 401
ht-degree: 1%

---

# 設定資料彙集

請依照&#x200B;*資料彙集*&#x200B;圖表中的步驟執行，以確保資料彙集所需的所有必要工作都以正確的順序執行。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

資料層會在頁面載入期間準備就緒，或資料層會在頁面載入後變更。

如果您在[初始化SDK階段](/help/dev/patterns/recs-atjs/initialize-sdk.md)期間已經對應資料，則在下列情況下必須執行此圖表中的步驟：

* 您的資料層會在相同頁面上以任何方式增加，而且您想要將該額外資料傳送至[!DNL Target]
* 您想要將產品目錄資料傳送至[!DNL Target Recommendations]

## 收集資料圖表 {#diagram}

下圖中的步驟編號與下列區段相對應。

![資料彙集圖表](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

按一下下列連結，導覽至所需的區段：

* [2.1：設定資料對應](#configure)
* [2.2實體屬性的連結](#entity-attributes)
* [2.3引發Adobe Target追蹤API](#fire-api)

## 2.1：設定資料對應 {#configure}

此步驟有助於確保已設定必須傳送至[!DNL Adobe Target]的所有資料。

+++檢視詳細資料

![設定資料對應圖表](/help/dev/patterns/recs-atjs/assets/configure-data-mapping-combined.png){width="400" zoomable="yes"}

**必要條件**

* 資料層應準備好所有必須傳送給[!DNL Target]的資料。

**讀數**

[targetPageParams函式](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**動作**

使用`targetPageParams()`函式設定必須傳送至[!DNL Target]的所有必要資料。

+++

[返回此頁面頂端的圖表。](#diagram)

## 2.2：實體屬性的連結 {#entity-attributes}

連結至實體屬性，以更新[!DNL Target Recommendations]的產品目錄。

+++檢視詳細資料

**讀數**

* [實體屬性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**考量事項**

* 傳遞實體屬性的替代方法是更新[!DNL Target] UI中的產品目錄，以使用[Recommendations產品摘要](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}。
* 傳遞實體屬性僅適用於資料層中有產品目錄資料的頁面。
* 在任何呼叫中傳遞`entity.event.detailsOnly=true`引數都具有優先權。

+++

[返回此頁面頂端的圖表。](#diagram)

## 2.3引發Adobe Target追蹤API {#fire-api}

此步驟有助於確保必須傳送至[!DNL Target]的所有資料都已傳送。

+++檢視詳細資料

![引發Adobe Target追蹤API圖表](/help/dev/patterns/recs-atjs/assets/fire-track-api-combined.png){width="400" zoomable="yes"}

**必要條件**

* 所有資料對應都必須使用[targetPageParams函式](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)完成。

**讀數**

* [adobe.target.trackEvent()方法](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**動作**

使用[adobe.target.trackEvent()方法](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)傳送必須傳送至[!DNL Target]的所有資料。

+++

[返回此頁面頂端的圖表。](#diagram)

繼續步驟3：[演算體驗](/help/dev/patterns/recs-atjs/render-experiences.md)

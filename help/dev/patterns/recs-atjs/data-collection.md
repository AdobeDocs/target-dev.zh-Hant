---
title: 設定資料彙集
description: 請確定資料收集的所有必要工作都以正確順序執行。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 66e0f18d-c78c-463b-8c47-132ef6332927
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 2%

---

# 設定資料彙集

請依照&#x200B;*資料彙集*&#x200B;圖表中的步驟執行，以確保資料彙集所需的所有必要工作都以正確的順序執行。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

資料層會在頁面載入期間準備就緒，或資料層會在頁面載入後變更。

如果您已在[初始化SDK階段](/help/dev/patterns/recs-atjs/initialize-sdk.md)期間對應資料，且符合下列條件，您必須執行此圖表中的步驟：

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

+++查看詳細資料

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

+++查看詳細資料

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

+++查看詳細資料

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

---
title: 設定資料彙集
description: 請確定資料收集的所有必要工作都以正確順序執行。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 30634afc84877a4e88e08f3b2173d4c0727f4362
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---

# 設定資料彙集

請依照以下檔案中的步驟操作： *資料彙集* 此圖表可確保資料收集所需的所有必要工作皆以正確順序執行。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

資料層會在頁面載入期間準備就緒，或資料層會在頁面載入後變更。

如果您已在以下期間對應資料： [初始化SDK階段](/help/dev/patterns/recs-atjs/initialize-sdk.md)，您必須在下列情況下執行此圖表中的步驟：

* 您的資料層會在相同頁面上以任何方式增加，並且您想要將該其他資料傳送至 [!DNL Target]
* 您要將產品目錄資料傳送至 [!DNL Target Recommendations]

## 收集資料圖表 {#diagram}

下圖中的步驟編號與下列區段相對應。

![資料收集圖表](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

按一下下列連結，導覽至所需的區段：

* [2.1：設定資料對應](#configure)
* [2.2實體屬性的連結](#entity-attributes)
* [2.3引發Adobe Target追蹤API](#fire-api)

## 2.1：設定資料對應 {#configure}

此步驟有助於確保必須傳送至的所有資料 [!DNL Adobe Target] 已設定。

+++查看詳細資料

![設定資料對應圖表](/help/dev/patterns/recs-atjs/assets/configure-data-mapping-combined.png){width="400" zoomable="yes"}

**必要條件**

* 資料層應準備好接收所有必須傳送到的資料 [!DNL Target].

**讀數**

[targetPageParams函式](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**動作**

使用 `targetPageParams()` 函式，可設定必須傳送至的所有必要資料 [!DNL Target].

+++

[返回此頁面頂端的圖表。](#diagram)

## 2.2：實體屬性的連結 {#entity-attributes}

連結到實體屬性以更新產品目錄 [!DNL Target Recommendations].

+++查看詳細資料

**讀數**

* [實體屬性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**考量事項**

* 傳遞實體屬性的另一種方式是更新以下專案中的產品目錄： [!DNL Target] 要使用的UI [Recommendations產品摘要](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}.
* 傳遞實體屬性僅適用於資料層中有產品目錄資料的頁面。
* 傳遞 `entity.event.detailsOnly=true` 任何呼叫中的引數都有優先權。

+++

[返回此頁面頂端的圖表。](#diagram)

## 2.3引發Adobe Target追蹤API {#fire-api}

此步驟有助於確保必須傳送至的所有資料 [!DNL Target] 「 」已傳送。

+++查看詳細資料

![Fire Adobe Target追蹤API圖表](/help/dev/patterns/recs-atjs/assets/fire-track-api-combined.png){width="400" zoomable="yes"}

**必要條件**

* 所有資料對應必須已使用 [targetPageParams函式](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**讀數**

* [adobe.target.trackEvent()方法](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**動作**

使用 [adobe.target.trackEvent()方法](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) 以傳送必須傳送至的所有資料 [!DNL Target].

+++

[返回此頁面頂端的圖表。](#diagram)


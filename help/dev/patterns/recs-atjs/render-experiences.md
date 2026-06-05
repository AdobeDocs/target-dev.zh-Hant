---
title: 演算體驗
description: 請確定轉譯體驗所需的所有步驟皆以正確順序執行。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 7cf0c70b-a4bc-46f4-9b33-099bdb7dd9a9
TQID: https://experienceleague.adobe.com/uHFbc8JEhjjGYIulJUvhkH7cXXht6Rht9rY43HjuNqg
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1144
ht-degree: 3%

---

# 演算體驗

請依照&#x200B;*轉譯器體驗*&#x200B;圖表中的步驟來確保轉譯器體驗所需的所有必要工作都以正確的順序執行。

>[!NOTE]
>
>如果您在&#x200B;*初始化SDKS*&#x200B;的[設定自動頁面載入要求步驟](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic)期間啟用了自動頁面載入要求，除非您想要呼叫Adobe Target SDK以使用地區位置要求呈現其他體驗，否則可以略過此活動。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

## 演算體驗圖表 {#diagram}

只有當您啟用[!UICONTROL 自動頁面載入請求]時，at.js提供的自動現成閃爍處理才有意義。 此選項會隱藏整個HTML內文，同時從[!DNL Target]擷取體驗。 在此情況下，您有責任處理忽隱忽現的情形。 搜尋可用於處理忽隱忽現情況的實作模式，以取得指引。

>[!NOTE]
>
>下圖中的步驟編號與下列區段相對應。 步驟編號沒有特定順序，也不會反映建立活動時在[!DNL Target] UI中採取的步驟順序。

![演算體驗圖表](/help/dev/patterns/recs-atjs/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

按一下下列連結，導覽至所需的區段：

* [3.1：促銷活動](#promotion)
* [3.2：購物車型條件](#cart)
* [3.3：以人氣為基礎的條件](#popularity)
* [3.4：以專案為基礎的條件](#item)
* [3.5：以使用者為基礎的條件](#user)
* [3.6：自訂條件](#custom)
* [3.7：提供包含規則中使用的屬性](#inclusion)
* [3.8：提供excludedIds](#exclude)
* [3.9：提供實體屬性，以更新Recommendations的產品目錄](#entity-attributes)
* [3.10：提供用來作為包含規則索引鍵的設定檔屬性](#keys)
* [3.11：引發頁面載入請求](#fire)
* [3.12：引發地區位置要求](#location)

## 3.1：促銷活動 {#promotion}

建立活動時，在[!DNL Target] UI中選擇「前面」或「後面」促銷活動，以新增促銷專案並控制其在建議設計中的放置位置。

+++檢視詳細資料

**可用選項**

* 依ID促銷
* [依集合促銷](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [依屬性促銷](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**需要實體引數**

* 使用「依屬性促銷」選項時，必須傳遞促銷活動中的專案屬性。

**讀數**

* [新增促銷活動](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/adding-promotions.html){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.2：購物車型條件 {#cart}

根據使用者的購物車內容提供建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL 瀏覽過這些專案、也瀏覽了其他專案的使用者]
* [!UICONTROL 瀏覽過這些專案、但購買了其他專案的使用者]
* [!UICONTROL 購買這些專案、也購買這些專案的使用者]

**需要實體引數**

* cartIds

**讀數**

* [購物車型](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.3：以人氣為基礎的條件 {#popularity}

根據您的網站上專案的整體人氣或根據訪客最喜愛或檢視次數最多的類別、品牌、型別等內的專案人氣提供建議。

+++檢視詳細資料

**可用的條件**

* 整個網站檢視次數最多
* 依類別檢視次數最多
* [!UICONTROL 檢視次數最多的專案屬性]
* 整個網站[!UICONTROL 最暢銷商品]
* [!UICONTROL 依類別排名的最暢銷商品]
* [!UICONTROL 依專案屬性的最暢銷商品]
* [!UICONTROL Analytics量度排名最前]

**需要實體引數**

* 如果條件是以目前或專案屬性為基礎，則為`entity.categoryId`或人氣的專案屬性。
* 所有網站中的「檢視次數最多/銷售最高」量度不需傳遞任何專案。

**讀數**

* [基於人氣](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.4：以專案為基礎的條件 {#item}

根據找到使用者正在檢視或最近檢視的專案的類似專案提供建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL 瀏覽過此專案、也瀏覽了其他專案的使用者]
* [!UICONTROL 瀏覽過此專案、但購買了其他專案的使用者]
* [!UICONTROL 購買了此專案、也購買了其他專案的使用者]
* [!UICONTROL 具有類似屬性的專案]

**需要實體引數**

* `entity.id`
* 如果有任何設定檔屬性當作索引鍵使用

**讀數**

* [基於專案](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.5：以使用者為基礎的條件 {#user}

根據使用者的行為提供建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL 最近查看的項目]
* [!UICONTROL 為您推薦]

**需要實體引數**

* `entity.id`

**讀數**

* [基於使用者](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.6：自訂條件 {#custom}

根據您上傳的自訂檔案提出建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL 自訂演演算法]

**需要實體引數**

`entity.id`或用作自訂演演算法索引鍵的屬性

**讀數**

* [自訂條件](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.7：提供包含規則中使用的屬性 {#inclusion}

+++檢視詳細資料

**讀數**

* [使用動態和靜態包含規則](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.8：提供excludedIds {#exclude}

傳遞要從建議中排除之實體的實體ID。 例如，您可以排除已在購物車中的項目。

+++檢視詳細資料

**讀數**

* [是否可以動態地排除實體?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.9：提供實體屬性以更新[!DNL Recommendations]的產品目錄 {#entity-attributes}

+++檢視詳細資料

**讀數**

* [實體屬性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

您也可以使用[!DNL Target] UI建立[產品摘要](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}，以更新[!DNL Recommendations]的產品目錄，進而完成此步驟。

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.10：提供用來作為包含規則索引鍵的設定檔屬性 {#keys}

提供上述任何Recommendations條件中，作為包含規則索引鍵所使用的設定檔屬性。

+++ 檢視詳細資料

**讀數**

* [輪廓屬性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.11：引發頁面載入請求 {#fire}

此步驟會觸發要求中具有`execute` > `pageLoad`裝載的[!DNL Delivery API]呼叫。 `getOffers()`方法會擷取體驗，而`applyOffers()`會轉譯頁面上的體驗。 需要`pageLoad`要求，才能轉譯在[視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC)中編寫的體驗。

+++檢視詳細資料

![引發頁面載入要求圖表](/help/dev/patterns/recs-atjs/assets/fire-page-load-request-combined.png){width="400" zoomable="yes"}

**必要條件**

* 必須使用`targetPageParams`函式完成所有資料對應。

**讀數**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**動作**

* 使用`getOffers`和`applyOffers`方法，以使用頁面載入請求API呼叫來擷取體驗。

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.12：引發地區位置要求(#location)

此步驟會在其要求中觸發具有`execute` > `mboxes`裝載的[!DNL Delivery API]呼叫。 `getOffers`方法會擷取體驗，而`applyOffers`會將體驗呈現至頁面。 您可以在`execute` > `mboxes`裝載下傳送多個mbox。

+++檢視詳細資料

![引發區域位置要求圖表](/help/dev/patterns/recs-atjs/assets/fire-regional-location-request-combined.png){width="400" zoomable="yes"}

**必要條件**

* 必須使用`targetPageParams`函式完成所有資料對應。

**讀數**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**動作**

* 使用`getOffers`和`applyOffers`方法，以使用頁面載入請求API呼叫來擷取體驗。

+++

[返回此頁面頂端的圖表。](#diagram)

繼續步驟4：[通知目標](/help/dev/patterns/recs-atjs/notify-target.md)。

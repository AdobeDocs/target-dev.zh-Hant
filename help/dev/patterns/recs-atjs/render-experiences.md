---
title: 演算體驗
description: 請確定轉譯體驗所需的所有步驟皆以正確順序執行。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 7cf0c70b-a4bc-46f4-9b33-099bdb7dd9a9
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 4%

---

# 演算體驗

請依照&#x200B;*轉譯器體驗*&#x200B;圖表中的步驟來確保轉譯器體驗所需的所有必要工作都以正確的順序執行。

>[!NOTE]
>
>如果您在&#x200B;*初始化SDK*&#x200B;中的[設定自動頁面載入要求步驟](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic)期間啟用了自動頁面載入要求，除非您想要呼叫Adobe Target SDK以使用地區位置要求呈現其他體驗，否則可以略過此活動。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

## 演算體驗圖表 {#diagram}

at.js提供的自動現成閃爍處理功能只有在您啟用[!UICONTROL Automatic Page Load Request]時才有意義。 此選項會隱藏整個HTML內文，同時從[!DNL Target]擷取體驗。 在此情況下，您有責任處理忽隱忽現的情形。 搜尋可用於處理忽隱忽現情況的實作模式，以取得指引。

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
* [3.9：提供實體屬性以更新Recommendations的產品目錄](#entity-attributes)
* [3.10：提供用來作為包含規則索引鍵的設定檔屬性](#keys)
* [3.11：引發頁面載入請求](#fire)
* [3.12：引發地區位置要求](#location)

## 3.1：促銷活動 {#promotion}

建立活動時，在[!DNL Target] UI中選擇「前面」或「後面」促銷活動，以新增促銷專案並控制其在建議設計中的放置位置。

+++查看詳細資料

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

+++查看詳細資料

**可用的條件**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**需要實體引數**

* cartIds

**讀數**

* [購物車型](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.3：以人氣為基礎的條件 {#popularity}

根據您的網站上專案的整體人氣或根據訪客最喜愛或檢視次數最多的類別、品牌、型別等內的專案人氣提供建議。

+++查看詳細資料

**可用的條件**

* [!UICONTROL Most Viewed Across the Site]
* [!UICONTROL Most Viewed by Category]
* [!UICONTROL Most Viewed by Item Attribute]
* [!UICONTROL Top Sellers Across the Site]
* [!UICONTROL Top Sellers by Category]
* [!UICONTROL Top Sellers by Item Attribute]
* [!UICONTROL Top by Analytics Metric]

**需要實體引數**

* 如果條件是以目前或專案屬性為基礎，則為`entity.categoryId`或人氣的專案屬性。
* 所有網站中的「檢視次數最多/銷售最高」量度不需傳遞任何專案。

**讀數**

* [以熱門程度為基礎](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.4：以專案為基礎的條件 {#item}

根據找到使用者正在檢視或最近檢視的專案的類似專案提供建議。

+++查看詳細資料

**可用的條件**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**需要實體引數**

* `entity.id`
* 如果有任何設定檔屬性當作索引鍵使用

**讀數**

* 以[專案為基礎](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.5：以使用者為基礎的條件 {#user}

根據使用者的行為提供建議。

+++查看詳細資料

**可用的條件**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**需要實體引數**

* `entity.id`

**讀數**

* [以使用者為基礎](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.6：自訂條件 {#custom}

根據您上傳的自訂檔案提出建議。

+++查看詳細資料

**可用的條件**

* [!UICONTROL Custom algorithm]

**需要實體引數**

`entity.id`或用作自訂演演算法索引鍵的屬性

**讀數**

* [自訂條件](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.7：提供包含規則中使用的屬性 {#inclusion}

+++查看詳細資料

**讀數**

* [使用動態和靜態包含規則](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.8：提供excludedIds {#exclude}

傳遞要從建議中排除之實體的實體ID。 例如，您可以排除已在購物車中的項目。

+++查看詳細資料

**讀數**

* [我可以動態排除實體嗎？](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.9：提供實體屬性以更新[!DNL Recommendations]的產品目錄 {#entity-attributes}

+++查看詳細資料

**讀數**

* [實體屬性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

您也可以使用[!DNL Target] UI建立[產品摘要](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}，以更新[!DNL Recommendations]的產品目錄，進而完成此步驟。

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.10：提供用來作為包含規則索引鍵的設定檔屬性 {#keys}

提供上述任何Recommendations條件中，作為包含規則索引鍵所使用的設定檔屬性。

+++ 檢視詳細資料

**讀數**

* [設定檔屬性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.11：引發頁面載入請求 {#fire}

此步驟會觸發要求中具有`execute` > `pageLoad`裝載的[!DNL Delivery API]呼叫。 `getOffers()`方法會擷取體驗，而`applyOffers()`會轉譯頁面上的體驗。 需要`pageLoad`要求，才能轉譯在[視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC)中編寫的體驗。

+++查看詳細資料

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

+++查看詳細資料

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

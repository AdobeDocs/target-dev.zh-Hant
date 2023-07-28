---
title: 演算體驗
description: 請確定轉譯體驗所需的所有步驟皆以正確順序執行。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 18f070005685699e2d1feb12a31802faa17e35f3
workflow-type: tm+mt
source-wordcount: '1032'
ht-degree: 5%

---

# 演算體驗

請依照以下檔案中的步驟操作： *演算體驗* 圖表可確保呈現體驗所需的所有必要任務都以正確順序執行。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

## 演算體驗圖表 {#diagram}

at.js提供的自動現成閃爍處理功能只有當您具備以下條件時才有意義： [!UICONTROL 自動頁面載入要求] 已啟用。 此選項會隱藏整個HTML內文，同時從擷取體驗 [!DNL Target]. 在此情況下，您有責任處理忽隱忽現的情形。 搜尋可用於處理忽隱忽現情況的實作模式，以取得指引。

下圖中的步驟編號與下列區段相對應。

![演算體驗圖表](/help/dev/patterns/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

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

新增提示的專案並控制其在您的Target Recommendations中的放置位置 [設計](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

+++檢視詳細資料

**可用選項**

* 依ID促銷
* [依集合促銷](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [依屬性促銷](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**需要實體引數**

* 使用「依屬性促銷」選項時，必須傳遞促銷活動中的專案屬性。

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.2：購物車型條件 {#cart}

根據使用者的購物車內容提供建議。

+++檢視詳細資料

**可用條件**

* [!UICONTROL 瀏覽過這些專案、也瀏覽了其他專案的使用者]
* [!UICONTROL 瀏覽過這些專案、但購買了其他專案的使用者]
* [!UICONTROL 購買了此專案、也購買了其他專案的使用者]

**需要實體引數**

* cartIds

**讀數**

* [購物車型](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.3：以人氣為基礎的條件 {#popularity}

根據您網站上的專案整體人氣或使用者最喜愛或檢視次數最多的類別、品牌、型別等內的專案人氣提供建議。

+++檢視詳細資料

**可用條件**

* [!UICONTROL 全網站檢視次數最多]
* [!UICONTROL 依類別檢視次數最多]
* [!UICONTROL 依專案屬性檢視次數最多]
* [!UICONTROL 全網站最暢銷商品]
* [!UICONTROL 依類別排名的最暢銷商品]
* [!UICONTROL 依專案屬性排名的最暢銷商品]
* [!UICONTROL 依Analytics量度排名最前]

**需要實體引數**

* `entity.categoryId` 或「人氣」的「專案屬性」（如果條件是以目前或專案屬性為基礎）。
* 所有網站中檢視次數最多/銷售最多的專案，均不得傳遞任何專案。

**讀數**

* [基於人氣](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.4：以專案為基礎的條件 {#item}

根據找到使用者正在檢視或最近檢視的專案的類似專案提供建議。

+++檢視詳細資料

**可用條件**

* [!UICONTROL 瀏覽過此項目、也瀏覽了其他項目的使用者]
* [!UICONTROL 瀏覽過此項目、但購買了其他項目的使用者]
* [!UICONTROL 購買了此項目、也購買了其他項目的使用者]
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

**可用條件**

* [!UICONTROL 最近查看的項目]
* [!UICONTROL 為您推薦]

**需要實體引數**

* `entity.id`

**讀數**

* [基於使用者](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.6：自訂條件 {#custom}

根據您上傳的自訂檔案提出建議

+++檢視詳細資料

**可用條件**

* [!UICONTROL 自訂演演算法]

**需要實體引數**

`entity.id` 或當作自訂演演算法索引鍵使用的屬性

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

* [是否可以動態地排除實體？ ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.9：提供實體屬性以更新產品目錄 [!DNL Recommendations] {#entity-attributes}

+++檢視詳細資料

**讀數**

* [實體屬性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

您也可以使用建立產品摘要來完成此步驟。 [!DNL Target] 更新產品目錄的UI [!DNL Recommendations].

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

此步驟會觸發 [!DNL Delivery API] 呼叫方式 `execute` > `pageLoad` 要求中的裝載。 此 `getOffers()` 方法擷取體驗和 `applyOffers()` 呈現頁面上的體驗。 呈現在中編寫的體驗需要pageLoad請求 [視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC)。

+++檢視詳細資料

![引發頁面載入請求圖表](/help/dev/patterns/assets/fire-page-load-request.png){width="100" zoomable="yes"}

**必要條件**

* 所有資料對應都必須使用 `targetPageParams` 函式。

**讀數**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**動作**

* 使用 `getOffers` 和 `applyOffers` 使用頁面載入請求API呼叫擷取體驗的方法。

+++

[返回此頁面頂端的圖表。](#diagram)

## 3.12：引發地區位置要求(#location)

此步驟會觸發 [!DNL Delivery API] 呼叫方式 `execute` > `mboxes` 要求中的裝載。 此 `getOffers` 方法擷取體驗和 `applyOffers` 將體驗呈現至頁面。 您可以在「 」下方傳送多個mbox `execute` > `mboxes` 裝載。

+++檢視詳細資料

![引發地區位置請求圖表](/help/dev/patterns/assets/fire-regional-location-request.png){width="100" zoomable="yes"}

**必要條件**

* 所有資料對應都必須使用 `targetPageParams` 函式。

**讀數**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**動作**

* 使用 `getOffers` 和 `applyOffers` 使用頁面載入請求API呼叫擷取體驗的方法。

+++

[返回此頁面頂端的圖表。](#diagram)
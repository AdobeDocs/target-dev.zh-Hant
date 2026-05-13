---
title: 初始化SDK
description: 請確定以正確的順序執行載入 [!DNL Adobe Target] at.js JavaScript程式庫的所有必要步驟。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 250a8382-1fdd-4a70-b712-a25af5adad71
TQID: https://experienceleague.adobe.com/PxAKvxntUCdacBLopvANAI7-8OWe-ELQqFRJu-n3RWo
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
  - id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1797
ht-degree: 4%

---

# 初始化SDK

請依照&#x200B;*初始化SDK*&#x200B;圖表中的步驟操作，以確保載入[!DNL Adobe Target] at.js JavaScript程式庫所需的所有必要工作皆以正確順序執行。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

## 初始化SDK圖表 {#diagram}

對於多頁應用程式，每當頁面重新載入，或訪客導覽至網站上的新頁面時，就會發生此流程。

>[!NOTE]
>
>下圖中的步驟編號與下列區段相對應。 步驟編號沒有特定順序，也不會反映建立活動時在[!DNL Target] UI中採取的步驟順序。

![初始化SDK圖表](/help/dev/patterns/recs-atjs/assets/diagram-initiaze-sdk.png){width="600" zoomable="yes"}

按一下下列連結，導覽至所需的區段：

* [1.1：載入訪客API SDK](#load)
* [1.2：設定客戶ID](#set)
* [1.3：設定自動頁面載入請求](#automatic)
* [1.4：設定忽隱忽現處理](#flicker)
* [1.5：設定資料對應](#data-mapping)
* [1.6：促銷活動](#promotion)
* [1.7：購物車型條件](#cart)
* [1.8：以人氣為準的標準](#popularity)
* [1.9：以專案為基礎的條件](#item)
* [1.10：以使用者為基礎的條件](#user)
* [1.11：自訂條件](#custom)
* [1.12：提供包含規則中使用的屬性](#inclusion)
* [1.13：提供excludedIds](#exclude)
* [1.14：傳遞entity.event.detailsOnly=true引數](#true)
* [1.15：設定遠端資料對應](#remote)
* [1.16：載入at.js](#web)

## 1.1：載入訪客API SDK {#load}

此步驟有助於確保`VisitorAPI.js`程式庫已正確載入、設定和初始化。

+++檢視詳細資料

![載入訪客API SDK圖表](/help/dev/patterns/recs-atjs/assets/load-visitor-combined.png){width="400" zoomable="yes"}

**必要條件**

* 若要使用訪客ID/API服務，貴公司必須啟用[!DNL Adobe Experience Cloud]並擁有[!UICONTROL Organization ID]。 如需詳細資訊，請參閱&#x200B;*身分識別服務說明*&#x200B;指南中的[Experience Cloud需求：組織ID](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html？){target=_blank}。
* 您需要`VisitorAPI.js`檔案。 如果您已實作[!DNL Adobe Analytics]，則應該已有此檔案。 您也可以透過[[!DNL Adobe Experience Platform] 標籤擴充功能](https://experienceleague.adobe.com/docs/tags.html){target=_blank}新增此檔案，或從[Adobe Analytics代碼管理員](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-analytics.html){target=_blank}下載此檔案。

**設定並參考VisitorAPI.js**

如需詳細資訊，請參閱[實作Target的Experience Cloud服務](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html){target=_blank}。

**讀數**

* [Experience Cloud Identity Service概觀](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html){target=_blank}
* [關於ID服務](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html){target=_blank}
* [Cookie 和 Experience Cloud 辨識服務](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html){target=_blank}
* [Experience Cloud Identity服務如何要求與設定ID](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html){target=_blank}
* [瞭解ID同步和匹配率](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html){target=_blank}

**動作**

* 將`VisitorAPI.js`檔案內嵌在您的網頁上。
* 瞭解訪客ID/API服務[&#128279;](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html){target=_blank}的可用設定。
* 載入`VisitorAPI.js`檔案後，請使用`Visitor.getInstance`方法，以您需要的必要設定進行初始化。
* 請熟悉[可用的方法](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html){target=_blank}。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.2：設定客戶ID {#set}

此步驟可協助確保訪客的已知ID （CRM ID、使用者ID等）繫結至[!DNL Adobe]的匿名ID，以進行跨裝置個人化。

+++檢視詳細資料

![設定客戶ID](/help/dev/patterns/recs-atjs/assets/set-customer-id-combined.png){width="400" zoomable="yes"}

**必要條件**

* 訪客的已知ID應在資料層中可用。

**設定客戶識別碼**
如需詳細資訊，請參閱[setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html){target=_blank}。

**讀數**

* [mbox3rdPartyId 的即時輪廓同步](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html){target=_blank}

**動作**

* 使用`visitor.setCustomerIDs`設定訪客已知識別碼。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.3：設定自動頁面載入請求 {#automatic}

此步驟可讓at.js擷取載入at.js JavaScript程式庫檔案時必須在頁面上呈現的所有體驗。

+++檢視詳細資料

![設定自動頁面載入要求](/help/dev/patterns/recs-atjs/assets/configure-automatic-page-request-combined.png){width="400" zoomable="yes"}

**必要條件**

* 並非資料層中的所有資料都必須傳送至[!DNL Target]。 請洽詢您的業務團隊（數位行銷團隊），判斷哪些資料對於實驗、最佳化和個人化很有價值。 只有此資料應傳送至[!DNL Target]。
* 確保您不會將任何個人識別資訊(PII)資料傳送至[!DNL Target]。

**設定自動頁面載入要求**

如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。

**讀數**

瞭解[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)中的`pageLoadEnabled`設定。

**動作**

* 修改`window.targetGlobalSettings`物件以啟用自動頁面載入要求。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.4：設定忽隱忽現處理 {#flicker}

此步驟有助於確保在提供體驗時沒有頁面閃爍。

+++檢視詳細資料

![設定忽隱忽現的處理圖](/help/dev/patterns/recs-atjs/assets/flicker-handling-combined.png){width="400" zoomable="yes"}

**必要條件**

* 與負責網頁效能的團隊討論使用at.js使用的預設方法控制忽隱忽現的利弊。 您可以搜尋可讓您使用自訂忽隱忽現處理解決方案的設計模式，例如載入器動畫。 如果您找不到模式，可以請求新的模式。

**設定忽隱忽現處理**

如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。

將`bodyHidingEnabled`設定為`true`會在頁面載入要求進行中時隱藏整個頁面內文。 如果您因任何原因（例如稍後資料尚未就緒）未啟用自動頁面載入要求，最好將此設定設為`false`。

如果您已停用`bodyHidingEnabled`，因為您不想引發APLR且想要稍後引發頁面要求，或您不需要忽隱忽現處理，則必須實作自己的忽隱忽現處理。 處理忽隱忽現的兩種方式：隱藏測試區段，或在測試區段上顯示引發器。

**讀數**

* [At.js 處理忽隱忽現情況的方式](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* 瞭解[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)中的bodyHiddenStyle和bodyHidingEnabled物件。

**動作**

* 修改`window.targetGlobalSettings`物件以設定`bodyHiddenStyle`和`bodyHidingEnabled`。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.5：設定資料對應 {#data-mapping}

此步驟有助於確保已設定必須傳送至[!DNL Target]的所有資料。

+++檢視詳細資料

![資料對應圖表](/help/dev/patterns/recs-atjs/assets/data-mapping-combined.png){width="400" zoomable="yes"}

**必要條件**

* 資料層應準備好所有必須傳送給[!DNL Target]的資料。
* Recommendations：豐富設定檔。
   * 傳遞`entity.id`以根據根據上次檢視產品的條件，擷取最近檢視條件與專案的資料。
   * 傳遞`entity.id`以根據最喜愛的類別來擷取熱門度條件的資料。
   * 如果自訂條件以該設定檔屬性為基礎，或用於任何條件中的包含規則篩選，請傳遞該設定檔屬性。
* 建議：擷取產品資料。
   * 其他實體引數（保留和自訂）可傳遞以擷取或更新[!DNL Recommendations]中的產品目錄。
   * 也可以使用[!DNL Target] UI或API的實體摘要來更新產品目錄。

**將資料對應至[!DNL Target]**

如需詳細資訊，請參閱[targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)。

**讀數**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [計劃和實作推薦](/help/dev/implement/recommendations/recommendations.md)
* [設定您的建議目錄](/help/dev/implement/recommendations/recommendations.md)

**動作**

* 使用`targetPageParams()`函式設定必須傳送至[!DNL Target]的所有必要資料。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.6：促銷活動 {#promotion}

新增提示的專案並控制它們在您[!DNL Target Recommendations] [設計](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}中的位置。

+++檢視詳細資料

**可用選項**

* 依ID促銷
* [依集合促銷](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [依屬性促銷](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**需要實體引數**

* 使用「依屬性促銷」選項時，必須傳遞促銷中的專案屬性。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.7：購物車型條件 {#cart}

根據使用者的購物車內容提供建議。

+++檢視詳細資料

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

## 1.8：以人氣為準的標準 {#popularity}

根據您網站上的專案整體人氣或使用者最喜愛或檢視次數最多的類別、品牌、型別等內的專案人氣提供建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL Most Viewed Across the Site]
* [!UICONTROL Most Viewed by Category]
* [!UICONTROL Most Viewed by Item Attribute]
* [!UICONTROL Top Sellers Across the Site]
* [!UICONTROL Top Sellers by Category]
* [!UICONTROL Top Sellers by Item Attribute]
* [!UICONTROL Top by Analytics Metric]

**需要實體引數**

* 如果條件是以目前專案或專案屬性為基礎，則為`entity.categoryId`或人氣的專案屬性。
* 所有網站中檢視次數最多/銷售最多的專案，均不得傳遞任何專案。

**讀數**

* [基於人氣](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.9：以專案為基礎的條件 {#item}

根據找到使用者正在檢視或最近檢視的專案的類似專案提供建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**需要實體引數**

* `entity.id`或任何用作索引鍵的設定檔屬性

**讀數**

* [基於專案](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.10：以使用者為基礎的條件 {#user}

根據使用者的行為提供建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**需要實體引數**

* `entity.id`

**讀數**

* [基於使用者](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.11：自訂條件 {#custom}

根據您上傳的自訂檔案提出建議。

+++檢視詳細資料

**可用的條件**

* [!UICONTROL Custom algorithm]

**需要實體引數**

`entity.id`或用作自訂演演算法索引鍵的屬性

**讀數**

* [自訂條件](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.12：提供包含規則中使用的屬性 {#inclusion}

+++檢視詳細資料

**讀數**

* [使用動態和靜態包含規則](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.13：提供excludedIds {#exclude}

傳遞要從建議中排除之實體的實體ID。 例如，您可以排除已在購物車中的項目。

+++檢視詳細資料

**讀數**

* [是否可以動態地排除實體?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.14：傳遞`entity.event.detailsOnly=true`引數 {#true}

使用實體屬性將產品或內容資訊傳遞至[!DNL Target Recommendations]。

+++檢視詳細資料

**讀數**

* [實體屬性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=en){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.15：設定遠端資料對應（遠端）

此步驟可確保已設定所有必須傳送至[!DNL Target]的資料。

+++檢視詳細資料

![遠端資料對應圖表](/help/dev/patterns/recs-atjs/assets/remote-data-mapping-combined.png){width="400" zoomable="yes"}

**必要條件**

* 資料層應準備好所有必須傳送給[!DNL Target]的資料。

**設定資料提供者**

如需詳細資訊，請參閱[資料提供者](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)。

**讀數**

[targetPageParams函式](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**動作**

使用`targetPageParams()`函式設定必須傳送至[!DNL Target]的所有必要資料。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.16：載入at.js {#web}

此步驟會確保載入及初始化at.js JavaScript程式庫。

+++檢視詳細資料

![載入Adobe Target at.js圖表](/help/dev/patterns/recs-atjs/assets/load-atjs-combined.png){width="400" zoomable="yes"}

**必要條件**

* 下載或要求您的數位行銷團隊取得`at.js 2.*x*` JavaScript程式庫檔案。

*讀數*

* [Target 的運作方式](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html){target=_blank}
* [at.js 如何運作](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [不使用標籤管理程式實作 Target](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**動作**

將at.js檔案內嵌於所有必須實驗、最佳化、個人化和資料收集的網頁上。

+++

[返回此頁面頂端的圖表。](#diagram)

繼續步驟2：[設定資料彙集](/help/dev/patterns/recs-atjs/data-collection.md)。

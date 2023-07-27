---
title: 初始化SDK
description: 請確定以正確順序執行載入Adobe Experience Platform Web SDK的所有必要步驟。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: ca23e3d9a94d0c7b746971b10dae96e1141ef93f
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 6%

---

# 初始化SDK

請依照以下檔案中的步驟操作： *初始化SDK* 確保載入 [!DNL Adobe Target] at.js JavaScript程式庫會以正確的順序執行。

>[!TIP]
>
>按一下此主題中的影像可展開至全熒幕。

## 初始化SDK圖表 {#diagram}

對於多頁應用程式，每當頁面重新載入，或訪客導覽至網站上的新頁面時，就會發生此流程。

下圖中的步驟編號與下列區段相對應。

![初始化SDK圖表](/help/dev/patterns/assets/diagram-initiaze-sdk.png){width="600" zoomable="yes"}

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

此步驟有助於確保 `VisitorAPI.js` 程式庫已正確載入、設定和初始化。

+++檢視詳細資料

![載入訪客API SDK圖表](/help/dev/patterns/assets/load-visitor-api-sdk.png){width="100" zoomable="yes"}

**必要條件**

* 若要使用訪客ID/API服務，貴公司必須啟用 [!DNL Adobe Experience Cloud] 並擁有 [!UICONTROL 組織ID]. 如需詳細資訊，請參閱 [Experience Cloud需求：組織識別碼](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?){target=_blank} 在 *Identity Service說明* 指南。
* 您需要 `VisitorAPI.js` 檔案。 請洽詢您的數位行銷團隊以取得此檔案。

**設定並參考VisitorAPI.js**

如需詳細資訊，請參閱 [實作適用於Target的Experience Cloud服務](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html){target=_blank}.

**讀數**

* [Experience CloudIdentity Service總覽](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html){target=_blank}
* [關於ID服務](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html){target=_blank}
* [Cookie 和 Experience Cloud 辨識服務](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html){target=_blank}
* [Experience CloudIdentity服務如何要求與設定ID](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html){target=_blank}
* [瞭解ID同步和匹配率](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html){target=_blank}

**動作**

* 內嵌 `VisitorAPI.js` 檔案。
* 閱讀關於 [訪客ID/API服務的可用設定](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html){target=_blank}.
* 在 `VisitorAPI.js` 載入檔案，請使用 `Visitor.getInstance` 方法，以使用您需要的必要設定進行初始化。
* 請熟悉 [可用方法](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html){target=_blank}.

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.2：設定客戶ID {#set}

此步驟有助於確保訪客的已知ID （CRM ID、使用者ID等）繫結至的匿名ID [!DNL Adobe] 適用於跨裝置個人化。

+++檢視詳細資料

![設定客戶ID](/help/dev/patterns/assets/set-customer-id.png){width="100" zoomable="yes"}

**必要條件**

* 訪客的已知ID應在資料層中可用。

**設定客戶ID**
如需詳細資訊，請參閱 [setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html){target=_blank}.

**讀數**

* [mbox3rdPartyId 的即時個人資料同步](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html){target=_blank}

**動作**

* 使用 `visitor.setCustomerIDs` 以設定訪客已知ID。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.3：設定自動頁面載入請求 {#automatic}

此步驟可讓at.js擷取載入at.js JavaScript程式庫檔案時必須在頁面上呈現的所有體驗。

+++檢視詳細資料

![設定自動頁面載入請求](/help/dev/patterns/assets/configure-automatic-page-request.png){width="100" zoomable="yes"}

**必要條件**

* 並非資料層中的所有資料都必須傳送到 [!DNL Target]. 請洽詢您的業務團隊（數位行銷團隊），判斷哪些資料對於實驗、最佳化和個人化很有價值。 僅此資料應傳送至 [!DNL Target].
* 請勿將任何個人識別資訊(PII)資料傳送至 [!DNL Target].

**設定自動頁面載入請求**

如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。

**讀數**

瞭解 `pageLoadEnabled` 設定於 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**動作**

* 修改 `window.targetGlobalSettings` 物件以啟用自動頁面載入請求。

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.4：設定忽隱忽現處理 {#flicker}

此步驟有助於確保在提供體驗時沒有頁面閃爍。

+++檢視詳細資料

![設定忽隱忽現的處理圖](/help/dev/patterns/assets/flicker-handling.png){width="100" zoomable="yes"}

**必要條件**

* 與負責網頁效能的團隊討論使用at.js使用的預設方法控制忽隱忽現的利弊。 您可以搜尋可讓您使用自訂忽隱忽現處理解決方案的設計模式，例如載入器動畫。 如果您找不到模式，可以請求新的模式。

**設定忽隱忽現處理**

如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。

設定 `bodyHidingEnabled` 至 `true` 在頁面載入請求進行中時隱藏整個頁面內文。 如果您因任何原因（例如稍後資料尚未準備就緒）未啟用自動頁面載入請求，最好將此設定設為 `false`.

如果您已停用 `bodyHidingEnabled` 因為您不想引發APLR且想要稍後引發頁面要求，或不需要忽隱忽現的處理，所以您必須實作自己的忽隱忽現處理。 處理忽隱忽現的兩種方式：隱藏測試區段，或在測試區段上顯示引發器。

**讀數**

* [At.js 處理忽隱忽現情況的方式](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* 瞭解中的bodyHiddenStyle和bodyHidingEnabled物件 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**動作**

* 修改 `window.targetGlobalSettings` 要設定的物件 `bodyHiddenStyle` 和 `bodyHidingEnabled`.

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.5：設定資料對應 {#data-mapping}

此步驟有助於確保必須傳送至的所有資料 [!DNL Target] 已設定。

+++檢視詳細資料

![資料對應圖表](/help/dev/patterns/assets/data-mapping.png){width="100" zoomable="yes"}

**必要條件**

* 資料層應準備好所有必須傳送至的資料 [!DNL Target].
* Recommendations：擴充設定檔。
   * 通過 `entity.id` 根據根據根據上次檢視產品的條件，擷取最近檢視條件和專案的資料。
   * 通過 `entity.id` 根據最喜愛的類別來擷取熱門度條件的資料。
   * 如果自訂條件以該設定檔屬性為基礎，或用於任何條件中的包含規則篩選，請傳遞該設定檔屬性。
* Recommendations：擷取產品資料。
   * 其他實體引數（保留和自訂）可傳遞以內嵌或更新中的產品目錄 [!DNL Recommendations].
   * 也可以使用實體摘要來更新產品目錄，請使用 [!DNL Target] UI或API。

**將資料對應至[!DNL Target]**

如需詳細資訊，請參閱 [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**讀數**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [計劃和實作 Recommendations](/help/dev/implement/recommendations/recommendations.md)
* [設定您的Recommendations目錄](/help/dev/implement/recommendations/recommendations.md)

**動作**

* 使用 `targetPageParams()` 函式，可設定必須傳送至的所有必要資料 [!DNL Target].

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.6：促銷活動 {#promotion}

新增提示的專案並控制其在您的中的位置 [!DNL Target Recommendations] [設計](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

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

## 1.8：以人氣為準的標準 {#popularity}

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

* `entity.categoryId` 或「基於人氣」的「專案屬性」（如果條件是以目前專案或專案屬性為基礎）。
* 所有網站中檢視次數最多/銷售最多的專案，均不得傳遞任何專案。

**讀數**

* [基於人氣](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.9：以專案為基礎的條件 {#item}

根據找到使用者正在檢視或最近檢視的專案的類似專案提供建議。

+++檢視詳細資料

**可用條件**

* [!UICONTROL 瀏覽過此項目、也瀏覽了其他項目的使用者]
* [!UICONTROL 瀏覽過此項目、但購買了其他項目的使用者]
* [!UICONTROL 購買了此項目、也購買了其他項目的使用者]
* [!UICONTROL 具有類似屬性的專案]

**需要實體引數**

* `entity.id` 或任何用作索引鍵的設定檔屬性

**讀數**

* [基於專案](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.10：以使用者為基礎的條件 {#user}

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

## 1.11：自訂條件 {#custom}

根據您上傳的自訂檔案提出建議。

+++檢視詳細資料

**可用條件**

* [!UICONTROL 自訂演演算法]

**需要實體引數**

`entity.id` 或當作自訂演演算法索引鍵使用的屬性

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

* [是否可以動態地排除實體？ ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.14：傳遞 `entity.event.detailsOnly=true` 引數 {#true}

使用實體屬性來傳遞產品或內容資訊至 [!DNL Target Recommendations].

+++檢視詳細資料

**讀數**

* [實體屬性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=en){target=_blank}

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.15：設定遠端資料對應（遠端）

此步驟可確保必須傳送至以下位置的所有資料： [!DNL Target] 已設定。

+++檢視詳細資料

![遠端資料對應圖表](/help/dev/patterns/assets/remote-data-mapping.png){width="100" zoomable="yes"}

**必要條件**

* 資料層應準備好接收所有必須傳送到的資料 [!DNL Target].

**設定資料提供者**

如需詳細資訊，請參閱 [資料提供者](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

**讀數**

[targetPageParams函式](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**動作**

使用 `targetPageParams()` 函式，可設定必須傳送至的所有必要資料 [!DNL Target].

+++

[返回此頁面頂端的圖表。](#diagram)

## 1.16：載入at.js {#web}

此步驟會確保載入及初始化at.js JavaScript程式庫。

+++檢視詳細資料

![載入Adobe Target Web SDK圖表](/help/dev/patterns/assets/load-web-sdk.png){width="100" zoomable="yes"}

**必要條件**

* 下載或要求數位行銷團隊取得Web SDK程式庫檔案： `at.js 2.*x*`

*讀數*

* [Target 的運作方式](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html){target=_blank}
* [at.js 如何運作](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [不使用標籤管理程式實作 Target](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**動作**

將at.js檔案內嵌於所有必須實驗、最佳化、個人化和資料收集的網頁上。

+++

[返回此頁面頂端的圖表。](#diagram)






























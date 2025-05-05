---
keywords: 實作， javascript程式庫， js， atjs，裝置上決策，裝置上決策， at.js，裝置上，裝置上，實作0
description: 瞭解如何使用at.js程式庫執行[!UICONTROL on-device decisioning]
title: 裝置上決策如何與at.js JavaScript程式庫搭配運作？
feature: at.js
exl-id: bd0e062f-c259-46f3-adba-e380af058ac8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '3478'
ht-degree: 4%

---

# 適用於at.js的[!UICONTROL On-device decisioning]

從2.5.0版開始，at.js提供[!UICONTROL on-device decisioning]。 [!UICONTROL On-device decisioning]可讓您在瀏覽器上快取[A/B測試](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=zh-Hant)和[體驗鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=zh-Hant) (XT)活動，以執行記憶體內部決策，而不會封鎖對[!DNL Adobe Target]Edge Network的網路要求。

>[!NOTE]
>
>[!UICONTROL On-device decisioning]可用於使用者端及伺服器端實作。 本文說明使用者端的[!UICONTROL on-device decisioning]。 如需伺服器端[!UICONTROL on-device decisioning]的相關資訊，請參閱伺服器端實作檔案[這裡](../../../server-side/sdk-guides/on-device-decisioning/overview.md)。

[!DNL Target]也能透過即時伺服器呼叫，靈活地從實驗和機器學習驅動的（ML驅動的）個人化活動中提供最相關和最新的體驗。 換句話說，當效能最重要時，您可以選擇使用[!UICONTROL on-device decisioning]。 但是，當需要最相關、最新和ML導向的體驗時，可以改為進行伺服器呼叫。

## [!UICONTROL on-device decisioning]有哪些優點？

[!UICONTROL on-device decisioning]的優點包括：

* **提供超快的決策和體驗。**&#x200B;在記憶體中及瀏覽器上執行分組和決策，以避免封鎖網路要求。
* **增強應用程式效能。**&#x200B;執行實驗並向您的客戶和使用者提供個人化，而不會損害一般使用者體驗。
* **提升Google網站品質分數。**&#x200B;由於決策是在記憶體中進行，請改善您線上業務的Google網站品質分數，讓消費者更容易發現它。
* **向即時分析學習。**&#x200B;透過[Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=zh-Hant) (A4T)報告即時取得您活動績效的深入分析。 A4T可讓您在關鍵時刻樞紐分析策略。

## 支援的功能

[!DNL Adobe Target] JS SDK讓客戶可靈活選擇資料的效能與最新狀態，以便做出決策。 換言之，如果透過機器學習提供最相關且最吸引人的個人化內容對您而言至關重要，則應進行即時伺服器呼叫。 但是，當效能較為重要時，就應該做出裝置上及記憶體中的決策。 若要讓[!UICONTROL on-device decisioning]運作，請參閱支援的功能清單：

* 活動類型
* 對象目標定位
* 配置方法

如需詳細資訊，請參閱[!UICONTROL on-device decisioning][&#128279;](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md)的支援功能。

## [!UICONTROL on-device decisioning]如何運作？

當您在啟用[!UICONTROL on-device decisioning]的情況下部署及初始化at.js時，系統會從最接近訪客的Akamai CDN下載[規則成品](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) （其中包含您的A/B和XT活動、對象及資產的[!UICONTROL on-device decisioning]），並在訪客的瀏覽器上在本機快取。 當從at.js提出擷取體驗的請求時，會根據快取規則成品中編碼的中繼資料，在記憶體中做出有關要傳回哪個體驗的決定。

## 決策方法

透過[!UICONTROL on-device decisioning]，[!DNL Target]引入名為Decisioning方法的新設定。 決策方法設定會指定at.js提供您體驗的方式。 決策方法有三個值：

* 僅限伺服器端
* 僅限裝置上
* 混合式

### 僅限伺服器端

僅伺服器端是預設的決策方法，可在您的Web屬性上實作和部署at.js 2.5.0+時立即使用。

僅使用伺服器端作為預設設定，表示所有決定都是在[!DNL Target]邊緣網路上做出，其中涉及封鎖伺服器呼叫。 此方法可增加延遲時間，但也有顯著的優點，例如可讓您套用[!DNL Target]的機器學習功能，包括[Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=zh-Hant)、[Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=zh-Hant) (AP)和[自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=zh-Hant)活動。

此外，使用[!DNL Target]的使用者設定檔來增強您的個人化體驗（此設定檔會跨工作階段和管道儲存），可為您的業務提供強大的成果。

最後，伺服器端僅可讓您使用Adobe Experience Cloud，並微調可透過Audience Manager和Adobe Analytics區段鎖定的對象。

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和[!DNL Adobe Target] Edge網路之間的互動情形。 此流程圖表會擷取新訪客和回訪訪客。

（按一下影像可展開至完整寬度。）

![伺服器端僅限流量圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/server-side-only.png "伺服器端僅限流量圖"){zoomable="yes"}

下列清單與圖表中的數字相對應：

| 步驟 | 說明 |
| --- | --- |
| 1 | 已從[Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=zh-Hant&)擷取Experience Cloud的訪客ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />   也能使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js資料庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | 提出頁面載入請求，包含所有已設定的引數，例如（ECID、客戶ID、自訂引數、使用者設定檔等）。 |
| 5 | 個人資料指令碼執行，然後注入個人資料存放區。<br />設定檔存放區會從對象資料庫中要求合格對象(例如從Adobe Analytics、Adobe Audience Manager等共用的對象)。<br />客戶屬性會透過批次程序傳送至個人資料存放區。 |
| 6 | 設定檔存放區用於對象資格和分組以篩選活動。 |
| 7 | 從已上線[!DNL Target]活動中決定體驗之後，就會選取產生的內容。 |
| 8 | at.js資料庫會隱藏頁面上與必須轉譯之體驗相關聯的對應元素。 |
| 9 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 10 | at.js資料庫會操控DOM來轉譯[!DNL Target]Edge Network中的體驗。 |
| 11 | 體驗會針對訪客呈現。 |
| 12 | 載入整個網頁。 |
| 13 | Analytics 資料傳送至「資料收集」伺服器。 |
| 14 | 目標資料會透過SDID來比對Analytics資料，然後經過處理放入Analytics報表儲存體中。 然後就可以在Analytics和[!DNL Target]中，透過[!UICONTROL Analytics for Target] (A4T)報表來檢視Analytics資料。 |

### 僅限裝置上

僅限裝置上決策方法必須設定在at.js 2.5.0+中，而[!UICONTROL on-device decisioning]只應在您的網頁中使用。

[!UICONTROL On-device decisioning]能以極快的速度提供您的體驗和個人化活動，因為決定是由包含您所有符合[!UICONTROL on-device decisioning]資格的活動的快取規則成品所做。

若要進一步瞭解哪些活動符合[!UICONTROL on-device decisioning]的資格，請參閱[!UICONTROL on-device decisioning][&#128279;](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md)中的支援功能。

只有在需要Target做出決定的所有頁面中，效能高度關鍵時，才應使用此決策方法。 此外，請記住，選取此決策方法時，不會傳遞或執行您不符合[!UICONTROL on-device decisioning]資格的[!DNL Target]活動。 at.js資料庫2.5.0+已設定為僅尋找快取規則成品以做出決策。

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和Akamai CDN之間的互動情形。 Akamai CDN會在訪客首次造訪時快取規則成品。 新訪客第一次造訪頁面時，必須從Akamai CDN下載JSON規則成品，才能在訪客的瀏覽器上在本機快取。 下載JSON規則成品後，會立即作出決定，而不會封鎖網路呼叫。 以下流程圖會擷取新訪客。

（按一下影像可展開至完整寬度。）

![僅限裝置上流量圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only.png "僅限裝置上流量圖"){zoomable="yes"}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target]個管理員伺服器可讓您符合[!UICONTROL on-device decisioning]資格的所有活動合格，產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 已從[Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=zh-Hant)擷取Experience Cloud的訪客ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也可以使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js程式庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | at.js程式庫會要求從最接近訪客的Akamai CDN擷取JSON規則成品。 |
| 5 | Akamai CDN會以JSON規則成品回應。 |
| 6 | JSON規則成品會快取至訪客瀏覽器的本機。 |
| 7 | at.js程式庫會解譯JSON規則成品並執行決定以擷取體驗並隱藏測試的元素。 |
| 8 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 9 | at.js程式庫會操控DOM來轉譯快取JSON規則成品中的體驗。 |
| 10 | 體驗會針對訪客呈現。 |
| 11 | 載入整個網頁。 |
| 12 | Analytics資料傳送至「資料收集」伺服器。 目標資料會透過SDID來比對Analytics資料，然後經過處理放入Analytics報表儲存體中。 然後就可以在Analytics和[!DNL Target]中，透過[!UICONTROL Analytics for Target] (A4T)報表來檢視Analytics資料。 |

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和快取JSON規則成品之間的互動，以供訪客的後續頁面點選或回訪使用。 由於JSON規則成品已快取並在瀏覽器上可用，因此會立即做出決定而不會封鎖網路呼叫。 此流程圖會擷取後續頁面導覽或回訪訪客。

（按一下影像可展開至完整寬度。）

![後續頁面導覽和重複造訪的僅限裝置上流程圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only-subsequent.png "後續頁面導覽和重複造訪的僅限裝置上流程圖"){zoomable="yes"}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target]個管理員伺服器可讓您符合[!UICONTROL on-device decisioning]資格的所有活動合格，產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 已從[Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=zh-Hant)擷取Experience Cloud的訪客ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也可以使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js程式庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | at.js程式庫會解譯JSON規則成品並在記憶體中執行決定以擷取體驗。 |
| 5 | 已測試的元素會隱藏。 |
| 6 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 7 | at.js程式庫會操控DOM來轉譯快取JSON規則成品中的體驗。 |
| 8 | 體驗會針對訪客呈現。 |
| 9 | 載入整個網頁。 |
| 10 | Analytics資料傳送至「資料收集」伺服器。 目標資料會透過SDID來比對Analytics資料，然後經過處理放入Analytics報表儲存體中。 然後就可以在Analytics和[!DNL Target]中，透過[!UICONTROL Analytics for Target] (A4T)報表來檢視Analytics資料。 |

### 混合式

混合式是必須在at.js 2.5.0+中設定的決策方法，當[!UICONTROL on-device decisioning]和需要對[!DNL Adobe Target] Edge網路進行網路呼叫的活動都必須執行時。

當您同時管理[!UICONTROL on-device decisioning]活動和伺服器端活動時，考慮如何在您的頁面上部署和布建[!DNL Target]可能會有點複雜和繁瑣。 使用混合式作為決策方法，[!DNL Target]知道何時必須對[!DNL Adobe Target] Edge網路進行伺服器呼叫，以進行需要伺服器端執行的活動，以及何時僅執行裝置上決策。

JSON規則成品包含中繼資料，以通知at.js mbox是否正在執行伺服器端活動或[!UICONTROL on-device decisioning]活動。 此決定方法可確保您要快速傳送的活動是透過[!UICONTROL on-device decisioning]完成，而針對需要更強大ML驅動個人化的活動，這些活動是透過[!DNL Adobe Target] Edge網路完成。

下圖說明對於首次造訪您頁面的新訪客，您的訪客、瀏覽器、at.js 2.5.0+、Akamai CDN與[!DNL Adobe Target]Edge Network之間的互動情形。 此圖表的摘要在於，透過[!DNL Adobe Target] Edge網路進行決定時，JSON規則成品是以非同步方式下載。

此方法可確保成品（可包含許多活動）的大小不會對決策的延遲產生負面影響。 同步下載JSON規則成品並在之後進行決策也可能會對延遲產生不利影響且可能不一致。 因此，混合決定方法是一種最佳實務建議，可一律為新訪客的決定發出伺服器端呼叫，而且會並行快取JSON規則成品。 對於任何後續的頁面造訪和回訪，決策是透過JSON規則成品從快取和記憶體中做出的。

（按一下影像可展開至完整寬度。）

![首次訪客的混合流程圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-first-time-visitor.png "首次訪客的混合流程圖"){zoomable="yes"}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target]個管理員伺服器可讓您符合[!UICONTROL on-device decisioning]資格的所有活動合格，產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 已從[Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=zh-Hant)擷取Experience Cloud的訪客ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也可以使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js程式庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | 向[!DNL Adobe Target]Edge Network提出頁面載入請求，包括已設定的所有引數，例如（ECID、客戶ID、自訂引數、使用者設定檔等）。 |
| 5 | 同時，at.js會要求從最接近訪客的Akamai CDN擷取JSON規則成品。 |
| 6 | ([!DNL Adobe Target]個Edge Network)設定檔指令碼執行，然後注入設定檔存放區。 設定檔存放區會從對象資料庫中要求合格對象(例如，從Adobe Analytics、Adobe Audience Manager等共用的對象)。 |
| 7 | Akamai CDN會以JSON規則成品回應。 |
| 8 | 設定檔存放區用於對象資格和分組以篩選活動。 |
| 9 | 從已上線[!DNL Target]活動中決定體驗之後，就會選取產生的內容。 |
| 10 | at.js資料庫會隱藏頁面上與必須轉譯之體驗相關聯的對應元素。 |
| 11 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 12 | at.js資料庫會操控DOM來轉譯[!DNL Target]Edge Network中的體驗。 |
| 13 | 體驗會針對訪客呈現。 |
| 14 | 載入整個網頁。 |
| 15 | Analytics資料傳送至「資料收集」伺服器。 目標資料會透過SDID來比對Analytics資料，然後經過處理放入Analytics報表儲存體中。 然後就可以在Analytics和[!DNL Target]中，透過[!UICONTROL Analytics for Target] (A4T)報表來檢視Analytics資料。 |

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和快取JSON規則成品之間的互動，以供後續頁面導覽或回訪使用。 在此圖表中，僅著重於針對後續頁面導覽或回訪而做出裝置上決策的使用案例。 請記住，根據特定頁面中啟用的活動，可能會進行伺服器端呼叫以執行伺服器端決策。

（按一下影像可展開至完整寬度。）

![後續頁面導覽和重複造訪的混合流量圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-subsequent.png "後續頁面導覽和重複造訪的混合流量圖"){zoomable="yes"}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target]個管理員伺服器可讓您符合[!UICONTROL on-device decisioning]資格的所有活動合格，產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 已從[Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=zh-Hant)擷取Experience Cloud的訪客ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也可以使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js程式庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | 系統會要求擷取體驗。 |
| 5 | at.js程式庫會確認JSON規則成品已快取，並在記憶體中執行決定以擷取體驗。 |
| 6 | 已測試的元素會隱藏。 |
| 7 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 8 | at.js程式庫會操控DOM來轉譯快取JSON規則成品中的體驗。 |
| 9 | 體驗會針對訪客呈現。 |
| 10 | 載入整個網頁。 |
| 11 | Analytics資料傳送至「資料收集」伺服器。 目標資料會透過SDID來比對Analytics資料，然後經過處理放入Analytics報表儲存體中。 然後就可以在Analytics和[!DNL Target]中，透過[!UICONTROL Analytics for Target] (A4T)報表來檢視Analytics資料。 |

## 如何啟用[!UICONTROL on-device decisioning]？

[!UICONTROL On-device decisioning]適用於使用At.js 2.5.0+的所有[!DNL Target]客戶。

若要啟用[!UICONTROL on-device decisioning]：

>[!NOTE]
>
>您必須擁有管理員或核准者[使用者角色](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html?lang=zh-Hant)，才能啟用或停用「裝置上決策」切換。

1. 按一下&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]**。
1. 在&#x200B;**[!UICONTROL Account details]**&#x200B;下方，將&#x200B;**[!UICONTROL On-Device Decisioning]**&#x200B;切換滑至「開啟」位置。

   ![[!UICONTROL On-device decisioning]切換](assets/on-device-decisioning-toggle.png)

   如果您啟用[!UICONTROL on-device decisioning]，則會顯示「將所有現有的[!UICONTROL on-device decisioning]合格活動包含在成品中」選項。
1. （視條件而定）如果您希望所有符合[!UICONTROL on-device decisioning]資格的即時[!DNL Target]活動自動納入成品中，請將切換滑至「開啟」位置。

   若將此切換保持關閉，表示您必須重新建立並啟動任何[!UICONTROL on-device decisioning]活動，才能將其包含在產生的規則成品中。 換言之，在開啟裝置上決策切換開關之前處於即時狀態的任何活動都不會納入規則成品中。

啟用「裝置上決策」切換後，[!DNL Target]會開始為您的使用者端產生和傳播[規則成品](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md)。

>[!WARNING]
>
>在初始化[!DNL Adobe Target] SDK以使用[!UICONTROL on-device decisioning]之前，請務必啟用切換功能。 規則成品必須先產生並傳播至Akamai CDN，[!UICONTROL on-device decisioning]才能運作。 第一個規則成品可能需要五到十分鐘的時間才會產生並傳播至Akamai CDN。

## 如何設定at.js 2.5.0+使用[!UICONTROL on-device decisioning]？

1. 按一下&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]**。
1. 在&#x200B;**[!UICONTROL Implementation Methods]** > **[!UICONTROL Main Implementation Method]**&#x200B;下方，按一下您的at.js版本（必須是at.js 2.5.0或更新版本）旁的&#x200B;**[!UICONTROL Edit]**。

   ![編輯主要實作方法設定](assets/main-implementation-method.png)

   >[!WARNING]
   >
   >在變更這些預設設定之前，請洽詢Client Care，以避免影響您目前的實施。

1. 選取所需的決策方法：

   * 僅限伺服器端
   * 僅限裝置上
   * 混合式

   ![編輯at.js設定面板](assets/global-settings.png)

### 全域設定

您可以為所有[!DNL Target]個決定設定預設決定方法。 各種決策方法是僅限伺服器端、僅限裝置上及混合。 在[!DNL Target] UI中選取的決策方法已在`decisioningMethod`欄位下的`window.targetGlobalSettings`中設定。 深入瞭解[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#decisioningmethod)中的`decisioningMethod`。

```javascript {line-numbers="true"}
<head> 
    <script type="text/javascript">

        window.targetGlobalSettings = { 
            clientCode: "yourClientCodeHere", 
            imsOrgId: "imsOrgId@AdobeOrg", 
            decisioningMethod: "on-device"

        }; 
    </script>

    <script type="text/javascript" src="at.js"></script> 
</head>
```

### 自訂設定

如果您在`window.targetGlobalSettings`中設定`decisioningMethod`，但想要根據您的使用案例覆寫每個[!DNL Adobe Target]決定的`decisioningMethod`，您可以在At.js2.5.0+的[getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)呼叫中指定`decisioningMethod`來執行此程式。

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
});
```

>[!NOTE]
>
>若要在getOffers()呼叫中使用「裝置上」或「混合」作為決策方法，請確定全域設定的「`decisioningMethod`」為「裝置上」或「混合」。 at.js資料庫2.5.0+必須知道是否在頁面上載入後立即下載及快取JSON規則成品。 如果全域設定的決策方法設為「伺服器端」，且「裝置上」或「混合」決策方法已傳遞至getOffers()呼叫，則at.js 2.5.0+不會快取JSON規則成品以執行您的裝置上決策。

### 成品快取TTL

Target代表您符合[!UICONTROL on-device decisioning]資格的活動，可作為包含中繼資料、規則和條件的成品。 會在Akamai CDN上快取此成品。 在使用者第一次造訪期間，使用者的瀏覽器會下載並快取代表您[!UICONTROL on-device decisioning]活動的成品。

後續造訪您的網站時，瀏覽器會自動檢查是否必須下載較新版本的成品。 這項檢查會增加延遲。 成品快取TTL會定義自上次成功下載以來，您不希望瀏覽器檢查更新成品的分鐘數。 時間範圍越長，效能就越好。 時間範圍越短，資料的時效性就越好，但代價是延遲時間增加。

## 我如何知道活動符合[!UICONTROL on-device decisioning]資格？

建立符合[!UICONTROL on-device decisioning]資格的活動後，活動的「概覽」頁面會顯示讀取「裝置上決策」資格的標籤。

![裝置上決策活動概觀頁面上的合格標籤。](assets/on-device-decisioning-eligible-label.png)

此標籤並不表示活動將一律透過[!UICONTROL on-device decisioning]傳遞。 只有當at.js 2.5.0+設定為使用[!UICONTROL on-device decisioning]時，此活動才會在裝置上執行。 如果at.js 2.5.0+未設定為使用裝置上，則此活動仍會透過從at.js進行的伺服器呼叫傳遞。

您可以透過「裝置上決策合格」篩選器，篩選在「活動」頁面上符合[!UICONTROL on-device decisioning]資格的所有活動。

![裝置上決策活動頁面上的合格篩選器。](assets/on-device-decisioning-filter.png)

>[!NOTE]
>
>建立和啟用[!UICONTROL on-device decisioning]合格活動後，可能需要五到十分鐘才會將其納入產生的規則成品中，並傳播至Akamai CDN存在點。

## 確保透過At.js 2.5.0傳遞我的[!UICONTROL on-device decisioning]活動的步驟摘要+?

1. 存取[!DNL Adobe Target] UI並導覽至「**[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account Details]**」以啟用&#x200B;**[!UICONTROL On-Device Decisioning]**&#x200B;切換。
1. 啟用&#x200B;**[!UICONTROL "Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact"]**&#x200B;切換。

   首次JSON規則成品產生最多可能需要10分鐘。

1. 建立並啟用[!UICONTROL on-device decisioning][&#128279;](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md)支援的活動型別，並確認其符合[!UICONTROL on-device decisioning]的資格。
1. 透過at.js設定UI將&#x200B;**[!UICONTROL Decisioning Method]**&#x200B;設定為&#x200B;**[!UICONTROL "Hybrid"]**&#x200B;或&#x200B;**[!UICONTROL "On-device only"]**。
1. 下載At.js 2.5.0+並部署至您的頁面。

---
keywords: 實作， javascript程式庫， js， atjs，裝置上決策，裝置上決策， at.js，裝置上，裝置上，實作0
description: 瞭解如何執行 [!UICONTROL 裝置上決策] 使用at.js資料庫
title: 裝置上決策如何與at.js JavaScript程式庫搭配運作？
feature: at.js
exl-id: bd0e062f-c259-46f3-adba-e380af058ac8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '3689'
ht-degree: 9%

---

# [!UICONTROL 裝置上決策] 適用於at.js

從2.5.0版開始，at.js提供 [!UICONTROL 裝置上決策]. [!UICONTROL 裝置上決策] 可讓您快取 [A/B測試](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) 和 [體驗鎖定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html) (XT)在瀏覽器上執行記憶體內決策的活動，不會向封鎖的網路請求 [!DNL Adobe Target] Edge Network。

>[!NOTE]
>
>[!UICONTROL 裝置上決策] 可用於使用者端及伺服器端實作。 本文說明 [!UICONTROL 裝置上決策] 適用於使用者端。 有關以下專案的資訊： [!UICONTROL 裝置上決策] 如需伺服器端，請參閱伺服器端實作檔案 [此處](../../../server-side/sdk-guides/on-device-decisioning/overview.md).

[!DNL Target] 也提供彈性，可透過即時伺服器呼叫，從您的實驗和機器學習導向（ML導向）的個人化活動中提供最相關和最新的體驗。 換言之，當效能最重要時，您可以選擇使用 [!UICONTROL 裝置上決策]. 但是，當需要最相關、最新和ML導向的體驗時，可以改為進行伺服器呼叫。

## 有哪些優點 [!UICONTROL 裝置上決策]？

的優點 [!UICONTROL 裝置上決策] 包括：

* **提供驚人的快速決策和體驗。** 在記憶體中及瀏覽器上執行分組和決策，以避免封鎖網路請求。
* **增強應用程式效能。** 執行實驗並為您的客戶和使用者提供個人化，而不會損害一般使用者體驗。
* **改善Google網站品質分數。** 隨著決策作業在記憶體中進行，提升您線上業務的Google網站品質分數，讓消費者更容易發現它。
* **向即時分析學習。** 透過即時瞭解您的活動績效 [目標分析](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) (A4T)報表。 A4T可讓您在關鍵時刻樞紐分析策略。

## 支援的功能

此 [!DNL Adobe Target] JS SDK讓客戶可靈活選擇資料的效能與最新狀態，以便做出決策。 換言之，如果透過機器學習提供最相關且最吸引人的個人化內容對您而言至關重要，則應進行即時伺服器呼叫。 但是，當效能較為重要時，就應該做出裝置上及記憶體中的決策。 的 [!UICONTROL 裝置上決策] 若要運作，請參閱支援的功能清單：

* 活動類型
* 對象目標定位
* 配置方法

如需詳細資訊，請參閱 [支援的功能 [!UICONTROL 裝置上決策]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md).

## 如何 [!UICONTROL 裝置上決策] 工作？

使用部署和初始化at.js時 [!UICONTROL 裝置上決策] 已啟用， a [規則成品](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) 包含您的 [!UICONTROL 裝置上決策] 若為A/B和XT活動，受眾和資產會從最接近訪客的Akamai CDN下載，並在訪客的瀏覽器上快取至本機。 當從at.js提出擷取體驗的請求時，會根據快取規則成品中編碼的中繼資料，在記憶體中做出有關要傳回哪個體驗的決定。

## 決策方法

替換為 [!UICONTROL 裝置上決策]， [!DNL Target] 引入稱為「決策方法」的新設定。 決策方法設定會指定at.js提供您體驗的方式。 決策方法有三個值：

* 僅限伺服器端
* 僅限裝置上
* 混合式

### 僅限伺服器端

僅伺服器端是預設的決策方法，可在您的Web屬性上實作和部署at.js 2.5.0+時立即使用。

僅使用伺服器端作為預設設定，表示所有決定都是在 [!DNL Target] 邊緣網路，其中涉及封鎖伺服器呼叫。 此方法可能會增加延遲，但也有顯著的好處，例如讓您能夠套用 [!DNL Target]的機器學習功能包括 [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)， [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) (AP)，以及 [自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) 活動。

此外，使用增強您的個人化體驗 [!DNL Target]的使用者設定檔會跨工作階段和管道儲存，可為您的業務提供強大的成果。

最後，伺服器端僅可讓您使用Adobe Experience Cloud，並微調可透過Audience Manager和Adobe Analytics區段鎖定的對象。

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和 [!DNL Adobe Target] 邊緣網路。 此流程圖表會擷取新訪客和回訪訪客。

（按一下影像可展開至完整寬度。）

![僅限伺服器端的流程圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/server-side-only.png "僅限伺服器端的流程圖"){zoomable=&quot;yes&quot;}

下列清單與圖表中的數字相對應：

| 步驟 | 說明 |
| --- | --- |
| 1 | 訪客ID的Experience Cloud擷取自 [Adobe Experience Cloud Identity服務](https://experienceleague.adobe.com/docs/id-service/using/home.html?). |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />   也能使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js資料庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | 提出頁面載入請求，包含所有已設定的引數，例如（ECID、客戶ID、自訂引數、使用者設定檔等）。 |
| 5 | 個人資料指令碼執行，然後注入個人資料存放區。<br />設定檔存放區會從對象資料庫中要求合格對象(例如，從Adobe Analytics、Adobe Audience Manager等共用的對象)。<br />客戶屬性會透過批次程序傳送至個人資料存放區。 |
| 6 | 設定檔存放區用於對象資格和分組以篩選活動。 |
| 7 | 從即時判斷體驗後，就會選取產生的內容 [!DNL Target] 活動。 |
| 8 | at.js資料庫會隱藏頁面上與必須轉譯之體驗相關聯的對應元素。 |
| 9 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 10 | at.js資料庫會操控DOM來轉譯來自的體驗 [!DNL Target] Edge Network。 |
| 11 | 體驗會針對訪客呈現。 |
| 12 | 載入整個網頁。 |
| 13 | Analytics 資料傳送至「資料收集」伺服器。 |
| 14 | 目標資料會透過 SDID 來比對 Analytics 資料，然後經過處理放入 Analytics 報表儲存體中。然後就可以在 Analytics 與 [!DNL Target] 中，透過 [!UICONTROL Analytics for Target] (A4T) 報表來檢視 Analytics 資料。 |

### 僅限裝置上

僅限裝置上決策方法必須設定於at.js 2.5.0+中，當 [!UICONTROL 裝置上決策] 您只能在網頁中使用。

[!UICONTROL 裝置上決策] 能以驚人的速度提供您的體驗和個人化活動，因為決定是由快取規則成品所做，該成品包含您符合資格的所有活動 [!UICONTROL 裝置上決策].

進一步瞭解哪些活動符合資格 [!UICONTROL 裝置上決策]，請參閱 [中支援的功能 [!UICONTROL 裝置上決策]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md).

只有在需要Target做出決定的所有頁面中，效能高度關鍵時，才應使用此決策方法。 此外，請記住，選取此決策方法時，您的 [!DNL Target] 不符合資格的活動 [!UICONTROL 裝置上決策] 將不會傳遞或執行。 at.js資料庫2.5.0+已設定為僅尋找快取規則成品以做出決策。

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和Akamai CDN之間的互動情形。 Akamai CDN會在訪客首次造訪時快取規則成品。 新訪客第一次造訪頁面時，必須從Akamai CDN下載JSON規則成品，才能在訪客的瀏覽器上在本機快取。 下載JSON規則成品後，會立即作出決定，而不會封鎖網路呼叫。 以下流程圖會擷取新訪客。

（按一下影像可展開至完整寬度。）

![僅限裝置上的流程圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only.png "僅限裝置上的流程圖"){zoomable=&quot;yes&quot;}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target] 管理員伺服器符合您所有活動的資格 [!UICONTROL 裝置上決策]，會產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 訪客ID的Experience Cloud擷取自 [Adobe Experience Cloud Identity服務](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也能使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js資料庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | at.js程式庫會要求從最接近訪客的Akamai CDN擷取JSON規則成品。 |
| 5 | Akamai CDN會以JSON規則成品回應。 |
| 6 | JSON規則成品會快取至訪客瀏覽器的本機。 |
| 7 | at.js程式庫會解譯JSON規則成品並執行決定以擷取體驗並隱藏測試的元素。 |
| 8 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 9 | at.js程式庫會操控DOM來轉譯快取JSON規則成品中的體驗。 |
| 10 | 體驗會針對訪客呈現。 |
| 11 | 載入整個網頁。 |
| 12 | Analytics 資料傳送至「資料收集」伺服器。目標資料會透過 SDID 來比對 Analytics 資料，然後經過處理放入 Analytics 報表儲存體中。然後就可以在 Analytics 與 [!DNL Target] 中，透過 [!UICONTROL Analytics for Target] (A4T) 報表來檢視 Analytics 資料。 |

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和快取JSON規則成品之間的互動，以供訪客的後續頁面點選或回訪使用。 由於JSON規則成品已快取並在瀏覽器上可用，因此會立即做出決定而不會封鎖網路呼叫。 此流程圖會擷取後續頁面導覽或回訪訪客。

（按一下影像可展開至完整寬度。）

![後續頁面導覽和重複造訪的僅限裝置上流量圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only-subsequent.png "後續頁面導覽和重複造訪的僅限裝置上流量圖"){zoomable=&quot;yes&quot;}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target] 管理員伺服器符合您所有活動的資格 [!UICONTROL 裝置上決策]，會產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 訪客ID的Experience Cloud擷取自 [Adobe Experience Cloud Identity服務](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也能使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js資料庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | at.js程式庫會解譯JSON規則成品並在記憶體中執行決定以擷取體驗。 |
| 5 | 已測試的元素會隱藏。 |
| 6 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 7 | at.js程式庫會操控DOM來轉譯快取JSON規則成品中的體驗。 |
| 8 | 體驗會針對訪客呈現。 |
| 9 | 載入整個網頁。 |
| 10 | Analytics 資料傳送至「資料收集」伺服器。目標資料會透過 SDID 來比對 Analytics 資料，然後經過處理放入 Analytics 報表儲存體中。然後就可以在 Analytics 與 [!DNL Target] 中，透過 [!UICONTROL Analytics for Target] (A4T) 報表來檢視 Analytics 資料。 |

### 混合式

混合式決策方法必須設定於at.js 2.5.0+中，當兩者皆為 [!UICONTROL 裝置上決策] 和需要網路呼叫的活動 [!DNL Adobe Target] 必須執行Edge網路。

當您同時管理兩者 [!UICONTROL 裝置上決策] 活動和伺服器端活動，在考慮如何部署和布建時，可能會有點複雜和繁瑣 [!DNL Target] 在您的頁面上。 使用混合決策方法， [!DNL Target] 知道何時必須對 [!DNL Adobe Target] 適用於需要伺服器端執行之活動的邊緣網路，以及僅執行裝置上決策的時機。

JSON規則成品包含中繼資料，以通知at.js mbox是否正在執行伺服器端活動或 [!UICONTROL 裝置上決策] 活動。 此決策方法可確保您打算快速傳送的活動透過完成 [!UICONTROL 裝置上決策] 而針對需要更強大ML驅動個人化的活動，這些活動會透過 [!DNL Adobe Target] 邊緣網路。

下圖說明您的訪客、瀏覽器、at.js 2.5.0+、Akamai CDN與 [!DNL Adobe Target] Edge Network ，適用於首次造訪您頁面的新訪客。 此圖表的優點在於，在透過進行決定時，JSON規則成品會以非同步方式下載 [!DNL Adobe Target] 邊緣網路。

此方法可確保成品（可包含許多活動）的大小不會對決策的延遲產生負面影響。 同步下載JSON規則成品並在之後進行決策也可能會對延遲產生不利影響且可能不一致。 因此，混合決定方法是一種最佳實務建議，可一律為新訪客的決定發出伺服器端呼叫，而且會並行快取JSON規則成品。 對於任何後續的頁面造訪和回訪，決策是透過JSON規則成品從快取和記憶體中做出的。

（按一下影像可展開至完整寬度。）

![首次訪客的混合流量圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-first-time-visitor.png "首次訪客的混合流量圖"){zoomable=&quot;yes&quot;}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target] 管理員伺服器符合您所有活動的資格 [!UICONTROL 裝置上決策]，會產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 訪客ID的Experience Cloud擷取自 [Adobe Experience Cloud Identity服務](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也能使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js資料庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | 系統會向發出頁面載入請求 [!DNL Adobe Target] Edge Network，包括所有已設定的引數，例如（ECID、客戶ID、自訂引數、使用者設定檔等）。 |
| 5 | 同時，at.js會要求從最接近訪客的Akamai CDN擷取JSON規則成品。 |
| 6 | ([!DNL Adobe Target] Edge Network)執行設定檔指令碼，然後注入設定檔存放區。 設定檔存放區會從對象資料庫中要求合格對象(例如，從Adobe Analytics、Adobe Audience Manager等共用的對象)。 |
| 7 | Akamai CDN會以JSON規則成品回應。 |
| 8 | 設定檔存放區用於對象資格和分組以篩選活動。 |
| 9 | 從即時判斷體驗後，就會選取產生的內容 [!DNL Target] 活動。 |
| 10 | at.js資料庫會隱藏頁面上與必須轉譯之體驗相關聯的對應元素。 |
| 11 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 12 | at.js資料庫會操控DOM來轉譯來自的體驗 [!DNL Target] Edge Network。 |
| 13 | 體驗會針對訪客呈現。 |
| 14 | 載入整個網頁。 |
| 15 | Analytics 資料傳送至「資料收集」伺服器。目標資料會透過 SDID 來比對 Analytics 資料，然後經過處理放入 Analytics 報表儲存體中。然後就可以在 Analytics 與 [!DNL Target] 中，透過 [!UICONTROL Analytics for Target] (A4T) 報表來檢視 Analytics 資料。 |

下圖說明您的訪客、瀏覽器、at.js 2.5.0+和快取JSON規則成品之間的互動，以供後續頁面導覽或回訪使用。 在此圖表中，僅著重於針對後續頁面導覽或回訪而做出裝置上決策的使用案例。 請記住，根據特定頁面中啟用的活動，可能會進行伺服器端呼叫以執行伺服器端決策。

（按一下影像可展開至完整寬度。）

![用於後續頁面導覽和重複造訪的混合流量圖](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-subsequent.png "用於後續頁面導覽和重複造訪的混合流量圖"){zoomable=&quot;yes&quot;}

下列清單與圖表中的數字相對應：

>[!NOTE]
>
>[!DNL Adobe Target] 管理員伺服器符合您所有活動的資格 [!UICONTROL 裝置上決策]，會產生JSON規則成品，並將其傳播至Akamai CDN。 系統持續監控您的活動是否有更新，以輸出新的JSON規則成品並傳播至Akamai CDN。

| 步驟 | 說明 |
| --- | --- |
| 1 | 訪客ID的Experience Cloud擷取自 [Adobe Experience Cloud Identity服務](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也能使用頁面上實作的選擇性預先隱藏程式碼片段，以非同步方式載入at.js資料庫。 |
| 3 | at.js資料庫會隱藏內文以防止閃爍。 |
| 4 | 系統會要求擷取體驗。 |
| 5 | at.js程式庫會確認JSON規則成品已快取，並在記憶體中執行決定以擷取體驗。 |
| 6 | 已測試的元素會隱藏。 |
| 7 | at.js程式庫會顯示內文，以便載入頁面的其餘部分以供訪客檢視。 |
| 8 | at.js程式庫會操控DOM來轉譯快取JSON規則成品中的體驗。 |
| 9 | 體驗會針對訪客呈現。 |
| 10 | 載入整個網頁。 |
| 11 | Analytics 資料傳送至「資料收集」伺服器。目標資料會透過 SDID 來比對 Analytics 資料，然後經過處理放入 Analytics 報表儲存體中。然後就可以在 Analytics 與 [!DNL Target] 中，透過 [!UICONTROL Analytics for Target] (A4T) 報表來檢視 Analytics 資料。 |

## 我該如何啟用 [!UICONTROL 裝置上決策]？

[!UICONTROL 裝置上決策] 可供所有人使用 [!DNL Target] 使用At.js 2.5.0+的客戶。

若要啟用 [!UICONTROL 裝置上決策]：

>[!NOTE]
>
>您必須擁有管理員或核准者 [使用者角色](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) 以啟用或停用「裝置上決策」切換按鈕。

1. 按一下 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 帳戶詳細資料]**.
1. 在 **[!UICONTROL 帳戶詳細資料]**，滑動 **[!UICONTROL 裝置上決策]** 切換至「開啟」位置。

   ![[!UICONTROL 裝置上決策] 切換](assets/on-device-decisioning-toggle.png)

   「包含所有現有 [!UICONTROL 裝置上決策] 如果您啟用「 」，則會顯示「成品」中的合格活動 [!UICONTROL 裝置上決策].
1. （視條件而定）如果您想要讓所有活動內容都上線，請將切換滑至「開啟」位置 [!DNL Target] 符合資格的活動 [!UICONTROL 裝置上決策] 自動包含在成品中。

   若將此切換保持關閉，表示您必須重新建立並啟用任何 [!UICONTROL 裝置上決策] 要包含在產生的規則成品中的活動。 換言之，在開啟裝置上決策切換開關之前處於即時狀態的任何活動都不會納入規則成品中。

啟用「裝置上決策」切換後， [!DNL Target] 開始產生和傳播 [規則人工因素](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) 適用於您的使用者端。

>[!WARNING]
>
>在初始化 [!DNL Adobe Target] 要使用的SDK [!UICONTROL 裝置上決策]. 規則成品必須先產生並傳播至Akamai CDN，才能用於 [!UICONTROL 裝置上決策] 才能運作。 第一個規則成品可能需要五到十分鐘的時間才會產生並傳播至Akamai CDN。

## 如何設定at.js 2.5.0+使用 [!UICONTROL 裝置上決策]？

1. 按一下 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 帳戶詳細資料]**.
1. 在 **[!UICONTROL 實作方法]** > **[!UICONTROL 主要實作方法]**，按一下 **[!UICONTROL 編輯]** ，位於您的at.js版本（必須是at.js 2.5.0或更新版本）旁。

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

您可以為所有「 」設定預設決策方法 [!DNL Target] 決定。 各種決策方法是僅限伺服器端、僅限裝置上及混合。 在中選取的決策方法 [!DNL Target] UI設定於 `window.targetGlobalSettings` 在 `decisioningMethod` 欄位。 進一步瞭解 `decisioningMethod` 在 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#decisioningmethod).

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

如果您設定 `decisioningMethod` 在 `window.targetGlobalSettings`，但想要覆寫 `decisioningMethod` 針對每個 [!DNL Adobe Target] 根據您的使用案例來決定，您可以透過指定以下步驟來執行此程式： `decisioningMethod` 在At.js2.5.0+中 [getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) 呼叫。

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
>若要使用「裝置上」或「混合」作為getOffers()呼叫中的決策方法，請確定全域設定具有 `decisioningMethod` 設為「裝置上」或「混合」。 at.js資料庫2.5.0+必須知道是否在頁面上載入後立即下載及快取JSON規則成品。 如果全域設定的決策方法設為「伺服器端」，且「裝置上」或「混合」決策方法已傳遞至getOffers()呼叫，則at.js 2.5.0+不會快取JSON規則成品以執行您的裝置上決策。

### 成品快取TTL

Target代表您符合資格的活動 [!UICONTROL 裝置上決策] 作為包含中繼資料、規則和條件的成品。 會在Akamai CDN上快取此成品。 在使用者第一次造訪期間，使用者的瀏覽器會下載並快取代表您的的成品 [!UICONTROL 裝置上決策] 活動。

後續造訪您的網站時，瀏覽器會自動檢查是否必須下載較新版本的成品。 這項檢查會增加延遲。 成品快取TTL會定義自上次成功下載以來，您不希望瀏覽器檢查更新成品的分鐘數。 時間範圍越長，效能就越好。 時間範圍越短，資料的時效性就越好，但代價是延遲時間增加。

## 我如何知道活動是 [!UICONTROL 裝置上決策] 符合資格？

建立符合以下條件的活動後： [!UICONTROL 裝置上決策] 符合資格，即會顯示「裝置上決策」符合資格的標籤，會顯示在活動的「概覽」頁面中。

![活動概觀頁面上的裝置上決策合格標籤。](assets/on-device-decisioning-eligible-label.png)

此標籤並不表示活動將一律透過 [!UICONTROL 裝置上決策]. 只有當at.js 2.5.0+設定為使用 [!UICONTROL 裝置上決策] 此活動是否會在裝置上執行。 如果at.js 2.5.0+未設定為使用裝置上，則此活動仍會透過從at.js進行的伺服器呼叫傳遞。

您可以篩選符合以下條件的所有活動： [!UICONTROL 裝置上決策] 可透過「裝置上決策」合格篩選器在「活動」頁面上符合資格。

![「活動」頁面上的「裝置上決策」合格篩選器。](assets/on-device-decisioning-filter.png)

>[!NOTE]
>
>建立和啟動符合以下條件的活動後： [!UICONTROL 裝置上決策] 符合資格，可能需要五到十分鐘，才會將其納入產生並傳播至Akamai CDN存在點的規則成品中。

## 步驟摘要，確保我的 [!UICONTROL 裝置上決策] 活動是透過At.js 2.5.0傳送+?

1. 存取 [!DNL Adobe Target] UI並導覽至 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 帳戶詳細資料]** 以啟用 **[!UICONTROL 裝置上決策]** 切換。
1. 啟用 **[!UICONTROL &quot;包含所有現有 [!UICONTROL 裝置上決策] 成品中的合格活動」]** 切換。

   首次JSON規則成品產生最多可能需要10分鐘。

1. 建立並啟用 [支援的活動型別 [!UICONTROL 裝置上決策]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md)，並確認它為 [!UICONTROL 裝置上決策] 符合資格。
1. 設定 **[!UICONTROL 決策方法]** 為 **[!UICONTROL &quot;Hybrid&quot;]** 或 **[!UICONTROL 「僅限裝置上」]** 透過at.js設定UI。
1. 下載At.js 2.5.0+並部署至您的頁面。

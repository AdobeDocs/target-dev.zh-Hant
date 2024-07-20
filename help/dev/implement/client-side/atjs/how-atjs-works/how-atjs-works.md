---
keywords: 系統圖表，忽隱忽現， at.js，實作， javascript資料庫， js， atjs， $8
description: 了解  [!DNL Target] at.js JavaScript 程式庫如何運作，包括可幫助您了解頁面載入時的工作流程的系統圖。
title: at.js Javascript 程式庫如何運作？
feature: at.js
exl-id: 9183797c-857b-4b7f-a573-6bb1d583f7b1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1127'
ht-degree: 66%

---

# at.js 如何運作

若要在用戶端實作 [!DNL Adobe Target]，您必須使用 at.js JavaScript 程式庫。

在 [!DNL Adobe Target] 的用戶端實作中，[!DNL Target] 會將與活動相關聯的體驗直接提供給用戶端瀏覽器。瀏覽器會決定要顯示哪個體驗，然後顯示其內容。在用戶端實作中，您可以使用 WYSIWYG 編輯器、[視覺體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC) 或非視覺化介面[表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)，建立您的測試和個人化體驗。

## 什麼是 at.js?

at.js程式庫是使用者端實作[!DNL Adobe Target]的實作程式庫。 at.js 程式庫可加快網頁實作的頁面載入速度，並為單頁應用程式提供更好的實作選項。 at.js 為建議的實作程式庫，且經常更新功能。我們建議所有客戶實作或移轉至[最新版本的at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md)。

如需詳細資訊，請參閱 [Target JavaScript 程式庫](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#libraries)。

在下圖所示的[!DNL Target]實作中，已實作下列Adobe Experience Cloud解決方案： [!DNL Analytics]、Target和[!DNL Audience Manager]。 此外，已實作下列[!DNL Experience Cloud]個核心服務： [!DNL Adobe Experience Platform]、[!UICONTROL Audiences]和[!UICONTROL Visitor ID Service]。

## at.js 1.*x* 和 at.js 2.x 工作流程圖表之間有何差異？

請參閱[從 at.js 1.x 升級為 at.js 2.x](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)，深入瞭解 2.O 中引入哪些與 1.*x* 版有所差異之處。

從高階角度來看，兩個版本之間存在幾項差異:

* at.js 2.x 沒有全域 mbox 要求概念，而是採用頁面載入要求。頁面載入要求可視為要求擷取網站初始頁面載入時應套用的內容。
* at.js 2.x可管理稱為[!UICONTROL Views]的概念，這些概念用於單頁應用程式(SPA)。 at.js 1.*x* 不知道這個概念。

## at.js 2.x 圖表

下列圖表可協助您瞭解使用[!UICONTROL Views]的at.js 2.x工作流程，以及如何藉由這套工作流程增強SPA整合。 如需 at.js 2.x 中所使用概念的詳細介紹，請參閱[實作單頁應用程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。

（按一下影像可展開至完整寬度。）

![使用at.js 2.x](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "使用at.js 2.x"){zoomable="yes"}的Target流程

| 步驟 | 詳細資料 |
| --- | --- |
| 1 | 如果使用者已通過驗證，呼叫會傳回 [!UICONTROL Experience Cloud ID]，而另一個呼叫會同步客戶 ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<br />也能使用將頁面上實作的程式碼片段預先隱藏的選項，以非同步方式載入 at.js。 |
| 3 | 提出頁面載入要求，包含所有已設定的參數 (MCID、SDID 和客戶 ID)。 |
| 4 | 設定檔指令碼執行，然後注入至[!UICONTROL Profile Store]。 Sto會從[!UICONTROL Audience Library]重新要求合格對象（例如從[!DNL Adobe Analytics]、[!DNL Audience Manager]等共用的對象）。<br />客戶屬性會透過批次程序傳送至 [!UICONTROL Profile Store]。 |
| 5 | [!DNL Target] 會根據 URL 要求參數和個人資料，決定可針對目前頁面和未來檢視傳回哪些活動和體驗給訪客。 |
| 6 | 目標內容會傳回至頁面，選擇性地包括其他個人化的個人資料值。<br />目前頁面上目標內容會儘快出現，不會有忽隱忽現的預設內容。<br />作為使用者在 SPA 中的操作結果而針對檢視顯示的內容將快取在瀏覽器中，這樣便可在透過 `triggerView()` 觸發檢視時立即套用，而不需要額外的伺服器呼叫。 |
| 7 | Analytics資料傳送至[!UICONTROL Data Collection]伺服器。 |
| 8 | 目標資料會透過SDID來比對Analytics資料，然後經過處理放入[!DNL Analytics]報表儲存體中。然後就可以在[!DNL Analytics]與[!DNL Target]中透過(A4T)報表檢視<br />[!DNL Analytics]資料。 |

現在，只要在SPA上實作`triggerView()`，系統就會從快取擷取[!UICONTROL Views]和動作，並在沒有伺服器呼叫的情況下顯示給使用者。 `triggerView()` 也會對 [!DNL Target] 後端發出通知要求，以便增加和記錄曝光計數。如需針對採用檢視的 SPA 瞭解 at.js 的詳細資訊，請參閱[實作單頁應用程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。

（按一下影像可展開至完整寬度。）

![Target流程at.js 2.x triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Target流程at.js 2.x triggerView"){zoomable="yes"}

| 步驟 | 詳細資料 |
| --- | --- |
| 1 | 在SPA中呼叫`triggerView()`以轉譯[!UICONTROL View]並套用動作來修改視覺元素。 |
| 2 | 從快取讀取檢視的目標內容。 |
| 3 | 目標內容會儘快出現，不會有忽隱忽現的預設內容。 |
| 4 | 通知要求已傳送至[!DNL Target] [!UICONTROL Profile Store]，以計算活動中的訪客數並增加量度。 |
| 5 | [!DNL Analytics]資料已傳送至[!UICONTROL Data Collection Servers]。 |
| 6 | [!DNL Target]資料透過SDID與[!DNL Analytics]資料相符，並且已處理至[!DNL Analytics]報表儲存體。 然後就可以透過A4T報表在[!DNL Analytics]和[!DNL Target]中檢視[!DNL Analytics]資料。 |

### 影片 - at.js 2.x 架構圖表

at.js 2.x 增強了Adobe Target 對 SPA 的支援，並與其他 Experience Cloud 解決方案整合。本影片說明整合方式。

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

如需詳細資訊，請參閱[ 了解 at.js 2.x 的運作方式](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html)。

## at.js 1.x 圖表

下列圖表可協助您瞭解at.js 1.x的工作流程。

（按一下影像可展開至完整寬度。）

![Target流程at.js 1.x](/help/dev/implement/client-side/assets/target-flow.png "Target流程at.js 1.x"){zoomable="yes"}

| 步驟 | 說明 | 呼叫 | 說明 |
|--- |--- |--- |--- |
| 1 | 如果使用者已通過驗證，呼叫會傳回Experience CloudID (MCID)；另一個呼叫會同步客戶ID。 | 2 | at.js 程式庫會同步載入並隱藏文件本文。 |
| 3 | 提出全域 mbox 請求，含所有已設定的參、MCID、SDID 和客戶 ID (可選)。 | 4 | 個人資料指令碼執行，然後注入個人資料存放區。存放區會從受眾資料庫中要求合格受眾(例如從Adobe Analytics、Audience Manager等共用的受眾)。<br />客戶屬性會透過批次程序傳送至個人資料存放區。 |
| 5 | [!DNL Target] 根據 URL、mbox 參數和個人資料，決定要傳回給訪客的活動和體驗。 | 6 | 已鎖定的目標內容會傳回至頁面，選擇性地包括其他個人化的個人資料值。<br />體驗會儘快出現，不會有忽隱忽現的預設內容。 |
| 7 | Analytics 資料傳送至「資料收集」伺服器。 | 8 | Target資料會透過SDID來比對Analytics資料，然後經過處理放入Analytics報表儲存體中。然後就可以在Analytics和[!DNL Target]中，透過[!UICONTROL Analytics for Target] (A4T)報表來檢視<br />Analytics資料。 |

### 影片 - 辦公時間：at.js 提示與總覽 (2019 年 6 月 26 日)

這支影片記錄了「辦公時間」，「辦公時間」是一項由[!UICONTROL Adobe Customer Care]團隊主導的計畫。

* 使用 at.js 的好處
* at.js 設定
* Flicker 處理
* 偵錯 at.js
* 已知問題
* 常見問答

>[!VIDEO](https://video.tv.adobe.com/v/27959/?quality=12)

## at.js 如何呈現具有 HTML 內容的選件

呈現具有 HTML 內容的選件時，at.js 會套用下列演算法:

1. 預先載入影像 (如果 HTML 內容中有任何 `<img>` 標記)。

1. 將 HTML 內容附加至 DOM 節點。

1. 執行內嵌指令碼 (含括在 `<script>` 標記中的程式碼)。

1. 非同步載入並執行遠端指令碼 (具有 `src` 屬性的 `<script>` 標記)。

重要注意事項:

* at.js 不對遠端指令碼的執行順序提供任何保證，因為這些會以非同步的方式載入。
* 內嵌指令碼在遠端指令碼上不應有任何相依性，因為這些會在之後載入及執行。

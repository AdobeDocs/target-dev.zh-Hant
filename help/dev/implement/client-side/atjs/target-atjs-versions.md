---
keywords: at.js版本、at.js版本、發行說明
description: 檢視 [!DNL Adobe Target] at.js JavaScript程式庫每個版本中變更的詳細資料。
title: 每個at.js版本包含什麼？
feature: at.js
exl-id: 609dacba-2ab8-45e9-b189-928d59938c98
source-git-commit: e00d56b2515124abd23979dfc3159999e80b0ab0
workflow-type: tm+mt
source-wordcount: '5018'
ht-degree: 62%

---

# at.js 版本詳細資料

有關 [!DNL Adobe Target] at.js JavaScript 程式庫每個版本中的變更的詳細資料。

>[!IMPORTANT]
>
>[!DNL Adobe Target]同時支援at.js 1。*x* 和 at.js 2 中的 Hide Body 和 Show Body 呼叫。*x*。
>
>at.js 1.*x*&#x200B;已進入維護模式。 [!DNL Target]團隊會在必要時發行錯誤修正和安全性修補程式。
>
>[!DNL Target]團隊提供at.js 2的完整支援。*x*&#x200B;和持續發行錯誤修正、安全性修補程式、功能和效能最佳化。
>
>您應該升級至其中一個1的最新版本。*x*&#x200B;或2。*x*&#x200B;以取得錯誤修正和安全性修補程式，以解決在對應主要版本的先前次要版本中發現的問題。

[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)中的標籤是升級at.js的偏好方法。 擴充功能開發人員不斷新增功能至其擴充功能，也經常修正錯誤。 這些更新會封裝成新版本的擴充功能，並可在Adobe Experience Platform目錄中提供作為升級版本。 如需詳細資訊，請參閱&#x200B;*標籤總覽*&#x200B;指南中的[擴充功能升級](https://experienceleague.adobe.com/docs/experience-platform/tags/ui/extensions/extension-upgrade.html?lang=zh-Hant)。

## at.js 2.11.8版（2025年3月31日）

* 解決字串尾碼驗證中CodeQL識別的漏洞，以防止在調整大小和移動作業期間出現邊緣案例。 (TNT-51516)

## at.js 2.11.7版（2025年2月26日）

* 修正無法使用`localStorage`時的遙測記錄。 遙測導致某些客戶在其瀏覽器中停用`localStorage`的問題。

## at.js 2.11.6版（2024年9月29日）

* 已修正導致[!DNL Target]無法在[!UICONTROL Visual Experience Composer] (VEC)或[!UICONTROL Form-Based Experience Composer]內以重新導向選件正確運作的問題。

## at.js 2.11.5版（2024年8月14日）

* 實作快取，以進行Cookie讀取和寫入作業，減少重複、高成本的字串剖析和操作額外負荷。
* 已實作較新的URL Search Params API （在可用時），因為它比手動剖析和處理字串更快。

## at.js 2.11.4版（2024年1月24日）

* 更新at.js以防止無效的地理資料傳送至傳送API。

## at.js 2.11.3版（2023年11月21日）

* 修正無法在`at-content-rendering-failed`個事件上傳送回應Token的問題。

## at.js 2.11.2版（2023年10月26日）

* 修正自訂事件所傳送的回應Token不一致的問題。

## at.js 2.11.1版（2023年10月13日）

* 修正了在執行at.js的頁面處於怪異模式時，造成無法攔截錯誤的問題。

## at.js 2.11.0版（2023年10月10日）

* 新增在`targetGlobalSettings`中設定自訂[!DNL Adobe Experience Platform] (AEP) `sandboxId`和`sandboxName`的支援，這些在`getOffer/getOffers`呼叫時傳遞至傳遞API。
* 在選取器中鏈結`:eq()`的陰影DOM修正。

## at.js 2.10.3版（2023年9月12日）

* 修正在未轉譯任何選件時錯誤觸發`at-content-rendering-succeeded`自訂事件的問題。 現在已觸發正確的事件`at-content-rendering-no-offers`。
* 已新增`eventToken`和`responseTokens`至`at-content-rendering-failed`自訂事件的錯誤物件。

## at.js 版本 2.10.2 (2023 年 3 月 7 日)

* 修正造成 `trackEvent` 函數總是傳回錯誤的問題。

## at.js 版本 2.10.1 (2023 年 2 月 2 日)

* 已修正涉及客群規則 (包含名稱中帶有點的參數) 的活動中未傳回預期體驗以進行裝置上決策的錯誤。
* 修正at.js 2.6.0中引入的錯誤，此錯誤會導致at.js引發傳遞呼叫，即使已啟用mboxDisable亦然。

## at.js 2.10.0版（2022年9月19日）

* 新增協力廠商Cookie支援。

## at.js 2.9.0 版 (2022 年 5 月 27 日)

* 新增的「[使用者代理用戶端提示](user-agent-and-client-hints.md)」支援。
* 修正了同一個頁面上多個 mbox 請求有不同印象 ID 的問題。

## at.js 2.8.1 版 (2022 年 1 月 28 日)

* 修正`pageLoad`在裝置上決策(ODD)混合執行模式中未對應到target-global-mbox的問題。
* 已修正 mbox 要求的分析詳細資料問題。
* 已升級 dev 相依項目，修正安全漏洞。

## at.js 2.8.0 版 (2022 年 1 月 7 日)

[!DNL Target] at.js JavaScript 程式庫現在會收集功能使用情況和效能遙測資料。不會收集個人資料。可透過將 `targetGlobalSettings` 中的 `telemetryEnabled` 設定為 False，選擇退出此功能。如需詳細資訊，請參閱 [targetGlobalSettings 中的 telemetryEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled)。

## at.js 2.7.0 版 (2021 年 10 月 28 日)

此版本包含下列增強功能：

* 新增 [Web 元件](https://developer.mozilla.org/en-US/docs/Web/Web_Components)的支援。在自訂元素及其內部元素上建立和測試個人化體驗和產品建議，必須有此版本的 at.js。此功能包含在 [!DNL Target Standard/Premium] 21.10.5 版。

## at.js 1.8.3 （2021年9月21日）

此版本包含下列變更：

* 已移除`reactor-window`和`reactor-document` Adobe Experience Platform Launch模組，以確保已設定`window.default`或`document-default`的客戶能正確使用Platform Launch組建。
* at.js 1.8.3現在明確設定`Samesite=None`和`Secure`，以確保協力廠商網域Cookie已正確設定。

## at.js 2.6.1 (2021 年 8 月 16 日)

* 當使用裝置上決策功能時，發生「沒有適用於混合模式的緩存成品」的錯誤修正。

## at.js 2.6.0 (2021 年 7 月 16 日)

* 當 at.js 設定 `secureOnly` 設為 `true` 時，為 Cookie 新增安全屬性。
* 現在可以在使用 `triggerView()` 時使用回應 Token。
* 修正了與 `CONTENT_RENDERING_NO_OFFERS` 事件相關的問題。現在，只要沒有從 [!DNL Target] 傳回內容，就會正確觸發此事件。
* 使用 `prefetch` 請求時會正確傳回 [!UICONTROL Analytics for Target] (A4T) 點擊量度詳細資料。
* UUID 產生不再使用 `Math.random()`，但須依賴 `window.crypto`。
* `sessionId` Cookie 過期在每次網路呼叫時會正確延長。
* 單頁應用程式(SPA)檢視快取初始化現在可以正確處理並接受`viewsEnabled`設定。 將`viewsEnabled`設定為`false`值現在會停用`triggerView()`函式。 檢視初始頁面載入[&#128279;](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md#order)的作業順序。

## at.js 2.5.0 （2021年5月13日）

此 at.js 版本包含下列增強功能和變更：

* [針對 at.js 的裝置上決策](/help/dev/implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md)支援。
* [預覽連結](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=zh-Hant)對 Automated Personalization 活動的支援

此版本也會移除對 Microsoft Internet Explorer 10 和更高版本的支援。

## at.js 2.4.1 (2021 年 3 月 23 日)

此 at.js 版本為維護版本，包含下列增強功能和修正：

* 已修正 `targetPageParams` 包含在 mbox 要求中的問題。 `targetPageParams` 應該只能包含在 `pageLoad` 要求中。 (TNT-40247)
* 在Adobe Experience Platform擴充功能中最佳化視窗和檔案全域參考。 (TNT-37124)

## at.js 2.4.0 (2021 年 1 月 14 日)

此 at.js 版本為維護版本，包含下列修正：

* 新增對傳送API customerId的統一設定檔/平台ID支援。
* 修正無效樣式標籤插入。

## at.js 2.3.3 （2020年11月13日）

此 at.js 版本為維護版本，包含下列修正：

* 修正了與mbox點選追蹤和A4T相關的問題。 在0n點按下，[!DNL Target]使用正確的mbox和mbox引數引發傳遞API呼叫。 但是，SDID與[!DNL Analytics]呼叫中的SDID不符，因此沒有點選拼接和轉換。 (TNT-38372)

## at.js 2.3.2 (2020 年 7 月 24 日)

此 at.js 版本為維護版本，包含下列修正：

* 修正指令碼或程式碼新增預設屬性至視窗或檔案時的錯誤。

## at.js 1.8.2 （2020年6月15日）

此 at.js 版本為維護版本，包含下列修正：

* 修正使用 CNAME 和 Edge Override (at.js 1) 時的問題。*x* 可能會錯誤建立伺服器網域，導致請 [!DNL Target] 請求失敗。(TNT-35064)

## at.js 2.3.1版本（2020年6月15日）

此 at.js 版本為維護版本，包含下列增強功能和修正：

* 透過 [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) 將 `deviceIdLifetime` 設定設為可覆寫。(TNT-36349)
* 修正使用 CNAME 和 Edge Override (at.js 2) 時的問題。*x* 可能會錯誤建立伺服器網域，導致請 [!DNL Target] 請求失敗。(TNT-35065)
* 修正使用[!DNL Target]擴充功能v2和[!UICONTROL Adobe Analytics Launch]擴充功能時，[!DNL Target]延遲[!DNL Analytics] `sendBeacon`呼叫的問題。 (TNT-36407、TNT-35990、TNT-36000)

## at.js 2.3.0版（2020年3月25日）

此 at.js 版本為維護版本，包含下列增強功能和修正：

* 支援在套用傳遞的[!DNL Target]選件時，在附加至頁面DOM的SCRIPT和STYLE標籤上設定內容安全性原則Nonce。 客戶可以設定`targetGlobalSettings.cspScriptNonce`和`targetGlobalSettings.cspStyleNonce`，讓at.js可以在套用的優惠方案上設定對應的指令碼和樣式標籤Nonce。 如需詳細資訊，請參閱[targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。
* 修正使用Google Tag Manager部署的Google Closure編譯器編譯at.js時的問題。
* 將at.js檢查Cookie從`check`重新命名為`at_check`，以避免與客戶的實作發生衝突。

## at.js 1.8.1版（2020年3月25日）

此 at.js 版本為維護版本，包含下列增強功能和修正：

* 將at.js檢查Cookie從`check`重新命名為`at_check`，以避免與客戶的實作發生衝突。

## at.js 2.2.0版（2019年10月10日）

此at.js版本包含下列增強功能和修正：

* 修正當頁面元素上不存在[!DNL Adobe Analytics]程式碼時，點選追蹤未回報[!DNL Analytics for Target] (A4T)中轉換的問題。
* 已改善在網頁上同時使用Experience Cloud ID Service (ECID) v4.4和at.js 2.2時的效能。
* 之前，ECID 曾進行兩次封鎖呼叫，之後 at.js 才能擷取體驗。 這已簡化為單一呼叫，可大幅提升效能。
* 修正預先擷取的檢視處理錯誤，其中來自預設選件的事件權杖未包含在已傳送通知中。

>[!NOTE]
>
>請將您的ECID擴充功能升級至v4.4，以運用此效能增強功能。

* at.js 2.2版也提供名為`serverState`的新設定。 當實施[!DNL Target]的混合整合時，此設定可用於最佳化頁面效能。 混合整合意指您在使用者端上同時使用at.js v2.2+和伺服器端的傳送API或[!DNL Target] SDK來傳送體驗。 `serverState`讓at.js v2.2+能夠直接從伺服器端擷取並傳回至使用者端的內容套用體驗，做為所提供頁面的一部分。 如需詳細資訊，請參閱[targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverstate)中的「serverState」。

## at.js 1.8.0版（2019年10月10日）

此at.js版本包含下列增強功能和修正：

* 已改善在網頁上同時使用Experience Cloud ID Service (ECID) v4.4和at.js 1.8時的效能。
* 之前，ECID 曾進行兩次封鎖呼叫，之後 at.js 才能擷取體驗。 這已簡化為單一呼叫，可大幅提升效能。

>[!NOTE]
>
>請將您的ECID擴充功能升級至v4.4，以運用此效能增強功能。

## at.js 版本 2.1.1 (2019 年 7 月 24 日)

此 at.js 版本為維護版本，包含下列增強功能和修正:

(括號內的問題編號供 Adobe 內部使用。)

* 修正在可視化體驗撰寫器 (VEC) 的目標與設定頁面上使用點擊追蹤量度時，導致多個指標引發的問題。(TNT-32812)
* 修正導致 `triggerView()` 無法多次呈現產品建議的問題。(TNT-32780)
* 修正 `triggerView()` 的問題，確保要求包含 Marketing Cloud ID (MCID) 資訊。(TNT-32776)
* 修正在即使沒有已儲存的視圖時，仍阻止 `triggerView()` 通知引發的問題。(TNT-32614)
* 修正由於使用 decodeURIcomponent 而導致錯誤的問題，在 URL 包含故障的查詢字串參數時會造成問題。(TNT-32710)
* 在透過 `Navigator.sendBeacon()` API 傳送的傳送要求內容中，指標標幟現已設定為「true」。(TNT-32683)
* 修正推薦產品建議無法在一些客戶的網站上顯示的問題。客戶可以看到傳送API呼叫中的選件內容，但網站上未套用該選件。 (TNT-32680)
* 修正導致多個體驗中點擊追蹤無法如運期般運作的問題。(TNT-32644)
* 修正在無法呈現第一個量度後，阻止 at.js 套用第二個量度的問題。(TNT-32628)
* 修正使用 `targetPageParams` 函數傳送 `mbox3rdPartyId` 時發生的問題，導致要求裝載無法出現於查詢參數或要求裝載中。(TNT-32613)
* 修正導致基於 Chromium 的瀏覽器 (包括 Google Chrome) 封鎖顯示和點按通知回應的問題。(TNT-32290)

## at.js version 2.1.0 (2019 年 6 月 3 日)

此版本包含下列功能和增強功能:

* **Adobe 選擇加入支援**: Adobe 選擇加入是簡化 Adobe 解決方案與同意管理平台整合的方法。如需 Adobe 選擇加入的詳細資訊，請參閱[隱私權與一般資料保護規範 (GDPR)](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)。

* **符合 CSP 產業標準**: at.js 不再使用 eval() 執行 JavaScript。

* **使用者端分析記錄**：無論是在使用者端或伺服器端，皆可讓客戶完全掌控要以何種方式將分析資料傳送至[!DNL Adobe Analytics]。

  如需詳細資訊，請參閱[使用者端 [!DNL Analytics] 記錄](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/before-implement.html?lang=zh-Hant#client-side)。

* **傳送通知**: 可讓開發人員在透過體驗的程式碼 (而不是透過 `applyOffer()` 或 `applyOffers()`) 呈現體驗時傳送通知。

  如需詳細資訊，請參閱 [adobe.target.sendNotifications(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)。

* **at.js 的大小約縮小了 24%**: at.js 的大小約縮小了 24%。較小的檔案大小可改善頁面載入效能，並縮短在頁面上載入 at.js 的時間。

## at.js 版本 2.0.1 (2019 年 3 月 19 日)

此為維護版本，包含下列增強功能和修正:

(括號內的問題編號供 Adobe 內部使用。)

* 修正 DOM 輪詢程式碼中導致某些客戶遇到 JavaScript 例外狀況的的競爭條件。(TNT-31869)
* 呈現的視圖已與點擊追蹤事件處理常式脫鉤的通知。起初，如果屬於已轉譯檢視的點選事件處理常式無法附加，則[!DNL Target]不會傳送通知。 [!DNL Target]現在會傳送檢視通知，即使找不到點按專案亦然。 (TNT-31969)
* 修正導致 request-succeeded 事件重新導向標幟一律設為 true 的問題。(TNT-31907)
* 修正導致 VEC 重新排列動作記錄為成功 (甚至在元素遺失時) 的問題。(TNT-31924)
* 修正導致某些客戶的通知不包含企業權限屬性 Token 的問題。(TNT-31999)

## at.js 版本 1.7.1 (2019 年 3 月 19 日)

此為維護版本，包含下列修正:

(括號內的問題編號供 Adobe 內部使用。)

* 修正 DOM 輪詢程式碼中導致某些客戶遇到 JavaScript 例外狀況的的競爭條件。(TNT-31869)

## at.js 版本 2.0.0

at.js 2.x 提供豐富的功能組合，讓貴公司能以新世代用戶端技術為基礎進行個人化。本次的新版本著重於升級 at.js，進而與單一頁面應用程式 (SPA) 產生和諧互動。

以下是幾個使用 at.js 2.x 特有 (舊版未提供) 的優點:

* 可以在頁面載入時將所有產品建議加入快取，把多次伺服器呼叫減少為一次。
* 大幅改善一般使用者在網站上的體驗，因為產品建議能透過快取立即顯示，避免傳統伺服器呼叫引發的延遲時間。
* 只要編寫一行程式碼以及請開發人員設定一次，行銷人員就能透過單一頁面應用程式上的可視化體驗撰寫器 (VEC) 建立及執行 A/B 和體驗 (XT) 活動。

at.js 2.x 引進以下新函數:

* getOffers()
* applyOffers()
* triggerView()

導入 at.js 2.x 後，以下函數已遭到淘汰:

* mboxCreate()
* mboxDefine
* registerExtension()

如需詳細資訊，請參閱[從 at.js 1.x 升級為 at.js 2.x](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md) 與 [at.js 函數](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md)。

>[!NOTE]
>
>如果您需要[一般資料保護規範](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) (GDPR)的Adobe選擇加入支援，目前必須使用at.js 1.7.0或at.js 2.1.0或更新版本。

## at.js 版本 1.7.0

at.js 1.7.0 提供 Adobe 選擇加入支援。「Adobe 選擇加入」是簡化 Adobe 解決方案與同意管理平台整合的方法。

如需 Adobe 選擇加入的詳細資訊，請參閱[隱私權與一般資料保護規範 &#x200B;](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) (GDPR)。

此版本也修正[!DNL Target]可能將重新導向URL引數覆寫為來自重新導向URL之引數的問題。

>[!NOTE]
>
>如果您需要GDPR的Adobe選擇加入支援，目前必須使用at.js 1.7.0或at.js 2.1.0或更新版本。

## at.js 版本 1.6.4

at.js 1.6.4 維護版本解決下列問題:

* 修正 Microsoft Internet Explorer 11 中導致套用重複產品建議的競爭條件顯現。

## at.js 版本 1.6.3

at.js 1.6.3 包含下列修正和增強功能:

* 從現在開始，當選取器含有開頭為數字、兩個連字號或連字號加數字 (如 #-123) 的 ID 或 CSS 類別時，將會逸出 CSS。(TNT-31061)
* 修正 at.js 1.6.2 導入的問題，亦即將來自不同活動的可視化體驗撰寫器 (VEC) 產品建議套用至同一個 CSS 選取器時，不會遵守活動優先順序。(TNT-31052)
* 修正在缺少承諾原生支援的環境中讓承諾逾時時發生的問題。(TNT-30974)
* 系統現在能透過內容呈現失敗事件正確擷取問題及回報。先前，系統可能會將 JavaScript 回報為成功執行，即使情況並非如此。(TNT-30599)

## at.js 版本 1.6.2

此維護版本解決下列問題:

* 修正導致部分客戶網站無限「非同步」迴圈的問題。

>[!WARNING]
>
>此外，at.js 1.6.2 版也包含 1.6.1 和 1.6.0 版的所有增強功能和修正。這兩個版本已無法下載。如果使用 1.6.1 或 1.6.0，建議升級至 1.6.2 版

at.js 1.6.1 版包含下列增強功能和修正:

* at.js 1.6.0 修正造成在 Microsoft Internet Explorer 11 中複製建議體驗的問題。(TNT-30593)
* at.js 現在會確保 Edge 覆寫邏輯檢查是否存在 Edge 叢集 Cookie，避免使用者在工作階段躍過 Edge 時有不同 Edge 編號。(TNT-30563)
* 修正 HTML 內容包含無效 JS 程式碼時，at.js 無法執行後續動作的問題。at.js 現在會記錄錯誤，並正確進行後續動作。(TNT-30546)
* 變更導致在重新導向頁面重新授權重新導向活動時，有例外情況。(TNT-30532)
* 修正正確要求逾時無法自 getOffer() API 要求傳播的問題。(TNT-30498)
* 修正 at.js 1.6.0 在使用檔案通訊協定時，無法儲存 Cookie 的問題。(TNT-30454)
* 修正使用[!DNL Analytics for Target] (A4T)時，並非所有體驗都隨重新導向傳送的問題。 (TNT-30444)
* 修正在[!DNL Target]呼叫成功後隱藏頁面的問題。 (TNT-30358)

at.js 1.6.0 版包含下列增強功能和修正:

* [!UICONTROL Analytics for Target] (A4T)整合現在會自動支援重新導向選件。 已移除用戶端解決方案。(TNT-30247)
* 現在預設啟用用戶端 Edge 路由傳送。(TNT-30261)
* 修正動作間有相依性時，進行可視化體驗撰寫器 (VEC) 動作的問題。(TNT-30248)

## at.js 版本 1.5.0

at.js 版本 1.5.0 現已可用。

* `at-request-succeeded` 事件的詳細資訊內含重新導向旗標。這個旗標是用來判斷頁面是否會重新導向至其他 URL。如果您想知道該 URL，請訂閱 `at-content-rendering-redirect`。(TNT-29834)
* 修正 `window.targetGlobalSettings.enabled` 在執行階段例外設為 false 時會失敗的問題。(TNT-29829)
* 修正使用自訂程式碼觸發全域 mbox 要求，以及使用主體隱藏時，造成頁面在可視化體驗撰寫器 (VEC) 載入失敗的問題。(TNT-29795)
* 新增對 `screenOrientation`、`devicePixelRatio` 和 `webGLRenderer` 的支援。這些新的[!DNL Target]要求引數用於iPhone X和其他新型裝置偵測。 如需詳細資訊，請參閱[行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=zh-Hant)。(TNT-29781)
* 修正偶爾未傳送 Adobe Audience Manager (AAM) 位置提示的問題。(TNT-29695)
* 若瀏覽器支援 at.js 1.5.0，at.js 1.5.0 會切換為 MutationObserver 進行選取器輪詢。at.js 1.0.0 以前的版本使用 MutationObserver polyfill，已證實會造成問題。為避免 polyfill 問題，版本 1.5.0 使用下列虛擬程式碼，決定要使用哪個排程機制:

  ```
  if MutationObserver is supported
    scheduler = MutationObserver
  else if document is visible
    scheduler = requestAnimationFrame
  else
    scheduler = setTimeout
  ```

## at.js 版本 1.3.0

at.js 1.3.0 版現已可用。

* 下列新事件可協助追蹤、除錯和自訂與 at.js 的互動:

   * LIBRARY_LOADED
   * REQUEST_START
   * CONTENT_RENDERING_START
   * CONTENT_RENDERING_NO_OFFERS
   * CONTENT_RENDERING_REDIRECT

  如需詳細資訊，請參閱[at.js自訂事件](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md)。

* 您可以使用來自資料提供者的其他參數來擴大 at.js 要求。資料提供者應新增至 `window.targetGlobalSettings` 下的 `dataProviders key`。

  如需詳細資訊，請參閱[資料提供者](atjs-functions/targetglobalsettings.md#data-providers)。

* at.js 要求現在使用 GET，但是當 URL 大小超過 2048 字元時，它會切換為使用 POST。有一個名為 `urlSizeLimit` 的新屬性，您可以在必要時增加大小限制。此變更允許[!DNL Target]將at.js與使用相同技術的AppMeasurement對齊。
* [!DNL Target]現在強制使用`adobe.target.applyOffer(options)`函式中的`mbox`機碼。 此機碼在過去是必要的，但現在[!DNL Target]強制使用它以確保[!DNL Target]有正確的驗證，且客戶正確地使用函式。
* at.js 已改善事件和點擊追蹤功能。at.js 使用 `navigator.sendBeacon()` 來傳送事件追蹤資料，並將在不支援 `navigator.sendBeacon()` 時退回同步 XHR。此次遞補主要影響 Internet Explorer 10 和 11 與一些版本的 Safari。Safari 將在近期的 iOS 11.3 版本中新增對 `navigator.sendBeacon()` 的支援。
* at.js 現在可以呈現產品建議，即便頁面是在背景索引標籤中開啟亦然。有些[!DNL Target]客戶遇到`requestAnimationFrame()`因背景標籤的瀏覽器節流行為而停用時的問題。
* 此版本新增了許多效能改善，包括檢查 Chrome CPU 設定檔時較短的呼叫堆疊。
* at.js 1.3.0 不再支援 Microsoft Internet Explorer 9 上的內容傳送如需詳細資訊，請參閱[支援的瀏覽器](/help/dev/before-implement/supported-browsers.md)。今後，所有要求會透過 `XMLHttpRequest` 執行，具有 CORS 支援而不沒有 JSONP 要求。此變更大幅改善安全性。

## at.js 版本 1.2.3

at.js 版本 1.2.3 現已可用。

* 新增 JSON 產品建議的支援。只有在使用表單式體驗撰寫器建立的活動中才支援 JSON 產品建議。目前使用 JSON 產品建議的唯一方式是透過直接 API 呼叫。請參閱[建立JSON選件](https://experienceleague.adobe.com/docs/target/using/experiences/offers/create-json-offer.html?lang=zh-Hant)。

## at.js 版本 1.2.2

at.js 版本 1.2.2 現已可用。

* 修正當[!DNL Target]程式庫使用QUIRKS模式載入頁面時，傳回JavaScript錯誤的問題。 (TNT-28312)
* 修正造成[!DNL Target]點選追蹤中斷[!DNL Analytics]個資料收集呼叫的問題。 (TNT-28261)
* 已修正當 `getOffer() params` 傳回空白字串時，造成 `targetPageParams()` 失敗的問題。(TNT-28359)
* 已修正使用僅 x 產生工作階段 ID 的問題。(TNT-28361)

## at.js 版本 1.2.1

at.js 版本 1.2.1 現已可用。

* 已修正在具有target=&quot;_blank&quot;的連結上點選追蹤防止[!DNL Target]在新索引標籤中開啟連結的問題。

## at.js 版本 1.2.0

at.js版本1.2現在已包括多數錯誤修正的維護版本形式提供。

* 已修正防止點擊追蹤特殊大小寫的預設動作的問題。(TNT-28089)
* 修正在具有`target="_blank"`的連結上點選追蹤防止[!DNL Target]在新索引標籤中開啟連結的問題。 (TNT-28072)
* 可以用作 Cookie 網域的 IP 位址。(TNT-28002)
* 已修正在具有全域 mbox 或其他地區 mbox 的重新導向產品建議中造成閃爍的問題。(TNT-27978)
* 修正在瀏覽和撰寫之間切換時， VEC內的[!UICONTROL Experience Targeting]活動設定失敗的問題。 (TNT-27942)
* 已修正點擊追蹤元素閃爍樣式類別上的不正確處理。(TNT-27896)
* 已修正造成全域 mbox 參數變得與所有 mbox 參數混合的問題。(TNT-27846)
* 進行變更以確保at.js已正確處理Handlebars、Mustache和其他使用者端範本資料庫。 (TNT-27831)
* 進行變更以確保 `sdidParamExpiry` 已正確初始化，並傳遞至訪客 API。這是已新增至 `at.js 1.1.0` 的迴歸。先前的at.js版本不受影響。 這只會影響使用重新導向產品建議和 A4T 的用戶端。(TNT-27791)
* 進行變更以確保會執行 `SCRIPT`，而無論使用的類型屬性為何。(TNT-27865)

## at.js 版本 1.1.0

**日期:** 2017 年 8 月 2 日

at.js版本1.1中包括下列增強功能和修正：

* 已新增回應 Token 處理。如需詳細資訊，請參閱[回應 Token](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=zh-Hant)。
* 已解決問題，使得 `document.currentScript polyfill` 不會干預 Angular 1.X。
* 進行變更以確保點擊追蹤不會干預可見性屬性。點擊追蹤元素會以 `at-element-click-tracking` CSS 類別標記，而非 `at-element-marker`。

## at.js 版本 1.0.0

**日期:** 2017 年 7 月 7 日

at.js 版本 1.0 中包括下列增強功能和修正:

* 支援非同步載入 at.js，可讓頁面載入更快速。
* 支援在非同步載入 at.js 時預先隱藏頁面內容。
* 停用內容傳遞時有更好的錯誤訊息。
* 傳遞多個活動時的效能改善。
* 支援 YUI Compressor。
* 在活動傳遞期間的自訂事件 Bug/錯誤報表。
* 修正 Microsoft Internet Explorer 11 中的效能問題。
* `getOffer()` 函數在部分網站上發生錯誤的修正。
* 以非同步方式載入[!DNL Target]資料庫。 如需詳細資訊，請參閱 [at.js 常見問題](/help/dev/implement/client-side/atjs/target-atjs-faq.md)。

## at.js 版本 0.9.7

**日期:** 2017 年 5 月 22 日

at.js版本0.9.7中包括下列增強功能和修正：

* 修正與可視化體驗撰寫器 (VEC) 中的 `insertAfter` 和 `insertBefore` 動作遺漏資產金鑰相關的問題。這些問題與從視覺產品建議移轉至產品建議範本有關。

## at.js 版本 0.9.6

**日期:** 2017 年 4 月 13 日

at.js版本0.9.6中包括下列增強功能和修正：

* 重新導向產品建議支援 A4T。下載並安裝at.js 0.9.6版後，您可以在使用[!UICONTROL Adobe Analytics as the Reporting Source for Target] (A4T)的活動中使用重新導向選件。 除了at.js版本0.9.6，還有您的實作必須符合以便使用重新導向選件和A4T的其他基本需求。 如需詳細資訊和須知的其他重要資訊，請參閱[重新導向產品建議 - A4T 常見問題集](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=zh-Hant)。
* 在at.js 0.9.6之前，當頁面上存在訪客API，且`visitorApiTimeout`設定太積極時，可能會發生[!DNL Target]在[!DNL Target]要求中未傳送任何MCID資料的情況。 這可能在使用 A4T 時導致 [!DNL Analytics] 中的問題，例如散亂的點擊。

  at.js 0.9.6已變更此行為，即便`visitorApiTimeout`設為假設1毫秒，[!DNL Target]將嘗試收集SDID、追蹤伺服器和客戶ID資料，並在[!DNL Target]要求中傳送那些資料。

* 已新增 `selectorsPollingTimeout` 設定。如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。
* 來自 `getOffer()` () 的回應格式已變更。如需詳細資訊，請參閱 [adobe.target.getOffer(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md)。
* 已針對不支援的 `<!DOCTYPE>` 宣告新增主控台記錄。
* 修正將多個預設選件傳遞至單一mbox時，[!DNL Target Classic]外掛程式未正確套用的問題。 (TGT-22664)
* 改善兩個字母上層網域(TLD)的Cookie設定，以確保為這些網域（例如， test.no、 autodrives.ca等）正確設定mbox Cookie。
* 用於擷取儲存 Cookie 時應該使用的上層網域的演算法在 at.js 版本 0.9.6 中已變更。因為此變更，無法將 Cookie 儲存至使用 IP 的位址。大部分時候，IP 位址是用於測試用途，但做為解決辦法，您可以使用 DNS 項目或調整本機機器上的主機檔案。
* 已修正當屬性為字串值而非整數時移動和重新排列動作的處理。

## at.js 版本 0.9.4

**日期:** 2017 年 1 月 19 日

* mbox名稱現在可以包含特殊字元，包括&amp;符號(&amp;)。

  如需允許的特殊字元清單，請參閱[at.js設定](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)。

* 已新增 `secureOnly` 設定，指出 at.js 是否應該僅使用 HTTPS 或根據頁面通訊協定，允許在 HTTP 與 HTTPS 之間切換。這是進階的設定，預設值為 False 並且可透過 `targetGlobalSettings` 覆寫。
* 「舊版瀏覽器支援」選項可在at.js版本0.9.3和更早版本中取得。 此選項已在 at.js 版本 0.9.4 中移除。

## at.js 版本 0.9.3

**日期:** 2016 年 10 月 10 日

* 當 at.js 設定中停用舊版瀏覽器時，請確保在 Microsoft Internet Explorer 11 中觸發 mbox 呼叫。
* 確保在動態遠端產品建議失敗 (例如，如果 URL 不正確並傳回 404 錯誤) 時會轉譯預設內容。
* 當 DOM 中找不到 VEC 點擊追蹤選取器時確保元素快速顯示。

## at.js 版本 0.9.2

**日期:** 2016 年 9 月 21 日

* 已新增 `optoutEnabled` 設定，以啟用或停用裝置圖表選擇退出。如果此設定設為 `true`，並且訪客選擇退出追蹤，訪客的瀏覽器將不會進行任何 mbox 呼叫。裝置圖表目前處於 Beta 版。此設定預設會設為`false`，但如果您使用裝置圖表，則必須設為`true`。
* 已針對通知機制新增 `CustomEvent` 支援。之前，您無法透過標準 DOM API (例如 `document.addEventListener()`) ()) 來使用 at.js 事件通知機制。現在您可以使用 `document.addEventListener()` 來訂閱 at.js 事件，例如要求事件和內容呈現事件。
* 已修正關於可視化體驗撰寫器 (VEC) 產品建議建立的問題。在此版本之前，[!DNL Target]只在所有選取器都相符時隱藏和取消隱藏選取器。 在at.js 0.9.2中，[!DNL Target]會在選取器符合時便加以取消隱藏。

## at.js 版本 0.9.1

**日期:** 2016 年 7 月 14 日

* 為訪客ID服務提供at.js逾時，其與服務本身的逾時無關。
* 更正0.9.0中影響在某些頁面上使用at.js和在其他頁面上使用mbox.js （現已被取代）實施的問題。
* 如果您使用[!DNL Adobe Analytics]作為活動的報表來源，若您使用的是mbox.js 61版（或更新版本）或at.js 0.9.1版（或更新版本），則不需要在活動建立期間指定追蹤伺服器。 at.js 程式庫會自動傳送追蹤伺服器值給 [!DNL Target]。 在活動建立期間，您可以將「目標與設定」頁面上的「追蹤伺服器」欄位保留空白。

## at.js 版本 0.9.0

**[!DNL Target]版本：** 16.6.1

**日期:** 2016 年 6 月 23 日

* 修正使用 VEC 產品建議時白色畫面的問題。使用at.js的任何人應該升級至這個新版本。
* 新 `registerExtension` API。

  這個新API可讓開發人員存取at.js中使用的特定jQuery模組，以為資料庫開發擴充功能（亦稱為外掛程式）。 此變更有一些隱含意義。這只會影響使用這些功能的使用者:

   * `getSettings()` API 已移除，但使用 `registerExtension()` 可發揮相同的功能。
   * `getTracking()` API 已移除，但使用 `registerExtension()` 可發揮相同的功能。

   * 必須更新現有的擴充功能 (例如 AngularJS 擴充功能)，才能使用 `registerExtension()` 方法。

* 新增at.js通知API。

  此通知系統的目標是針對at.js在頁面上的行為以及在發生問題時提供更多深入分析。 VEC 的常見問題是 IT 發行變更了頁面、VEC 選擇器中斷，以及測試停止正確傳送內容。此通知系統的一個目標是要讓頁面知道此傳送的問題，讓開發人員可以存取此資訊，將資訊傳遞至 [!DNL Adobe Analytics] 之類的系統，並且可將警示傳送至業務擁有者，通知其測試中斷的訊息。

* 新 `targetGlobalSettings()` API 方法。

  您可以覆寫at.js資料庫中的設定，而非在[!DNL Target Standard/Premium] UI中或使用REST API進行設定。

## at.js 版本 0.8.0

**日期:** 2016 年 5 月 5 日

這是at.js資料庫的第一個官方版本。

at.js是適用於[!DNL Target]的新實作程式庫，專為典型Web實作和單頁應用程式而設計。

at.js 取代了 實施的 mbox.js。[!DNL Adobe Target]

除了眾多優點以外，at.js還能改進Web實施的頁面載入時間、改進安全性，以及為單頁應用程式提供更好的實施選項。

at.js 包含 target.js 所附元件，因此不再需要呼叫 target.js。

實施 at.js 時，請注意以下事項:

* 不支援 Internet Explorer 8 版之前的舊版。
* 非同步實施表示舊版整合（例如[!UICONTROL Test&Target to SiteCatalyst]外掛程式）可能無法運作。
* 不支援參考mbox.js物件與方法的[!DNL Target]外掛程式。
* 所有對 [!DNL Target] 的呼叫都是透過 XMLHTTPRequest，而內容是透過 JSON 傳回。

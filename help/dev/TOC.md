---
user-guide-title: Adobe Target開發人員指南
breadcrumb-title: Target開發人員指南
user-guide-description: 了解如何量身打造客戶體驗並將其個人化，以便在您的網站和行動網站、應用程式、社交媒體及其他數位頻道上獲得最大收入。
source-git-commit: 697822cd7c5afcaac988d61035af56491301dc74
workflow-type: tm+mt
source-wordcount: '789'
ht-degree: 42%

---


# Adobe Target開發人員指南 {#developer}

+ [Adobe Target開發人員指南](overview.md)
+ 入門 {#implementation}
   + 實作之前 {#before-implement}
      + [實作之前](before-implement/considerations-before-you-implement-target.md)
      + [準備實作 Target](before-implement/prepare-to-implement-target.md)
   + 隱私權與安全性 {#privacy}
      + [隱私權概觀](before-implement/privacy/privacy.md)
      + [隱私權與資料保護規範](before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)
      + [Target Cookie](before-implement/privacy/cookie-behavior.md)
      + [刪除 Target Cookie](before-implement/privacy/cookie-deleting.md)
      + [第三方Cookie淘汰對Target (at.js)的影響](/help/dev/before-implement/privacy/third-party-cookie-deprecation.md)
      + [Google Chrome SameSite Cookie 原則](before-implement/privacy/google-chrome-samesite-cookie-policies.md)
      + [Apple 智慧型追蹤預防 (ITP) 2.x](before-implement/privacy/apple-itp-2x.md)
      + [內容安全性政策 (CSP)](before-implement/privacy/content-security-policy.md)
      + [允許清單 Target 邊緣節點](before-implement/privacy/allowlist-edges.md)
   + 將資料傳入 Target 的方法 {#methods}
      + [方法概觀](before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md)
      + [頁面參數](before-implement/methods-to-get-data-into-target/page-parameters.md)
      + [頁面中輪廓屬性](before-implement/methods-to-get-data-into-target/in-page-profile-attributes.md)
      + [指令碼輪廓屬性](before-implement/methods-to-get-data-into-target/script-profile-attributes.md)
      + [資料提供者](before-implement/methods-to-get-data-into-target/data-providers.md)
      + [大量設定檔更新API](before-implement/methods-to-get-data-into-target/bulk-profile-update-api.md)
      + [單一設定檔更新API](before-implement/methods-to-get-data-into-target/single-profile-update-api.md)
      + [客戶屬性](before-implement/methods-to-get-data-into-target/customer-attributes.md)
      + [輪廓 API 設定](before-implement/methods-to-get-data-into-target/profile-api-settings.md)
   + [Target 安全性概觀](before-implement/target-security-overview.md)
   + [受支援的瀏覽器](before-implement/supported-browsers.md)
   + [TLS (傳輸層安全性) 加密變更](before-implement/tls-transport-layer-security-encryption.md)
   + [CNAME 與 Adobe Target](before-implement/implement-cname-support-in-target.md)
+ 使用者端實施 {#client-side}
   + [概觀: 為用戶端 Web 實作 Target](implement/client-side/overview.md)
   + Adobe Experience Platform Web SDK實作 {#aep}
      + [Adobe Experience Platform Web SDK實作概觀](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)
      + [使用Adobe Target和Platform Web SDK進行個人化](/help/dev/implement/client-side/aep-web-sdk/target-overview.md)
      + [實作單頁應用程式](/help/dev/implement/client-side/aep-web-sdk/spa-implementation.md)
      + [存取回應Token](/help/dev/implement/client-side/aep-web-sdk/accessing-response-tokens.md)
      + [使用mbox第三方ID](/help/dev/implement/client-side/aep-web-sdk/using-mbox-3rdpartyid.md)
      + [比較at.js程式庫與Web SDK](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md)
   + at.js 如何運作 {#at-js}
      + [at.js JavaScript程式庫概觀](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)
      + [at.js運作概觀](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
      + [At.js 處理忽隱忽現情況的方式](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
      + [at.js 整合](/help/dev/implement/client-side/atjs/how-atjs-works/target-atjs-integrations.md)
   + 如何部署 at.js {#deploy-at-js}
      + [如何部署 at.js](implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md)
      + [使用 Adobe Experience Platform Launch 實施 Target](implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)
      + [不使用標籤管理程式實作 Target](implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)
      + [使用動態標籤管理員 (DTM) 實作 Target](implement/client-side/atjs/how-to-deployatjs/implement-target-using-dtm.md)
      + [實作適用於單頁應用程式 (SPA) 的 Target](implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)
   + 裝置上決策 {#on-device-decisioning}
      + [裝置上決策概觀](implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md)
      + [支援的功能](implement/client-side/atjs/on-device-decisioning/supported-features.md)
      + [規則成品](implement/client-side/atjs/on-device-decisioning/rule-artifact.md)
      + [疑難排解](implement/client-side/atjs/on-device-decisioning/troubleshooting-on-device-decisioning.md)
   + at.js 函數 {#functions-overview}
      + [at.js 函數概觀](implement/client-side/atjs/atjs-functions/atjs-functions.md)
      + [adobe.target.getOffer()](implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md)
      + [adobe.target.getOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
      + [adobe.target.applyOffer()](implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md)
      + [adobe.target.applyOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)
      + [adobe.target.triggerView() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)
      + [adobe.target.trackEvent()](implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
      + [mboxCreate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)
      + [targetGlobalSettings()](implement/client-side/atjs/atjs-functions/targetglobalsettings.md)
      + [mboxDefine() 和 mboxUpdate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)
      + [targetPageParams()](implement/client-side/atjs/atjs-functions/targetpageparams.md)
      + [targetPageParamsAll()](implement/client-side/atjs/atjs-functions/targetpageparamsall.md)
      + [registerExtension() - at.js 1.x](implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)
      + [sendNotifications() - at.js 2.1](implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)
      + [at.js 自訂事件](implement/client-side/atjs/atjs-functions/atjs-custom-events.md)
      + [使用 Adobe Experience Cloud Debugger 除錯 at.js](implement/client-side/target-debugging-atjs/target-debugging-atjs.md)
      + [使用雲端型例項搭配 Target](implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md)
   + [at.js 常見問答](implement/client-side/atjs/target-atjs-faq.md)
   + [at.js 版本詳細資料](implement/client-side/atjs/target-atjs-versions.md)
   + [從 at.js 1.x 升級為 at.js 2.x](implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)
   + [at.js Cookie](implement/client-side/atjs/atjs-cookies.md)
   + [使用者代理和使用者端提示](implement/client-side/atjs/user-agent-and-client-hints.md)
   + 瞭解全域 mbox {#global-mbox}
      + [了解全域 mbox 概觀](implement/client-side/atjs/global-mbox/global-mbox-overview.md)
      + [自訂全域 mbox](implement/client-side/atjs/global-mbox/customize-global-mbox.md)
      + [使用來自舊版實作的全域 mbox](implement/client-side/atjs/global-mbox/mbox-global-target-standard.md)
      + [傳遞參數給全域 mbox](implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)
      + [全域 mbox 常見問答](implement/client-side/atjs/global-mbox/global-mbox-faq.md)
+ 伺服器端實作 {#server-side}
   + [伺服器端：實作 Target 概觀](implement/server-side/server-side-overview.md)
   + [Target SDK快速入門](implement/server-side/sdk-guides/getting-started/getting-started.md)
   + [範例應用程式](implement/server-side/sdk-guides/sample-apps/sample-apps.md)
   + [從 Target 舊版 API 轉變為 Adobe I/O](implement/server-side/transition-from-target-classic-apis.md)
   + 核心原則 {#core-principles}
      + [核心原則概觀](implement/server-side/sdk-guides/core-principles/overview.md)
      + [使用者ID和分組](implement/server-side/sdk-guides/core-principles/user-identification-and-bucketing.md)
      + [對象目標定位](implement/server-side/sdk-guides/core-principles/audience-targeting.md)
      + [事件追蹤](implement/server-side/sdk-guides/core-principles/event-tracking.md)
      + [使用者許可權和屬性](implement/server-side/sdk-guides/core-principles/user-permissions-and-properties.md)
   + 整合 {#integration}
      + [整合概述](implement/server-side/sdk-guides/integration-with-experience-cloud/overview.md)
      + [Experience Cloud ID服務(ECID)](implement/server-side/sdk-guides/integration-with-experience-cloud/ecid.md)
      + [Analytics for Target (A4T) 報告](implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md)
      + [AAM 區段](implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md)
   + 裝置上決策 {#on-device-decisioning}
      + [裝置上決策概觀](implement/server-side/sdk-guides/on-device-decisioning/overview.md)
      + 規則成品 {#rule-artifact}
         + [規則成品概觀](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)
         + [透過Adobe Target SDK下載](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-sdk.md)
         + [透過JSON裝載下載](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-json.md)
         + [規則成品範例](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-example.md)
      + [使用功能標幟執行A/B測試](implement/server-side/sdk-guides/on-device-decisioning/execute-ab-tests-with-feature-flags.md)
      + [使用屬性執行功能測試](implement/server-side/sdk-guides/on-device-decisioning/execute-feature-tests-with-attributes.md)
      + [管理功能測試的轉出](implement/server-side/sdk-guides/on-device-decisioning/manage-rollouts-for-feature-tests.md)
      + [提供個人化](implement/server-side/sdk-guides/on-device-decisioning/deliver-personalization.md)
      + [支援的功能概述](implement/server-side/sdk-guides/on-device-decisioning/supported-features.md)
      + [裝置上決策疑難排解](implement/server-side/sdk-guides/on-device-decisioning/troubleshooting.md)
      + [最佳實務](implement/server-side/sdk-guides/best-practices/best-practices.md)
   + Node.js SDK參考 {#node-js}
      + [Node.js SDK概觀](implement/server-side/node-js/overview.md)
      + [安裝Node.js SDK](implement/server-side/node-js/install-sdk.md)
      + [初始化Node.js SDK](implement/server-side/node-js/initialize-sdk.md)
      + [取得選件(Node.js)](implement/server-side/node-js/get-offers.md)
      + [取得屬性(Node.js)](implement/server-side/node-js/get-attributes.md)
      + [傳送通知(Node.js)](implement/server-side/node-js/send-notifications.md)
      + [SDK事件(Node.js)](implement/server-side/node-js/sdk-events.md)
      + [記錄器(Node.js)](implement/server-side/node-js/logger.md)
      + [Proxy設定(Node.js)](implement/server-side/node-js/proxy-configuration.md)
   + Java SDK參考 {#java}
      + [Java SDK概觀](implement/server-side/java/overview.md)
      + [安裝Java SDK](implement/server-side/java/install-sdk.md)
      + [初始化Java SDK](implement/server-side/java/initialize-sdk.md)
      + [取得選件(Java)](implement/server-side/java/get-offers.md)
      + [取得屬性(Java)](implement/server-side/java/get-attributes.md)
      + [傳送通知(Java)](implement/server-side/java/send-notifications.md)
      + [SDK Events (Java)](implement/server-side/java/sdk-events.md)
      + [Logger (Java)](implement/server-side/java/logger.md)
      + [非同步請求(Java)](implement/server-side/java/asynchronous-requests.md)
      + [Proxy設定(Java)](implement/server-side/java/proxy-configuration.md)
      + [自訂HTTP使用者端設定(Java)](implement/server-side/java/custom-http-client.md)
      + [公用程式方法(Java)](implement/server-side/java/utility-methods.md)
   + .NET SDK參考 {#net}
      + [.NET SDK概觀](implement/server-side/net/overview.md)
      + [安裝.Net SDK](implement/server-side/net/install-sdk.md)
      + [初始化.NET SDK](implement/server-side/net/initialize-sdk.md)
      + [取得選件(.NET)](implement/server-side/net/get-offers.md)
      + [取得屬性(.NET)](implement/server-side/net/get-attributes.md)
      + [傳送通知(.NET)](implement/server-side/net/send-notifications.md)
      + [SDK事件(.NET)](implement/server-side/net/sdk-events.md)
      + [非同步要求(.NET)](implement/server-side/net/asynchronous-requests.md)
   + Python SDK參考 {#python}
      + [Python SDK概觀](implement/server-side/python/overview.md)
      + [安裝Python SDK](implement/server-side/python/install-sdk.md)
      + [初始化Python SDK](implement/server-side/python/initialize-sdk.md)
      + [取得選件(Python)](implement/server-side/python/get-offers.md)
      + [取得屬性(Python)](implement/server-side/python/get-attributes.md)
      + [傳送通知(Python)](implement/server-side/python/send-notifications.md)
      + [SDK事件(Python)](implement/server-side/python/sdk-events.md)
      + [非同步請求(Python)](implement/server-side/python/asynchronous-requests.md)
      + [記錄器(Python)](implement/server-side/python/logger.md)
+ [混合實施](implement/hybrid/hybrid-overview.md)
+ [Recommendations實施](implement/recommendations/recommendations.md)
+ [Recommendations實作Beta版](/help/dev/implement/recommendations/recommendations-beta.md)
+ 行動應用程式實施 {#mobile-apps}
   + [適用於行動應用程式的 Target 概觀](implement/mobile/overview.md)
   + [Target 行動裝置預覽](implement/mobile/target-mobile-preview.md)
   + [使用定位服務](implement/mobile/use-location-service.md)
   + [適用於行動應用程式的 Target 常見問答](implement/mobile/mobile-faq.md)
   + [透過AEP Mobile SDK，在具有Web檢視的原生應用程式中實作Target](/help/dev/implement/mobile/native-app.md)
+ 電子郵件實作 {#implement-email}
   + [電子郵件：實作 Target 概觀](implement/email/overview.md)
   + [為影像建立 Adbox](implement/email/testing-content-with-the-adbox.md)
   + [測試電子郵件影像 Adbox](implement/email/testing-email-image-adbox.md)
   + [使用重導程式](implement/email/working-with-redirectors.md)
+ API指南 {#api}
   + [Target API總覽](/help/dev/before-administer/target-api-overview.md)
   + [設定Target API的驗證](/help/dev/before-administer/configure-authentication.md)
   + 傳送API指南 {#delivery-api}
      + [傳送API總覽](/help/dev/implement/delivery-api/overview.md)
      + [與傳送API互動的SDK](/help/dev/before-implement/delivery-api-overview/sdks.md)
      + [快速入門](/help/dev/before-implement/delivery-api-overview/getting-started.md)
      + [使用者許可權(Premium)](/help/dev/before-implement/delivery-api-overview/user-permissions.md)
      + [識別訪客](/help/dev/before-implement/delivery-api-overview/identifying-visitors.md)
      + [單一或批次傳遞](/help/dev/before-implement/delivery-api-overview/single-or-batch.md)
      + [預先擷取](/help/dev/before-implement/delivery-api-overview/prefetch.md)
      + [通知](/help/dev/before-implement/delivery-api-overview/notifications.md)
      + [與Experience Cloud整合](before-implement/delivery-api-overview/integration.md)
      + [考量事項和已知限制](/help/dev/before-implement/delivery-api-overview/known-limitations.md)
      + [使用者端提示](/help/dev/before-implement/delivery-api-overview/client-hints.md)
      + [傳送 API](/help/dev/implement/delivery-api/delivery-api.md)
   + 管理API {#admin-api}
      + [管理API總覽](before-administer/admin-api-overview/admin-api-overview.md)
      + [Adobe Target管理API](/help/dev/administer/admin-api/admin-api-overview-new.md)
   + 設定檔API {#profile-apis}
      + [設定檔API總覽](/help/dev/administer/profile-api/profiles-api.md)
      + [擷取設定檔](/help/dev/administer/profile-api/profile-fetch.md)
      + [更新設定檔](/help/dev/administer/profile-api/profile-api-overview.md)
      + [單一設定檔更新API](/help/dev/administer/profile-api/profile-single-api.md)
      + [大量設定檔更新API](/help/dev/administer/profile-api/profile-bulk-api.md)
   + [報告 API](/help/dev/administer/reporting-api/reporting-api.md)
   + Recommendations API {#recommendations-api}
      + [Recommendations API概覽](before-administer/recs-api/overview.md)
      + [使用API管理您的目錄](before-administer/recs-api/manage-catalog.md)
      + [管理自訂條件](before-administer/recs-api/manage-custom-criteria.md)
      + [搭配建議使用傳送API](before-administer/recs-api/fetch-recs-server-side-delivery-api.md)
      + [Recommendations API](/help/dev/administer/recommendations-api/recommendations-api.md)
   + 模型API {#models-api}
      + [模型API （加入封鎖清單）概觀](before-administer/models-api.md)
      + [模型API](/help/dev/administer/models-api/models-api-overview.md)
   + [ADOBE ADMIN CONSOLE API](/help/dev/before-implement/delivery-api-overview/adobe-console-api.md)
   + [Adobe Experience Platform Edge Network伺服器API](/help/dev/before-implement/delivery-api-overview/aep-edge-network-server-api.md)
+ 實作模式 {#implementation-patterns}
   + [實作模式概觀](/help/dev/patterns/pattern-overview.md)
   + 使用at.js的Recommendations實施模式 {#atjs}
      + [使用at.js的Recommendations實施模式概覽](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md)
      + [初始化SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md)
      + [設定資料彙集](/help/dev/patterns/recs-atjs/data-collection.md)
      + [演算體驗](/help/dev/patterns/recs-atjs/render-experiences.md)
      + [通知Target](/help/dev/patterns/recs-atjs/notify-target.md)



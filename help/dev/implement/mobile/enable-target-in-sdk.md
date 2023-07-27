---
keywords: 行動應用程式, 行動應用程式 sdk, target 行動應用程式, 行動 target sdk, 行動應用程式 sdk, 在 sdk 中啟用 target
description: 瞭解如何將Adobe Mobile Services SDK新增至您的行動應用程式。
title: 如何啟用 [!DNL Target] 在 [!DNL Adobe Mobile SDK]？
feature: Implement Mobile
exl-id: 4263b96a-23c8-4513-8302-00080122181d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 49%

---

# 啟用 [!DNL Target] 在SDK中

新增 [!UICONTROL AdobeMobile Services SDK] 至您的應用程式。

>[!IMPORTANT]
>
>支援 [!DNL Adobe Mobile] 版本4。*x* SDK已於2021年8月31日結束，不建議再用於 [!DNL Adobe Target] 行動使用者。
>
>此 [適用於行動應用程式的Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} 是推薦的供電解決方案 [!DNL Adobe Experience Cloud] 行動應用程式中的解決方案和服務。

1. 如果您尚未在您的應用程式中安裝 Adobe Mobile Services SDK，請使用您的 Analytics 或 Experience Cloud 憑證，並從 [Adobe Mobile Services](https://mobilemarketing.adobe.com/) 網站下載 SDK。

1. 新增 [!DNL Adobe Mobile Services SDK] 至您的應用程式。

   您可以在[「核心實施和生命週期」](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/dev-qs.html)下找到說明。

1. 新增用戶端代碼、逾時和啟用 SSL。

   在 Experience Cloud 中，開啟 Mobile Services，然後前往&#x200B;**[!UICONTROL 「管理應用程式設定」]**>**[!UICONTROL 「SDK Target 選項」]**。

   新增您的 [!DNL Target] clientcode和逾時。 clientcode 是您的帳戶或公司所特有。逾時是以秒為單位的時間，直到 [!DNL Target] 將會等待回應，再顯示預設內容。 確定已在 Adobe Mobile Services 的「管理應用程式設定」頁面中勾選&#x200B;**[!UICONTROL 「使用 HTTPS」]**&#x200B;選項。iOS如果未啟用HTTPS，除非您將 [!DNL Target] 伺服器。

   ![替代影像](assets/mobile-clientcode.png)

1. 建立/找到應用程式後，請尋找應用程式設定並下載所需的SDK。

   ![替代影像](assets/download-sdk.png)

>[!WARNING]
>
> 如果您無法存取行動行銷介面，可以直接在您的應用程式代碼的配置檔案中進行變更; 不過，它不會與使用者介面中的設定頁面同步。

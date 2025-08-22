---
title: Adobe Target傳送API概觀
description: Adobe Target傳送API概觀
keywords: 傳送api
exl-id: e760bddc-b1ae-4b7b-bff2-aba81c6b6d34
feature: APIs/SDKs
source-git-commit: ccc27e66207e58dcd33865e5d28a51644e8e1931
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# 傳送API總覽

[!DNL Adobe Target Delivery API]是以REST為基礎。 本檔案說明構成[!DNL Adobe Target] [!DNL Delivery API]的資源。 HTTP方法可用來對這些資源執行作業。

使用[!UICONTROL Adobe Target's Delivery API]，您可以：

* 跨網路提供體驗，包括SPA和行動頻道，以及非瀏覽器型IoT裝置，例如連線電視、資訊站或店內數位熒幕。
* 從任何可進行HTTP/s呼叫的伺服器端平台或應用程式提供體驗。
* 無論使用者使用哪些管道或裝置與您的業務互動，皆可為使用者提供一致且個人化的體驗。
* 快取伺服器中工作階段內使用者的體驗，以避免多個API呼叫，進而獲得更好的效能。
* 從伺服器端順暢地與[!DNL Adobe Experience Cloud]產品（例如[!DNL Adobe Analytics]、[!DNL Adobe Audience Manager]和[!DNL Experience Cloud ID Service]）整合。

>[!IMPORTANT]
>
>透過[!DNL Recommendations]更新您的[!UICONTROL Catalog] [!DNL Delivery API]時請小心。 [!DNL Delivery API]是公開的，因此請避免使用它來填入建議目錄中的可點按專案。 這樣做可能會引入失效的內容並汙染您的目錄。
>
>最佳實務：
>
>僅使用[!DNL Delivery API]來更新符合下列條件的目錄屬性：
>* 經常變更（例如價格、庫存量）。
>* 遵循可在您的網站上輕鬆驗證的預定義格式。
>* 請勿將其用於新增或修改可點按專案或其他未經驗證的內容。
>
>如有需要，您可以透過傳送API請求客戶支援以停用目錄更新。

如需詳細資訊，請參閱[[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank}檔案。

>[!NOTE]
>
>您仍然可以存取[舊版/v1/mbox和/v2/batchmbox API檔案](https://developers.adobetarget.com/api/legacy-api/index.html)。 不過，功能將會在傳送API中開發（如此處的紀錄），不會後援至舊版API。

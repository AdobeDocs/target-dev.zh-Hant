---
keywords: 實作、實施、設定、設定、單一設定檔更新
description: 使用單一設定檔更新API將資料匯入 [!DNL Target] 。
title: 如何使用[!UICONTROL Single Profile Update API]將資料匯入 [!DNL Target] ？
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 3%

---

# [!UICONTROL Single Profile Update API]

[!DNL Adobe Target] [!UICONTROL Single Profile Update API]可讓您傳送單一使用者的設定檔更新。 [!UICONTROL Single Profile Update API]幾乎與[!UICONTROL Bulk Profile Update API]相同，但一次會更新一個訪客設定檔，內嵌於API呼叫而非.cvs檔案。

[!UICONTROL Single Profile Update API]和通常用於必須發生與尚未實作[!DNL Target]的管道中發生的交易相關的更新時。 例如，您想要更新執行某些離線動作之單一訪客的設定檔。 動作包括聯絡客服中心、提供貸款、在商店中使用忠誠卡、存取資訊站等。

將[!UICONTROL Single Profile Update API]與[[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)對照。

## 資源

如需詳細資訊，請參閱：

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)

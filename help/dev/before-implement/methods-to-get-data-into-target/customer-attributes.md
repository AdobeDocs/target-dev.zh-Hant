---
keywords: 實作、實施、設定、設定、客戶屬性
description: 使用客戶屬性將資料匯入 [!DNL Target] 。
title: 如何使用客戶屬性將資料帶入 [!DNL Target] ？
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
TQID: https://experienceleague.adobe.com/bzK915y7fvjfZjTkSK2QWHDzmIN9SdAQiEguiUlc-r8
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 216
ht-degree: 13%

---

# 客戶屬性

客戶屬性可讓您透過FTP將訪客設定檔資料上傳至[!DNL Adobe Experience Cloud]。 上傳後，請使用[!DNL Adobe Analytics]和[!DNL Adobe Target]中的資料。

Target Standard客戶可以套用五個屬性，[!DNL Target Premium]客戶可以套用200個屬性。

## 格式

具有[!DNL Experience Cloud] ID (ECID)和屬性名稱/值配對的.csv檔案會透過FTP或手動在Experience Cloud UI中上傳。

## 範例使用案例

您的CRM或其他內部系統儲存了您想要與[!DNL Adobe Experience Cloud]共用的重要資訊，包括[!DNL Target]和[!DNL Analytics]。

## 方法的優點

上傳客戶資料會在Target中為該訪客建立設定檔專案，即使[!DNL Target]尚未看到該訪客。

[!DNL Target]和[!DNL Analytics]中會自動提供相同的資料。

FTP 是比 API 更簡單的實作方法。

## 注意事項

Target Standard客戶可以套用五個屬性，[!DNL Target Premium]客戶可以套用200個屬性

只能透過「客戶屬性」來更新值，而不是在頁面上更新。

需要 Experience Cloud ID (ECID) 實作。

## 程式碼範例

在[建立客戶屬性來源及上傳資料檔案](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=zh-Hant)中可以找到詳細資料。

### 相關資訊的連結

[建立客戶屬性來源及上傳資料檔案](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=zh-Hant)。

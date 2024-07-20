---
keywords: 實作、實施、設定、設定、客戶屬性
description: 使用客戶屬性將資料匯入 [!DNL Target] 。
title: 如何使用客戶屬性將資料帶入 [!DNL Target] ？
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 14%

---

# 客戶屬性

客戶屬性可讓您透過FTP將訪客設定檔資料上傳至[!DNL Adobe Experience Cloud]。 上傳後，請使用[!DNL Adobe Analytics]和[!DNL Adobe Target]中的資料。

Target Standard客戶可以套用五個屬性，[!DNL Target Premium]客戶可以套用200個屬性。

## 格式

具有[!DNL Experience Cloud]個ID (ECID)和屬性名稱/值組的.csv檔案會透過FTP上傳或手動在Experience CloudUI中上傳。

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

在[建立客戶屬性來源及上傳資料檔案](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html)中可以找到詳細資料。

### 相關資訊的連結

[建立客戶屬性來源及上傳資料檔案](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html)。

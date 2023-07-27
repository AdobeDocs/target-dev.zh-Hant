---
keywords: 實作、實施、設定、設定、客戶屬性
description: 將資料匯入 [!DNL Target] 使用客戶屬性。
title: 如何將資料帶入 [!DNL Target] 使用客戶屬性？
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 24%

---

# 客戶屬性

客戶屬性可讓您透過 FTP 將訪客設定檔資料上傳至 [!DNL Adobe Experience Cloud]. 上傳後，即可在以下位置使用這些資料： [!DNL Adobe Analytics] 和 [!DNL Adobe Target].

Target Standard客戶可套用五個屬性： [!DNL Target Premium] 客戶可套用200個屬性。

## 格式

具有的.csv檔案 [!DNL Experience Cloud] ID (ECID)和屬性名稱/值配對會透過FTP上傳，或在Experience CloudUI中手動上傳。

## 範例使用案例

您的CRM或其他內部系統會儲存您想要與其分享的重要資訊 [!DNL Adobe Experience Cloud]，包括 [!DNL Target] 和 [!DNL Analytics].

## 方法的優點

上傳客戶資料會在Target中為該訪客建立設定檔專案，即使 [!DNL Target] 尚未看到訪客。

相同資料會自動出現在 [!DNL Target] 和 [!DNL Analytics].

FTP 是比 API 更簡單的實作方法。

## 注意事項

Target Standard客戶可套用五個屬性： [!DNL Target Premium] 客戶可套用200個屬性

只能透過「客戶屬性」來更新值，而不是在頁面上更新。

需要 Experience Cloud ID (ECID) 實作。

## 程式碼範例

詳情請參閱 [建立客戶屬性來源及上傳資料檔案](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).

### 相關資訊的連結

[建立客戶屬性來源及上傳資料檔案](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).

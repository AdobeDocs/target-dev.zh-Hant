---
keywords: 實作，實施，設定，設定，大量設定檔更新api
description: 將資料匯入 [!DNL Target] 使用 [!UICONTROL 大量設定檔更新API].
title: 如何將資料帶入 [!DNL Target] 使用 [!UICONTROL 大量設定檔更新API]？
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: af9db32d59bdf32f2b9fade267922803250377dd
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 23%

---

# 大量設定檔更新API

透過API傳送.csv檔案至 [!DNL Adobe Target] 更新許多訪客的訪客設定檔。 一次呼叫就能以多個頁面內設定檔屬性來更新每一個訪客設定檔。

此選項類似於 [!UICONTROL 客戶屬性] 但有一些差異：

* [!UICONTROL 客戶屬性] 使用FTP上傳。此 [!UICONTROL 目標大量設定檔更新API] 使用HTTPPOSTAPI。
* [!UICONTROL 客戶屬性] 資料可與以下專案共用： [!DNL Analytics]. [!UICONTROL 大量設定檔更新] 只能用於 [!DNL Target].
* [!UICONTROL 客戶屬性] 支援為使用者建立設定檔 [!DNL Target] 尚未看見。
   * [!UICONTROL 大量設定檔更新API] v2：您不需要指定每個引數的所有引數值 `pcId`. 已為任何專案建立設定檔 `pcId` 或 `mbox3rdPartyId` 在中找不到 [!DNL Target].
   * [!UICONTROL 大量設定檔更新API] v1：此 [!UICONTROL 大量設定檔更新API] 現有更新 [!DNL Target] 僅限設定檔。 如果您使用v1，則不會為遺失建立設定檔 `pcIds` 或 `mbox3rdPartyIds`.
* [!UICONTROL 客戶屬性] 需要使用 [!UICONTROL EXPERIENCE CLOUDID] (ECID)和來源ID的使用，例如CRM ID或忠誠度ID。
* 此 [!UICONTROL 大量設定檔更新API] 需要TNT ID或 `mbox3rdPartyId`.
* 您無法在 `mbox3rdPartyID` 中傳送下列字元: 加號 (+) 和正斜線 (/)。

## 格式

.csv檔案必須透過參照以下每個訪客： [!DNL Target] PCID或 `mbox3rdPartyId`. 此 [!UICONTROL EXPERIENCE CLOUDID] 不支援(ECID)。 所有設定檔屬性/值都是透過 API 來建立和更新。API 文件中提供格式詳細資料。

## 範例使用案例

您的CRM或其他內部系統會儲存您想要一致更新到中的訪客寶貴資料 [!DNL Target]，而不需在頁面實作中公開設定檔資料。

## 方法的優點

* 設定檔屬性的數量不限。
* 透過網站傳送的設定檔屬性可以透過API更新，反之亦然。

## 注意事項

* 批次檔的大小必須小於 50 MB。此外，每次上傳的總列數不得超過 500,000 列。
* 更新通常在一小時內發生，但可能需要長達24小時的時間才會反映。
* 您可以上傳後續批次中超過24小時期間的一或多列數量沒有限制。 不過，在上班時間可以節流汲取程序，以確保其他程序的執行效率。
* 對相同的，在之間不使用mbox呼叫的連續V2批次更新呼叫 `thirdPartyIds` 覆寫在第一次批次更新呼叫中更新的屬性。

## 資源

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
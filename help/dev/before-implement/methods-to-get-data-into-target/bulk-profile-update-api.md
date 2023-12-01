---
keywords: 實作，實施，設定，設定，大量設定檔更新api
description: 將資料匯入 [!DNL Target] 使用 [!UICONTROL 大量設定檔更新API].
title: 如何將資料帶入 [!DNL Target] 使用 [!UICONTROL 大量設定檔更新API]？
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 43f4fb8345a77ccb0e112fe196e7e0944cc468c9
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 4%

---

# 大量設定檔更新API

此 [!DNL Adobe Target] [!UICONTROL 大量設定檔更新API] 可讓您使用批次檔案大量更新網站多個訪客的使用者設定檔。

使用 [!UICONTROL 大量設定檔更新API]，您可以方便的以設定檔引數的形式傳送詳細的訪客設定檔資料給許多使用者 [!DNL Target] 來自任何外部來源。 外部來源可能包括客戶關係管理(CRM)或銷售點(POS)系統，這些通常無法在網頁上使用。

## [!UICONTROL 客戶屬性] 與 [!UICONTROL 大量設定檔更新API]

此選項類似於 [!UICONTROL 客戶屬性] 但有一些差異：

* [!UICONTROL 客戶屬性] 使用FTP上傳。此 [!UICONTROL 目標大量設定檔更新API] 使用HTTPPOSTAPI。
* [!UICONTROL 客戶屬性] 資料可與以下專案共用： [!DNL Analytics]. 此 [!UICONTROL 大量設定檔更新] 只能用於 [!DNL Target].
* [!UICONTROL 客戶屬性] 支援為使用者建立設定檔 [!DNL Target] 尚未看見。
   * [!UICONTROL 大量設定檔更新API] v2：您不需要指定每個引數的所有引數值 `pcId`. 已為任何專案建立設定檔 `pcId` 或 `mbox3rdPartyId` 在中找不到 [!DNL Target].
   * [!UICONTROL 大量設定檔更新API] v1：此 [!UICONTROL 大量設定檔更新API] 現有更新 [!DNL Target] 僅限設定檔。 如果您使用v1，則不會為遺失建立設定檔 `pcIds` 或 `mbox3rdPartyIds`.
* [!UICONTROL 客戶屬性] 需要使用 [!UICONTROL EXPERIENCE CLOUDID] (ECID)和來源ID的使用，例如CRM ID或忠誠度ID。
* 此 [!UICONTROL 大量設定檔更新API] 需要TNT ID或 `mbox3rdPartyId`.
* 您無法在 `mbox3rdPartyID` 中傳送下列字元: 加號 (+) 和正斜線 (/)。

## 資源

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
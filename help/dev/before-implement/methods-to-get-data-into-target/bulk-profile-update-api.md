---
keywords: 實作，實施，設定，設定，大量設定檔更新api
description: 使用[!UICONTROL Bulk Profile Update API]將資料匯入 [!DNL Target] 。
title: 如何使用[!UICONTROL Bulk Profile Update API]將資料匯入 [!DNL Target] ？
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 7%

---

# 大量設定檔更新API

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API]可讓您使用批次檔案，大量更新多個網站訪客的使用者設定檔。

使用[!UICONTROL Bulk Profile Update API]，您可以方便地將許多使用者的詳細訪客設定檔資料以設定檔引數的形式從任何外部來源傳送到[!DNL Target]。 外部來源可能包括客戶關係管理(CRM)或銷售點(POS)系統，這些通常無法在網頁上使用。

將[!UICONTROL Bulk Profile Update API]與[[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)對照。

## [!UICONTROL Customer attributes]與[!UICONTROL Bulk Profile Update API]

此選項與[[!UICONTROL customer attributes]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md)類似，但有一些差異：

* [!UICONTROL Customer attributes]使用FTP上傳。 [!UICONTROL Target Bulk Profile Update API]使用HTTPPOSTAPI。
* [!UICONTROL Customer attribute]資料可以與[!DNL Analytics]共用。 [!UICONTROL Bulk Profile Update]只能在[!DNL Target]中使用。
* [!UICONTROL Customer attributes]支援為使用者[!DNL Target]建立設定檔，目前尚未顯示。
   * [!UICONTROL Bulk Profile Update API] v2：您不需要為每個`pcId`指定所有引數值。 已為[!DNL Target]中找不到的任何`pcId`或`mbox3rdPartyId`建立設定檔。
   * [!UICONTROL Bulk Profile Update API] v1： [!UICONTROL Bulk Profile Update API]僅更新現有的[!DNL Target]設定檔。 如果您使用v1，則不會為遺失`pcIds`或`mbox3rdPartyIds`建立設定檔。
* [!UICONTROL Customer attributes]需要使用[!UICONTROL Experience Cloud ID] (ECID)和使用來源ID，例如CRM ID或忠誠度ID。
* [!UICONTROL Bulk Profile Update API]需要TNT ID或`mbox3rdPartyId`。
* 您無法在 `mbox3rdPartyID` 中傳送下列字元: 加號 (+) 和正斜線 (/)。

## 資源

如需詳細資訊，請參閱：

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
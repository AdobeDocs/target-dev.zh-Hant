---
keywords: 實作，實施，設定，設定，大量設定檔更新api
description: 使用[!UICONTROL 大量設定檔更新API]將資料匯入 [!DNL Target] 。
title: 如何使用[!UICONTROL 大量設定檔更新API]將資料匯入 [!DNL Target] ？
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
TQID: https://experienceleague.adobe.com/vBcIsBR6wwYr7MbRh7UJvt71ULDEq6iXVZ5spZlif-0
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 284
ht-degree: 5%

---

# 大量設定檔更新API

[!DNL Adobe Target] [!UICONTROL 大量設定檔更新API]可讓您使用批次檔案，大量更新多個網站訪客的使用者設定檔。

使用[!UICONTROL 大量設定檔更新API]，您可以從任何外部來源以設定檔引數的形式，將許多使用者的詳細訪客設定檔資料傳送至[!DNL Target]。 外部來源可能包括客戶關係管理(CRM)或銷售點(POS)系統，這些通常無法在網頁上使用。

將[!UICONTROL 大量設定檔更新API]與[[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)對照。

## [!UICONTROL 客戶屬性]與[!UICONTROL 大量設定檔更新API]

此選項與[[!UICONTROL 客戶屬性]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md)類似，但有一些差異：

* [!UICONTROL 客戶屬性]使用FTP上傳。 [!UICONTROL 目標大量設定檔更新API]使用HTTP POST API。
* [!UICONTROL 客戶屬性]資料可以與[!DNL Analytics]共用。 [!UICONTROL 大量設定檔更新]只能在[!DNL Target]中使用。
* [!UICONTROL 客戶屬性]支援為使用者[!DNL Target]建立設定檔，目前尚未顯示。
   * [!UICONTROL 大量設定檔更新API] v2：您不需要為每個`pcId`指定所有引數值。 已為[!DNL Target]中找不到的任何`pcId`或`mbox3rdPartyId`建立設定檔。
   * [!UICONTROL 大量設定檔更新API] v1： [!UICONTROL 大量設定檔更新API]只會更新現有的[!DNL Target]設定檔。 如果您使用v1，則不會為遺失`pcIds`或`mbox3rdPartyIds`建立設定檔。
* [!UICONTROL 客戶屬性]需要使用[!UICONTROL Experience Cloud ID] (ECID)和使用來源ID，例如CRM ID或忠誠度ID。
* [!UICONTROL 大量設定檔更新API]需要TNT ID或`mbox3rdPartyId`。
* 您無法在 `mbox3rdPartyID` 中傳送下列字元: 加號 (+) 和正斜線 (/)。

## 資源

如需詳細資訊，請參閱：

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
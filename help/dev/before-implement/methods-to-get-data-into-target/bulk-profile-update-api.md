---
keywords: 實作、實施、設定、設定、大量設定檔更新
description: 將資料匯入 [!DNL Target] 使用大量設定檔更新API。
title: 如何將資料帶入 [!DNL Target] 是否要使用大量設定檔更新API？
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 3ae2391dea9994c0ddc1df39d74cccf6e067c1a4
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 57%

---

# 大量設定檔更新 API

透過API傳送.csv檔案至 [!DNL Adobe Target] 更新許多訪客的訪客設定檔。 一次呼叫就能以多個頁面內設定檔屬性來更新每一個訪客設定檔。

此選項與客戶屬性類似，但有一些差異：

* 客戶屬性會使用FTP上傳，而 [!UICONTROL 目標大量設定檔更新API] 使用HTTPPOSTAPI。
* 客戶屬性資料可與共用 [!DNL Analytics]. [!UICONTROL 大量設定檔更新只能 Target 中使用。]
* 客戶屬性支援為使用者建立設定檔 [!DNL Target] 尚未看見。 大量設定檔更新API更新已存在 [!DNL Target] 僅限設定檔。
* 客戶屬性需使用Experience CloudID (ECID)和來源ID （例如CRM ID或忠誠度ID）。
* 「大量設定檔更新 API」需要 TNT ID 或 `mbox3rdPartyId`。
* 您無法在 `mbox3rdPartyID` 中傳送下列字元: 加號 (+) 和正斜線 (/)。

## 格式

.csv檔案必須透過訪客參照每個訪客 [!DNL Target] PCID或 `mbox3rdPartyId`. 不支援 Experience Cloud ID (ECID)。所有設定檔屬性/值都是透過 API 來建立和更新。API 文件中提供格式詳細資料。

## 範例使用案例

CRM 或其他內部系統中儲存關於訪客的實用資料 (您希望持續更新至 Target)，不會讓設定檔資料在頁面實作中曝光。

## 方法的優點

設定檔屬性的數量不限。

透過網站傳送的設定檔屬性可以透過 API 更新，反之亦然。

## 注意事項

批次檔的大小必須小於 50 MB。此外，每次上傳的總列數不得超過 500,000 列。

更新通常在一小時內發生，但可能需要長達24小時的時間才會反映

在 24 小時期間內，不限制後續批次中可上傳的列數。不過，在上班時間可以節流汲取程序，以確保其他程序的執行效率。

對相同的 thirdPartyIds 採用連續 [V2 批次更新呼叫](https://developers.adobetarget.com/api/#updating-profiles)，且其中不需使用 mbox 呼叫，會覆寫第一次批次更新呼叫所更新的屬性。

## 程式碼範例

請參閱[更新設定檔](https://developers.adobetarget.com/api/#updating-profiles)。

### 相關資訊的連結

[更新設定檔](https://developers.adobetarget.com/api/#updating-profiles)

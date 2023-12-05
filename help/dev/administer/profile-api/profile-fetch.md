---
title: 擷取設定檔
description: 瞭解如何使用Adobe Target設定檔API來擷取訪客資料，以便用於 [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
source-git-commit: ee53a8f0210480d9b70dc77a3a5cd8d92d2f2e3d
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# 更新設定檔

A [!DNL Target] 有兩種方式可擷取設定檔：使用 `tntid` 或 `thirdPartyId`.

## 使用tntid

[!DNL Target] 自動指派 `tntid` 適用於每個請求。

以下範例顯示使用擷取設定檔的要求格式 `tntid`：

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

取代 `<your-client-code>` 和 `your-tnt-id` 並引發GET要求。 以下是使用的設定檔擷取呼叫範例 `tntid`；

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## 使用thirdPartyId

[!DNL Adobe Target] 設定檔可使用您自己的識別碼來擴充(例如：CRM id、 `uuid`、會籍編號等)。

另請參閱 [更新設定檔](/help/dev/administer/profile-api/profile-api-overview.md) 以瞭解如何附加 `thirdPartyId` 至您的設定檔。

以下範例顯示使用擷取設定檔的要求格式 `thirdPartyId`：

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

取代 `<your-client-code>` 和 `your-thirdpartyid` 並引發GET要求。 以下是使用的設定檔擷取呼叫範例 [!UICONTROL thirdpartyid]：

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

進行此呼叫時， [!DNL Target] 會先嘗試在邊緣請求中所述的叢集中找出設定檔，或設定檔所在的任何位置並傳回內容。 設定檔內容會以JSON格式傳回。

## 驗證

此 [!DNL Target Profile API] 可以透過開啟驗證來保護 [!DNL Target] UI，如此處所述。 開啟驗證後，所有設定檔API請求必須在請求標頭中設定設定檔驗證Token。 代號本身可使用 [!DNL Target] UI或使用中所述步驟 [設定檔驗證Token](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank} 區段。

## 測光

這些呼叫不會計入您的mbox呼叫。

## 錯誤處理

若是呼叫 `/thirdPartyId` 具有無效或已過期 `thirdPartyId` 已指定：

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

如果設定檔找不到或已過期：

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```

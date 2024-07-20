---
title: 擷取設定檔
description: 瞭解如何使用Adobe Target設定檔API來擷取訪客資料，以用於 [!DNL Target]。
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: b422ae68-49b3-4d60-9ea4-0fa67b6934b0
source-git-commit: b8ccfdcaff6aa17a325727df0a9ffd716e44519b
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---

# 擷取設定檔

可透過三種方式擷取[!DNL Target]設定檔：使用`[!DNL Experience Cloud Visitor ID]` (`ECID`)、`tntid`或`thirdPartyId`。

## 使用[!DNL Experience Cloud Visitor ID] (ECID)

您可以根據`ECID`擷取設定檔。 HTTP方法必須GET。

URL看起來像下面的範例：

```
https://<clientCode>.tt.omtrdc.net/rest/v1/profiles/marketingCloudVisitorId/<ECID>?client=<clientCode>
```

將`<clientCode>`取代為您的[!DNL Target] [!UICONTROL Client Code]，並將`<ECID>`取代為您的[!DNL Experience Cloud Visitor ID] ([!DNL Marketing Cloud Visitor ID])。

## 使用tntid

[!DNL Target]會自動為每個請求指派`tntid`。

下列範例顯示使用`tntid`擷取設定檔的要求格式：

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

取代`<your-client-code>`和`your-tnt-id`並引發GET要求。 以下是使用`tntid`的設定檔擷取呼叫範例：

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## 使用thirdPartyId

[!DNL Adobe Target]設定檔可使用您自己的識別碼來擴充（例如： CRM ID、`uuid`、會員編號等）。

請參閱[更新設定檔](/help/dev/administer/profile-api/profile-api-overview.md)以瞭解如何將`thirdPartyId`附加至您的設定檔。

下列範例顯示使用`thirdPartyId`擷取設定檔的要求格式：

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

取代`<your-client-code>`和`your-thirdpartyid`並引發GET要求。 以下是使用[!UICONTROL thirdpartyid]的設定檔擷取呼叫範例：

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

進行此呼叫時，[!DNL Target]會先嘗試在邊緣要求中所述的叢集中找出設定檔，或找出設定檔所在的位置，然後傳回內容。 設定檔內容會以JSON格式傳回。

## 驗證

從[!DNL Target] UI開啟驗證即可保護[!DNL Target Profile API]，如下所述。 開啟驗證後，所有設定檔API請求必須在請求標頭中設定設定檔驗證Token。 權杖本身可以使用[!DNL Target] UI或上述[設定檔驗證Token](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank}區段中說明的步驟來產生。

## 測光

這些呼叫不會計入您的mbox呼叫。

## 錯誤處理

若是呼叫`/thirdPartyId`，但指定了無效或過期的`thirdPartyId`：

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

如果設定檔找不到或已過期：

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```

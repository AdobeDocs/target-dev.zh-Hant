---
title: Adobe Target大量設定檔更新API
description: 瞭解如何使用 [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API]將多位訪客的設定檔資料傳送至 [!DNL Target] 以用於目標定位。
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 0f38d109-5273-4f73-9488-80eca115d44d
source-git-commit: 39f0ab4a6b06d0b3415be850487552714f51b4a2
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 7%

---

# [!DNL Adobe Target Bulk Profile Update API]

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API]可讓您使用批次檔案，大量更新多個網站訪客的使用者設定檔。

使用[!UICONTROL Bulk Profile Update API]，您可以方便地將許多使用者的詳細訪客設定檔資料以設定檔引數的形式從任何外部來源傳送到[!DNL Target]。 外部來源可能包括客戶關係管理(CRM)或銷售點(POS)系統，這些通常無法在網頁上使用。

| 版本 | URL範例 | 功能 |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/profile/batchUpdate` | 僅支援大量設定檔更新。 |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate` | <ul><li>如果找不到，請建立設定檔。</li><li>每列狀態更新。</li></ul> |

>[!NOTE]
>
>[!DNL Bulk Profile Update API]的版本2 (v2)是目前的版本。 不過，[!DNL Target]仍持續支援版本1 (v1)。
>
>* **不依賴`PCID`的獨立實作，請使用版本2**：如果您的[!DNL Target]實作使用[!DNL Experience Cloud ID] (ECID)作為匿名訪客的設定檔識別碼之一，您不得使用`pcId`作為版本2 (v2)批次檔案中的金鑰。 將`pcId`與[!DNL Bulk Profile Update API]的版本2搭配使用，是針對不依賴[!DNL Target]的獨立的`ECID`實作。
>
>* **依賴`thirdPartID`的實作，使用版本1**：如果您想要在批次檔案中使用`ECID`做為金鑰，使用`pcId`進行設定檔識別的實作應使用API版本1 (v1)。 如果您的實作使用`thirdPartyId`來識別設定檔，則建議使用`thirdPartyId`作為索引鍵的第2版(v2)。

## [!UICONTROL Bulk Profile Update API]的優點

* 設定檔屬性的數量不限。
* 透過網站傳送的設定檔屬性可以透過API更新，反之亦然。

## 注意事項

* 批次檔的大小必須小於 50 MB。此外，每次上傳的總列數不得超過 500,000 列。
* 更新通常在一小時內發生，但可能需要長達24小時的時間才會反映。
* 您可以上傳後續批次中超過24小時期間的一或多列數量沒有限制。 不過，在上班時間可以節流汲取程序，以確保其他程序的執行效率。
* 對相同的thirdPartyIds採用連續v2批次更新呼叫，且其中不需使用mbox呼叫，會覆寫第一次批次更新呼叫所更新的屬性。
* [!DNL Adobe]不保證100%的批次設定檔資料將上線並保留在Target中，因此可用於目標定位。 在目前的設計中，小部分資料（最多佔大型生產批次的0.1%）有可能不會上線或保留。

## 批次檔案

若要大量更新設定檔資料，請建立批次檔案。 批次檔案是文字檔，其值由逗號分隔，類似於以下範例檔案。

``` ```
batch=pcId,param1,param2,param3,param4
123,value1
124,value1,,,value4
125,,value2
126,value1,value2,value3,value4
``` ```

>[!NOTE]
>
>`batch=`引數為必要項，必須在檔案開頭指定。

您在[!DNL Target]伺服器的POST呼叫中參考此檔案，以處理該檔案。 建立批次檔案時，請考量下列事項：

* 檔案的第一列必須指定欄標題。
* 第一個標頭應該是`pcId`或`thirdPartyId`。 不支援[!UICONTROL Marketing Cloud visitor ID]。 [!UICONTROL pcId]是[!DNL Target]產生的訪客ID。 `thirdPartyId`是使用者端應用程式所指定的ID，它是透過mbox呼叫傳遞至[!DNL Target]做為`mbox3rdPartyId`。 它必須在這裡稱為`thirdPartyId`。
* 基於安全理由，您在批次檔案中指定的引數和值必須使用UTF-8進行URL編碼。 引數和值可以轉送至其他邊緣節點，以透過HTTP請求處理。
* 引數只能使用`paramName`格式。 引數在[!DNL Target]中顯示為`profile.paramName`。
* 如果您使用[!UICONTROL Bulk Profile Update API] v2，則不需要為每個`pcId`指定所有引數值。 已為`pcId`中找不到的任何`mbox3rdPartyId`或[!DNL Target]建立設定檔。 如果您使用v1，則不會為遺失的pcIds或mbox3rdPartyIds建立設定檔。
* 批次檔的大小必須小於 50 MB。此外，總列數不應超過500,000。 此限制可確保伺服器不會因太多請求而泛濫。
* 您可以傳送多個檔案。 不過，您一天內傳送之所有檔案的總列數，每個使用者端不應超過100萬列。
* 您可以上傳的屬性數目沒有限制。 不過，外部設定檔資料的總計大小不得超過64 KB，其中包括客戶屬性、設定檔API、In-Mbox設定檔引數以及設定檔指令碼輸出。
* 引數和值區分大小寫。

## HTTP POST要求

向[!DNL Target]個邊緣伺服器發出HTTP POST要求以處理檔案。 以下為使用curl命令建立batch.txt檔案的HTTP POST要求範例：

``` ```
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
``` ```

其中:

BATCH.TXT是檔案名稱。 CLIENTCODE是[!DNL Target]使用者端代碼。

如果您不知道使用者端代碼，請在[!DNL Target]使用者介面中按一下&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]**。 使用者端代碼會顯示在[!UICONTROL Account Details]區段中。

### 檢查回應

設定檔API會傳回批次的提交狀態以進行處理，連結「batchStatus」底下至顯示特定批次工作整體狀態的其他URL。

### 範例API回應

以下程式碼片段為設定檔API回應的範例：

```
<response>
    <success>true</success>
    <batchStatus>http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383</batchStatus>
    <message>Batch submitted for processing</message>
</response>
```

如果發生錯誤，回應會包含`success=false`以及錯誤的詳細訊息。

### 預設批次狀態回應

按一下上述`batchStatus` URL連結時，成功的預設回應如下所示：

```
<response><batchId>demo4-1701473848678-13029383</batchId><status>complete</status><batchSize>1</batchSize></response>
```

狀態列位的預期值為：

| 狀態  | 詳細資料 |
| --- | --- |
| [!UICONTROL complete] | 設定檔批次更新請求已成功完成。 |
| [!UICONTROL incomplete] | 設定檔批次更新請求仍在處理中，尚未完成。 |
| [!UICONTROL stuck] | 設定檔批次更新請求卡住，無法完成。 |

### 詳細批次狀態URL回應

將引數`showDetails=true`傳遞至上述`batchStatus` URL，即可擷取更詳細的回應。

例如：

```
http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383&showDetails=true
```

#### 詳細回應

```
<response>
    <batchId>demo4-1701473848678-13029383</batchId>
    <status>complete</status>
    <batchSize>1</batchSize>
    <consumedCount>1</consumedCount>
    <successfulUpdates>1</successfulUpdates>
    <profilesNotFound>0</profilesNotFound>
    <failedUpdates>0</failedUpdates>
</response>
```

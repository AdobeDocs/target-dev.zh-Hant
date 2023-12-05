---
title: Adobe Target大量設定檔更新API
description: 瞭解如何使用 [!DNL Adobe Target] [!UICONTROL 大量設定檔更新API] 將多位訪客的設定檔資料傳送至 [!DNL Target] 以用於目標定位。
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: a6f47c99cfc419771c1a6674990c415a2035ab4e
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 8%

---

# [!DNL Adobe Target Bulk Profile Update API]

此 [!DNL Adobe Target] [!UICONTROL 大量設定檔更新API] 可讓您使用批次檔案大量更新網站多個訪客的使用者設定檔。

使用 [!UICONTROL 大量設定檔更新API]，您可以方便的以設定檔引數的形式傳送詳細的訪客設定檔資料給許多使用者 [!DNL Target] 來自任何外部來源。 外部來源可能包括客戶關係管理(CRM)或銷售點(POS)系統，這些通常無法在網頁上使用。

| 版本 | URL範例 | 功能 |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/profile/batchUpdate` | 僅支援大量設定檔更新。 |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/v2/profile/batchUpdate` | <ul><li>如果找不到，請建立設定檔。</li><li>每列狀態更新。</li></ul> |

>[!NOTE]
>
>的第2版(v2) [!UICONTROL 大量設定檔更新API] 是目前版本。 不過， [!DNL Target] 仍支援版本1 (v1)。

## 大量設定檔更新API的優點

* 設定檔屬性的數量不限。
* 透過網站傳送的設定檔屬性可以透過API更新，反之亦然。

## 注意事項

* 批次檔的大小必須小於 50 MB。此外，每次上傳的總列數不得超過 500,000 列。
* 您可以上傳後續批次中超過24小時期間的一或多列數量沒有限制。 不過，在上班時間可以節流汲取程序，以確保其他程序的執行效率。
* 對相同的thirdPartyIds採用連續v2批次更新呼叫，且其中不需使用mbox呼叫，會覆寫第一次批次更新呼叫所更新的屬性。
* [!DNL Adobe] 不保證100%的批次設定檔資料都會上線並保留在Target中，因此可用於目標定位。 在目前的設計中，小部分資料（最多佔大型生產批次的0.1%）有可能不會上線或保留。

## 批次檔案

若要大量更新設定檔資料，請建立批次檔案。 批次檔案是文字檔，其值由逗號分隔，類似於以下範例檔案。

``````
batch=pcId, param1, param2, param3, param4 123, value1 124, value1,,, value4 125,, value2 126, value1, value2, value3, value4
``````

您在POST呼叫中參考此檔案 [!DNL Target] 伺服器來處理檔案。 建立批次檔案時，請考量下列事項：

* 檔案的第一列必須指定欄標題。
* 第一個標題應為 `pcId` 或 `thirdPartyId`. 此 [!UICONTROL Marketing Cloud訪客ID] 不受支援。 [!UICONTROL pcId] 是 [!DNL Target]-generated visitorID. `thirdPartyId` 是使用者端應用程式指定的ID，傳遞至 [!DNL Target] 透過mbox呼叫，做為 `mbox3rdPartyId`. 它必須在這裡稱為 `thirdPartyId`.
* 基於安全理由，您在批次檔案中指定的引數和值必須使用UTF-8進行URL編碼。 引數和值可以轉送至其他邊緣節點，以透過HTTP請求處理。
* 引數必須採用格式 `paramName` 僅限。 引數顯示於 [!DNL Target] 作為 `profile.paramName`.
* 如果您使用 [!UICONTROL 大量設定檔更新API] v2，您不需要指定每個引數的所有引數值 `pcId`. 已為任何專案建立設定檔 `pcId` 或 `mbox3rdPartyId` 在中找不到 [!DNL Target]. 如果您使用v1，則不會為遺失的pcIds或mbox3rdPartyIds建立設定檔。
* 批次檔的大小必須小於 50 MB。此外，總列數不應超過500,000。 此限制可確保伺服器不會因太多請求而泛濫。
* 您可以傳送多個檔案。 不過，您一天內傳送之所有檔案的總列數，每個使用者端不應超過100萬列。
* 您上傳的屬性數量沒有限制。 不過，設定檔的整體大小（包括系統資料）不應超過2000 KB。 [!DNL Adobe] 建議您將少於1000 KB的儲存空間用於設定檔屬性。
* 引數和值區分大小寫。

## HTTPPOST要求

向發出HTTPPOST請求 [!DNL Target] 邊緣伺服器來處理檔案。 以下是使用curl命令為batch.txt檔案提出的HTTPPOST請求範例：

``````
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
``````

其中:

BATCH.TXT是檔案名稱。 CLIENTCODE是 [!DNL Target] 使用者端代碼。

如果您不知道使用者端代碼，請在 [!DNL Target] 使用者介面點按 **[!UICONTROL 管理]** > **[!UICONTROL 實施]**. 使用者端代碼會顯示在 [!UICONTROL 帳戶詳細資料] 區段。

### Inspect的回應

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

如果發生錯誤，回應會包含 `success=false` 以及錯誤的詳細訊息。

### 預設批次狀態回應

符合上述條件時的成功預設回應 `batchStatus` 被點按的URL連結如下所示：

```
<response><batchId>demo4-1701473848678-13029383</batchId><status>complete</status><batchSize>1</batchSize></response>
```

狀態列位的預期值為：

| 狀態  | 詳細資料 |
| --- | --- |
| [!UICONTROL complete] | 設定檔批次更新請求已成功完成。 |
| [!UICONTROL 不完整] | 設定檔批次更新請求仍在處理中，尚未完成。 |
| [!UICONTROL 卡住] | 設定檔批次更新請求卡住，無法完成。 |

### 詳細批次狀態URL回應

傳遞引數可擷取更詳細的回應 `showDetails=true` 至 `batchStatus` 以上的url。

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

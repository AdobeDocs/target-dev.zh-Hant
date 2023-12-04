---
title: Adobe Target大量設定檔更新API
description: 瞭解如何使用 [!DNL Adobe Target] [!UICONTROL 大量設定檔更新API] 將多位訪客的設定檔資料傳送至 [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: 8bc819823462fae71335ac3b6c871140158638fe
workflow-type: tm+mt
source-wordcount: '727'
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

v2會傳回依設定檔劃分的狀態，而v1隻會傳回整體狀態。 回應包含不同URL的連結，該URL具有每個設定檔的成功訊息。

### 範例回應

```
true http://mboxedge19.tt.omtrdc.net/m2/demo/v2/profile/batchStatus?batchId=demo-1845664501&m2Node=00 Batch submitted for processing
```

如果發生錯誤，回應會包含 `success=false` 以及錯誤的詳細訊息。

成功的回應如下所示：

``````
demo-1845664501 1436187396849-250353.03_03 success 2403081156529-351655.03_03 success 2403081156529-351656.03_03 success 1436187396849-250351.01_00 success 
``````

狀態列位的預期值為：

**成功**：設定檔已更新。 如果找不到設定檔，則會使用批次中的值建立一個設定檔。
**錯誤**：由於失敗、例外狀況或訊息遺失，未更新或建立設定檔。
**擱置中**：尚未更新或建立設定檔。




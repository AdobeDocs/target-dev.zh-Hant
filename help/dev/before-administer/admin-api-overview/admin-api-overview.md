---
title: Adobe Target管理API總覽
description: ' [!DNL Adobe Target Admin API]的總覽'
exl-id: 1168d376-c95b-4c5a-b7a2-c7815799a787
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1312'
ht-degree: 2%

---

# Target管理員API總覽

本文提供成功瞭解及使用[!DNL Adobe Target Admin API]所需的背景資訊概觀。 下列內容假設您瞭解如何[設定[!DNL Adobe Target Admin API]的驗證](../configure-authentication.md)。

>[!NOTE]
>
>如果您想要透過UI管理[!DNL Target]，請參閱&#x200B;*Adobe Target商業從業者指南*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en)的[管理區段。
>
>管理員API和設定檔API通常是整體參照（「管理員和設定檔API」），但也可能單獨參照（「管理員API」和「設定檔API」）。 Recommendations API是[!DNL Target] Admin API的特定實作。

## 開始之前

在為[Admin API](../../administer/admin-api/admin-api-overview-new.md)提供的所有程式碼範例中，將{tenant}取代為您的租使用者值，`your-bearer-token`取代為您使用JWT產生的存取權杖，將`your-api-key`取代為您從[Adobe Developer Console](https://developer.adobe.com/console/home)取得的API金鑰。 如需有關租使用者和JWT的詳細資訊，請參閱有關如何為Adobe[!DNL Target]管理員API [設定驗證](../configure-authentication.md)的文章。

## 版本設定

所有API都有相關版本。 請務必提供您要使用之正確版本的API。

如果要求包含裝載(POST或PUT)，則會使用要求的`Content-Type`標頭來指定版本。

如果要求不包含裝載(GET、DELETE或OPTIONS)，則會使用`Accept`標頭來指定版本。

如果未提供版本，呼叫將預設為V1 (application/vnd.adobe.target.v1+json)。

>[!NOTE]
>
>如果未指定正確的版本（例如，如果您使用V2裝載但未能指定Content-Type標頭），如果API無法回溯相容，API會回應不支援的錯誤。

不支援功能的錯誤訊息

```
{
    "httpStatus": 406,
    "requestId": "8752b736-cf71-4d81-86c3-94be2b5ae648",
    "requestTime": "2018-02-02T21:39:06.405Z",
    "errors": [
        {
            "errorCode": "Unsupported.Feature",
            "message": "Unsupported features detected"
        }
    ]
}
```

管理Postman集合

Postman是應用程式，可讓您輕鬆引發API呼叫。 此[Target Admin API Postman集合](https://developers.adobetarget.com/api/#admin-postman-collection)包含需要使用「活動」、「對象」、「選件」、「報表」、「Mbox」及「環境」進行驗證的所有Target Admin API呼叫

## 回應代碼

以下是Target管理員API的常見回應代碼。

| 狀態  | 含義 | 說明 |
| --- | --- | --- |
| 200 | [確定](https://www.rfc-editor.org/rfc/rfc7231#section-6.3.1) | 確定 |  |
| 400 | [錯誤請求](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.1) | 錯誤請求。 請求中提供的資料很可能無效。 |  |
| 401 | [未獲授權](https://www.rfc-editor.org/rfc/rfc7235#section-3.1) | 不允許使用者執行此作業。 |  |
| 403 | [禁止存取](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.3) | 禁止存取此資源。 |  |
| 404 | 找不到[](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.4) | 找不到參照的資源。 |  |

## 活動

活動可讓您測試或個人化使用者的內容。 活動可為下列其中一種型別：

* [A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [體驗鎖定 (XT)](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html)
* [Recommendations](https://experienceleague.adobe.com/docs/target/using/activities/recommendations-activity.html)
* [自動個人化](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [多變數測試(MVT)](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)

## 批次更新

多個管理員API可作為單一批次請求執行。

### 批次執行呼叫

`POST /{tenant}/target/batch`

將多個API呼叫棧疊在一起，並在單一批次中執行。

批次處理可讓您傳遞單一HTTP請求中數個作業的指示。 您也可以指定相關作業之間的相依性（如下節所述）。 TNT將處理每個獨立的作業（可能同時進行），並將依序處理您的相依作業。 完成所有操作後，將傳回整合的回應，並關閉HTTP連線。

批次API採用以JSON陣列表示的邏輯HTTP要求陣列 — 每個要求都有方法(對應至HTTP方法GET/PUT/POST/DELETE等)、relativeUrl （admin/rest/之後的URL部分）、選用標頭陣列（對應至HTTP標頭）和選用內文(適用於POST和PUT要求)。 批次API會傳回以JSON陣列表示的邏輯HTTP回應陣列 — 每個回應都有狀態代碼、選用的標頭陣列和選用的內文（這是JSON編碼字串）。 若要進行批次要求，請建置JSON物件，以說明要執行的各個作業。 允許的最大運算元為256 （從0到255）。

指定要求中作業之間的相依性依預設，批次API要求中指定的作業是獨立的 — 它們可以在伺服器上以任意順序執行，而且一個作業中的錯誤不會影響其他作業的執行。

通常，請求中的操作是相依的 — 例如，一個操作的輸出可用於下一個操作的輸入。 例如，operationId=0中建立的選件，需要用於行銷活動建立operationId=1。

若要將兩個批次作業連結在一起，請在相依作業中指定所需作業的ID，例如：&quot;dependsOnOperationId&quot; ：5。 此外，透過批次作業的POST請求建立的資源ID可用於「relativeUrl」和「body」中的相依作業。

#### 許可權與節流

為了執行批次API動作，基礎使用者必須至少擁有「編輯者」許可權（針對每個個別作業，若需要使用者擁有的其他許可權，該個別作業將失敗）。 常見的節流策略會套用至批次API動作，就像每個作業都是個別執行一樣。

當所有作業都完成時，批次處理完成，作業可能成功(2xx statusCode)、失敗（4xx、5xx狀態代碼）或因相依性作業失敗或已略過，而略過。

#### 要求物件引數

| 屬性 | 說明 | 限制 | 預設值 |
| --- | --- | --- | --- |
| 內文 | HTTP批次作業的內文。 將會在POST和PUT以外的所有動作中忽略。 可參考先前批次動作的ID，例如：「offerId」：「{operationIdResponse：0}」、「segmentId」：「{operationIdResponse：1}」 | 應為有效的JSON；若參考operationIdResponse，則參考的operationId回應應為有效的ID，且該動作上的方法應POST | 空白物件{} |  |
| dependsOnOperationIds | 條件約束ID的清單，可保證目前的作業只有在指定的作業順利完成時才執行。 可用於實現作業的鏈結。 | 最多允許255個操作；只允許唯一值；應指向陣列中的有效operationId；不允許循環相依性 |  |  |
| 標頭 | 要連同特定操作一起傳送的鍵值標頭陣列。 如果批次API的驗證已透過「授權」標頭執行，其也將針對個別作業複製。 | 陣列中允許的標頭數上限為50 | Content-Type： application/json |  |
| 標題 — >名稱 | 標頭名稱 | 與其他標頭名稱之間應是唯一的。 rfc的標頭不區分大小寫，否則值會相互覆寫。 |  |  |
| 標題 — >值 | 標頭值 | 不適用 | 空字串 |  |
| 方法 | 要使用的HTTP方法。 可用選項：GET、POST、PUT、PATCH、DELETE | 僅允許GET、POST、PUT、PATCH、DELETE方法 |  |  |
| operationId | 作業ID，用來識別其他作業中的作業，以取得回應和參考結果。 | 在其他作業中是唯一的；值介於0到255之間 |  |  |
| 操作 | 要在批次中執行的作業清單。 順序不相關。 | 最多允許256個操作 |  |  |
| relativeUrl | 管理員rest API的相對URL，「/admin/rest/」之後的部分。 可以包含查詢字串引數，例如： &quot;/v2/campaigns？limit=10&amp;offset=10&quot;。 可以參照包含先前批次動作ID的URL，例如： &quot;/v1/offers/{operationIdResponse：0}&quot;。 若傳送查詢引數，引數必須經過URL編碼。 | 應該以/ （相對）開頭；僅支援新的有效JSON API；如果是relativeURL無效，則會傳回特定操作的404回應；如果引用operationIdResponse，則引用的operationId回應應為有效的ID，且該動作上的方法應為POST |  |  |

#### 範例要求物件

```
{
  "operations": [
    {
      "operationId": 1,
      "dependsOnOperationIds~": [0],
      "method": "POST",
      "relativeUrl": "/v1/offers",
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json"
        }
      ],
      "body~": {
        "key": "value"
      }
    }
  ]
}
```

#### 回應物件引數

| 參數 | 說明 |
| --- | --- |
| operationId | 作業ID，用來識別其他作業中的作業，其ID與POST要求中已傳送的作業相同。 |  |
| 已略過 | 布林旗標，標示作業是否已執行或略過。 如果目前操作相依於失敗的操作（傳回與2xx不同的statusCode值），則為true。 |  |
| 狀態代碼 | 傳回，則會略過所有相依作業（不執行）。 |  |
| 標頭 | 要作為特定操作的回應傳送的鍵值標頭陣列。 |  |
| 標題 — >名稱 | 標頭名稱 |  |
| 標題 — >值 | 標頭值 |  |
| 內文 | HTTP批次回應作業的內文 |  |

#### 範例回應物件

```
{
  "results": [
    {
      "operationId": 1,
      "skipped~": false,
      "statusCode~": 200,
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json; charset=UTF-8"
        }
      ],
      "body~": {
        "id": 5
      }
    }
  ]
}
```

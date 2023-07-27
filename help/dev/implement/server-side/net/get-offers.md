---
title: 在中使用getOffers() [!DNL Adobe Target] 使用.NET SDK時
description: 瞭解如何使用getOffers()執行決定並從中擷取體驗 [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 4d1d1cbd-c7e5-4146-9fea-08e01923874d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 13%

---

# 取得選件(.NET)

## 說明

`GetOffers()` 用於執行決定和從中擷取體驗 [!DNL Adobe Target].

## 方法

`TargetClient.GetOffers` 方法簽章。

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.GetOffers(TargetDeliveryRequest request)
```

`TargetDeliveryRequest` 建立工具： `TargetDeliveryRequest.Builder`.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryRequest.Builder TargetDeliveryRequest.Builder()
```

## 參數

此 `TargetDeliveryRequest.Builder` 物件具有下列結構：

| 名稱 | 類型 | 必要 | 說明 |
| --- | --- | --- | --- |
| 上下文 | 上下文 | 是 | 指定要求的內容 |
| sessionId | 字串 | 無 | 用於連結多個 [!DNL Target] 請求 |
|  thirdPartyId | 字串 | 無 | 您公司可透過每次呼叫傳送之使用者的識別碼 |
| Cookie | 清單 | 否 | 先前傳回的Cookie清單 [!DNL Target] 相同使用者的請求。 |
| customerIds | 地圖 | 否 | 客戶ID採用與VisitorId相容的格式 |
| 執行 | ExecuteRequest | 否 | 要執行的PageLoad或mbox要求。 將會立即在伺服器端進行評估 |
| 預先擷取 | PrefetchRequest | 否 | Views、PageLoad或mbox要求預先擷取。 傳迴轉換時要傳回的通知權杖。 |
| 通知 | 清單 | 否 | 用於傳送有關顯示哪些預先擷取內容的通知 |
| requestId | 字串 | 無 | 回應中會傳回的要求ID。 若不存在，則自動產生。 |
| impressionId | 字串 | 無 | 如果存在，則具有相同ID的第二個和後續請求將不會增加活動/量度的曝光數。 若不存在，則自動產生。 |
| environmentId | 長整數 | 否 | 有效的使用者端環境ID。 如果未指定主機，則會根據提供的主機來決定主機。 |
| 屬性 | 屬性 | 否 | 透過Token欄位指定at_property。 它可用來控制傳遞的範圍。 |
| trace | 追蹤 | 否 | 啟用傳送API的追蹤。 |
| qaMode | QAMode | 否 | 使用此物件來啟用請求中的QA模式。 |
| locationHint | 字串 | 無 | [!DNL Target] 邊緣叢集位置提示。 用於針對此要求鎖定指定的邊緣叢集。 |
| 訪客 | 訪客 | 否 | 用於提供自訂訪客API物件。 |
| id | VisitorId | 否 | 包含訪客識別碼的物件。 例如： tntId、thirdParyId、mcId、customerIds。 |
| experienceCloud | Experience Cloud | 否 | 指定與Audience Manager和Analytics的整合。 若未提供，則會使用Cookie自動填入。 |
| tntId | 字串 | 無 | 中的主要識別碼 [!DNL Target] 適用於使用者。 已從targetCookies擷取。 若未提供，則為自動產生。 |
| mcId | 字串 | 無 | 用於在不同Adobe解決方案(ECID)之間合併和共用資料。 已從targetCookies擷取。 若未提供，則為自動產生。 |
| trackingServer | 字串 | 無 | Adobe Analytics伺服器，用於 [!DNL Adobe Target] 和 [!DNL Adobe Analytics] 以正確地彙整資料。 |
| trackingServerSecure | 字串 | 無 | 此 [!UICONTROL Adobe Analytics Secure Server] 以便 [!DNL Adobe Target] 和 [!DNL Adobe Analytics] 以正確地彙整資料。 |
| 決策方法 | 決策方法 | 否 | 可用於針對裝置上決策明確設定ON_DEVICE或HYBRID決策方法 |

每個欄位的值都應符合 [Target傳送API](/help/dev/implement/delivery-api/overview.md) 要求規格。

## 回應

此 `TargetDeliveryResponse` 傳回者 `TargetClient.GetOffers()` 具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 請求 | TargetDeliveryRequest&#x200B; | [Target傳送API](/help/dev/implement/delivery-api/overview.md) 請求 |
| 回應 | DeliveryResponse&#x200B; | [Target傳送API](/help/dev/implement/delivery-api/overview.md)*回應 |
| 狀態  | HttpStatusCode | 回應HTTP狀態代碼 |
| 訊息 | string | 回應狀態訊息或錯誤訊息 |
| 位置 | 位置 | [!DNL Target] 位置名稱，包括全域mbox名稱和mbox/檢視，僅供遠端決策使用 |
| GetCookies | 字典 | 傳回此使用者的工作階段中繼資料字典。 接下來需要傳遞此內容 [!DNL Target] 此使用者的要求。 |
| Visitorstate | IDictionary | 訪客狀態將於使用者端設定，以供訪客API Javascript程式庫初始化使用 |

此 `TargetCookie` 用來儲存使用者工作階段資料的物件具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 名稱 | string | Cookie 名稱 |
| 值 | string | Cookie值 |
| MaxAge | int | 此 `MaxAge` 選項可方便您設定相對於目前時間（以秒為單位）的過期時間 |

您不必擔心Cookie會過期。 [!DNL Target] 控點 `MaxAge` 在SDK內。

## 範例

## \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
    .Build();

var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```

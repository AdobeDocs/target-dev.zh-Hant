---
title: 在中使用getOffers() [!DNL Adobe Target] 使用Java SDK時
description: 瞭解如何使用getOffers()執行決定並從中擷取體驗 [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9d7bf956-9d6a-4b4f-a401-2e6814f17f3d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 13%

---

# 取得選件(Java)

## 說明

`getOffers()` 用於執行決定和從中擷取體驗 [!DNL Adobe Target].

## 方法

### getOffers

此 `TargetClient.getOffers` 方法簽章如下所示。

**請求**

```javascript {line-numbers="true"}
TargetDeliveryResponse TargetClient.getOffers(TargetDeliveryRequest request)
```

TargetDeliveryRequest的建立方式 `TargetDeliveryRequest.builder`.

**回應**

```javascript {line-numbers="true"}
TargetDeliveryRequestBuilder TargetDeliveryRequest.builder()
```

## 參數

此 `[!UICONTROL TargetDeliveryRequestBuilder]` 物件具有下列結構：

| 名稱 | 類型 | 必要 | 說明 |
| --- | --- | --- | --- |
| 上下文 | 上下文 | 是 | 指定要求的內容 |
| sessionId |  | 字串 | 無 | 用於連結多個 [!DNL Target] 請求 |
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
| mcId | 字串 | 無 | 用於合併和共用不同資料之間的資料 [!DNL Adobe] 解決方案(ECID)。 已從targetCookies擷取。 若未提供，則為自動產生。 |
| trackingServer | 字串 | 無 | Adobe Analytics伺服器，用於 [!DNL Adobe Target] 和 [!DNL Adobe Analytics] 以正確地彙整資料。 |
| trackingServerSecure | 字串 | 無 | 此 [!UICONTROL Adobe Analytics Secure Server] 以便 [!DNL Adobe Target] 和 [!DNL Adobe Analytics] 以正確地彙整資料。 |
| 決策方法 | 決策方法 | 否 | 可用於針對裝置上決策明確設定ON_DEVICE或HYBRID決策方法 |

每個欄位的值都應符合 *[!UICONTROL 目標檢視傳送API]* 要求規格。 若要進一步瞭解 *[!UICONTROL 目標檢視傳送API]*，請參閱 [http://developers.adobetarget.com/api/#view-delivery-overview](http://developers.adobetarget.com/api/#view-delivery-overview)


## 回應

此 `TargetDeliveryResponse` 傳回者 `TargetClient.getOffers(`)具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 請求 | TargetDeliveryRequest&#x200B; | *[!DNL Target]* 請求 |
| 回應 | DeliveryResponse | *[!DNL Target]* 回應 |
| Cookie | 清單 | 此使用者的工作階段中繼資料清單。 需要在此使用者的下一個Target要求中傳遞。 |
| visitorState | 地圖 | 將在使用者端上設定供訪客API使用的訪客狀態 |
| responseStatus | 回應狀態 | 代表回應狀態的物件 |

此 `ResponseStatus` 回應中包含下列欄位：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 狀態 | int | 從傳回的HTTP狀態 [!DNL Target] |
| 訊息 | 字串 | HTTP狀態不是200時的狀態訊息 |
| remoteMboxes | 字串清單 | 用於裝置上決策。 包含具有無法完全在裝置上決定的遠端活動的mbox清單。 |
| remoteView | 字串清單 | 用於裝置上決策。 包含檢視清單，這些檢視具有無法完全在裝置上決定的遠端活動。 |

此 `TargetCookie` 用來儲存使用者工作階段資料的物件具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 名稱 | 字串 | Cookie 名稱 |
| value | 字串 | Cookie值，該值將會轉換為字串 |
| maxAge | 數字 | maxAge選項可方便您設定相對於目前時間（以秒為單位）的過期時間 |

您不必擔心Cookie會過期。 Target會在SDK內處理maxAge。

## 範例

**請求**

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .execute(new ExecuteRequest().setMboxes(mboxRequests))
        .build();
```

**回應**

```javascript {line-numbers="true"}
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

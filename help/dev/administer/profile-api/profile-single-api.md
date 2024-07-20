---
title: Adobe Target單一設定檔更新API
description: 瞭解如何使用 [!DNL Adobe Target] [!UICONTROL Single Profile Update API]將單一訪客的個人資料傳送至 [!DNL Target]。
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 4e022db3-215f-461b-9222-38ce2f2dbc28
source-git-commit: e2462d12cf58ab5a588c13a96df5e6abafb9d675
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 3%

---

# [!DNL Adobe Target Single Profile Update API]

[!DNL Adobe Target] [!UICONTROL Single Profile Update API]可讓您傳送單一使用者的設定檔更新。 [!UICONTROL Single Profile Update API]幾乎與[!UICONTROL Bulk Profile Update API]相同，但一次會更新一個訪客設定檔，內嵌於API呼叫而非.cvs檔案。

[!UICONTROL Single Profile Update API]和通常用於必須發生與尚未實作[!DNL Target]的管道中發生的交易相關的更新時。 例如，您想要更新執行某些離線動作之單一訪客的設定檔。 動作包括聯絡客服中心、提供貸款、在商店中使用忠誠卡、存取資訊站等。

[!UICONTROL Single Profile Update API]的優點包括：

* 設定檔屬性的數量不限。
* 透過網站傳送的設定檔屬性可以透過API更新，反之亦然。

## 注意事項

* [!UICONTROL Single Profile Update API]受限於在任何連續24小時的期間內執行100萬項更新。
* 更新通常在一小時內發生，但可能需要長達24小時的時間才會反映。

  如果您必須傳送更多更新，或需要在較短的時間範圍內處理更新，請考慮透過使用者端更新（偏好設定）或透過[!DNL Adobe Target]伺服器端[傳送API](/help/dev/implement/delivery-api/overview.md)傳送異動設定檔更新。

* [!UICONTROL Single Profile Update API]是伺服器對伺服器的API，並非設計成可在網頁內運作。 若要從您的網頁更新訪客設定檔，您可以使用[trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)函式或[傳送API](/help/dev/implement/delivery-api/overview.md)。

## 格式

指定設定檔引數，格式為`profile.paramName=value`。

若要更新`pcId`的設定檔，請使用：

``````
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
``````

若要更新`mbox3rdPartyId`的設定檔，請使用：

``````
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
``````

[!UICONTROL Single Profile Update API]僅供更新。 如果未找到任何專案，則不會建立設定檔。

## 附註

* 引數和值必須使用UTF-8進行URL編碼。
* 引數格式為`profile.paramName`。
* 並非所有pcIds和mbox3rdPartyIds都必須存在所有引數值。
* 引數和值區分大小寫。
* 同時支援GET和POST。
* 目前的大小限製為：GET為8 KB，POST為60 KB。

## 回應

上述請求的範例回應如下所示：

`trueRequest successfully submitted`

此回應表示回應已提交且很快將進行處理。

---
keywords: 實作、實施、設定、設定、單一設定檔更新
description: 將資料匯入 [!DNL Target] 使用單一設定檔更新API。
title: 如何將資料帶入 [!DNL Target] 使用單一設定檔更新API？
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 734bda64915a08f2edba37cbbb66b2de581c2237
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 11%

---

# 單一設定檔更新API

幾乎與 [!UICONTROL 大量設定檔更新API] 在 [!DNL Adobe Target]，但一次會更新一個訪客設定檔，使其符合API呼叫，而不是使用.csv檔案。

## 格式

必須透過以下方式識別訪客 [!DNL Target] `mboxPC` 值或 `mbox3rdPartyId` 值。 此 [!UICONTROL EXPERIENCE CLOUDID] 不支援(ECID)。

## 範例使用案例

您要更新執行某些離線動作之單一訪客的設定檔。 動作包括聯絡客服中心、提供貸款、在商店中使用忠誠卡、存取資訊站等。

## 方法的優點

* 設定檔屬性的數量不限。*
* 透過網站傳送的設定檔屬性可以透過API更新，反之亦然。

## 注意事項

* 限制每24小時期間1,000,000次API呼叫（1百萬）。
* 只更新設定檔。無法為潛在使用者建立設定檔 [!DNL Target] 尚未檢視。
* 更新通常在一小時內發生，但可能需要長達24小時的時間才會反映。

## 程式碼範例

支援 GET 和 POST。

```
https://CLIENT.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr1=0&profile.attr2=1...
```

## 資源

* [更新設定檔](https://developers.adobetarget.com/api/#updating-profiles)

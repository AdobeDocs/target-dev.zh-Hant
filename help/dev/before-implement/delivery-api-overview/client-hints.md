---
title: Adobe Target Delivery API使用者端提示
description: 如何在 [!DNL Adobe Target] 傳送API中使用使用者端提示？
exl-id: 317b9d7d-5b98-464e-9113-08b899ee1455
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# 使用者端提示和[!UICONTROL Adobe Target Delivery API]

使用者端提示必須傳送至優惠方案請求上的[!DNL Adobe Target]。

一般而言，建議將所有可用的使用者端提示傳送至[!DNL Target]。 如需詳細資訊，請參閱[使用者端實作](../../implement/client-side/overview.md)區段中的[使用者代理和使用者端提示](/help/dev/implement/client-side/atjs/user-agent-and-client-hints.md)。

## 傳送API直接呼叫

### 從瀏覽器

在此情況下，瀏覽器將會透過請求標題自動傳送低平均資訊量使用者端提示給[!DNL Target]。 但此實作有幾項瀏覽器層級的限制。 第一 — 除非透過https提出請求，否則不會從瀏覽器傳送任何使用者端提示標頭。 第二 — 使用者端提示不會在第一次請求時傳送給頁面上的[!DNL Target]。 使用者端提示標頭只會在第二次請求時傳送，並在其後傳送所有請求。 這表示[!DNL Target]在第一個頁面造訪時無法執行對象細分和個人化。 為了解決這兩個限制，我們強烈建議在瀏覽器中使用使用者代理使用者端提示API ，直接收集使用者端提示，並在要求裝載時傳送。

### 從伺服器

在此情況下，使用者端提示必須在傳送API要求上從瀏覽器手動轉送至[!DNL Target]。

```
curl -X POST 'http://mboxedge28.tt.omtrdc.net/rest/v1/delivery?client=myClientCode&sessionId=abcdefghijkl00014' -d '{
  "context": {
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36",
    "clientHints": {
      "Sec-CH-UA-Model": "iPhone",
      "Sec-CH-UA-Mobile": true,
      "Sec-CH-UA-Platform": "iOS",
      "Sec-CH-UA": "[ { \"brand\": \"Chromium\", \"version\": \"91\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99\" } ]",
      "Sec-CH-UA-Full-Version-List": "[ { \"brand\": \"Chromium\", \"version\": \"91.1.1.1\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99.1.1.1\" } ]",
      "Sec-CH-UA-Platform-Version": "10.0.0",
      "Sec-CH-UA-Arch": "x86",
      "Sec-CH-UA-Bitness": "64"
    }
  },
  "execute": {
    "mboxes": [{
      "name": "home",
      "index": 1
    }]
  }
}'
```

## 格式化

使用者端提示標頭Sec-CH-UA和Sec-CH-UA-Full-Version-List的格式與使用者端提示瀏覽器API (navigator.userAgentData.brands/navigator.userAgentData.getHighEntropyValues)的結果不同。 傳遞API接受這兩種格式。 傳送API會將值標準化為請求標頭中使用的格式，在存取設定檔指令碼中的使用者端提示時，請務必牢記。

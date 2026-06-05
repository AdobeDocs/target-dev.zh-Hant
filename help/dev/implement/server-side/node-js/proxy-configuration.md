---
title: 在 [!DNL Adobe Target] Node.js SDK中實作Proxy設定
description: 瞭解如何在 [!DNL Adobe Target] Node.js SDK中設定[!UICONTROL TargetClient] Proxy設定。
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
TQID: https://experienceleague.adobe.com/kaE-ZEOTteaVp5kWSHiVYCvEiHuQHSMqeWRq6r-mJaA
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 84
ht-degree: 0%

---

# Proxy設定(Node.js)

若要為SDK節點的HTTP請求設定Proxy，請覆寫SDK在初始化期間使用的擷取API。

下列是基本範例，說明如何在`TargetClient`初始化期間覆寫`fetchApi`以新增Proxy：

```javascript {line-numbers="true"}
const { ProxyAgent } = require("undici");

const proxyAgent = new ProxyAgent("your proxy address here");

const fetchImpl = (url, options) => {
  const fetchOptions = options;
  fetchOptions.dispatcher = proxyAgent;
  return fetch(url, fetchOptions);
};

client = TargetClient.create({
    ...,
    fetchApi: fetchImpl
});
```

請注意，這僅適用於節點版本18.2+，其中`undici.fetch`是節點的預設`fetch`。
請造訪[Node SDK範例存放庫](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
舊版節點的proxy設定範例及詳細資訊。

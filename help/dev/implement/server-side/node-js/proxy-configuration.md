---
title: 在 [!DNL Adobe Target] Node.js SDK中實作Proxy設定
description: 瞭解如何在 [!DNL Adobe Target] Node.js SDK中設定[!UICONTROL TargetClient] Proxy設定。
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Proxy設定(Node.js)

若要設定Node SDK之HTTP請求的Proxy，請覆寫SDK在初始化期間使用的擷取API。

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

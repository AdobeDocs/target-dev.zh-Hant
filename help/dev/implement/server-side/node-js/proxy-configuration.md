---
title: 在中實作Proxy設定 [!DNL Adobe Target] Node.js SDK
description: 瞭解如何設定 [!UICONTROL TargetClient] 中的Proxy設定 [!DNL Adobe Target] Node.js SDK。
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---

# Proxy設定(Node.js)

若要設定Node SDK之HTTP請求的Proxy，請覆寫SDK在初始化期間使用的擷取API。

下列是顯示如何覆寫的基本範例 `fetchApi` 期間為 `TargetClient` 初始化以新增Proxy：

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

請注意，這僅適用於節點18.2+版，其中 `undici.fetch` 為預設值 `fetch` 用於節點。
請造訪 [節點SDK範例存放庫](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
舊版節點的proxy設定範例及詳細資訊。

---
title: 初始化 [!DNL Adobe Target] Node.js SDK以記錄要求
description: 瞭解如何在 [!DNL Adobe Target] Node.js SDK中記錄要求。
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
TQID: https://experienceleague.adobe.com/tC6xT-eAHOO17h1BK-PwWTBmwg3Dy0Wj8KYrV3W-VR4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 83
ht-degree: 2%

---

# 記錄器(Node.js)

## 說明

當[初始化SDK](initialize-sdk.md)時，`options.logger`物件是選用物件。 不過，為了在問題發生時有效地除錯，初始化SDK時應該提供`logger`物件。

`logger`物件必須有`debug()`和`error()`方法。 當提供適當的記錄器時，例如`console`，[!DNL Target]將記錄請求和回應。

## 範例

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  logger: console
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
    }
};

const response = await targetClient.getOffers({ request, targetCookie });
```

您應該會看到主控台中列印的請求和回應。

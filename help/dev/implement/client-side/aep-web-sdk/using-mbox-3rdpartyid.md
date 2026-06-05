---
title: mbox3rdPartyId 的即時輪廓同步
description: 瞭解如何搭配 [!DNL Adobe Experience Platform Web SDK]使用mbox3rdPartyId。
keywords: 個人化；target；adobe target；renderDecisions；sendEvent；mbox3rdPartyId；
feature: AEP Web SDK
exl-id: 1c5067ef-38b3-4bf1-bd39-ea0f2cbd1074
TQID: https://experienceleague.adobe.com/Ej2sYVnBD9orRTlsMQG85JJV7dvn-9gnABDa0b8uBlM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 165
ht-degree: 24%

---

# 使用mbox3rdPartyId

[!DNL Adobe Target]中的`mbox3rdPartyId`是您公司的訪客ID，例如您公司的忠誠度計畫的會員ID。

當訪客登入某個公司的網站時，該公司通常會建立 ID，此 ID 會連結至該訪客的帳戶、忠實客戶卡、會員編號或適用於該公司的其他識別碼。 [了解更多](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=zh-Hant)

## 如何搭配[!DNL Platform Web SDK]使用`mbox3rdPartyId`

### 步驟1：設定`Target Third Party ID Namespace`

使用您要用作mbox第三方ID的ID名稱空間，在您的[資料流](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/datastreams/overview)中設定`Target Third Party ID Namespace`。 [進一步瞭解ID名稱空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=zh-Hant)

![Experience Platform UI顯示Target協力廠商ID名稱空間欄位。](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

### 步驟2：傳送`mbox3rdpartyId`至[!DNL Target]

使用您在步驟1中設定的ID名稱空間，在`sendEvent`命令中將`mbox3rdpartyId`傳送至[!DNL Target]。
[進一步瞭解傳送ID](/help/dev/implement/client-side/aep-web-sdk/using-mbox-3rdpartyid.md)

```javascript
alloy("sendEvent", {
  xdm: {
    "identityMap": {
      "ID_NAMESPACE": [ // Replace `ID_NAMESPACE` with the namespace you have configured in Step 1.
        {
          "id": "1234",
          "authenticatedState": "authenticated"
        }
      ]
    }
  }
});
```

---
title: mbox3rdPartyId 的即時輪廓同步
description: 瞭解如何搭配 [!DNL Adobe Experience Platform Web SDK]使用mbox3rdPartyId。
keywords: 個人化；target；adobe target；renderDecisions；sendEvent；mbox3rdPartyId；
feature: AEP Web SDK
source-git-commit: 27759841c8d6a4ac4cc72e735f1dc6512c71aea5
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 4%

---

# 什麼是mbox3rdPartyId

`mbox3rdPartyId`中的[!DNL Adobe Target]是您公司的訪客ID，例如您公司的忠誠度計畫的會員ID。

當訪客登入某個公司的網站時，該公司通常會建立ID，此ID會連結至該訪客的帳戶、熟客卡、會員編號或適用於該公司的其他識別碼。 [了解更多](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html#)

## 如何搭配`mbox3rdPartyId`使用[!DNL Platform Web SDK]

### 步驟1：設定`Target Third Party ID Namespace`

使用您要用作mbox第三方ID的ID名稱空間，在您的`Target Third Party ID Namespace`資料流[中設定](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview)。 [進一步瞭解ID名稱空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html)

![Experience Platform UI顯示Target協力廠商ID名稱空間欄位。](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

### 步驟2：傳送`mbox3rdpartyId`至[!DNL Target]

使用您在步驟1中設定的ID名稱空間，在`mbox3rdpartyId`命令中將[!DNL Target]傳送至`sendEvent`。
[進一步瞭解傳送ID](../../identity/overview.md#syncing-identities)

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

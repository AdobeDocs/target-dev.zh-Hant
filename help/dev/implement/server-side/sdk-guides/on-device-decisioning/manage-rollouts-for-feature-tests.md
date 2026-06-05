---
title: 管理功能測試的轉出
description: 瞭解如何使用[!UICONTROL 裝置上決策]管理功能測試的轉出。
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
TQID: https://experienceleague.adobe.com/soG8leVV3R4Y4FSns5oIJ43oziIhtOb2zJ5bkFYxeo0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 596
ht-degree: 1%

---

# 管理功能測試的轉出

## 步驟摘要

1. 為您的組織啟用[!UICONTROL 裝置上決策]
1. 建立[!UICONTROL A/B測試]活動
1. 定義您的功能和轉出設定
1. 在您的應用程式中實作及演算功能
1. 實施應用程式中事件的追蹤
1. 啟用您的A/B活動
1. 視需要調整轉出和流量分配

## &#x200B;1. 為您的組織啟用[!UICONTROL 裝置上決策]

啟用裝置上決策可確保在幾乎零延遲的情況下執行A/B活動。 若要啟用此功能，請瀏覽至[!DNL Adobe Target]中的&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]** > **[!UICONTROL 帳戶詳細資料]**，並啟用&#x200B;**[!UICONTROL 裝置上決策]**&#x200B;切換功能。

![替代影像](assets/asset-odd-toggle.png)

>[!NOTE]
>
>您必須擁有管理員或核准者[使用者角色](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html)，才能啟用或停用[!UICONTROL 裝置上決策]切換功能。

啟用[!UICONTROL 裝置上決策]切換後，[!DNL Adobe Target]會開始為您的使用者端產生&#x200B;*規則成品*。

## &#x200B;2. 建立[!UICONTROL A/B測試]活動

1. 在[!DNL Adobe Target]中，導覽至&#x200B;**[!UICONTROL 活動]**&#x200B;頁面，然後選取&#x200B;**[!UICONTROL 建立活動]** > **[!UICONTROL A/B測試]**。

   ![替代影像](assets/asset-ab.png)

1. 在&#x200B;**[!UICONTROL 建立A/B測試活動]**&#x200B;強制回應視窗中，保留預設的&#x200B;**[!UICONTROL 網頁]**&#x200B;選項(1)，選取&#x200B;**[!UICONTROL 表單]**&#x200B;作為您的體驗撰寫器(2)，選取&#x200B;**[!UICONTROL 預設Workspace]**&#x200B;搭配&#x200B;**[!UICONTROL 無屬性限制]** (3)，然後按一下&#x200B;**[!UICONTROL 下一步]** (4)。

   ![替代影像](assets/asset-form.png)

## &#x200B;3. 定義您的功能和轉出設定

在活動建立的&#x200B;**[!UICONTROL 體驗]**&#x200B;步驟中，提供活動的名稱(1)。 輸入應用程式中要管理功能轉出的位置名稱(2)。 例如，`ondevice-rollout`或`homepage-addtocart-rollout`是位置名稱，指出管理功能轉出的目的地。 在下列範例中，`ondevice-rollout`是為體驗A定義的位置。您可以選擇新增對象細分(4)，以限制活動的資格。

![替代影像](assets/asset-location-rollout.png)

1. 在相同頁面的&#x200B;**[!UICONTROL Content]**&#x200B;區段中，選取下拉式清單(1)中的&#x200B;**[!UICONTROL 建立JSON選件]**，如下所示。

   ![替代影像](assets/asset-offer.png)

1. 在出現的&#x200B;**[!UICONTROL JSON資料]**&#x200B;文字方塊中，使用有效的JSON物件(2)，針對您打算在體驗A (1)中隨此活動推出的功能，輸入功能標幟變數。

   ![替代影像](assets/asset-json-a-rollout.png)

1. 按一下「下一步」**&#x200B;** (1)以進入活動建立的&#x200B;**[!UICONTROL 鎖定目標]**&#x200B;步驟。

   ![替代影像](assets/asset-next-2-t-rollout.png)

1. 在&#x200B;**[!UICONTROL 鎖定目標]**&#x200B;步驟中，保留&#x200B;**[!UICONTROL 所有訪客]**&#x200B;對象(1)，以簡化操作。 但是將流量分配(2)調整為10%。 此功能將限製為您網站訪客的10%。 按一下「下一步」 (3)以前往&#x200B;**[!UICONTROL 目標與設定]**&#x200B;步驟。

   ![替代影像](assets/asset-next-2-g-rollout.png)

1. 在&#x200B;**[!UICONTROL 目標與設定]**&#x200B;步驟中，選擇&#x200B;**[!UICONTROL Adobe Target]** (1)做為&#x200B;**[!UICONTROL 報告Source]**，以便在[!DNL Adobe Target] UI中檢視您的活動結果。

1. 選擇&#x200B;**[!UICONTROL 目標量度]**&#x200B;以測量活動。 在此範例中，成功的轉換取決於使用者是否購買專案，如使用者是否到達orderConfirm (2)位置所示。

1. 按一下&#x200B;**[!UICONTROL 儲存並關閉]** (3)以儲存活動。

   ![替代影像](assets/asset-conv-rollout.png)

## &#x200B;4. 在您的應用程式中實作及演算功能

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
targetClient.getAttributes(["ondevice-rollout"]).then(function(attributes) {
      const featureFlags = attributes.asObject("ondevice-rollout");

      // Your flag variables are now available in the featureFlags object variable.
      //If you failed to qualify for the Activity, you will have an empty object.
      console.log(featureFlags);
    });
```

>[!TAB Java]

```java {line-numbers="true"}
    Attributes attrs = targetJavaClient.getAttributes(targetDeliveryRequest, "ondevice-rollout");
    Map<String, Object> featureFlags = attrs.toMboxMap("ondevice-rollout");
​
    // Your flag variables are now available in the featureFlags object variable.
    //If you failed to qualify for the Activity, you will have an empty object.
    System.out.println(featureFlags);
```

>[!ENDTABS]

## &#x200B;5. 實施應用程式中事件的追蹤

讓功能標幟變數在應用程式中可用後，您就可以用它來啟用任何已是應用程式一部分的功能。 如果訪客不符合活動的資格，表示他們未包含在定義為對象的10%貯體中。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

if(featureFlags.enable == "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}

// alternatively, the getValue method could be used on the Attributes object.

if(attributes.getValue("ondevice-rollout", "enable") === "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}
```

>[!TAB Java]

```java {line-numbers="true"}
//... Code removed for brevity
​
if("yes".equals(String.valueOf(featureFlags.get("enable")))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
​
// alternatively, the getString method could be used on the Attributes object.
​
if("yes".equals(attrs.getString("ondevice-rollout", "enable"))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
```

>[!ENDTABS]

## &#x200B;6. 啟用您的轉出活動

![替代影像](assets/asset-activate-rollout.png)

## &#x200B;7. 視需要調整轉出和流量分配

啟動活動後，請隨時編輯活動，以視需要增加或減少流量分配。

將流量分配從10%提高至50%，因為初始轉出成功。

![替代影像](assets/asset-adjust-rollout.png)

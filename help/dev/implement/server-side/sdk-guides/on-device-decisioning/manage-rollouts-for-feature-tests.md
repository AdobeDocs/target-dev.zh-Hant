---
title: 管理功能測試的轉出
description: 瞭解如何使用管理功能測試的轉出 [!UICONTROL 裝置上決策].
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 0%

---

# 管理功能測試的轉出

## 步驟摘要

1. 啟用 [!UICONTROL 裝置上決策] 適用於您的組織
1. 建立 [!UICONTROL A/B測試] 活動
1. 定義您的功能和轉出設定
1. 在您的應用程式中實作及演算功能
1. 實施應用程式中事件的追蹤
1. 啟用您的A/B活動
1. 視需要調整轉出和流量分配

## 1.啟用 [!UICONTROL 裝置上決策] 適用於您的組織

啟用裝置上決策可確保在幾乎零延遲的情況下執行A/B活動。 若要啟用此功能，請瀏覽至 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 帳戶詳細資料]** 在 [!DNL Adobe Target]，並啟用 **[!UICONTROL 裝置上決策]** 切換。

![替代影像](assets/asset-odd-toggle.png)

>[!NOTE]
>
>您必須擁有管理員或核准者 [使用者角色](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) 啟用或停用 [!UICONTROL 裝置上決策] 切換。

啟用 [!UICONTROL 裝置上決策] 切換， [!DNL Adobe Target] 開始產生 *規則人工因素* 適用於您的使用者端。

## 2.建立 [!UICONTROL A/B測試] 活動

1. 在 [!DNL Adobe Target]，導覽至 **[!UICONTROL 活動]** 頁面，然後選取 **[!UICONTROL 建立活動]** > **[!UICONTROL A/B測試]**.

   ![替代影像](assets/asset-ab.png)

1. 在 **[!UICONTROL 建立A/B測試活動]** 強制回應視窗，保留預設值 **[!UICONTROL Web]** 已選取選項(1)，選取 **[!UICONTROL 表單]** 由於您的體驗撰寫器(2)，請選取「 」 **[!UICONTROL 預設工作區]** 替換為 **[!UICONTROL 無屬性限制]** (3)，然後按一下 **[!UICONTROL 下一個]** (4)。

   ![替代影像](assets/asset-form.png)

## 3.定義您的功能和轉出設定

在 **[!UICONTROL 體驗]** 活動建立的步驟，為您的活動提供名稱(1)。 輸入應用程式中要管理功能轉出的位置名稱(2)。 例如，  `ondevice-rollout` 或 `homepage-addtocart-rollout` 是位置名稱，可指示管理功能轉出的目的地。 在下列範例中， `ondevice-rollout` 是為體驗A定義的位置。您可以選擇新增對象細分(4)，以限制活動的資格。

![替代影像](assets/asset-location-rollout.png)

1. 在 **[!UICONTROL 內容]** 區段，選取 **[!UICONTROL 建立JSON選件]** (1)中，如下所示。

   ![替代影像](assets/asset-offer.png)

1. 在 **[!UICONTROL JSON資料]** 文字方塊中，為您要在體驗A (1)中隨此活動推出的功能，使用有效的JSON物件(2)輸入功能標幟變數。

   ![替代影像](assets/asset-json-a-rollout.png)

1. 按一下 **[!UICONTROL 下一個]** (1)前進到 **[!UICONTROL 目標定位]** 建立活動的步驟。

   ![替代影像](assets/asset-next-2-t-rollout.png)

1. 在 **[!UICONTROL 目標定位]** 步驟，保留 **[!UICONTROL 所有訪客]** 對象(1)，以簡化操作。 但是將流量分配(2)調整為10%。 此功能將限製為您網站訪客的10%。 按一下「下一步」(3)以前往 **[!UICONTROL 目標與設定]** 步驟。

   ![替代影像](assets/asset-next-2-g-rollout.png)

1. 在 **[!UICONTROL 目標與設定]** 步驟，選擇 **[!UICONTROL Adobe Target]** (1)作為 **[!UICONTROL 報表來源]** 若要檢視您的活動結果，請 [!DNL Adobe Target] UI。

1. 選擇 **[!UICONTROL 目標量度]** 以測量活動。 在此範例中，成功的轉換取決於使用者是否購買專案，如使用者是否到達orderConfirm (2)位置所示。

1. 按一下 **[!UICONTROL 儲存並關閉]** (3)儲存活動。

   ![替代影像](assets/asset-conv-rollout.png)

## 4.在您的應用程式中實作及呈現功能

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

## 5.針對應用程式中的事件實作追蹤

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

## 6.啟用您的轉出活動

![替代影像](assets/asset-activate-rollout.png)

## 7.視需要調整轉出和流量分配

啟動活動後，請隨時編輯活動，以視需要增加或減少流量分配。

將流量分配從10%提高至50%，因為初始轉出成功。

![替代影像](assets/asset-adjust-rollout.png)

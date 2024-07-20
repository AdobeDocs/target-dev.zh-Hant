---
title: 使用屬性執行功能測試
description: 使用屬性執行功能測試
feature: APIs/SDKs
exl-id: c89d337c-20a9-454c-930c-79d9217e23b6
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 0%

---

# 使用屬性執行功能測試

## 步驟摘要

1. 為您的組織啟用[!UICONTROL on-device decisioning]
1. 建立[!UICONTROL A/B Test]活動
1. 定義您的A和B
1. 新增對象
1. 設定流量分配
1. 將流量分佈設為變數
1. 設定報告
1. 新增追蹤KPI的量度
1. 實作程式碼以使用屬性執行功能測試
1. 實作程式碼以追蹤轉換事件
1. 使用屬性啟用功能測試

>[!NOTE]
>
>假設您是一間零售電子商務公司。 當客戶瀏覽及排序您的產品目錄時，您想要提高轉換率。 您有假設認為，某些排序演演算法和分頁策略會產生比其他演演算法更好的結果。 為了測試此理論，您決定執行功能測試，該測試涉及使用一般使用者的不同排序選項來重新設計排序Widget。 您想要確保此功能測試會在幾乎零延遲的情況下執行，以免對使用者體驗造成負面影響，並扭曲結果。

## 1.為您的組織啟用[!UICONTROL on-device decisioning]

啟用裝置上決策可確保在幾乎零延遲的情況下執行A/B活動。 若要啟用此功能，請瀏覽至[!DNL Adobe Target]中的&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]**，並啟用&#x200B;**[!UICONTROL On-Device Decisioning]**&#x200B;切換按鈕。

![替代影像](assets/asset-odd-toggle.png)

>[!NOTE]
>
>您必須擁有管理員或核准者[使用者角色](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html)，才能啟用或停用&#x200B;**[!UICONTROL On-Device Decisioning]**&#x200B;切換功能。

啟用&#x200B;**[!UICONTROL On-Device Decisioning]**&#x200B;切換後，[!DNL Adobe Target]會開始為您的使用者端產生&#x200B;*規則成品*。

## 2.建立[!UICONTROL A/B Test]活動

1. 在[!DNL Adobe Target]中，導覽至&#x200B;**[!UICONTROL Activities]**&#x200B;頁面，然後選取&#x200B;**[!UICONTROL Create Activity]** > **[!UICONTROL A/B test]**。

   ![替代影像](assets/asset-ab.png)

1. 在&#x200B;**[!UICONTROL Create A/B Test Activity]**&#x200B;強制回應視窗中，保留預設的&#x200B;**[!UICONTROL Web]**&#x200B;選項為已選取(1)、選取&#x200B;**[!UICONTROL Form]**&#x200B;作為您的體驗撰寫器(2)、選取具有&#x200B;**[!UICONTROL No Property Restrictions]** (3)的&#x200B;**[!UICONTROL Default Workspace]**，然後按一下&#x200B;**[!UICONTROL Next]** (4)。

   ![替代影像](assets/asset-form.png)

## 3.定義您的A和B

1. 在活動建立的&#x200B;**[!UICONTROL Experiences]**&#x200B;步驟中，提供活動的名稱(1)並新增第二個體驗，即體驗B，方法是按一下&#x200B;**[!UICONTROL Add Experience]** (2)按鈕。 輸入應用程式中要使用屬性執行特徵測試的位置(3)名稱。 在下列範例中，`product-results-page`是為體驗A定義的位置。（也是為體驗B定義的位置。）

   ![替代影像](assets/asset-location.png)

   **[!UICONTROL Experience A]**&#x200B;將包含代表您商業邏輯執行下列動作的JSON：

   * 透過`test_sorting`功能標幟啟動排序演演算法功能
   * 執行`sorting_algorithm _**_attribute`中定義的建議排序演演算法
   * 依`pagination_limit`中定義的分頁策略所定義，每頁傳回50項產品

1. 在體驗A中，按一下以選取&#x200B;**[!UICONTROL Create JSON Offer]**，將內容從&#x200B;**[!UICONTROL Default Content]**&#x200B;變更為JSON，如下所示(1)。

   ![替代影像](assets/asset-offer.png)

1. 定義具有`test_sorting`、`sorting_algorithm`和`pagination_limit`旗標和屬性的JSON，這些旗標和屬性將用於起始分頁限製為50個產品的建議排序演演算法。

   >[!NOTE]
   >
   >當[!DNL Adobe Target]儲存使用者以檢視體驗A時，將會傳回範例中具有已定義屬性的JSON。 在您的程式碼中，您需要檢查功能標幟`test_sorting`的值，以檢視是否應開啟排序功能。 若是如此，您將會使用`sorting_algorithm`屬性的建議值，在產品清單檢視中顯示建議產品。 為您的應用程式顯示的產品限制將為50，因為這是`pagination_limit`屬性的值。

   ![替代影像](assets/asset-sorting.png)

   **[!UICONTROL Experience B]**&#x200B;將定義代表您的商業邏輯執行下列動作的JSON：

   * 透過test_sorting功能旗標啟動排序演演算法功能
   * 執行`sorting_algorithm _**_attribute`中定義的`best_sellers`排序演演算法
   * 依`pagination_limit`中定義的分頁策略所定義，每頁傳回50項產品

   >[!NOTE]
   >
   >當[!DNL Adobe Target]儲存使用者以檢視體驗B時，將會傳回範例中具有已定義屬性的JSON。 在您的程式碼中，您需要檢查功能標幟`test_sorting`的值，以檢視是否應開啟排序功能。 若是如此，您將會使用`sorting_algorithm`屬性的`best_sellers`值，在產品清單檢視中顯示最暢銷的產品。 為您的應用程式顯示的產品限制將為50，因為這是`pagination_limit`屬性的值。

   ![替代影像](assets/asset-sorting-b.png)

## 4.新增對象

在&#x200B;**[!UICONTROL Targeting]**&#x200B;步驟中，保留&#x200B;**[!UICONTROL All Visitors]**&#x200B;對象。 這可讓您瞭解排序功能的影響，以及哪個演演算法和專案數量對結果影響最大。

![替代影像](assets/asset-audience-b.png)

## 5.設定流量分配

定義訪客百分比，以用來測試排序演演算法和分頁策略。 換言之，您要將這個測試轉出到您的使用者中哪個百分比？ 在此範例中，若要將此測試部署給所有登入的使用者，請將流量分配維持在100%。

![替代影像](assets/asset-allocation-100.png)

## 6.將流量分佈設為變數

定義會看見建議與最暢銷商品排序演演算法的訪客百分比，限製為每頁50項產品。 在此範例中，流量分配在體驗A和B之間維持50/50的分割比例。

![替代影像](assets/asset-variations-50.png)

## 7.設定報告

在&#x200B;**[!UICONTROL Goals & Settings]**&#x200B;步驟中，選擇&#x200B;**[!UICONTROL Adobe Target]**&#x200B;作為&#x200B;**[!UICONTROL Reporting Source]**，以便在[!DNL Adobe Target] UI中檢視您的A/B測試結果，或選擇&#x200B;**[!UICONTROL Adobe Analytics]**&#x200B;以便在Adobe Analytics UI中檢視這些結果。

![替代影像](assets/asset-reporting-b.png)

## 8.新增追蹤KPI的量度

選擇&#x200B;**[!UICONTROL Goal Metric]**&#x200B;以使用屬性測量功能測試。 在此範例中，成功取決於使用者是否購買產品，取決於他們看到的排序演演算法和分頁策略。

## 9.使用屬性實施功能測試至您的應用程式

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const options = {
  client: "testClient",
  organizationId: "ABCDEF012345677890ABCDEF0@AdobeOrg",
  decisioningMethod: "on-device",
  events: {
    clientReady: targetClientReady
  }
};
const targetClient = TargetClient.create(options);

function targetClientReady() {
  return targetClient.getAttributes(["product-results-page"]).then(function(attributes) {
    const test_sorting = attributes.getValue("product-results-page", "test-sorting");
    const sorting_algorithm = attributes.getValue("product-results-page", "sorting_algorithm");
    const pagination_limit = attributes.getValue("product-results-page", "pagination_limit");
  });
}
```

>[!TAB Java]

```java {line-numbers="true"}
import com.adobe.target.edge.client.ClientConfig;
import com.adobe.target.edge.client.TargetClient;
import com.adobe.target.delivery.v1.model.ChannelType;
import com.adobe.target.delivery.v1.model.Context;
import com.adobe.target.delivery.v1.model.ExecuteRequest;
import com.adobe.target.delivery.v1.model.MboxRequest;
import com.adobe.target.edge.client.entities.TargetDeliveryRequest;
import com.adobe.target.edge.client.model.TargetDeliveryResponse;

ClientConfig config = ClientConfig.builder()
    .client("testClient")
    .organizationId("ABCDEF012345677890ABCDEF0@AdobeOrg")
    .build();
TargetClient targetClient = TargetClient.create(config);
MboxRequest mbox = new MboxRequest().name("product-results-page").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
    .context(new Context().channel(ChannelType.WEB))
    .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
    .build();
Attributes attributes = targetClient.getAttributes(request, "product-results-page");
String testSorting = attributes.getString("product-results-page", "test-sorting");
String sortingAlgorithm = attributes.getString("product-results-page", "sorting_algorithm");
String paginationLimit = attributes.getString("product-results-page", "pagination_limit");
```

>[!ENDTABS]

## 10.實作程式碼以追蹤轉換事件

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

//When a conversion happens
TargetClient.sendNotifications({
    targetCookie,
    "request" : {
      "notifications" : [
        {
          type: "click",
          timestamp : Date.now(),
          id: "conversion",
          mbox : {
            name : "product-results-page"
          }
        }
      ]
    }
})
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);

ExecuteRequest executeRequest = new ExecuteRequest();

NotificationDeliveryService notificationDeliveryService = new NotificationDeliveryService();

Notification notification = new Notification();
notification.setId("conversion");
notification.setImpressionId(UUID.randomUUID().toString());
notification.setType(MetricType.CLICK);
notification.setTimestamp(System.currentTimeMillis());
notification.setTokens(
    Collections.singletonList(
        "IbG2Jz2xmHaqX7Ml/YRxRGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="));

TargetDeliveryRequest targetDeliveryRequest =
    TargetDeliveryRequest.builder()
        .context(context)
        .execute(executeRequest)
        .notifications(Collections.singletonList(notification))
        .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
notificationDeliveryService.sendNotification(request);

Attributes attributes = targetClient.getAttributes(request, "product-results-page");
String testSorting = attributes.getString("product-results-page", "test-sorting");
String sortingAlgorithm = attributes.getString("product-results-page", "sorting_algorithm");
String paginationLimit = attributes.getString("product-results-page", "pagination_limit");
```

>[!ENDTABS]

## 11.使用屬性啟用功能測試

![替代影像](assets/asset-activate.png)

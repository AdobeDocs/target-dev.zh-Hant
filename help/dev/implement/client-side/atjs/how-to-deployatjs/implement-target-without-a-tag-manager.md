---
keywords: 實作target，實作，實作at.js，標籤管理員，裝置上決策，裝置上決策
description: 瞭解如何指定設定（帳戶詳細資料、實作方法等） 實作 [!DNL Adobe Target] 不使用標籤管理員的at.js資料庫。
title: 我可以實作嗎？ [!DNL Target] 沒有Tag Manager？
feature: Implement Server-side
exl-id: f675ae21-105d-4aa3-9926-59291f1136b5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1752'
ht-degree: 44%

---

# 實作 [!DNL Target] 不使用標籤管理程式

實作的相關資訊 [!DNL Adobe Target] 不使用標籤管理員或標籤於 [!DNL Adobe Experience Platform].

>[!NOTE]
>
>中的標籤 [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 是實施的偏好方法 [!DNL Target] 和at.js資料庫。 下列資訊不適用於在中使用標籤 [!DNL Adobe Experience Platform] 實作 [!DNL Target].

若要存取「實作」頁面，請按一下 **[!UICONTROL 管理]** > **[!UICONTROL 實施]**.

您可以在此頁面指定下列設定：

* 帳戶詳細資料
* 實作方法
* 設定檔API
* 偵錯工具
* 隱私權

>[!NOTE]
>
>您可以覆寫at.js資料庫中的設定，而非在 [!DNL Target] Standard/Premium UI或使用REST API。 如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。

## 帳戶詳細資料

您可以檢視下列帳戶詳細資料。 無法變更這些設定。

| 設定 | 說明 |
| --- | --- |
| [!UICONTROL 用戶端代碼] | 用戶端代碼是使用 [!DNL Target] API 時通常需要的用戶端專用字元序列。 |
| [!UICONTROL IMS 組織 ID] | 此 ID 會將您的實施連結至 Adobe Experience Cloud 帳戶。 |
| [!UICONTROL 裝置上決策] | 若要啟用裝置上決策，請將切換開關滑動至「開啟」位置。<p>裝置上決策可讓您在伺服器上快取A/B和體驗鎖定目標(XT)行銷活動，並以幾乎零延遲的執行記憶體內決策。 如需詳細資訊，請參閱 [裝置上決策簡介](../../../server-side/sdk-guides/on-device-decisioning/overview.md). |
| [!UICONTROL 將所有符合裝置上決策條件的活動納入成品中] | （視條件而定）如果您啟用裝置上決策，便會顯示此選項。<p>如果您想要所有即時狀態，請將切換滑至「開啟」位置 [!DNL Target] 符合裝置上決策資格的活動會自動包含在成品中。<p>若將此切換保持關閉，表示您必須重新建立並啟動任何裝置上決策活動，才能將其納入產生的規則成品中。 |

## 實作方法

您可在「實作方法」面板中設定下列設定：

### 全域設定

>[!NOTE]
>
>這些設定會套用至全部 [!DNL Target] .js程式庫。 在「實作方法」區段中執行變更後，您必須下載程式庫並在實作中更新。

| 設定 | 說明 |
| --- | --- |
| [!UICONTROL 已啟用頁面載入（自動建立全域mbox）] | 選擇是否將全域 mbox 呼叫內嵌在 at.js 檔案中，以便每次載入頁面時自動觸發。 |
| [!UICONTROL 全域 mbox] | 選取全域 mbox 的名稱。依預設，此名稱為 target-global-mbox。<p>對於 at.js，mbox 名稱中可以使用特殊字元 (包括 &amp;)。 |
| [!UICONTROL 逾時（秒）] | 如果 [!DNL Target] 在已定義的期間內沒有回應內容，伺服器呼叫會逾時，並顯示預設內容。在訪客工作階段期間會繼續嘗試其他呼叫。預設值為 5 秒。<p>at.js 程式庫會使用 `XMLHttpRequest` 中的逾時設定。逾時是在觸發請求時開始計時，而於 [!DNL Target] 從伺服器收到回應時停止。如需詳細資訊，請參閱 Mozilla 開發人員網路上的 [XMLHttpRequest.timeout](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/timeout)。<p>如果在收到回應之前就發生指定的逾時，則會顯示預設內容，而訪客可能算為活動的參與者，因為所有資料收集都發生在 [!DNL Target] 邊緣。如果請求到達 [!DNL Target] 邊緣，訪客即納入計算。<p>設定逾時設定時，請考量下列事項:<ul><li>如果值太低，即使訪客應該算為活動的參與者，使用者還是可能幾乎都看到預設內容。</li><li>如果值太高，而如果您長時間使用本文隱藏，訪客可能會在網頁上看到空白區域或空白頁面。</li></ul>若要充分瞭解 mbox 回應時間，請在瀏覽器的開發人員工具中查看「網路」標籤。您也可以使用第三方 Web 效能監控工具，例如 Catchpoint。<p>**注意**: [visitorApiTimeout](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#visitorapitimeout) 設定可確保 [!DNL Target] 不會為了訪客 API 回應而等待太久。此設定和這裡說明的 at.js 逾時設定不影響彼此。 |
| [!UICONTROL 設定檔存留期] | 此設定會決定訪客設定檔儲存多久。依預設，訪客設定檔會儲存兩週。此設定最多可增加90天。<p>若要變更「設定檔存留期」設定，請聯絡[客戶服務](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html#reference_ACA3391A00EF467B87930A450050077C)。 |

### 主要實作方法

>[!NOTE]
>
>[!DNL Adobe Target]  同時支援at.js 1.*x* 與 at.js 2.*x* 之間的對應。升級至任一主要版本的at.js最新更新，以確保您執行的是支援的版本。

若要下載所需的at.js版本，請按一下適當的 **下載** 按鈕。

若要編輯at.js設定，請按一下 **[!UICONTROL 編輯]** ，位於所需的at.js版本旁。

>[!WARNING]
>
>在變更這些預設設定之前，請先諮詢 [客戶服務](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html#reference_ACA3391A00EF467B87930A450050077C) 以便不影響目前的實施。

除了上述設定以外，您也可以使用下列特定的at.js設定：

| 設定 | 說明 |
|--- |--- |
| 跨網域 | 針對at.js v1。*x*，指定跨網域功能是否 `disabled` (瀏覽器會在您的網域中設定Cookie （僅限第一方Cookie）)， `x only` （瀏覽器只會在Target網域中設定Cookie），或兩者皆設定，方法是選取 `enabled` （瀏覽器同時設定第一方和第三方Cookie）。 若是at.js v2.10和更新版本，請指定跨網域功能是否為 `enabled` （瀏覽器同時設定第一方和第三方Cookie）或 `disabled` （瀏覽器不會設定第三方Cookie）。 |
| 自訂資料庫標題 | 新增任何自訂 JavaScript 以包括在資料庫頂端。 |
| 自訂資料庫頁尾 | 新增任何自訂 JavaScript 以包含在程式庫底部。 |

### 設定檔API

啟用或停用透過 API 批次更新的驗證，並產生設定檔驗證 Token。

如需詳細資訊，請參閱 [設定檔API設定](/help/dev/before-implement/methods-to-get-data-into-target/profile-api-settings.md).

### 偵錯工具

產生授權權杖以使用進階 [!DNL Target] 偵錯工具。 按一下 **[!UICONTROL 產生新的驗證Token]**.

![產生新驗證權杖](../../../../before-implement/methods-to-get-data-into-target/assets/debugger-auth-token.png)

### 隱私權

這些設定可讓您使用 [!DNL Target] 符合適用的資料隱私權法律。

從「模糊化訪客IP位址」下拉式清單中選擇所需的設定：

* 最後一個八位元模糊化
* 整個IP模糊化
* 無

如需詳細資訊，請參閱[隱私權](/help/dev/before-implement/privacy/privacy.md)。

>[!NOTE]
>
>「舊版瀏覽器支援」選項可在at.js版本0.9.3和更早版本中取得。 at.js 0.9.4 版中移除了此選項。如需 at.js 支援的瀏覽器清單，請參閱[支援的瀏覽器](/help/dev/before-implement/supported-browsers.md)。<p>舊版瀏覽器是指不完全支援 CORS (跨來源資源共用) 的舊型瀏覽器。這些瀏覽器包括 Internet Explorer 瀏覽器 11 版以前的版本，以及 Safari 6 版及更舊版本。如果舊版瀏覽器支援已停用， [!DNL Target] 未在這些瀏覽器的報表中傳遞內容或計算訪客。 如果已啟用此選項，建議在舊版瀏覽器上執行品質保證，以確保良好的客戶體驗。

## 下載 at.js

使用下載程式庫的指示 [!DNL Target] 介面或下載API。

>[!NOTE]
>
>[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 是實施的偏好方法 [!DNL Target] 和at.js資料庫。 下列資訊不適用於在中使用標籤 [!DNL Adobe Experience Platform] 實作 [!DNL Target].
>
>[!DNL Adobe Target] 同時支援at.js 1.*x* 與 at.js 2.*x* 之間的對應。請升級至任一主要版本at.js的最新更新，以確保您執行的是支援的版本。 如需每一個版本有何功能的詳細資訊，請參閱 [at.js 版本詳細資料](/help/dev/implement/client-side/atjs/target-atjs-versions.md)。

### 使用下載at.js [!DNL Target] 介面

若要從下載at.js [!DNL Target] 介面：

1. 按一下「**[!UICONTROL 管理]** > 「**[!UICONTROL 實施]**」。
1. 在「實作方法」段落中，按一下 **[!UICONTROL 下載]** 按鈕（位於所需的at.js版本旁）。

### 使用下載at.js [!DNL Target] 下載API

若要使用 API 來下載 at.js:

1. 取得用戶端程式碼。

   您的使用者端代碼可在 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** 第頁，共 [!DNL Target] 介面。

1. 取得您的管理員編號。

   載入此 URL:

   ```
   https://admin.testandtarget.omniture.com/rest/v1/endpoint/<varname>client code</varname>
   ```

   取代 `client code` 使用步驟1取得的使用者端代碼。

   載入此 URL 的結果應該類似下列範例:

   ```
   { 
     "api": "https://admin6.testandtarget.omniture.com/admin/rest/v1" 
   }
   ```

   在此範例中，&quot;6&quot; 是管理員編號。

1. 下載 at.js.

   使用下列結構載入此 URL. 載入此 URL 會開始下載自訂的 at.js 檔案。

   ```
   https://admin<varname>admin number</varname>.testandtarget.omniture.com/admin/rest/v1/libraries/atjs/download?client=<varname>client code</varname>&version=<version number>
   ```

   * 取代 `admin number` 連同您的管理員編號。
   * 取代 `client code` 使用步驟1取得的使用者端代碼。
   * 取代 `version number` ，並使用所需的at.js版本號碼（例如2.2）。

>[!WARNING]
>
>此 [!DNL Target] team只會維護兩個at.js版本：最新版本和次新版本。 請視需要升級 at.js，以確保您執行的是支援的版本。如需每一個版本有何功能的詳細資訊，請參閱 [at.js 版本詳細資料](/help/dev/implement/client-side/atjs/target-atjs-versions.md)。

## at.js 實作

at.js 應實作於網站上每個頁面的 `<head>` 元素中。

典型實施 [!DNL Target] 不使用標籤管理程式，例如中的標籤 [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 看起來像這樣：

```
<!doctype html> 
<html> 
<head> 
    <meta charset="utf-8"> 
    <title>Title of the Page</title> 
    <!--Preconnect and DNS-Prefetch to improve page load time--> 
    <link rel="preconnect" href="//<client code>.tt.omtrdc.net"> 
    <link rel="dns-prefetch" href="//<client code>.tt.omtrdc.net"> 
    <!--/Preconnect and DNS-Prefetch--> 
    <!--Data Layer to enable rich data collection and targeting--> 
    <script> 
        var digitalData = { 
            "page": { 
                "pageInfo": { 
                    "pageName": "Home" 
                } 
            } 
        }; 
    </script> 
    <!--/Data Layer--> 
    <!-- targetPageParams(), targetPageParamsAll(), Data Providers or targetGlobalSettings() functions to enrich the visitor profile or modify the library settings--> 
    <script> 
        targetPageParams = function() { 
            return { 
                "a": 1, 
                "b": 2, 
                "pageName": digitalData.page.pageInfo.pageName, 
                "profile": { 
                    "age": 26, 
                    "country": { 
                        "city": "San Francisco" 
                    } 
                } 
            }; 
        }; 
    </script> 
    <!--/targetPageParams()--> 
 
    <!--jQuery or other helper libraries should be implemented before at.js if you would like to use their methods in Target--> 
    <script src="jquery-3.3.1.min.js"></script> 
    <!--/jQuery--> 
    <!--Target's JavaScript SDK, at.js--> 
    <script src="at.js"></script> 
    <!--/at.js--> 
</head>
<body> 
    The default content of the page 
</body> 
</html>
```

請考量下列重要注意事項:

* HTML5 Doctype (例如 `<!doctype html>`)。 不支援或較舊的doctypes可能會導致 [!DNL Target] 無法提出要求。
* 「預先連結」和「預先擷取」可能有助於加速網頁載入。如果您使用這些設定，請確定您取代 `<client code>` 使用您自己的使用者端代碼，您可從 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** 頁面。
* 如果有資料層，最好在 at.js 載入前，盡可能在網頁的 `<head>` 中詳細定義。這個位置能讓您在最大程度上使用此資訊 [!DNL Target] 進行個人化。
* 特殊 [!DNL Target] 函式，例如 `targetPageParams()`， `targetPageParamsAll()`、資料提供者和 `targetGlobalSettings()` 應在資料層載入後、at.js載入前定義。 或者，這些函式可以儲存在「編輯at.js設定」頁面的「資料庫標題」區段中，並儲存為at.js資料庫本身的一部分。 如需這些函式的詳細資訊，請參閱 [at.js函式](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md).
* 如果您使用JavaScript輔助程式庫（例如jQuery），請在以下日期前加入這些程式庫： [!DNL Target] 因此您可以在建置時使用其語法和方法 [!DNL Target] 體驗。
* 在網頁的 `<head>` 中加入 at.js。

## 追蹤轉換

訂購確認 mbox 會記錄關於您的網站上訂單的詳細資料，並允許根據收入和訂單報告。訂購確認 mbox 也可以促進建議演算法，例如「購買了產品 x、也購買了產品 y 的使用者」

>[!NOTE]
>
>如果使用者在您的網站上進行購買，Adobe建議您實作訂單確認mbox，即使您將Analytics用於 [!DNL Target] (A4T)以用於報表。

1. 在訂單詳細資料頁面中，請依照下方的模式插入 mbox 指令檔。
1. 使用目錄中的動態或靜態值來取代大寫的字母。

   >[!TIP]
   >
   >您也可以在任何mbox中傳遞訂單資訊(名稱不需是 `orderConfirmPage`)。 您也可以將訂單資訊傳遞至同一個促銷活動中的多個 mbox。

   ```
   <script type="text/javascript"> 
   adobe.target.trackEvent({ 
       "mbox": "orderConfirmPage", 
       "params":{  
           "orderId": "ORDER ID FROM YOUR ORDER PAGE",  
           "orderTotal": "ORDER TOTAL FROM YOUR ORDER PAGE",  
           "productPurchasedId": "PRODUCT ID FROM YOUR ORDER PAGE, PRODUCT ID2, PRODUCT ID3"  
       } 
   }); 
   </script> 
   ```

>[!NOTE]
>
>在訂單確認mbox中，使用逗號分隔多個產品ID。

「訂購確認」mbox 會使用下列參數:

| 參數 | 說明 |
|--- |--- |
| orderId | 要進行轉換計算之訂單的唯一識別值。<p>`orderId` 必須是唯一的。報表中會忽略重複的訂單。 |
| orderTotal | 購買貨幣值。<p>請勿加上貨幣符號。請使用小數點 (而非逗點) 表示小數值。 |
| productPurchasedId (選用) | 訂單中購買之產品 ID 的逗點分隔清單。<p>這些產品 ID 會顯示在稽核報表中，以支援其他報表分析。 |

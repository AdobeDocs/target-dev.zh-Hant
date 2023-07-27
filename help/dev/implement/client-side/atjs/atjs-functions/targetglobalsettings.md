---
keywords: serverstate， targetGlobalSettings， targetglobalsettings， globalSettings， globalsettings，全域設定， at.js，函式，函式， clientCode， clientcode， serverDomain， serverdomain， cookieDomain， serverstate5， serverstate6， serverstate7， serverstate8， serverstate9， targetGlobalSettings0， targetGlobalSettings1， targetGlobalSettings2， targetGlobalSettings5， cookiedomain， crossCrossDomain， crocrossDomain， cross， globalMboxAutoCreate， visitorApiTimeout， defaultContentHiddenStyle， defaultContentVisibleStyle， bodyHiddenStyle， bodyHidingEnabled， imsOrgId， secureOnly， overrideMboxEdgeServerTimeout， cookiedomain5， cookiedomain6， cookiedomain7， cookiomain8， cookiomain9， crossDomain0， crossCrossCroadDomain1， crossCrossDomain1， crossCrossCrossCrossDomain1， cross2， crossCrossDomain2， cross3， cross Domain4、crossDomain5、optoutEnabled、optout、opt out、selectorsPollingTimeout、dataProviders、Hybrid Personalization、deviceIdLifetime
description: 使用 [!UICONTROL targetGlobalSettings()] 的函式 [!DNL Adobe Target] at.js JavaScript程式庫以覆寫設定，而非使用 [!DNL Target] UI或REST API。
title: 如何使用 [!UICONTROL targetGlobalSettings()] 功能？
feature: at.js
exl-id: f6218313-6a70-448e-8555-b7b039e64b2c
source-git-commit: d5d25c6a559dafe446d26cca6c03d8e693cbd508
workflow-type: tm+mt
source-wordcount: '2521'
ht-degree: 19%

---

# [!UICONTROL targetGlobalSettings()]

您可以使用覆寫at.js資料庫中的設定 `[!UICONTROL targetGlobalSettings()]`，而不是在中進行設定 [!DNL Target] UI或使用REST API。

## 設定

您可以覆寫下列設定:

### artifectlocation

* **類型:** 字串
* **預設值**：無
* **說明**：的完整URL [裝置上決策規則成品](../../../server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)

### bodyHiddenStyle

* **類型:** 字串
* **預設值**：內文{不透明度： 0 }
* **說明**：僅用於 `globalMboxAutocreate === true` 將閃爍的機會最小化。

  如需詳細資訊，請參閱 [at.js 處理忽隱忽現情況的方式](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)。

### bodyHidingEnabled

* **型別**：布林值
* **預設值**： true
* **說明**：用來控制發生下列情況時的忽隱忽現情形 `target-global-mbox` 用於傳遞在視覺化體驗撰寫器中建立的選件，也稱為視覺化選件。

### clientCode

* **類型:** 字串
* **預設值**：透過UI設定的值。
* **說明**：代表使用者端代碼。

### cookieDomain

* **類型:** 字串
* **預設值**：如果可能的話，設定為最上層網域。
* **說明**：代表儲存Cookie時所使用的網域。

### crossDomain

* **類型:** 字串
* **預設值**：透過UI設定的值。
* **說明**：指出是否啟用跨網域追蹤。 允許的值取決於您的at.js版本。 針對at.js v1。*x*，指定跨網域功能是否 `disabled` (瀏覽器會在您的網域中設定Cookie （僅限第一方Cookie）)， `x only` (瀏覽器在中設定Cookie [!DNL Target]的網域)，或兩者皆選 `enabled` （瀏覽器同時設定第一方和第三方Cookie）。 若是at.js v2.10和更新版本，請指定跨網域功能是否為 `enabled` （瀏覽器同時設定第一方和第三方Cookie）或 `disabled` （瀏覽器不會設定第三方Cookie）。

### cspScriptNonce

* **型別**：請參閱 [內容安全性原則](#content-security-policy) 底下。
* **預設值**：請參閱 [內容安全性原則](#content-security-policy) 底下。
* **說明**：請參閱 [內容安全性原則](#content-security-policy) 底下。

### cspStyleNonce

* **型別**：請參閱 [內容安全性原則](#content-security-policy) 底下。
* **預設值**：請參閱 [內容安全性原則](#content-security-policy) 底下。
* **說明**：請參閱 [內容安全性原則](#content-security-policy) 底下。

### dataProviders

* **型別**：請參閱 [資料提供者](#data-providers) 底下。
* **預設值**：請參閱 [資料提供者](#data-providers) 底下。
* **說明**：請參閱 [資料提供者](#data-providers) 底下。

### 決策方法

* **類型:** 字串
* **預設值**：伺服器端
* **其他值**：裝置上、混合
* **說明**：請參閱下方的決策方法。

  **決策方法**

  透過裝置上決策， [!DNL Target] 推出稱為「決策方法」的新設定，指定at.js提供您體驗的方式。 此 `decisioningMethod` 有三個值：僅限伺服器端、僅限裝置上以及混合。 時間 `decisioningMethod` 設定於 `targetGlobalSettings()`，可作為所有使用者的預設決策方法 [!DNL Target] 決定。

  **僅限伺服器端**：

  僅伺服器端是預設的決策方法，可在您的Web屬性上實作和部署at.js 2.5以上版本時立即使用。

  僅使用伺服器端作為預設設定，表示所有決定都是在 [!DNL Target] 邊緣網路，其中涉及封鎖伺服器呼叫。 此方法可能會增加延遲，但也有顯著的好處，例如讓您能夠套用 [!DNL Target]的機器學習功能包括 [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)， [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) (AP)，以及 [自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) 活動。

  此外，使用增強您的個人化體驗 [!DNL Target]的使用者設定檔會跨工作階段和管道儲存，可為您的業務帶來強大的成果。

  最後，伺服器端僅可讓您使用Adobe Experience Cloud，並微調可透過Audience Manager和Adobe Analytics區段鎖定的對象。

  **僅限裝置上**：

  僅限裝置上決策是必須在at.js 2.5以上版本中設定的決策方法，而裝置上決策則只應在整個網頁中使用。

  裝置上決策能夠以極快的速度提供您的體驗和個人化活動，因為決策是由快取規則成品做出的，該成品包含您符合裝置上決策資格的所有活動。

  若要瞭解哪些活動符合裝置上決策的資格，請參閱支援的功能區段。

  只有在需要決定的所有頁面的效能都極重要時，才應使用此決策方法 [!DNL Target]. 此外，請記住，選取此決策方法時，您的 [!DNL Target] 不符合裝置上決策資格的活動將不會傳送或執行。 at.js資料庫2.5+已設定為僅尋找快取規則成品以做出決策。

  **混合式**：

  混合式決策方法當裝置上決策和活動都需要對進行網路呼叫時，必須在at.js 2.5+中設定。 [!DNL Adobe Target] 必須執行Edge網路。

  當您同時管理裝置上決策活動和伺服器端活動時，在考慮如何部署和布建時，可能會有點複雜和繁瑣 [!DNL Target] 在您的頁面上。 使用混合決策方法， [!DNL Target] 知道何時必須對 [!DNL Adobe Target] 適用於需要伺服器端執行之活動的邊緣網路，以及僅執行裝置上決策的時機。

  JSON規則成品包含中繼資料，可通知at.js mbox是否正在執行伺服器端活動或裝置上決策活動。 此決策方法可確保透過裝置上決策完成您想要快速傳送的活動，而對於需要更強大ML驅動個人化的活動，這些活動會透過 [!DNL Adobe Target] 邊緣網路。

### defaultContentHiddenStyle

* **類型:** 字串
* **預設值**：可見性：隱藏
* **說明**：僅用於包住使用類別名稱為「mboxDefault」的DIV，並透過執行的mbox `mboxCreate()`， `mboxUpdate()`，或 `mboxDefine()` 以隱藏預設內容。

### defaultContentVisibleStyle

* **類型:** 字串
* **預設值**：可見性：可見
* **說明**：僅用於包住使用類別名稱為「mboxDefault」的DIV，並透過執行的mbox `mboxCreate()`， `mboxUpdate()`，或 `mboxDefine()` 以顯示套用的選件（若有）或預設內容。

### deviceIdLifetime

* **型別**：數字
* **預設值**：63244800000毫秒= 2年
* **說明**：時間長度 `deviceId` 儲存在Cookie中。

>[!NOTE]
>
>at.js 2.3.1版或更新版本可覆寫deviceIdLifetime設定。

### 已啟用

* **型別**：布林值
* **預設值**： true
* **說明**：啟用時，會 [!DNL Target] 擷取體驗和DOM操作以產生體驗的要求會自動執行。 此外， [!DNL Target] 呼叫可透過手動執行 `getOffer(s)` / `applyOffer(s)`.

  停用時， [!DNL Target] 系統不會自動或手動執行請求。

### globalMboxAutoCreate

* **型別**：數字
* **預設值**：透過UI設定的值。
* **說明**：指出是否應觸發全域mbox要求。

### imsOrgId

* **類型:** 字串
* **預設值**： true
* **說明**：代表IMS組織ID。

### optinEnable

* **型別**：布林值
* **預設值**： false
* **說明**： [!DNL Target] 透過Adobe Experience Platform支援選擇加入功能，有助於支援同意管理策略。 選擇加入功能可讓客戶控制引發 [!DNL Target] 標記的方法和時機。也可選擇透過Adobe Experience Platform預先核准 [!DNL Target] 標籤之間。 若要啟用在中使用選擇加入的功能 [!DNL Target] at.js資料庫，新增 `optinEnabled=true` 設定。 在Adobe Experience Platform中，您必須在擴充功能安裝檢視中，從「GDPR選擇加入」下拉式清單選取「啟用」。 請參閱 [Adobe Experience Platform檔案](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 以取得更多詳細資料。 如需此設定與隱私權及資料保護規範(包括歐盟一般資料保護規範(GDPR)和加州消費者隱私保護法(CCPA))相關的詳細資訊，請參閱 [隱私權與資料保護規範](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md).

### optoutEnabled

* **型別**：布林值
* **預設值**： false
* **說明**：指示是否 [!DNL Target] 應呼叫訪客API `isOptedOut()` 函式。 這屬於裝置圖表啟用的一部分。

### overrideMboxEdgeServer

* **型別**：布林值
* **預設值**： true （從at.js版本1.6.2開始）
* **說明**：指示我們是否應該使用 `<clientCode>.tt.omtrdc.net` 網域或 `mboxedge<clusterNumber>.tt.omtrdc.net` 網域。

  如果此值為 true，則會將 `mboxedge<clusterNumber>.tt.omtrdc.net` 網域儲存至 Cookie. 目前未使用 [CNAME](/help/dev/before-implement/implement-cname-support-in-target.md) 使用at.js 1.8.2和at.js 2.3.1之前的at.js版本時。如果您有此問題，請考慮 [更新at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md) 更新至支援的版本。

### overrideMboxEdgeServerTimeout

* **型別**：數字
* **預設值**： 1860000 => 31分鐘
* **說明**：指出包含的Cookie存留期 `mboxedge<clusterNumber>.tt.omtrdc.net` 值。

### Pageloadenabled

* **型別**：布林值
* **預設值**： true
* **說明**：啟用時，會自動擷取頁面載入時必須傳回的體驗。

### pollingInterval

* **型別**：數字
* **預設值**：300000 （以毫秒為單位的五分鐘）
* **說明**： at.js擷取裝置上決策成品新版本並更新快取的間隔。 300000是允許的最小值 `pollingInterval`.

### secureOnly

* **型別**：布林值
* **預設值**： false
* **說明**：指出at.js是否應該僅使用HTTPS或根據頁面通訊協定，允許在HTTP與HTTPS之間切換。 若設為true，secureOnly也會將Secure和SameSite屬性設為mbox Cookie。

### selectorsPollingTimeout

* **型別**：數字
* **預設值**：5000毫秒= 5秒
* **說明**：在at.js 0.9.6中， [!DNL Target] 推出了這項新設定，且可以透過覆寫此設定 `targetGlobalSettings`.

  此 `selectorsPollingTimeout` 設定代表使用者端願意等候選取器所識別的所有元素出現在頁面上的時間。

  透過可視化體驗撰寫器 (VEC) 建立的活動，其具有的選件包含選取器。

### serverDomain

* **類型:** 字串
* **預設值**：透過UI設定的值。
* **說明**：代表 [!DNL Target] edge server。

### serverState

* **型別**：請參閱 [混合個人化](#hybrid-personalization) 底下。
* **預設值**：請參閱 [混合個人化](#hybrid-personalization) 底下。
* **說明**：請參閱 [混合個人化](#hybrid-personalization) 底下。

### telemetryEnabled

* **型別**：布林值
* **預設值**： true
* **說明**：啟用後，Adobe會收集SDK功能使用情況和效能遙測資料。 不會收集個人資料。

### timeout

* **型別**：數字
* **預設值**：透過UI設定的值。
* **說明**：代表 [!DNL Target] 邊緣要求逾時。

### viewsEnabled {#viewsenabled}

* **型別**：布林值
* **預設值**： true
* **說明**：啟用時，會在頁面載入時自動擷取檢視。 時間 `triggerView` 稱為，適用的檢視會顯示在瀏覽器中。 如果停用此選項，頁面載入時不會擷取檢視，並且 `triggerView` 不執行任何動作。 at.js 2.*x* 中受到支援。

### visitorApiTimeout

* **型別**：數字
* **預設值**：2000毫秒= 2秒
* **說明**：代表訪客API請求逾時。

## 使用狀況

此函式可在at.js載入前或其中定義 **管理** > **實施** > **編輯at.js設定** > **程式碼設定** > **資料庫標題**.

資料庫標頭欄位允許輸入自由格式的 JavaScript。自訂程式碼看起來應該類似於下列範例:

```javascript {line-numbers="true"}
window.targetGlobalSettings = {
   timeout: 200, // using custom timeout
   visitorApiTimeout: 500, // using custom API timeout
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com
};
```

## 資料提供者 {#data-providers}

此設定可讓客戶收集來自協力廠商資料提供者（例如Demandbase、BlueKai和自訂服務）的資料，並將資料傳遞至 [!DNL Target] 作為全域mbox請求中的mbox引數。 它透過非同步和同步要求，以支援來自多個提供者的資料收集。使用此方法可讓您方便管理預設頁面內容的忽隱忽現情形，同時對每個提供者包含獨立的逾時計算，以限制對頁面效能的影響

>[!NOTE]
>
>資料提供者需使用at.js 1.3或更新版本。

以下影片包含更多資訊:

| 影片 | 說明 |
|--- |--- |
| [使用 Adobe Target 中的資料提供者](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html) | 資料提供者這項功能可以讓您輕鬆將資料從第三方傳入 Target。第三方可能是氣象服務、DMP，甚至是您自己的 Web 服務。接著，您就能使用此資料來建立對象、鎖定內容及擴充訪客設定檔。 |
| [實作 Adobe Target 中的資料提供者](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html) | 實作詳細資訊和如何使用Adobe的範例 [!DNL Target]的dataProviders功能，可從第三方資料提供者擷取資料，並將其傳遞至 [!DNL Target] 要求。 |

`window.targetGlobalSettings.dataProviders` 設定是資料提供者的陣列。

每個資料提供者具有下列結構:

| 機碼 | 類型 | 說明 |
|--- |--- |--- |
| 名稱 | 字串 | 提供者的名稱。 |
| version | 字串 | 提供者版本。此機碼將用於提供者演進。 |
| timeout | 數字 | 如果這是網路要求，則代表提供者逾時。此機碼為選用。 |
| provider | 函數 | 包括提供者資料擷取邏輯的函數。<p>函數有單一必要參數: `callback`。callback 參數為函數，僅應該在成功擷取資料或發生錯誤時叫用。<p>callback 預期兩個參數:<ul><li>error: 指出是否發生錯誤。如果各項都正常，則此參數應該設為 null。</li><li>params： JSON物件，代表將在下列檔案中傳送的引數： [!DNL Target] 要求。</li></ul> |

下列範例顯示的資料提供者正在使用同步執行:

```javascript {line-numbers="true"}
var syncDataProvider = {
  name: "simpleDataProvider",
  version: "1.0.0",
  provider: function(callback) {
    callback(null, {t1: 1});
  }
};

window.targetGlobalSettings = {
  dataProviders: [
    syncDataProvider
  ]
};
```

在at.js處理之後 `window.targetGlobalSettings.dataProviders`，則 [!DNL Target] 請求將包含新引數： `t1=1`.

如果您想要新增至的引數，以下是範例 [!DNL Target] 會從第三方服務（例如Bluekai、Demandbase等）擷取請求：

```javascript {line-numbers="true"}
var blueKaiDataProvider = {
   name: "blueKai",
   version: "1.0.0",
   provider: function(callback) {
      // simulating network request
     setTimeout(function() {
       callback(null, {t1: 1, t2: 2, t3: 3});
     }, 1000);
   }
}

window.targetGlobalSettings = {
   dataProviders: [
      blueKaiDataProvider
   ]
};
```

在at.js處理之後 `window.targetGlobalSettings.dataProviders`，則 [!DNL Target] 請求將包含其他引數： `t1=1`， `t2=2` 和 `t3=3`.

以下範例使用資料提供者來收集氣象API資料，並將資料以引數形式傳送到 [!DNL Target] 要求。 此 [!DNL Target] 請求會有其他引數，例如 `country` 和 `weatherCondition`.

```javascript {line-numbers="true"}
var weatherProvider = {
      name: "weather-api",
      version: "1.0.0",
      timeout: 2000,
      provider: function(callback) {
        var API_KEY = "caa84fc6f5dc77b6372d2570458b8699";
        var lat = 44.426767399999996;
        var lon = 26.1025384;
        var url = "//api.openweathermap.org/data/2.5/weather?";
        var data = {
          lat: lat,
          lon: lon,
          appId: API_KEY
        }

        $.ajax({
          type: "GET",
                url: url,
          dataType: "json",
          data: data,
          success: function(data) {
            console.log("Weather data", data);
            callback(null, {
              country: data.sys.country,
              weatherCondition: data.weather[0].main
            });
          },
          error: function(err) {
            console.log("Error", err);
            callback(err);
          }
        });
      }
    };

    window.targetGlobalSettings = {
      dataProviders: [weatherProvider]
    };
```

使用 `dataProviders` 設定時，請考慮下列各項:

* 如果新增至 `window.targetGlobalSettings.dataProviders` 的資料提供者非同步，則會並行執行。訪客 API 要求將與新增至 `window.targetGlobalSettings.dataProviders` 的函數並行執行，以允許最低的等候時間。
* at.js 不會嘗試將資料快取。如果資料提供者擷取資料一次，則資料提供者應該確定已將該資料快取，並且當叫用該提供者函數時，可做為第二個叫用的快取資料。

## 內容安全性政策

at.js 2.3.0+支援在套用傳遞時，在附加至頁面DOM的SCRIPT和STYLE標籤上設定內容安全性原則Nonce [!DNL Target] 選件。

SCRIPT和STYLE Nonce應該設定在 `targetGlobalSettings.cspScriptNonce` 和 `targetGlobalSettings.cspStyleNonce` 相應地在at.js 2.3.0+載入之前。 請參閱下列範例：

```javascript {line-numbers="true"}
...
<head>
 <script nonce="<script_nonce_value>">
window.targetGlobalSettings = {
  cspScriptNonce: "<csp_script_nonce_value>",
  cspStyleNonce: "<csp_style_nonce_value>"
};
 </script>
 <script nonce="<script_nonce_value>" src="at.js"></script>
...
</head>
...
```

晚於 `cspScriptNonce` 和 `cspStyleNonce` 指定設定後，at.js 2.3.0+會在套用時附加至DOM的所有SCRIPT和STYLE標籤上，將這些設定設為Nonce屬性 [!DNL Target] 選件。

## 混合個人化

`serverState` 是at.js v2.2+中提供的設定，當混合整合以下專案時，可用來最佳化頁面效能： [!DNL Target] 已實作。 混合整合意指您在使用者端上同時使用at.js v2.2+和傳送API或 [!DNL Target] 伺服器端的SDK提供體驗。 `serverState` 讓 at.js v2.2+ 能夠直接從伺服器端擷取並傳回至用戶端的內容套用體驗，做為所提供頁面的一部分。

### 先決條件

您必須混合整合 [!DNL Target].

* **伺服器端**：您必須使用 [傳送API](/help/dev/implement/delivery-api/overview.md) 或 [Target SDK](/help/dev/implement/server-side/sdk-guides/getting-started/getting-started.md).
* **使用者端**：您必須使用 [at.js 2.2版或更新版本](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

### 程式碼範例

若要深入瞭解其運作方式，請參閱下列程式碼範例，瞭解伺服器上的程式碼範例。 程式碼假設您使用 [Target Node.js SDK](https://github.com/adobe/target-nodejs-sdk).

```javascript {line-numbers="true"}
// First, we fetch the offers via Target Node.js SDK API, as usual
const targetResponse = await targetClient.getOffers(options);
// A successfull response will contain Target Delivery API request and response objects, which we need to set as serverState
const serverState = {
  request: targetResponse.request,
  response: targetResponse.response
};
// Finally, we should set window.targetGlobalSettings.serverState in the returned page, by replacing it in a page template, for example
const PAGE_TEMPLATE = `
<!doctype html>
<html>
<head>
  ...
  <script>
    window.targetGlobalSettings = {
      overrideMboxEdgeServer: true,
      serverState: ${JSON.stringify(serverState, null, " ")}
    };
  </script>
  <script src="at.js"></script>
</head>
...
</html>
`;
// Return PAGE_TEMPLATE to the client ...
```

範例 `serverState` 檢視預先擷取的物件JSON如下所示：

```javascript {line-numbers="true"}
{
 "request": {
  "requestId": "076ace1cd3624048bae1ced1f9e0c536",
  "id": {
   "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "context": {
   "channel": "web",
   "timeOffsetInMinutes": 0
  },
  "experienceCloud": {
   "analytics": {
    "logging": "server_side",
    "supplementalDataId": "7D3AA246CC99FD7F-1B3DD2E75595498E"
   }
  },
  "prefetch": {
   "views": [
    {
     "address": {
      "url": "my.testsite.com/"
     }
    }
   ]
  }
 },
 "response": {
  "status": 200,
  "requestId": "076ace1cd3624048bae1ced1f9e0c536",
  "id": {
   "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "testclient",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
   "views": [
    {
     "name": "home",
     "key": "home",
     "options": [
      {
       "type": "actions",
       "content": [
        {
         "type": "setHtml",
         "selector": "#app > DIV.app-container:eq(0) > DIV.page-container:eq(0) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(1) > DIV.heading:eq(0) > H1.title:eq(0)",
         "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > H1:nth-of-type(1)",
         "content": "<span style=\"color:#FF0000;\">Latest</span> Products for 2020"
        }
       ],
       "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
       "responseTokens": {
        "profile.memberlevel": "0",
        "geo.city": "dublin",
        "activity.id": "302740",
        "experience.name": "Experience B",
        "geo.country": "ireland"
       }
      }
     ],
     "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
    }
   ]
  }
 }
}
```

頁面載入瀏覽器後，at.js套用所有 [!DNL Target] 優惠來自 `serverState` 立即，不會對 [!DNL Target] edge. 此外，at.js只會預先隱藏其 [!DNL Target] 內容擷取的伺服器端提供選件，因此會積極地影響頁面載入效能和一般使用者體驗。

### 重要附註

使用時，請考慮下列事項 `serverState`：

* 目前，at.js v2.2僅支援透過serverState為以下專案提供體驗：

   * VEC建立的活動，會在頁面載入時執行。
   * 預先擷取的檢視。

     若使用SPA [!DNL Target] 檢視和 `triggerView()` 在at.js API中，at.js v2.2會快取伺服器端預先擷取之所有檢視的內容，並在透過觸發每個檢視時套用這些內容 `triggerView()`，同樣不會觸發對的任何其他內容擷取呼叫 [!DNL Target].

   * **注意**：目前，不支援在伺服器端擷取的mbox `serverState`.

* 套用時 `serverState` 選件，at.js會考慮 `pageLoadEnabled` 和 `viewsEnabled` 設定（例如「頁面載入」選件），若 `pageLoadEnabled` 設定為false。

  若要開啟這些設定，請啟用切換功能 **管理>實作>編輯>啟用頁面載入**.

  ![啟用頁面載入設定](../../assets/page-load-enabled-setting.png)

* 如果您使用 `serverState` 和使用 `<script>` 標籤時，請確保您的HTML內容使用 `<\/script>` 而非 `</script>`. 如果您使用 `</script>`，瀏覽器會解譯 `</script>` 作為內嵌SCRIPT的結尾，則可能會中斷HTML頁面。

### 其他資源

若要進一步瞭解如何 `serverState` 有效，請檢視下列資源：

* [程式碼範例](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/advanced-atjs-integration-serverstate).
* [單頁應用程式(SPA)範例應用程式，搭配 `serverState`](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/react-shopping-cart-demo).

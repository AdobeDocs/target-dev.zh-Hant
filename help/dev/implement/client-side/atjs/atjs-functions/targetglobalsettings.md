---
keywords: serverstate， targetGlobalSettings， targetglobalsettings， globalSettings， globalsettings，全域設定， at.js，函式，函式， clientCode， clientcode， serverDomain， serverdomain， cookieDomain， serverstate5， serverstate6， serverstate7， serverstate8， serverstate9， targetGlobalSettings0， targetGlobalSettings1， targetGlobalSettings2， targetGlobalSettings5， cookiedomain， crossCrossDomain， crocrossDomain， cross， globalMboxAutoCreate， visitorApiTimeout， defaultContentHiddenStyle， defaultContentVisibleStyle， bodyHiddenStyle， bodyHidingEnabled， imsOrgId， secureOnly， overrideMboxEdgeServerTimeout， cookiedomain5， cookiedomain6， cookiedomain7， cookiomain8， cookiomain9， crossDomain0， crossCrossCroadDomain1， crossCrossDomain1， crossCrossCrossCrossDomain1， cross2， crossCrossDomain2， cross3， cross Domain4、crossDomain5、optoutEnabled、optout、opt out、selectorsPollingTimeout、dataProviders、Hybrid Personalization、deviceIdLifetime
description: 使用 [!DNL Adobe Target] at.js JavaScript資料庫的[!UICONTROL targetGlobalSettings()]函式來覆寫設定，而不使用 [!DNL Target] UI或REST API。
title: 如何使用[!UICONTROL targetGlobalSettings()]函式？
feature: at.js
exl-id: f6218313-6a70-448e-8555-b7b039e64b2c
source-git-commit: 12cf430b65695d38d1651f2a97df418d82d231f3
workflow-type: tm+mt
source-wordcount: '2565'
ht-degree: 18%

---

# [!UICONTROL targetGlobalSettings()]

您可以使用`[!UICONTROL targetGlobalSettings()]`覆寫at.js資料庫中的設定，而非在[!DNL Target] UI中或使用REST API進行設定。

## 設定

您可以覆寫下列設定:

### aepSandboxId

* **類型:** 字串
* **預設值**： null
* **描述**：選用引數，用來傳送[!DNL Adobe Experience Platform]沙箱ID以與[!DNL Target]共用在非預設沙箱中建立的[!DNL Adobe Experience Platform]目的地。 如果`aepSandboxId`不是空值，也必須提供`aepSandboxName`。

### aepSandboxName

* **類型:** 字串
* **預設值**： null
* **描述**：選用引數，用來傳送[!DNL Adobe Experience Platform]沙箱名稱，以與[!DNL Target]共用在非預設沙箱中建立的[!DNL Adobe Experience Platform]目的地。 如果`aepSandboxName`不是空值，也必須提供`aepSandboxId`。

### artifectlocation

* **類型:** 字串
* **預設值**：無
* **描述**： [裝置上決策規則成品](../../../server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)的完整URL

### bodyHiddenStyle

* **類型:** 字串
* **預設值**：內文{不透明度： 0 }
* **描述**：僅在`globalMboxAutocreate === true`時使用，以將閃爍的機會最小化。

  如需詳細資訊，請參閱[at.js處理忽隱忽現情況的方式](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)。

### bodyHidingEnabled

* **型別**：布林值
* **預設值**： true
* **描述**：當使用`target-global-mbox`傳遞在視覺化體驗撰寫器中建立的選件（也稱為視覺選件）時，用來控制閃爍。

### clientCode

* **類型:** 字串
* **預設值**：透過UI設定的值。
* **描述**：代表使用者端代碼。

### cookieDomain

* **類型:** 字串
* **預設值**：如果可能的話，設定為最上層網域。
* **描述**：代表儲存Cookie時所使用的網域。

### crossDomain

* **類型:** 字串
* **預設值**：透過UI設定的值。
* **描述**：指出是否啟用跨網域追蹤。 允許的值取決於您的at.js版本。 針對at.js v1。*x*，藉由選取`enabled` （瀏覽器同時設定第一方和第三方Cookie），指定跨網域功能是`disabled` (僅瀏覽器在您的網域中設定Cookie （第一方Cookie）)、`x only` （僅瀏覽器在[!DNL Target]的網域中設定Cookie）或兩者皆有。 針對at.js v2.10和更新版本，請指定跨網域功能是`enabled` （瀏覽器同時設定第一方和第三方Cookie）還是`disabled` （瀏覽器未設定第三方Cookie）。

### cspScriptNonce

* **型別**：請參閱下方的[內容安全性原則](#content-security-policy)。
* **預設值**：請參閱下方的[內容安全性原則](#content-security-policy)。
* **描述**：請參閱下方的[內容安全性原則](#content-security-policy)。

### cspStyleNonce

* **型別**：請參閱下方的[內容安全性原則](#content-security-policy)。
* **預設值**：請參閱下方的[內容安全性原則](#content-security-policy)。
* **描述**：請參閱下方的[內容安全性原則](#content-security-policy)。

### dataProviders

* **型別**：請參閱下方的[資料提供者](#data-providers)。
* **預設值**：請參閱下方的[資料提供者](#data-providers)。
* **描述**：請參閱下方的[資料提供者](#data-providers)。

### 決策方法

* **類型:** 字串
* **預設值**：伺服器端
* **其他值**：裝置上，混合
* **描述**：請參閱下方的決策方法。

  **決策方法**

  透過裝置上決策，[!DNL Target]引入稱為「決策方法」的新設定，以指示at.js如何提供您的體驗。 `decisioningMethod`有三個值：僅限伺服器端、僅限裝置上以及混合式。 當`decisioningMethod`設定在`targetGlobalSettings()`中時，它會做為所有[!DNL Target]決定的預設決定方法。

  **僅限伺服器端**：

  僅伺服器端是預設的決策方法，可在您的Web屬性上實作和部署at.js 2.5以上版本時立即使用。

  僅使用伺服器端作為預設設定，表示所有決定都是在[!DNL Target]邊緣網路上做出，其中涉及封鎖伺服器呼叫。 此方法可增加延遲時間，但也有顯著的優點，例如可讓您套用[!DNL Target]的機器學習功能，包括[Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)、[Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) (AP)和[自動鎖定目標](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html)活動。

  此外，使用[!DNL Target]的使用者設定檔來增強您的個人化體驗（此設定檔會跨工作階段和管道儲存），可為您的業務提供強大的成果。

  最後，伺服器端僅可讓您使用Adobe Experience Cloud，並微調可透過Audience Manager和Adobe Analytics區段鎖定的對象。

  **僅限裝置上**：

  僅限裝置上決策是必須在at.js 2.5以上版本中設定的決策方法，而裝置上決策則只應在整個網頁中使用。

  裝置上決策能夠以極快的速度提供您的體驗和個人化活動，因為決策是由快取規則成品做出的，該成品包含您符合裝置上決策資格的所有活動。

  若要瞭解哪些活動符合裝置上決策的資格，請參閱支援的功能區段。

  只有在需要[!DNL Target]決定的所有頁面的效能都非常關鍵時，才應該使用此決定方法。 此外，請記住，選取此決策方法時，將不會傳送或執行您不符合裝置上決策資格的[!DNL Target]活動。 at.js資料庫2.5+已設定為僅尋找快取規則成品以做出決策。

  **混合式**：

  混合式決策方法必須在at.js 2.5+中設定，當需要對[!DNL Adobe Target] Edge網路進行網路呼叫的裝置上決策和活動都必須執行時。

  當您同時管理裝置上決策活動和伺服器端活動時，在考慮如何在您的頁面上部署和布建[!DNL Target]時，可能會有點複雜和繁瑣。 使用混合式作為決策方法，[!DNL Target]知道何時必須對[!DNL Adobe Target] Edge網路進行伺服器呼叫，以進行需要伺服器端執行的活動，以及何時僅執行裝置上決策。

  JSON規則成品包含中繼資料，可通知at.js mbox是否正在執行伺服器端活動或裝置上決策活動。 此決策方法可確保您要快速傳送的活動能透過裝置上決策完成，而針對需要更強大ML驅動個人化的活動，這些活動會透過[!DNL Adobe Target] Edge網路完成。

### defaultContentHiddenStyle

* **類型:** 字串
* **預設值**：可見度：隱藏
* **描述**：僅用於包住使用類別名稱為「mboxDefault」的DIV，並透過`mboxCreate()`、`mboxUpdate()`或`mboxDefine()`執行以隱藏預設內容的mbox。

### defaultContentVisibleStyle

* **類型:** 字串
* **預設值**：可見度：可見
* **說明**：僅用於包住使用類別名稱為「mboxDefault」的DIV，並透過`mboxCreate()`、`mboxUpdate()`或`mboxDefine()`執行以顯示套用的選件（若有）或預設內容的mbox。

### deviceIdLifetime

* **型別**：數字
* **預設值**： 63244800000毫秒= 2年
* **描述**：在Cookie中儲存的時間量`deviceId`。

>[!NOTE]
>
>at.js 2.3.1版或更新版本可覆寫deviceIdLifetime設定。

### 已啟用

* **型別**：布林值
* **預設值**： true
* **描述**：啟用時，會自動執行擷取體驗的[!DNL Target]要求以及轉譯體驗的DOM操作。 此外，可以透過`getOffer(s)` / `applyOffer(s)`手動執行[!DNL Target]呼叫。

  停用時，不會自動或手動執行[!DNL Target]要求。

### globalMboxAutoCreate

* **型別**：數字
* **預設值**：透過UI設定的值。
* **Description**：指出是否應該觸發全域mbox要求。

### imsOrgId

* **類型:** 字串
* **預設值**： true
* **描述**：代表IMS組織ID。

### optinEnable

* **型別**：布林值
* **預設值**： false
* **說明**： [!DNL Target]透過Adobe Experience Platform提供選擇加入功能支援，以協助支援您的同意管理策略。 選擇加入功能可讓客戶控制引發 [!DNL Target] 標記的方法和時機。也可選擇透過Adobe Experience Platform預先核准[!DNL Target]標籤。 若要啟用在[!DNL Target] at.js資料庫中使用選擇加入的功能，請新增`optinEnabled=true`設定。 在Adobe Experience Platform中，您必須在擴充功能安裝檢視中，從「GDPR選擇加入」下拉式清單選取「啟用」。 如需詳細資訊，請參閱[Adobe Experience Platform檔案](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)。 如需此設定與隱私權及資料保護規範(包括歐盟一般資料保護規範(GDPR)及加州消費者隱私保護法(CCPA))相關的詳細資訊，請參閱[隱私權及資料保護規範](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)。

### optoutEnabled

* **型別**：布林值
* **預設值**： false
* **描述**：指出[!DNL Target]是否應呼叫訪客API `isOptedOut()`函式。 這屬於裝置圖表啟用的一部分。

### overrideMboxEdgeServer

* **型別**：布林值
* **預設值**： true （從at.js 1.6.2版開始true）
* **描述**：指出應使用`<clientCode>.tt.omtrdc.net`網域或`mboxedge<clusterNumber>.tt.omtrdc.net`網域。

  如果此值為true，則會將`mboxedge<clusterNumber>.tt.omtrdc.net`網域儲存至Cookie。 使用at.js 1.8.2和at.js 2.3.1之前的at.js版本時，目前無法使用[CNAME](/help/dev/before-implement/implement-cname-support-in-target.md)。如果您有此問題，請考慮將[at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md)更新至較新且支援的版本。

### overrideMboxEdgeServerTimeout

* **型別**：數字
* **預設值**： 1860000 => 31分鐘
* **描述**：指出包含`mboxedge<clusterNumber>.tt.omtrdc.net`值的Cookie存留期。

### Pageloadenabled

* **型別**：布林值
* **預設值**： true
* **描述**：啟用時，會自動擷取頁面載入時必須傳回的體驗。

### pollingInterval

* **型別**：數字
* **預設值**： 300000 （以毫秒為單位的五分鐘）
* **描述**： at.js擷取裝置上決策成品的新版本並更新快取的間隔。 300000是`pollingInterval`允許的最小值。

### secureOnly

* **型別**：布林值
* **預設值**： false
* **描述**：指出at.js是否應該僅使用HTTPS或根據頁面通訊協定，允許在HTTP與HTTPS之間切換。 若設為true，secureOnly也會將Secure和SameSite屬性設為mbox Cookie。

### selectorsPollingTimeout

* **型別**：數字
* **預設值**： 5000毫秒= 5秒
* **描述**：在at.js 0.9.6中，[!DNL Target]引進了這項新設定，而且可透過`targetGlobalSettings`覆寫。

  `selectorsPollingTimeout`設定代表使用者端願意等候選取器所識別的所有元素出現在頁面上的時間。

  透過可視化體驗撰寫器 (VEC) 建立的活動，其具有的選件包含選取器。

### serverDomain

* **類型:** 字串
* **預設值**：透過UI設定的值。
* **描述**：代表[!DNL Target]邊緣伺服器。

### serverState

* **型別**：請參閱下方的[混合式個人化](#hybrid-personalization)。
* **預設值**：請參閱下方的[混合式個人化](#hybrid-personalization)。
* **描述**：請參閱下方的[混合式個人化](#hybrid-personalization)。

### telemetryEnabled

* **型別**：布林值
* **預設值**： true
* **描述**：啟用時，Adobe會收集SDK功能使用情況和效能遙測資料。 不會收集個人資料。

### timeout

* **型別**：數字
* **預設值**：透過UI設定的值。
* **描述**：代表[!DNL Target]邊緣要求逾時。

### viewsEnabled {#viewsenabled}

* **型別**：布林值
* **預設值**： true
* **描述**：啟用時，頁面載入時會自動擷取檢視。 呼叫`triggerView`時，瀏覽器中會顯示適用的檢視。 如果停用此選項，頁面載入時將不會擷取檢視，`triggerView`不會執行任何動作。 at.js 2.*x* 中受到支援。

### visitorApiTimeout

* **型別**：數字
* **預設值**： 2000毫秒= 2秒
* **描述**：代表訪客API要求逾時。

## 使用狀況

可以在at.js載入前或在&#x200B;**管理** > **實作** > **編輯at.js設定** > **程式碼設定** > **資料庫標題**&#x200B;中定義此函式。

資料庫標頭欄位允許輸入自由格式的 JavaScript。自訂程式碼看起來應該類似於下列範例:

```javascript {line-numbers="true"}
window.targetGlobalSettings = {
   timeout: 200, // using custom timeout
   visitorApiTimeout: 500, // using custom API timeout
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com
};
```

## 資料提供者 {#data-providers}

此設定可讓客戶收集來自協力廠商資料提供者（例如Demandbase、BlueKai和自訂服務）的資料，並在全域mbox要求中以mbox引數的形式傳遞資料至[!DNL Target]。 它透過非同步和同步要求，以支援來自多個提供者的資料收集。使用此方法可讓您方便管理預設頁面內容的忽隱忽現情形，同時對每個提供者包含獨立的逾時計算，以限制對頁面效能的影響

>[!NOTE]
>
>資料提供者需使用at.js 1.3或更新版本。

以下影片包含更多資訊:

| 影片 | 說明 |
|--- |--- |
| [使用 Adobe Target 中的資料提供者](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html) | 資料提供者這項功能可以讓您輕鬆將資料從第三方傳入 Target。第三方可能是氣象服務、DMP，甚至是您自己的 Web 服務。接著，您就能使用此資料來建立對象、鎖定內容及擴充訪客設定檔。 |
| [實作 Adobe Target 中的資料提供者](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html) | 實作詳細資料和範例，說明如何使用Adobe[!DNL Target]的dataProviders功能，從協力廠商資料提供者擷取資料，並在[!DNL Target]要求中傳遞。 |

`window.targetGlobalSettings.dataProviders` 設定是資料提供者的陣列。

每個資料提供者具有下列結構:

| 索引鍵 | 類型 | 說明 |
|--- |--- |--- |
| 名稱 | 字串 | 提供者的名稱。 |
| version | 字串 | 提供者版本。此機碼將用於提供者演進。 |
| timeout | 數字 | 如果這是網路要求，則代表提供者逾時。此機碼為選用。 |
| provider | 函數 | 包括提供者資料擷取邏輯的函數。<p>函式有單一必要引數： `callback`。 callback 參數為函數，僅應該在成功擷取資料或發生錯誤時叫用。<p>callback 預期兩個參數:<ul><li>error: 指出是否發生錯誤。如果各項都正常，則此參數應該設為 null。</li><li>params： JSON物件，代表將在[!DNL Target]要求中傳送的引數。</li></ul> |

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

at.js處理`window.targetGlobalSettings.dataProviders`之後，[!DNL Target]要求將包含新引數： `t1=1`。

如果您要新增至[!DNL Target]要求的引數是從第三方服務（例如Bluekai、Demandbase等）擷取，以下是範例：

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

at.js處理`window.targetGlobalSettings.dataProviders`之後，[!DNL Target]要求將包含其他引數： `t1=1`、`t2=2`和`t3=3`。

下列範例使用資料提供者來收集氣象API資料，並將資料以引數形式在[!DNL Target]要求中傳送。 [!DNL Target]要求會有其他引數，例如`country`和`weatherCondition`。

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

at.js 2.3.0+支援在套用傳遞的[!DNL Target]選件時，在附加至頁面DOM的SCRIPT和STYLE標籤上設定內容安全性原則Nonce。

在at.js 2.3.0+載入之前，SCRIPT和STYLE Nonce應相應地設定為`targetGlobalSettings.cspScriptNonce`和`targetGlobalSettings.cspStyleNonce`。 請參閱下列範例：

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

指定`cspScriptNonce`和`cspStyleNonce`設定後，at.js 2.3.0+會在套用[!DNL Target]選件時，將其附加至DOM的所有SCRIPT和STYLE標籤上，將這些設定為Nonce屬性。

## 混合個人化

`serverState`是at.js v2.2+中提供的設定，可在實施[!DNL Target]的混合整合時用來最佳化頁面效能。 混合整合意指您在使用者端上同時使用at.js v2.2+和伺服器端的傳送API或[!DNL Target] SDK來傳送體驗。 `serverState` 讓 at.js v2.2+ 能夠直接從伺服器端擷取並傳回至用戶端的內容套用體驗，做為所提供頁面的一部分。

### 先決條件

您必須混合式整合[!DNL Target]。

* **伺服器端**：您必須使用[傳送API](/help/dev/implement/delivery-api/overview.md)或[目標SDK](/help/dev/implement/server-side/sdk-guides/getting-started/getting-started.md)。
* **使用者端**：您必須使用[at.js 2.2版或更新版本](/help/dev/implement/client-side/atjs/target-atjs-versions.md)。

### 程式碼範例

若要深入瞭解其運作方式，請參閱下列程式碼範例，瞭解伺服器上的程式碼範例。 程式碼假設您使用[Target Node.js SDK](https://github.com/adobe/target-nodejs-sdk)。

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

檢視預先擷取的範例`serverState`物件JSON如下所示：

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

頁面載入瀏覽器後，at.js會立即套用來自`serverState`的所有[!DNL Target]選件，而不會對[!DNL Target]邊緣引發任何網路呼叫。 此外，at.js只會預先隱藏內容擷取伺服器端中有[!DNL Target]個選件可用的DOM元素，因而對頁面載入效能和一般使用者體驗會有正面影響。

### 重要附註

使用`serverState`時，請考慮下列事項：

* 目前，at.js v2.2僅支援透過serverState為以下專案提供體驗：

   * VEC建立的活動，會在頁面載入時執行。
   * 預先擷取的檢視。

     若是SPA在at.js API中使用[!DNL Target]檢視和`triggerView()`，at.js v2.2會快取伺服器端預先擷取之所有檢視的內容，並在透過`triggerView()`觸發每個檢視時套用這些內容，如此一來，就不會觸發對[!DNL Target]的任何其他內容擷取呼叫。

   * **注意**： `serverState`目前不支援伺服器端擷取的mbox。

* 套用`serverState`選件時，at.js會考慮`pageLoadEnabled`和`viewsEnabled`設定，例如，如果`pageLoadEnabled`設定為False，將不會套用頁面載入選件。

  若要開啟這些設定，請在&#x200B;**管理>實作>編輯>頁面載入已啟用**&#x200B;中啟用切換。

  ![已啟用頁面載入設定](../../assets/page-load-enabled-setting.png)

* 如果您使用`serverState`並在傳回的內容中使用`<script>`標籤，請確定您的HTML內容使用`<\/script>`而非`</script>`。 如果您使用`</script>`，瀏覽器會將`</script>`解譯為內嵌SCRIPT的結尾，而且可能會中斷HTML頁面。

### 其他資源

若要深入瞭解`serverState`的運作方式，請參閱下列資源：

* [範常式式碼](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/advanced-atjs-integration-serverstate)。
* [具有`serverState`](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/react-shopping-cart-demo)的單頁應用程式(SPA)範例應用程式。

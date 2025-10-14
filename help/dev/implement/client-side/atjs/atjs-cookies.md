---
keywords: at.js、2.0、1.x、Cookie
description: 有關 [!DNL Adobe Target] at.js 2.x和at.js 1.x如何處理Cookie的詳細資料
title: at.js Cookies
feature: at.js
exl-id: 154a844a-6855-4af7-8aed-0719b4c389f5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1716'
ht-degree: 72%

---

# at.js Cookie

有關 at.js 2.x 和 at.js 1 的資訊。*x* Cookie 行為。

## at.js 2.x Cookie 行為

若使用at.js 2.x版（最高至2.10.0版，但不包括此版本），*僅支援第一方Cookie*。 就像使用 at.js 1.*x*，第一方Cookie「mbox」儲存在`clientdomain.com`中，其中`clientdomain`為您的網域。

at.js 會產生一個工作階段 ID 並將其儲存於 Cookie 中。第一個回應包含任何活動資訊，以及 [!DNL Target] 伺服器產生的 `TNT` 或 `PC ID`。然後 at.js 會將 `TNT/PC ID` 寫入 Cookie 中。

`AMCV_###@AdobeOrg`第一方Cookie一律由Experience CloudID服務設定，不過已在[!DNL Target]個要求中傳遞`ECID`。

>[!NOTE]
>
>若使用at.js 2.10.0版或更新版本，同時支援第一方和跨網域Cookie。

### 支援第三方Cookie和跨網域追蹤

跨網域追蹤可將兩個相關網站上網域不同的工作階段視為單一工作階段。您可以建立橫跨 [!DNL Target] 和 `siteA.com` 的 `siteB.com` 活動，讓訪客在跨越網域時仍可維持相同的體驗。此功能與 at.js 1.*x* 第三方和第一方 Cookie 行為整合。

>[!NOTE]
>
>若使用at.js 2.10.0版及更新版本，同時支援協力廠商Cookie和跨網域追蹤。


## at.js 1.*x* Cookie行為

針對 at.js 1.*x* 版，Cookie 的行為取決於其為第一方 Cookie、具有第一方 Cookie 的第三方 Cookie，還是獨立的第三方 Cookie。

### 何時使用第一方或第三方 Cookie

您的網站設定會決定您要使用何種 Cookie。嘗試瞭解第一方和第三方Cookie時，瞭解[!DNL Target]的運作方式會很有幫助。 如需詳細資訊，請參閱[運作方式 [!DNL Adobe Target] &#x200B;](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html?lang=zh-Hant)。

cookie 有三種主要的使用狀況:

1. 單一網域。

   所有測試都會在單一高階網域 (`anysub.domain.com`、`www.domain.com`、`store.domain.com` 等) 中進行。

   只使用第一方 Cookie。這是預設值。

1. 使用者橫跨網域，您也想要在這些網域中追蹤和測試其行為。

   範例: 一位使用者造訪您的網站進行購物，但透過 Yahoo 商店簽出。三種方式 (與您的帳戶代表一起決定最佳方式):

   * 啟用第一方和第三方 Cookie。
   * 僅啟用第三方 Cookie (非常罕見，但其優點是會將 at.js Cookie 保留在網域外部)。
   * 僅啟用第一方 Cookie，並在跨網域時傳遞 `mboxSession` 參數。

     必須將 `mboxSession` 參數傳遞至參照 at.js 的登陸頁面。該頁面不能是中繼重新導向器頁面。

1. 您只會在第三方網站上使用 AdBox 或 Flashbox。

   兩種方式 (與您的帳戶服務經理一起決定最佳方式):

   * 啟用第一方和第三方 Cookie。

     Flashbox 及動態創作元素需要第一方和第三方 Cookie。

   * 僅啟用第三方 Cookie。

     此方式僅用於其中執行之 AdBox 不含 Onsite 定位的罕見個案。

### 第一方 Cookie 行為

第一方 Cookie 儲存在 `clientdomain.com`，其中 `clientdomain` 為您的網域。

at.js 會產生一個 `mboxSession ID` 並將其儲存於 Cookie 中。第一個 回應會包含選件和 JavaScript，以將應用程式產生的 `mboxPC ID` 儲存於 的 Cookie。

>[!NOTE]
>
>`AMCV_###@AdobeOrg`第一方Cookie一律以Experience Cloud的訪客ID設定。

### 第三方 Cookie 行為

第三方 Cookie 儲存在 `clientcode.tt.omtrdc.net`，而第一方 Cookie 儲存在 `clientdomain.com`，其中 `clientdomain` 為您的網域。

at.js 產生一個 `mboxSession ID`。第一個位置請求會傳回 HTTP 回應標頭，此標頭會嘗試設定名為 `mboxSession` 和 `mboxPC` 的第三方 Cookie，之後會傳回附有額外參數 (`mboxXDomainCheck=true`) 的重新導向請求。

若瀏覽器接受第三方 Cookie，則重新導向請求會包含這些 Cookie 並傳回選件。

若瀏覽器拒絕第三方 Cookie，則重新導向請求不會包含這些 Cookie，而網頁上的所有置會顯示預設內容。由於未設定 Cookie，因此每個頁面請求會再次發生上述相同的程序。

### 第三方和第一方 Cookie 行為

第三方 Cookie 儲存在 `clientcode.tt.omtrdc.net`，而第一方 Cookie 儲存在 `clientdomain.com`，其中 `clientdomain` 為您的網域。

at.js 產生一個 `mboxSession ID`。第一個位置請求會傳回 HTTP 回應標頭，此標頭會嘗試設定名為 `mboxSession` 和 `mboxPC` 的第三方 Cookie，之後會傳回附有額外參數 (`mboxXDomainCheck=true`) 的重新導向請求。

若瀏覽器接受第三方 Cookie，則重新導向請求會包含這些 Cookie 並傳回選件。

部分瀏覽器會拒絕第三方 Cookie。如果第三方 Cookie 被封鎖，第一方 Cookie 仍會運作。[!DNL Target] 會嘗試傳送第三方 Cookie，如果無法傳送，則 [!DNL Target] 只能追蹤用戶端的特定網域。如果第三方 Cookie 被封鎖，除非跨網域的連結附加 `mboxSession`，否則就不能跨網域追蹤。在這種情況下會設定另一個第一方 Cookie，並與之前的網域第一方 Cookie 同步。

## Cookie 設定

Cookie 具有各種預設設定。您可以視需要變更這些設定，但不包括 Cookie 持續時間。變更 Cookie 設定時，請洽詢您的帳戶代表。

| 設定 | 資訊 |
|--- |--- |
| Cookie 名稱 | mbox。 |
| Cookie 網域 | 您從中提供內容的第一層與第二層網域。因為是使用公司所提供的網域，所以這些會是第一方 Cookie。範例: `mycompany.com`。 |
| 伺服器網域 | `clientcode.tt.omtrdc.net`，使用您帳戶的用戶端代碼。 |
| Cookie 持續時間 | Cookie會保留在訪客的瀏覽器上達兩年時間，期限從訪客最後登入算起。<P>`deviceIdLifetime`設定可在[at.js 2.3.1版或更新版本](../atjs/target-atjs-versions.md)中覆寫。 如需詳細資訊，請參閱 [targetGlobalSettings()](../../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。 |
| P3P 原則 | 此 Cookie 會依大部分瀏覽器中預設設定的要求，以 P3P 原則進行發佈。P3P 原則會為瀏覽器指出是誰提供此 Cookie，以及將如何使用此資訊。 |

此 Cookie 會保留一些值，以管理您訪客體驗 促銷活動的方式:

| 值 | 定義 |
|--- |--- |
| session ID | 每個使用者作業會有一個唯一 ID。依預設，這會持續 30 分鐘。 |
| pc ID | 每位訪客瀏覽器的半永久性 ID。持續 14 天。 |
| check | 用來決定訪客是否支援 Cookie 的簡單測試值。每次訪客請求頁面時都會進行設定。 |
| disable | 如果訪客的載入時間超過Adobe Experience Platform Web SDK或at.js檔案中所設定的逾時時間，則設定此選項。 依預設，這會持續 1 個小時。 |

## Apple WebKit追蹤變更在[!DNL Target]上對Safari訪客的影響

請記住下列事項:

### [!DNL Adobe Target]的追蹤如何運作？

| Cookie | 詳細資料 |
|--- |--- |
| 第一方網域 | 這是[!DNL Target]客戶的標準實作。  「mbox」Cookie 設定在客戶的網域中。 |
| 第三方追蹤 | 協力廠商追蹤對於[!DNL Target]和[!DNL Adobe Audience Manager] (AAM)中的廣告和鎖定目標使用案例很重要。  第三方追蹤需要跨網站指令碼技術。[!DNL Target]使用在`clientcode.tt.omtrd.net`網域中設定的兩個Cookie：「mboxSession」和「mboxPC」。 |

### Apple 採用什麼方法?

從 Apple: 

「智慧型追蹤預防是新的 WebKit 功能，可進一步限制 Cookie 和其他網站資料，以減少跨網站追蹤。」

「這就是所謂的跨網站追蹤，而 `example-tracker.com` 使用的 Cookie 稱為第三方 Cookie。在我們的測試中，我們發現的熱門網站都有超過 70 個這種追蹤器，全部都安靜地收集使用者的資料。」

| 方法 | 詳細資料 |
|--- |--- |
| 智慧型追蹤預防 | 如需詳細資訊，請參閱 WebKit Open Source Web Browser Engine 網站上的[智慧型追蹤預防](https://webkit.org/blog/7675/intelligent-tracking-prevention/)。 |
| Cookie | Safari 如何處理 Cookie:<ul><li>使用者直接存取的網域上不存在的第三方 Cookie 永不儲存。這不是新的行為。Safari 尚不支援第三方 Cookie。</li><li>使用者直接存取的網域上所設定的第三方 Cookie 會在 24 小時後清除。</li><li>如果第一方網域已分類為跨網站追蹤使用者，則會在 30 天後清除第一方 Cookie。在線上將使用者送往不同網域的大型公司可能會發生此問題。Apple 並未明確解釋這些網域如何分類，或網域如何判斷本身是否已分類為跨網站追蹤使用者。</li></ul> |
| 使用「機器學習」來辨別跨網站的網域 | 從 Apple: <P>機器學習分類器: 機器學習模型用來分類哪些私人控制的最上層網域有能力根據收集的統計資料，而跨網站追蹤使用者。從各種收集的統計資料中，有三個向量出現強烈訊號，表示應該根據目前的追蹤實務來分類: 一些唯一網域下的子資源、一些唯一網域下的子範圍，以及重新導向到的一些唯一網域。資料收集和分類全都在裝置上進行。<P>不過，如果使用者與當作最上層網域的example.com （通常稱為第一方網域）互動，「智慧型追蹤預防」會將此視為訊號，認為使用者對此網站有興趣，並依此時間表來暫時調整其行為：<P>如果使用者在過去24小時與example.com互動，則其Cookie將在`example.com`為第三方時可用。 這允許「在 Y 上以我的 X 帳戶登入」登入情節。<ul><li>當作最上層網域來造訪的網域不受影響。例如，OKTA 網站</li><li>識別跨多個唯一網域之目前頁面的子網域或子框架的網域。</li></ul> |

### 對 Adob 有何影響?

| 受影響的功能 | 詳細資料 |
|--- |--- |
| 支援選擇退出 | Apple 的 WebKit 追蹤變更會暫停選擇退出支援。<P>[!DNL Target]選擇退出在`clientcode.tt.omtrdc.net`網域中使用Cookie。 如需更多詳細資料，請參閱[隱私](/help/dev/before-implement/privacy/privacy.md)。<P>[!DNL Target]支援兩種選擇退出：<ul><li>由用戶端決定 (用戶端管理選擇退出連結)。</li><li>一個透過Adobe，可讓使用者退出所有客戶的所有[!DNL Target]功能。</li></ul>兩種方法都使用第三方 Cookie。 |
| [!DNL Target]個活動 | 客戶可以為其[!DNL Target]帳戶選擇其[設定檔存留期長度](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html?lang=zh-Hant) — 最長90天。 問題是如果帳戶的設定檔存留期超過30天，且第一方Cookie因為客戶的網域已標示為跨網站追蹤而被清除，則Safari訪客的行為將在[!DNL Target]的以下區域中受到影響：<P>**[!DNL Target]報告**：如果Safari使用者進入活動，30天後回訪，然後轉換，則該使用者會計為兩個訪客和一次轉換。<P>對於使用[!DNL Analytics]作為報表來源(A4T)的活動，此行為是相同的。<P>**設定檔與活動成員資格**：<ul><li>第一方 Cookie 到期時會清除設定檔資料。</li><li>第一方 Cookie 到期時會清除活動成員資格。</li><li> 對於使用第三方Cookie實作或第一方和第三方Cookie實作的帳戶，[!DNL Target]在Safari中無法運作。 請注意，這不是新的行為。Safari 暫時還不允許第三方 Cookie。</li></ul><P>**建議**：如果擔心客戶網域可能標示為跨工作階段追蹤訪客，最好在[!DNL Target]中將設定檔存留期設為30天或更少。 這樣可確保在 Safari 和所有其他瀏覽器中以類似方式追蹤使用者。 |

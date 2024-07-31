---
keywords: 概覽和參考資料， webkit， cookie，第一方，第三方，第一方，第三方， $8
description: 瞭解Target Cookie行為（第一方Cookie、具有第一方Cookie的第三方Cookie，或單獨使用第三方Cookie）。
title: 我可以在哪裡找到有關Target Cookie的資訊？
feature: at.js
role: Developer
source-git-commit: 39f390a0e5eedf8c6957333759d31d96ed11b321
workflow-type: tm+mt
source-wordcount: '1580'
ht-degree: 53%

---

# Target Cookie

Cookie 的行為取決於其屬於第一方 Cookie、具有第一方 Cookie 的第三方 Cookie，或是獨立的第三方 Cookie。

>[!NOTE]
>
>本主題包含 `mboxSession` 和 `mboxPC` 的相關資訊。實作最佳實務建議您不要使用Cookie資料連結或儲存敏感資訊： `mboxSession`或`mboxPC`。

另請參閱[刪除Target Cookie](/help/dev/before-implement/privacy/cookie-deleting.md)。

## 何時使用第一方或第三方 Cookie

您的網站設定會決定您要使用何種 Cookie。嘗試瞭解第一方與第三方 Cookie 時，建議先瞭解 Target 的運作方式。如需詳細資訊，請參閱[Adobe Target的運作方式](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html)。

cookie 有三種主要的使用狀況:

1. 單一網域。

   所有測試都在一個頂層網域（`www.domain.com`、`store.domain.com`、`anysub.domain.com`等）中進行。

   方法：僅使用第一方Cookie （預設）。

1. 使用者橫跨網域，您也想要在這些網域中追蹤和測試其行為。

   範例: 一位使用者造訪您的網站進行購物，但透過 Yahoo 商店簽出。三種方式 (與您的帳戶代表一起決定最佳方式):

   * 啟用第一方和第三方 Cookie。
   * 僅啟用協力廠商（很少見，但優點在於可將mbox Cookie排除在網域之外）。
   * 僅啟用第一方 Cookie，並在跨網域時傳遞 `mboxSession` 參數。

     `mboxSession`引數必須傳遞至登陸頁面，並從JavaScript資料庫(Adobe Experience Platform Web SDK或at.js)參考。 該頁面不能是中繼重新導向器頁面。

1. 您只會在第三方網站上使用 adbox 或 Flashbox。

   兩種方式（請與您的客戶代表合作以決定最佳方式）：

   * 啟用第一方和第三方 Cookie。

     Flashbox 及動態創作元素需要第一方和第三方 Cookie。

   * 僅啟用第三方 Cookie。

     此方式僅用於其中執行之 AdBox 不含 Onsite 定位的罕見個案。

## 第一方 Cookie 行為

第一方Cookie儲存在clientdomain.com中，其中`clientdomain`為您的網域。

JavaScript程式庫會產生一個`mboxSession ID`並將其儲存於Target Cookie中。 第一個mbox回應會包含選件和JavaScript，以將應用程式產生的`mboxPC ID`儲存在mbox Cookie中。

>[!NOTE]
>
>AMCV_###@AdobeOrg第一方Cookie一律以Experience Cloud的訪客ID設定。

## 第三方 Cookie 行為

第三方Cookie儲存在clientcode.tt.omtrdc.net中，而第一方Cookie儲存在clientdomain.com中，其中`clientdomain`為您的網域。

JavaScript程式庫產生`mboxSession ID`。 第一個位置請求會傳回 HTTP 回應標頭，此標頭會嘗試設定名為 `mboxSession` 和 `mboxPC` 的第三方 Cookie，之後會傳回附有額外參數 (`mboxXDomainCheck=true`) 的重新導向請求。

若瀏覽器接受第三方 Cookie，則重新導向請求會包含這些 Cookie 並傳回選件。

若瀏覽器拒絕第三方 Cookie，則重新導向請求不會包含這些 Cookie，而網頁上的所有置會顯示預設內容。由於未設定 Cookie，因此每個頁面請求會再次發生上述相同的程序。

>[!NOTE]
>
>若未封鎖第三方Cookie，則會設定demdex.net Cookie。

## 第三方和第一方 Cookie 行為

第三方Cookie儲存在clientcode.tt.omtrdc.net中，而第一方Cookie儲存在clientdomain.com中，其中`clientdomain`為您的網域。

JavaScript程式庫產生`mboxSession ID`。 第一個位置請求會傳回 HTTP 回應標頭，此標頭會嘗試設定名為 `mboxSession` 和 `mboxPC` 的第三方 Cookie，之後會傳回附有額外參數 (`mboxXDomainCheck=true`) 的重新導向請求。

若瀏覽器接受第三方 Cookie，則重新導向請求會包含這些 Cookie 並傳回選件。

部分瀏覽器會拒絕第三方 Cookie。如果第三方 Cookie 被封鎖，第一方 Cookie 仍會運作。Target會嘗試設定第三方Cookie，如果無法設定，則Target只能追蹤使用者端的特定網域。 如果第三方Cookie遭到封鎖，則跨網域追蹤無法運作，除非在跨網域的連結中附加`mboxSession`。 在這種情況下會設定另一個第一方 Cookie，並與之前的網域第一方 Cookie 同步。

## Cookie 設定

Cookie 具有各種預設設定。如有需要，您可以變更這些設定，但Cookie持續時間除外。 變更 Cookie 設定時，請洽詢您的帳戶代表。

| 設定 | 資訊 |
|--- |--- |
| Cookie 名稱 | mbox。 |
| Cookie 網域 | 您從中提供內容的第一層與第二層網域。因為是從您公司的網域提供，所以Cookie是第一方Cookie。<br />範例： `mycompany.com`。 |
| 伺服器網域 | `clientcode.tt.omtrdc.net`，使用您帳戶的用戶端代碼。 |
| Cookie 持續時間 | Cookie會保留在訪客的瀏覽器上，從上次登入起兩週。 您無法變更 Cookie 持續時間。 |
| P3P 原則 | 此 Cookie 會依大部分瀏覽器中預設設定的要求，以 P3P 原則進行發佈。P3P原則會向瀏覽器指出誰正在提供Cookie及資訊的使用方式。 |

Cookie會保留各種值，以管理訪客體驗行銷活動的方式：

| 值 | 定義 |
|--- |--- |
| session ID | 每個使用者作業會有一個唯一 ID。根據預設，此ID持續30分鐘。 |
| pc ID | 每位訪客瀏覽器的半永久性 ID。持續 14 天。 |
| check | 用來決定訪客是否支援 Cookie 的簡單測試值。每次訪客請求頁面時都會進行設定。 |
| disable | 如果訪客的載入時間超過JavaScript程式庫檔案中所設定的逾時時間，則設定此選項。 根據預設，此值會持續一小時。 |

## Apple WebKit 追蹤變更在 Target 上對 Safari 訪客的影響

**Adobe Target如何進行追蹤？**

| Cookie | 詳細資料 |
|--- |--- |
| 第一方網域 | 適用於Target客戶的標準實作。 「mbox」Cookie 設定在客戶的網域中。 |
| 第三方追蹤 | 對於Target和Adobe Audience Manager (AAM)中的廣告和鎖定目標使用案例，第三方追蹤很重要。 第三方追蹤需要跨網站指令碼技術。 Target 使用 `clientcode.tt.omtrd.net` 網域中設定的兩個 Cookie:「mboxSession」和「mboxPC」。 |

**Apple 採用什麼方法?**

從 Apple: 

「智慧型追蹤預防是新的 WebKit 功能，可進一步限制 Cookie 和其他網站資料，以減少跨網站追蹤。」

「這就是所謂的跨網站追蹤，而 `example-tracker.com` 使用的 Cookie 稱為第三方 Cookie。在我們的測試中，我們發現的熱門網站都有超過 70 個這種追蹤器，全部都安靜地收集使用者的資料。」

| 方法 | 詳細資料 |
|--- |--- |
| 智慧型追蹤預防 | 如需詳細資訊，請參閱 WebKit Open Source Web Browser Engine 網站上的[智慧型追蹤預防](https://webkit.org/blog/7675/intelligent-tracking-prevention/)。 |
| Cookie | Safari 如何處理 Cookie:<ul><li>使用者直接存取的網域上不存在的第三方 Cookie 永不儲存。這不是新的行為。Safari 尚不支援第三方 Cookie。</li><li>使用者直接存取的網域上所設定的第三方 Cookie 會在 24 小時後清除。</li><li>如果第一方網域已分類為跨網站追蹤使用者，則會在 30 天後清除第一方 Cookie。在線上將使用者送往不同網域的大型公司可能會發生此問題。Apple尚未明確這些網域究竟如何分類，或網域如何判斷這些網域是否已分類為跨網站追蹤使用者。</li></ul> |
| 使用「機器學習」來辨別跨網站的網域 | 來自Apple：<br />機器學習分類器：機器學習模型用來根據收集的統計資料，分類哪些私人控制的最上層網域可以跨網站追蹤使用者。 從各種收集的統計資料中，有三個向量出現強烈訊號，表示應該根據目前的追蹤實務來分類: 一些唯一網域下的子資源、一些唯一網域下的子範圍，以及重新導向到的一些唯一網域。資料收集和分類全都在裝置上進行。<br />但是，如果使用者與當作最上層網域的`example.com`互動（通常稱為第一方網域），「智慧型追蹤預防」會將此視為訊號，認為使用者對此網站有興趣，並依此時間表來暫時調整其行為：<br />如果使用者在過去24小時與`example.com`互動，則其Cookie在`example.com`是第三方時可用。 此作法允許「在Y上以我的X帳戶登入」登入情境。<ul><li>當作最上層網域來造訪的網域不受影響。 例如，OKTA 網站</li><li>識別跨多個唯一網域之目前頁面的子網域或子框架的網域。</li></ul> |

**Adobe受到什麼影響？**

| 受影響的功能 | 詳細資料 |
|--- |--- |
| 支援選擇退出 | Apple 的 WebKit 追蹤變更會暫停選擇退出支援。<br />Target 選擇退出會在 `clientcode.tt.omtrdc.net` 網域中使用 Cookie。如需更多詳細資料，請參閱[隱私](/help/dev/before-implement/privacy/privacy.md)。<br />Target 支援兩種選擇退出:<ul><li>由用戶端決定 (用戶端管理選擇退出連結)。</li><li>透過 Adobe 讓所有客戶的使用者退出所有 Target 功能。</li></ul>兩種方法都使用第三方 Cookie。 |
| Target 活動 | 客戶可以為其Target帳戶選擇其[設定檔存留期長度](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html) （最長90天）。 問題是如果帳戶的設定檔存留期超過30天，且第一方Cookie因為客戶的網域已標示為跨網站追蹤而被清除，則在Target的下列區域中，Safari訪客的行為會受影響： <br />**[!UICONTROL Target reports]**：如果Safari使用者進入活動，30天之後回訪，然後轉換，則該使用者會計為兩個訪客和一次轉換。<br />對於使用Analytics作為報表來源(A4T)的活動，此行為是相同的。<br />**[!UICONTROL Profile & activity membership]**：<ul><li>第一方 Cookie 到期時會清除設定檔資料。</li><li>第一方 Cookie 到期時會清除活動成員資格。</li><li> 對於採用第三方 Cookie 實作或第一方和第三方 Cookie 實作的帳戶，Target 在 Safari 中沒有作用。這不是新的行為。Safari已不允許第三方Cookie一段時間。</li></ul><br />**[!UICONTROL Suggestions]**：如果擔心客戶網域可能標示為跨工作階段追蹤訪客，最好在Target中將設定檔存留期設為30天或更少。 此限制可確保在Safari和所有其他瀏覽器中以類似方式追蹤使用者。 |

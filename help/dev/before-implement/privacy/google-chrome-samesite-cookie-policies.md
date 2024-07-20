---
keywords: google， samesite， cookie， chrome 80， ietf
description: 探索 [!DNL Adobe Target] 如何處理Google Chrome版本80引進的SameSite IETF標準，以及遵守這些政策需要做哪些事。
title: ' [!DNL Target] 如何處理 Google 的 Samesite Cookie 政策？'
feature: Privacy & Security
exl-id: 58a83def-9625-4d44-914f-203509c6c434
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1973'
ht-degree: 69%

---

# Google Chrome SameSite Cookie 政策

根據預設，Google 將從 2020 年初發佈的 Chrome 80 開始對使用者實施新的 Cookie 政策。 本文說明有關新的SameSite Cookie政策的所有須知事項、[!DNL Adobe Target]如何支援這些政策，以及如何使用[!DNL Target]來遵守Google Chrome的新SameSite Cookie政策。

從 Chrome 80 開始，網頁開發人員必須明確指定哪些 Cookie 可以跨網站運作。 這是 Google 為了改善網頁上的隱私權和安全性而計劃發佈的眾多公告中的第一個。

有鑑於 Facebook 在隱私和安全性方面正面臨困境，其他各大供應商 (例如 Apple 及現在的 Google) 已快速利用此機會來打造新的身分，以便成為隱私和安全性領域的贏家。 Apple 在今年年初率先宣佈透過 ITP 2.1 及最近的 ITP 2.2 改變其 Cookie 政策。在 ITP 2.1 中，Apple 完全封鎖了第三方 Cookie，並且只會將瀏覽器上建立的 Cookie 保留七天。 在 ITP 2.2 中，Cookie 只會保留一天。 Google的公告遠不及Apple激進，但這是向著相同最終目標邁出的第一步。 如需有關 Apple 政策的詳細資訊，請參閱 [Apple 智慧追蹤防護 (ITP) 2.x](/help/dev/before-implement/privacy/apple-itp-2x.md)。

## 什麼是 Cookie 以及如何加以使用？

在我們深入探討Google的Cookie政策變更之前，讓我們先來談談什麼是Cookie以及如何加以使用。 簡單地說，Cookie 就是儲存在網頁瀏覽器中的小型文字檔案，用來記住使用者的屬性。

Cookie很重要是因為可在使用者瀏覽網頁時用來增強使用者的體驗。 例如，如果您正在電子商務網站上購物，並在購物車中加入某件商品，但是在這次造訪期間並未登入或購買，則Cookie會記住您的商品，並將其保留在購物車中以供您下次造訪時購買。 或者想像一下，如果您每次造訪最愛的社交媒體網站時，都被迫要重新輸入使用者名稱和密碼會是什麼樣的情況。 Cookie 也可以解決這個問題，因為 Cookie 所儲存的資訊可幫助網站識別您的身分。 這些類型的 Cookie 稱為第一方 Cookie，因為它們是由您造訪的網站所建立及使用。

也有第三方 Cookie。 為了更深入瞭解這些內容，請參考此範例：

假設一家名為「Friends」的社群媒體公司提供「分享」按鈕，其他網站可實作該按鈕，以便Friends使用者在Friends動態消息上分享網站內容。 現在，使用者在使用「分享」按鈕的新聞網站上閱讀一篇新聞文章，並點選該按鈕，該文章就會自動在其Friends帳戶上發佈。

為了做到這件事，瀏覽器在載入新聞文章時會從 `platform.friends.com` 擷取 Friends 的「分享」按鈕。 在這個過程中，瀏覽器會將包含使用者登入認證的 Friends Cookie 附加到傳送給 Friends 伺服器的要求中。 這可讓Friends代表使用者將新聞文章發佈到其動態頁，使用者無需登入。

在使用第三方 Cookie 時，這些都是可以辦到的事。 在此情況下，第三方Cookie會儲存在瀏覽器上以供`platform.friends.com`使用，因此`platform.friends.com`可以代表使用者在Friends應用程式上發佈貼文。

請暫時想像一下如何在沒有第三方 Cookie 的情況下達成此案例的目的，這時使用者勢必要遵循許多手動步驟。 首先，使用者必須複製新聞文章的連結。 接下來，使用者必須個別登入 Friends 應用程式。 然後，使用者會點選「建立貼文」按鈕。 之後，使用者會複製該連結，然後貼到文字欄位中，最後點選「發佈」。 如您所見，第三方 Cookie 可立即改善使用者體驗，因為可大幅減少手動步驟。

一般而言，第三方Cookie讓資料得以儲存在使用者的瀏覽器上，而不需要使用者明確造訪網站。

## 安全性的顧慮

雖然 Cookie 可強化使用者體驗並增強廣告效果，但同時也帶來安全性漏洞，像是跨網站請求偽造 (CSRF) 攻擊。 例如，如果使用者登入銀行網站來支付信用卡帳單，然後在離開網站時並未登出，之後又在相同工作階段中瀏覽某個惡意網站，則可能會發生 CSRF 攻擊。 惡意網站可能會包含向銀行網站發出請求的程式碼，而載入網頁時就會執行該程式碼。 由於使用者仍在銀行網站上通過了驗證，所以工作階段Cookie可用來發起CSRF攻擊，起始從使用者的銀行帳戶轉帳的事件。 這是因為每當您造訪網站時，所有 Cookie 都會附加到 HTTP 要求中。 也是因為有了這些安全性顧慮，所以 Google 現在正嘗試減輕其影響。

## [!DNL Target] 如何使用 Cookie？

說了這麼多，讓我們看看[!DNL Target]如何使用Cookie。 為了讓您能在第一時間使用 [!DNL Target]，您需要在網站上安裝 [!DNL Target] JavaScript 程式庫。 如此可讓您在造訪您網站的使用者的瀏覽器上放置第一方 Cookie。 當使用者與您的網站互動時，您可以透過JavaScript資料庫將使用者的行為和興趣資料傳遞給[!DNL Target]。 [!DNL Target] JavaScript程式庫會使用第一方Cookie來擷取有關使用者的身分識別資訊，以對應到使用者的行為和興趣資料。 然後 [!DNL Target] 會使用該資料來強化您的個人化活動。

Target 有時也會使用第三方 Cookie。 如果您擁有在不同網域上線的多個網站，而且想要追蹤這些網站上的使用者歷程，您可以利用跨網站追蹤來使用第三方 Cookie。 當您在 [!DNL Target] JavaScript 程式庫中啟用跨網站追蹤時，您的帳戶就會開始使用第三方 Cookie。 當使用者從某個網域跳到另一個網域時，瀏覽器會與Target的後端伺服器溝通，而在這個過程中，將會在使用者的瀏覽器上建立並放置第三方Cookie。 透過存在於使用者的瀏覽器上的第三方Cookie，[!DNL Target]將能夠為單一使用者提供跨不同網域的一致體驗。

## Google的新Cookie方式

為了在跨網站傳送 Cookie 時提供保護措施來保護使用者，Google 打算為名為 SameSite 的 IETF 標準新增支援，這需要網頁開發人員在 Set-Cookie 標頭中使用 SameSite 屬性元件來管理 Cookie。

有三個不同的值可傳遞至 SameSite 屬性: 嚴格、Lax 或無。

| 值 | 說明 |
| --- | --- |
| 嚴格 | 只有在造訪最初設定的網域時，才能存取具有此設定的 Cookie。也就是說，嚴格會完全封鎖 Cookie，以免跨網站使用。此選項最適合用於需要高度安全性的應用情境，例如銀行。 |
| Lax | 只有在 Same-Site 要求或是具有非等冪 HTTP 要求 (例如 `HTTP GET`) 的頂層導覽中，才會傳送具有此設定的 Cookie。 因此，如果 Cookie 可供第三方使用，應使用此選項，但新增的安全性優勢可保護使用者，避免受到 CSRF 攻擊的危害。 |
| 無 | 具有此設定的 Cookie 與現今 Cookie 的運作方式相同。 |

秉持上述原則，Chrome 80 為使用者引進了兩項獨立設定：「預設 SameSite Cookie」和「不含 SameSite 的 Cookie 必須是安全的」。 Chrome 80 中預設會啟用這些設定。

![SameSite 對話框](../assets/samesite.png)

* **預設SameSite Cookie**：設定後，所有未指定SameSite屬性的Cookie都會自動強制使用`SameSite = Lax`。
* **不含 SameSite 的 Cookie 必須是安全的**：在設定後，沒有 SameSite 屬性或使用 `SameSite = None`的 Cookie 必須是安全的。 在此情境下，安全代表所有瀏覽器要求都必須遵守 HTTPS 通訊協定。 未遵守此要求的 Cookie 會遭到拒絕。 所有網站都應該使用 HTTPS 以符合此要求。

## [!DNL Target]遵循Google的安全性最佳實務

在Adobe，我們總是想要支援業界最新的最佳做法來提供安全性和隱私保護。 我們在此很高興宣佈，[!DNL Target] 可支援 Google 推出的全新安全性和隱私設定。

針對「預設 SameSite Cookie」設定，[!DNL Target] 將繼續提供個人化，而不會對您造成任何影響和干擾。 [!DNL Target] 會使用第一方 Cookie 且將繼續正常運作，因為 Google Chrome 已套用 `SameSite = Lax` 標幟。

針對「不含SameSite的Cookie必須是安全的」選項，如果您沒有在Target中選擇加入跨網域追蹤功能，[!DNL Target]中的第一方Cookie將會繼續運作。

不過，當您選擇加入使用跨網域追蹤，跨多個網域運用 [!DNL Target] 時，Chrome 會要求針對第三方 Cookie 使用 `SameSite = None` 和 Secure 標幟。 這表示，您必須確保您的網站有使用 HTTPS 通訊協定。 [!DNL Target] 中的用戶端程式庫將會自動使用 HTTPS 通訊協定，並在 [!DNL Target] 中為第三方 Cookie 附加 `SameSite = None` 和 Secure 標幟，以確保所有活動都可繼續進行。

## 您需要做什麼？

若要了解您需要做什麼事才能讓 [!DNL Target] 繼續為 Google Chrome 80+ 使用者工作，請參閱下表中的下列欄：

* **Target JavaScript 程式庫**：網站上使用 at.js 1.*x* 或 at.js 2.*x* 時。
* **預設 SameSite Cookie = 已啟用**：如果您的使用者已啟用「預設 SameSite Cookie」，這對您會有何影響，而為了讓 [!DNL Target] 能夠繼續正常運作，是否有任何需要做的事。
* **不含 SameSite 的 Cookie 必須是安全的 = 已啟用**：如果您的使用者已啟用「不含 SameSite 的 Cookie 必須是安全的」，這對您會有何影響，而為了讓 [!DNL Target] 能夠繼續正常運作，是否有任何需要做的事。

| Target JavaScript 程式庫 | 預設 SameSite Cookie = 已啟用 | 不含 SameSite 的 Cookie 必須是安全的 = 已啟用 |
| --- | --- | --- |
| at.js 1.*x* 與第一方 Cookie。 | 沒有任何影響。 | 如果您沒有使用跨網域追蹤，就沒有影響。 |
| at.js 1.*x*，並啟用跨網域追蹤。 | 沒有任何影響。 | 必須為您的網站啟用 HTTPS 通訊協定。<br />Target使用第三方Cookie來追蹤使用者，而Google需要第三方Cookie才能有`SameSite = None`和Secure標幟。 Secure 標幟要求您的網站必須使用 HTTPS 通訊協定。 |
| at.js 2.*x* | 沒有任何影響。 | 沒有任何影響。 |

## [!DNL Target] 需要做什麼？

我們需要在平台上做哪些事來幫助您遵守新的 Google Chrome 80+ SameSite Cookie 政策？

| Target JavaScript 程式庫 | 預設 SameSite Cookie = 已啟用 | 不含 SameSite 的 Cookie 必須是安全的 = 已啟用 |
| --- | --- | --- |
| at.js 1.*x* 與第一方 Cookie。 | 沒有任何影響。 | 如果您沒有使用跨網域追蹤，就沒有影響。 |
| at.js 1.*x*，並啟用跨網域追蹤。 | 沒有任何影響。 | at.js 1.*x*，並啟用跨網域追蹤。 |
| at.js 2.*x* | 沒有任何影響。 | 沒有任何影響。 |

## 如果您未改用HTTPS通訊協定會有什麼影響？

唯一會影響您的使用情境是當您在 [!DNL Target] 中透過 at.js 1.*x* 使用供跨網域追蹤功能時。 如果不改用 Google 所要求的 HTTPS，您會在各個網域中看到不重複訪客數量激增，因為 Google 將會刪除我們使用的第三方 Cookie。 而且由於第三方 Cookie 會被刪除，所以 [!DNL Target] 將無法為該使用者提供一致且個人化的體驗，因為使用者會在不同網域中導覽。 第三方 Cookie 主要用於識別在您擁有的不同網域中導覽的單一使用者。

## 結論

在產業大幅演化並為消費者打造更安全Web環境的同時，Adobe全心致力於協助客戶，以確保一般使用者安全和隱私的方式，向其提供個人化體驗。 您只需要依照上述最佳做法，並利用[!DNL Target]來遵守Google Chrome的新SameSite Cookie政策。

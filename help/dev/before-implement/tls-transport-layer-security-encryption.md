---
keywords: tls， tls 1.0，傳輸層安全性，加密， tls 1.1， tls 1.2
description: 瞭解 [!DNL Target] 如何使用TLS （傳輸層安全性）通訊協定來維持最高安全性標準，提升客戶資料的安全性。
title: ' [!DNL Target] 如何使用TLS來提供安全性？'
feature: Privacy & Security
exl-id: f5ea2272-27ab-49c9-b096-b15dd277d4e5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1197'
ht-degree: 42%

---

# TLS (傳輸層安全性) 加密變更

有關[!DNL Adobe]和[!DNL Adobe Target]如何使用TLS （傳輸層安全性）維持最高安全性標準及提升客戶資料安全性的變更資訊。

傳輸層安全性 (TLS) 是目前針對須透過網路安全交換資料的網頁瀏覽器和其他應用程式，部署最廣泛的安全通訊協定。Adobe的安全性法規遵循標準規定須結束支援舊版通訊協定，並強制採用TLS 1.2，以使用最新、最安全的版本。

>[!WARNING]
>
>自2020年3月1日起，[!DNL Target]不再支援Visual Experience Composer (VEC)、Enhanced Experience Composer (EEC)、活動傳送、API等的TLS 1.1加密。 請升級至TLS 1.2以避免任何問題。

我們預計這不會對客戶資料或報告造成重大影響。

## 具有已啟用增強體驗撰寫器 (EEC) 的可視化體驗撰寫器 (VEC)

自2020年3月1日起，TLS 1.2是預設值，將不再支援TLS 1.1。

Adobe 會分階段將客戶轉移至 TLS 1.2。對於那些已經符合 1.2 規範之網域的使用者，我們會將其轉移至 TLS 1.2，無需進行任何變更。大部分的客戶網域已支援TLS 1.2；不過，如果您的網域不支援TLS 1.2，我們會像今天一樣將這些網域保留在TLS 1.1上（直到2020年3月）。

在此移轉階段，您應該不會遇到任何問題。如果VEC已停止載入先前正常運作的網站，請[開啟Client Care票證](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?#reference_ACA3391A00EF467B87930A450050077C)，說明移轉作業可能為背後原因。

然而，如果您是使用TSL 1.1但不支援TLS 1.2的客戶之一，則您應規劃網域/基礎架構到TLS 1.2的移動。我們將繼續支援TLS 1.1通訊協定，直到2020年3月1日。 自2020年3月1日起，[!DNL Target]將不支援透過增強體驗撰寫器功能用於VEC的TLS 1.1通訊協定。

雖然我們強烈建議大家繼續使用 TLS 1.2，不過如果您是新客戶但&#x200B;*不*&#x200B;支援 TLS 1.2，請聯絡客戶服務，告知他們您需要針對增強體驗撰寫器使用 TLS 1.1。不過，請計畫改用TLS 1.2，因為自2020年3月1日起不再支援您。

## 活動傳送

自2020年3月1日起，[!DNL Target]伺服器將不再支援TLS 1.1。經過此變更後，[!DNL Target]伺服器將不再接受訪客的請求，這些訪客使用不支援TLS 1.2 （或更新版本）的較舊裝置或網頁瀏覽器。 因此，僅支援 TLS 1.1 (或預設支援 TLS 1.1) 的舊裝置和瀏覽器將不會從 Adobe Target 接收活動內容。將呈現該網站的預設內容。

一些受影響的舊裝置和瀏覽器包括:

* Google Chrome (Android適用的Chrome)版本29及舊版
* Opera瀏覽器(Opera Mobile) 12.17版及舊版
* Mozilla Firefox (Firefox for Mobile) 26版本及舊版
* Android 4.3 和之前版本
* Windows 7 上的 Internet Explorer 8-10 和之前版本
* Windows Phone 8.0 上的 Internet Explorer 10
* Safari 6.0.4/OS X10.8.4 和之前版本

計畫進行此變更時，請注意下列事項（請注意，2020年3月1日的最後期限會影響所有這些專案）：

* 確保預設網站適用於相容裝置和瀏覽器。
* 請注意，在您的[!DNL Target]報表中，訪客人數可能會看到訪客人數小幅下降。
* 您可能需要變更專為鎖定不支援TLS 1.2的舊裝置或瀏覽器而建立的對象。傳送至這些裝置和瀏覽器將無法繼續運作。

如需有關支援的瀏覽器及其版本的詳細資訊，請參閱[支援的瀏覽器](supported-browsers.md)。

## [!DNL Adobe Target] API

自2020年3月1日起，[!DNL Target] API將不再支援TLS 1.1加密技術。 存取該 API 的客戶應確認他們不會受到影響。

* 使用Java 7搭配預設設定的API使用者端將需要進行修改以支援TLS 1.2。如需詳細資訊，請參閱Java網站上的[變更使用者端端點預設TLS通訊協定版本：從TLS 1.0到TLS 1.2](https://www.java.com/en/configure_crypto.html)。
* 使用 Java 8 的 API 用戶端已預設為 TLS 1.2，應該不會受到影響。
* 使用其他架構的 API 用戶端需聯絡其供應商，以瞭解 TLS 1.2 支援的詳細資訊。

## 存取Experience Cloud解決方案介面

由於[!DNL Target] Standard/Premium介面已經需要[最新的網頁瀏覽器](supported-browsers.md)，我們並未預見任何問題。 如果您無法連接到 Target，則應該將瀏覽器升級到最新版本。

## 如何檢查瀏覽器使用的TLS版本

若要使用Google Chrome檢視您網站上的TLS版本：

1. 在Chrome中開啟受影響的網站。
1. 在Chrome功能表（三個垂直的點）中，按一下「更多工具>開發人員工具」 。

   ![Chrome Developer Tool](assets/chrome-developer-tools.png)

1. 開啟「安全性」標籤，然後檢查「連線」下的TLS版本資訊：

   ![TLS版本詳細資料](assets/chrome-tls-version.png)

>[!NOTE]
>
>這些指示在發佈時為最新狀態，且可能會有所變更。 快速網際網路搜尋應該有助於這些指示的變更。 其他瀏覽器有類似的步驟。

## 支援1.2以下的TLS版本的瀏覽器的預期行為

本節說明僅當您使用at.js實作時，支援1.2以下版本TLS的瀏覽器會發生什麼事。 為了進行比較，本節也說明支援TLS 1.2的瀏覽器的預期情況。

### 中央端點

| [!DNL Target] JavaScript實作 | 詳細資料 |
|--- |--- |
| at.js | 啟用TLS 1.0或TLS 1.1時：<ul><li>若使用瀏覽器開發人員工具，你將會在「網路」標籤上看到「200 OK」。這表示要求已成功。</li><li>使用者會看到「無法安全地連線至此頁面」訊息。此訊息說明這可能是由於網站使用了過期或不安全的 TLS 安全性設定。</li><li>未顯示任何主控台錯誤。</li></ul>啟用 TLS 1.2 的瀏覽器:<ul><li>已下載 at.js 檔案。</li></ul> |

### Edge端點

| [!DNL Target] JavaScript實作 | 詳細資料 |
|--- |--- |
| Adobe Experience Platform Web SDK | 啟用TLS 1.0或TLS 1.1時：<ul><li>若使用瀏覽器開發人員工具，你將會在「網路」標籤上看到「200 OK」。這表示要求已成功。</li><li>使用者會看到「無法安全地連線至此頁面」訊息。此訊息說明這可能是由於網站使用了過期或不安全的 TLS 安全性設定。</li><li>未顯示任何主控台錯誤。</li><li>已提供預設內容。</li></ul>啟用 TLS 1.2 的瀏覽器:<ul><li>已提供選件內容。</li></ul> |
| at.js  | 啟用TLS 1.0或TLS 1.1時：<ul><li>若使用瀏覽器開發人員工具，你將會在「網路」標籤上看到「200 OK」。這表示要求已成功。</li><li>使用者會看到「無法安全地連線至此頁面」訊息。此訊息說明這可能是由於網站使用了過期或不安全的 TLS 安全性設定。</li><li>未顯示任何主控台錯誤。</li><li>已提供預設內容。</li></ul>啟用 TLS 1.2 的瀏覽器:<ul><li>已提供選件內容。</li></ul> |

### 以瀏覽器版本對象為鎖定目標的活動（Internet Explorer版本6、7或8）

客群停止運作。

| [!DNL Target] JavaScript實作 | 詳細資料 |
|--- |--- |
| Adobe Experience Platform Web SDK | Internet Explorer 10版之前的版本不支援Platform SDK。 |
| at.js | 版本 10 以前的 Internet Explorer 版本不支援 at.js。 |

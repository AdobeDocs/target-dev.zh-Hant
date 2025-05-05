---
keywords: 實作、at.js非javascript、adbox、重新導向程式、mbox
description: 瞭解如何在非JavaScript案例（例如使用AdBox或重新導向程式）中實作 [!DNL Adobe Target] 。
title: 如何為電子郵件實作 [!DNL Target] ？
feature: Implement Email
exl-id: dda00b75-5d58-4405-ae58-75e7883a30ed
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 63%

---

# 電子郵件：實作[!DNL Target]

關於在非JavaScript案例（例如使用AdBox或重新導向程式）中實作[!DNL Target]的資訊。

您可以追蹤廣告及其他離站內容的造訪。您也可偵測同位使用者進出您網站的狀況，並根據其網路瀏覽習慣呈現一致的網站體驗。只要使用單一URL，AdBox即可進行測試而無須使用JavaScript或at.js。

AdBox適用於沒有at.js的網站，例如加盟網站。 若您的活動需要動態創意素材 (例如，您需要在廣告中展示已從購物車拿掉的產品)，則無法使用 AdBox。

AdBox 廣告與重新導向程式可結合任何種類的活動使用。下表比較 AdBox 和重新導向程式，以及各自的使用時機：

| | 用途 | 使用時機 | URL 結構 | 選件類型 | 選件內容 |
|--- |--- |--- |--- |--- |--- |
| AdBox | 將不同的影像傳回至廣告 | 變更廣告的內容 | `clientcode&#x200B;.tt.&#x200B;omtrdc&#x200B;.net/&#x200B;m2&#x200B;/&#x200B;clientcode/ubox/&#x200B;image?` | 重新導向選件 | 影像 URL |
| 重新導向程式 | 將訪客重新導向至其他網頁 | 變更廣告的著陸頁面 | `clientcode&#x200B;.tt.omtrdc.net/&#x200B;m2/clientcode&#x200B;/ubox/page?` | 重新導向選件 | 網頁 URL |

## 安全性最佳實務

請注意，使用「重新導向程式」，您可能會面臨「開啟重新導向弱點」的風險。 為避免第三方未授權使用重新導向程式連結，我們建議您使用「已授權的主機」來允許列出預設的重新導向URL網域。 [!DNL Target]使用主機來允許列出您要允許重新導向的網域。 如需詳細資訊，請參閱[建立允許清單，指定在&#x200B;*主機*&#x200B;中授權傳送mbox呼叫至 [!DNL Target]](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=zh-Hant#allowlist)的主機。

## 限制

* 沒有像標準 mbox 那樣的用戶端逾時。如果[!DNL Target]完全關閉，則廣告的訪客將不會看到內容，甚至不會看到預設內容。
* 第三方 Cookie 用來追蹤造訪。如果 PCId 不一樣，依預設，訪客的第三方會與任何現有的第一方設定檔合併。
* 若要使用 AdBox 本身的第一方 Cookie，您必須在 URL 中傳遞 mBox 工作階段。請連絡您的帳戶代表來進行這項作業。
* 若要使用第一方 Cookie 來追蹤廣告點按次數，請在 URL 中傳遞 mbox 工作階段。請連絡您的帳戶代表來進行這項作業。
* 若要在相同頁面上使用多個 AdBox，則您必須在 URL 中傳遞 Mbox 作業。請連絡您的帳戶代表來進行這項作業。您在相同頁面上可能擁有一個 AdBox 和一個「重新導向程式」連結 (因為「重新導向程式」實際上位於第二個頁面)。

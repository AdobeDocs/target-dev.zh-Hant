---
keywords: 實作，非javascript， mbox， adbox
description: 使用AdBox傳遞使用 [!DNL Adobe Target]的站外實作影像。 AdBox就像mbox，但是由URL所控制，而非JavaScript。
title: 如何為影像建立Adbox？
feature: Implement Email
exl-id: ad1eb6c4-7a16-4054-ae76-57971261e931
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 71%

---

# 為影像建立 Adbox

使用AdBox在離站實作中使用[!DNL Adobe Target]傳遞影像。

AdBox 類似 mbox，不過它是由 URL 來控制，而非 JavaScript。AdBox 是使用特殊的「AdBox URL」建立，此 URL 會將「廣告」mbox (或 AdBox) 載入到您的 Adobe 帳戶。請在您的活動中使用此 AdBox 取代 mbox。請在電子郵件或其他非 JavaScript 實施中使用 AdBox URL 而非直接影像參考。

如需選取正確設定的說明，請參閱[非JavaScript型實作](/help/dev/implement/email/overview.md)。

1. 建立 AdBox URL:

   ```
   https://myClientCode.tt.omtrdc.net/m2/myClientCode/ubox/
   image?mbox=emailHeroImage123_320x200&
   mboxDefault=http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fimg%2Flogo%2Egif
   ```

   * 其中 `myClientCode` 代表貴公司的用戶端代碼。您的公司用戶端代碼為全部小寫並且不帶特殊字元。

     您的使用者端代碼可在[!DNL Target]介面的&#x200B;**[!UICONTROL Administation]** > **[!UICONTROL Implementation]**&#x200B;頁面最上方取得。

   * 其中 `image` 是呼叫類型。在此範例中，它是一個影像。

   * 其中 `emailHeroImage123_320x200` 是 AdBox 的名稱。

   * 其中 `http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fimg%2Flogo%2Egif` 是 mbox 的預設內容。這必須為影像。

     此網址必須經過 URL 編碼，且必須為絕對參照。您可以使用[HTMLURL編碼參考](https://www.w3schools.com/tags/ref_urlencode.asp)快速將您的URL編碼。

1. 為每個替代影像建立[重新導向選件](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html?lang=zh-Hant)。

   >[!NOTE]
   >
   >AdBox 必須以「重新導向選件」或是預設的內容選件載入。其他選件類型將無法運作。因為 AdBox 是 URL，它只能顯示所收到的 URL，所以只有「重新導向選件」可以運作。

1. 建立活動。

   請參閱[非 JavaScript 型實作](/help/dev/implement/email/overview.md)，以瞭解符合您目標的正確設定。

1. 完成活動上的品質保證。

   作為最佳作法，請建立一份虛擬頁面，然後確認對您所有的環境來說，所有的體驗、預設內容與報表都如預期般在所有瀏覽器類型上運作。

1. 啟動活動。

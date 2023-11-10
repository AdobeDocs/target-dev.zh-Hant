---
title: 如何使用傳送API擷取Recommendations
description: 本文會引導開發人員完成使用Adobe Target Delivery API擷取建議內容所需的步驟。
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 9b391f42-2922-48e0-ad7e-10edd6125be6
source-git-commit: d98c7b890f7456de0676cadce5d6c70bc62d6140
workflow-type: tm+mt
source-wordcount: '1520'
ht-degree: 1%

---

# 使用傳送API擷取Recommendations

Adobe Target和Adobe Target Recommendations API可用來傳送網頁的回應，也可用於非HTML型體驗，包括應用程式、熒幕、主控台、電子郵件、資訊站和其他顯示裝置。 換言之，當無法使用Target資料庫和JavaScript時， [Target傳送API](/help/dev/implement/delivery-api/overview.md) 仍可讓您存取所有的Target功能，以提供個人化的體驗。

>[!NOTE]
>
>請求包含實際建議（建議產品或專案）的內容時，請使用Target Delivery API。

若要擷取建議，請使用適當的內容資訊傳送Adobe Target傳送APIPOST呼叫，其中可能包括使用者ID （用於設定檔特定建議，例如使用者最近檢視的專案）、相關mbox名稱、mbox引數、設定檔引數或其他屬性。 回應將包含JSON或HTML格式的建議entity.ids （並可能包含其他實體資料），這些資料隨後可顯示在裝置中。

此 [傳送API](/help/dev/implement/delivery-api/overview.md) 適用於Adobe Target會公開標準Target請求提供的所有現有功能。

傳送API：

* 可讓您以RESTful方式擷取位置和對象的體驗或選件。
* 不需要驗證。
* 僅限貼文。
* 不會處理Cookie或重新導向呼叫。
* 不需要或無法辨識「使用者角色」。 它只會擷取內容或向Target邊緣伺服器報告事件。

若要使用「傳送API」來傳送Target體驗（包括建議），請執行下列步驟：

1. 使用表單式撰寫器（而非視覺化體驗撰寫器）建立Target活動(A/B、XT、AP或Recommendations)。
1. 使用傳送API針對您剛建立的Target活動所產生的請求，取得回應。

&lt;! — 問：為何需要執行這兩個步驟？ 如果您有為mbox定義的表單式建議，有中同時具有傳送API步驟以擷取結果又有什麼好處？ 為什麼不能讓表單式記錄將結果傳送到目的地裝置……?? 答：請參閱以下使用案例……這是您想要「攔截」暫止結果，以便在顯示結果之前執行更多作業的時間。 例如，庫存水準的即時比較。 --->

## 使用表單式體驗撰寫器建立建議

若要建立可與Delivery API搭配使用的建議，請使用 [表單式撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html).

1. 首先，建立並儲存JSON型設計以用於您的建議。 如需範例JSON，以及有關在設定表單式活動時如何傳回JSON回應的背景資訊，請參閱以下檔案： [建立建議設計](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html). 在此範例中，設計命名為 *簡單JSON。*
   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

1. 在Target中導覽至 **[!UICONTROL 活動]** > **[!UICONTROL 建立活動]** > **[!UICONTROL Recommendations]**，然後選取 **[!UICONTROL 表單]**.

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

1. 選取屬性，然後按一下 **[!UICONTROL 下一個]**.
1. 定義您想要使用者收到建議回應的位置。 以下範例使用名為的位置 *api_charter*. 選取您先前建立的JSON型設計，並命名為 *簡單JSON。*
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
1. 儲存並啟用建議。 它會產生結果。 [一旦結果準備就緒](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html)，您可以使用傳送API來擷取它們。

## 使用傳送API

的語法 [傳送API](/help/dev/implement/delivery-api/overview.md) 為：

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. 請注意，使用者端代碼為必要項。 提醒您，您可以導覽至「 」，在Adobe Target中找到您的使用者端代碼 **[!UICONTROL Recommendations]** > **[!UICONTROL 設定]**. 請注意 **使用者端代碼** 中的值 **建議API Token** 區段。
   ![client-code.png](assets/client-code.png)
1. 取得使用者端代碼後，請建構您的傳送API呼叫。 以下範例開頭為 **[!UICONTROL 網頁批次Mbox傳送API呼叫]** 提供於 [傳送API Postman集合](../../implement/delivery-api/overview.md/#section/Getting-Started/Postman-Collection)，進行相關修改。 例如：
   * 此 **瀏覽器** 和 **地址** 物件已從 **內文**，因為非HTML使用案例不需要這些引數
   * *api_charter* 在此範例中列為位置名稱
   * 會指定entity.id，因為此推薦是根據內容相似度，而內容相似度需要傳遞目前的專案索引鍵至Target。
     ![server-side-Delivery-API-call.png](assets/server-side-delivery-api-call2.png)
請記得正確設定查詢引數。 例如，請務必指定 `{{CLIENT_CODE}}` 視需要。 &lt;!— Q：在更新的呼叫語法中，entity.id會列為profileParameter，而非mboxParameter （與舊版相同）。 ---> &lt;! — 問：舊影像 ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) 舊的隨附文字： 「請注意，此建議是根據內容相似產品，以及透過mboxParameters傳送的entity.id。」 —>
     ![client-code3](assets/client-code3.png)
1. 傳送要求。 這會針對以下專案執行： *api_charter* 位置，已在上面執行有效建議，並使用您的JSON設計定義，這將輸出建議實體清單。
1. 根據JSON設計接收回應。
   ![server-side-create-recs-json-response2.png](assets/server-side-create-recs-json-response2.png)
回應包括索引鍵ID以及建議實體的實體ID。

以這種方式搭配使用Delivery API與Recommendations，可讓您在非HTML裝置上向訪客顯示建議之前，先執行其他步驟。 例如，您可以在顯示最終結果之前，從傳送API取得回應，從其他系統（例如CMS、PIM或電子商務平台）執行實體屬性詳細資訊（詳細目錄、價格、評等專案）的額外即時查詢。

使用本指南中概述的方法，您可以取得任何應用程式來利用Target的回應，提供個人化建議！

## 範例實施

下列資源提供各種非HTML重點實作的範例。 請記住，由於涉及的系統和裝置，每個實作都是唯一的。

| 資源 | 詳細資料 |
| --- | --- |
| [Adobe Target無所不在 — 實作伺服器端或物聯網中](https://expleague.azureedge.net/labs/L733/index.html) | Adobe Summit 2019實驗室針對運用Adobe Target伺服器端API的React應用程式提供實作體驗。 |
| [行動應用程式中的Adobe Target (不含Adobe SDK)](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | 本指南會說明如何在行動應用程式中設定Adobe Target，而不安裝Adobe SDK。 此解決方案使用Tealium SDK Webview與遠端命令模組傳送及接收對Adobe訪客API (Experience Cloud)與Adobe Target API的請求。 |
| [在Experience Platform Launch和實作Target API中設定Target擴充功能](https://developer.adobe.com/client-sdks/documentation/adobe-target/) | 在Experience Platform Launch中設定Target擴充功能、將Target擴充功能新增至您的應用程式，以及實作Target API以要求活動、預先擷取選件和進入視覺預覽模式的步驟。 |
| [Adobe Target節點使用者端](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | 開放原始碼Target Node.js SDK v1.0 |
| [伺服器端概述](../../implement/server-side/server-side-overview.md) | Adobe Target伺服器端傳送API、伺服器端批次傳送API、Node.js SDK和Adobe Target Recommendations API的相關資訊。 |
| [電子郵件中的Adobe Campaign Content Recommendations](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | 說明如何透過Adobe Campaign中的Adobe Target和Adobe I/O Runtime運用電子郵件中的內容建議的部落格。 |

## 使用API管理Recommendations設定

大部分情況下，建議會在Adobe Target UI中設定，然後透過Target API使用或存取，原因如以上各節所述。 這種UI-API協調很常見。 不過，有時使用者可能想要透過API執行所有動作，包括設定以及結果的使用。 雖然不太常見，但使用者完全可以設定、執行、 *和* 完全使用API來利用建議的結果。

我們在 [較早的區段](manage-catalog.md) 如何管理Adobe Target Recommendations實體並在伺服器端傳送。 同樣地， [Adobe Developer Console](https://developer.adobe.com/console/home) 可讓您管理條件、促銷活動、集合和設計範本，而無須登入Adobe Target。 您可能會找到所有Recommendations API的完整清單 [此處](https://developer.adobe.com/target/administer/recommendations-api/)，以下提供摘要以供參考。

| 資源 | 詳細資料 |
| --- | --- |
| [集合](https://developer.adobe.com/target/administer/recommendations-api/#tag/Collections) | 清單、建立、取得、編輯和刪除集合。 |
| [條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Criteria) | 列出並取得條件。 |
| [設計](https://developer.adobe.com/target/administer/recommendations-api/#tag/Designs) | 列出、建立、取得、編輯、刪除及驗證設計。 |
| [實體](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities) | 儲存、刪除和取得實體。 |
| [促銷活動](https://developer.adobe.com/target/administer/recommendations-api/#tag/Promotions) | 列出、建立、取得、編輯和刪除促銷活動。 |
| [類別條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Category-Criteria) | 清單、建立、取得、編輯和刪除類別條件。 |
| [自訂條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Custom-Criteria) | 列出、建立、取得、編輯和刪除自訂條件。 |
| [專案條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Item-Criteria) | 列出、建立、取得、編輯和刪除專案條件。 |
| [人氣條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Popularity-Criteria) | 列出、建立、取得、編輯和刪除熱門程度條件。 |
| [設定檔屬性條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Profile-Attribute-Criteria) | 列出、建立、取得、編輯和刪除設定檔屬性條件。 |
| [最近條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Recent-Criteria) | 列出、建立、取得、編輯和刪除最近使用的條件。 |
| [序列條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Sequence-Criteria) | 清單、建立、取得、編輯和刪除序列條件。 |

## 參考檔案

* [Adobe Target Delivery API檔案](/help/dev/implement/delivery-api/overview.md)
* [將 Recommendations 與電子郵件整合](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## 摘要與評論

恭喜！完成本指南後，您已瞭解如何：
* [使用Recommendations API管理您的目錄](manage-catalog.md)
* [使用Recommendations API管理自訂條件](manage-custom-criteria.md)
* [搭配Recommendations使用傳送API](fetch-recs-server-side-delivery-api.md)

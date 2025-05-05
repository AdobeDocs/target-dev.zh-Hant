---
title: 如何使用傳送API擷取建議
description: 本文會引導開發人員完成使用Adobe Target Delivery API擷取建議內容所需的步驟。
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 9b391f42-2922-48e0-ad7e-10edd6125be6
source-git-commit: 526445fccee9b778b7ac0d7245338f235f11d333
workflow-type: tm+mt
source-wordcount: '1286'
ht-degree: 1%

---

# 使用傳送API擷取建議

Adobe Target和Adobe Target Recommendations API可用於針對網頁提供回應，也可用於非HTML型體驗，包括應用程式、熒幕、主控台、電子郵件、資訊站和其他顯示裝置。 換言之，當無法使用Target資料庫和JavaScript時，[Target傳送API](/help/dev/implement/delivery-api/overview.md)仍可讓您存取所有的Target功能，以提供個人化的體驗。

>[!NOTE]
>
>請求包含實際建議（建議產品或專案）的內容時，請使用Target Delivery API。

若要擷取建議，請使用適當的內容資訊傳送Adobe Target傳送API POST呼叫，其中可能包括使用者ID （用於設定檔特定建議，例如使用者最近檢視的專案）、相關mbox名稱、mbox引數、設定檔引數或其他屬性。 回應將包含JSON或HTML格式的建議entity.ids （並可能包含其他實體資料），這些資料隨後可顯示在裝置中。

適用於Adobe Target的[傳送API](/help/dev/implement/delivery-api/overview.md)會公開標準Target請求提供的所有現有功能。

傳送API：

* 可讓您以RESTful方式擷取位置和對象的體驗或選件。
* 不需要驗證。
* 僅限貼文。
* 不會處理Cookie或重新導向呼叫。
* 不需要或無法辨識「使用者角色」。 它只會擷取內容或向Target邊緣伺服器報告事件。

若要使用「傳送API」來傳送Target體驗（包括建議），請執行下列步驟：

1. 使用表單式撰寫器（而非視覺化體驗撰寫器）建立Target活動（A/B、XT、AP或Recommendations）。
1. 使用傳送API針對您剛建立的Target活動所產生的請求，取得回應。

&lt;！ — 問：為何需要執行這兩個步驟？ 如果您有為mbox定義的表單式建議，有中同時具有傳送API步驟以擷取結果又有什麼好處？ 為什麼不能讓表單式記錄將結果傳送到目的地裝置……?? 答：請參閱以下使用案例……這是您想要「攔截」暫止結果，以便在顯示結果之前執行更多作業的時間。 例如，庫存水準的即時比較。 —>

## 使用表單式體驗撰寫器建立建議

若要建立可與傳送API搭配使用的建議，請使用[表單式撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=zh-Hant)。

1. 首先，建立並儲存JSON型設計以用於您的建議。 如需範例JSON，以及有關設定表單式活動時如何傳回JSON回應的背景資訊，請參閱[建立建議設計](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html?lang=zh-Hant)的相關檔案。 在此範例中，設計名為&#x200B;*簡單JSON。*
   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

1. 在Target中，導覽至&#x200B;**[!UICONTROL Activities]** > **[!UICONTROL Create Activity]** > **[!UICONTROL Recommendations]**，然後選取&#x200B;**[!UICONTROL Form]**。

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

1. 選取屬性，然後按一下&#x200B;**[!UICONTROL Next]**。
1. 定義您想要使用者收到建議回應的位置。 下列範例使用名為&#x200B;*api_charter*&#x200B;的位置。 選取您先前建立且名為&#x200B;*簡單JSON.*的JSON型設計
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
1. 儲存並啟用建議。 它會產生結果。 [結果就緒後](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html?lang=zh-Hant)，您可以使用傳送API來擷取結果。

## 使用傳送API

[傳送API](/help/dev/implement/delivery-api/overview.md)的語法為：

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. 請注意，使用者端代碼為必要項。 提醒您，您可以導覽至&#x200B;**[!UICONTROL Recommendations]** > **[!UICONTROL Settings]**，在Adobe Target中找到您的使用者端代碼。 請注意&#x200B;**建議API Token**&#x200B;區段中的&#x200B;**使用者端代碼**&#x200B;值。
   ![client-code.png](assets/client-code.png)
1. 取得使用者端代碼後，請建構您的傳送API呼叫。 以下範例以[傳送API Postman集合](../../implement/delivery-api/overview.md/#section/Getting-Started/Postman-Collection)中提供的&#x200B;**[!UICONTROL Web Batched Mboxes Delivery API Call]**&#x200B;開始，並進行相關修改。 例如：
   * 已從&#x200B;**內文**&#x200B;移除&#x200B;**瀏覽器**&#x200B;和&#x200B;**位址**&#x200B;物件，因為非HTML使用案例不需要它們
   * 此範例中將&#x200B;*api_charter*&#x200B;列為位置名稱
   * 會指定entity.id，因為此推薦是根據內容相似度，而內容相似度需要傳遞目前的專案索引鍵至Target。

     ![server-side-Delivery-API-call.png](assets/server-side-delivery-api-call2.png)
請記得正確設定查詢引數。 例如，請務必視需要指定`{{CLIENT_CODE}}`。 &lt;！— Q：在更新的呼叫語法中，entity.id會列為profileParameter，而非mboxParameter （與舊版相同）。 —> &lt;！ — 問：舊影像![server-side-create-recs-post.png](assets/server-side-create-recs-post.png)舊的隨附文字： 「請注意，此建議是以透過mboxParameters傳送的entity.id為基礎的內容類似產品為基礎。」 —>
     ![client-code3](assets/client-code3.png)
1. 傳送要求。 這會針對&#x200B;*api_charter*&#x200B;位置執行，該位置上執行有效建議，並使用您的JSON設計定義，這將輸出建議實體清單。
1. 根據JSON設計接收回應。
   ![server-side-create-recs-json-response2.png](assets/server-side-create-recs-json-response2.png)
回應包括索引鍵ID以及建議實體的實體ID。

以這種方式搭配建議使用傳送API，可讓您在非HTML裝置上向訪客顯示建議之前，先執行其他步驟。 例如，您可以在顯示最終結果之前，從傳送API取得回應，從其他系統(例如CMS、PIM或電子商務平台)執行實體屬性詳細資訊（詳細目錄、價格、評等專案）的額外即時查詢。

使用本指南中概述的方法，您可以取得任何應用程式來利用Target的回應，提供個人化建議！

## 範例實施

下列資源提供各種非HTML重點實作的範例。 請記住，由於涉及的系統和裝置，每個實作都是唯一的。

| 資源 | 詳細資料 |
| --- | --- |
| [在Experience Platform Launch中設定Target擴充功能並實作Target API](https://developer.adobe.com/client-sdks/documentation/adobe-target/) | 在Experience Platform Launch中設定Target擴充功能、將Target擴充功能新增至您的應用程式，以及實作Target API以要求活動、預先擷取選件和進入視覺預覽模式的步驟。 |
| [Adobe Target節點使用者端](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | 開放來源Target Node.js SDK v1.0 |
| [伺服器端概述](../../implement/server-side/server-side-overview.md) | Adobe Target伺服器端傳送API、伺服器端批次傳送API、Node.js SDK和Adobe Target Recommendations API的相關資訊。 |
| [電子郵件中的Adobe Campaign內容建議](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | 說明如何透過Adobe Campaign中的Adobe Target和Adobe I/O Runtime運用電子郵件中的內容建議的部落格。 |

## 使用API管理Recommendations設定

大部分情況下，建議會在Adobe Target UI中設定，然後透過Target API使用或存取，原因如以上各節所述。 這種UI-API協調很常見。 不過，有時使用者可能想要透過API執行所有動作，包括設定以及結果的使用。 雖然不太常見，但使用者完全可以使用API來設定、執行&#x200B;*和*&#x200B;並善用建議的結果。

我們在[先前的](manage-catalog.md)章節中瞭解了如何管理Adobe Target Recommendations實體並在伺服器端傳送它們。 同樣地，[Adobe Developer Console](https://developer.adobe.com/console/home)可讓您管理條件、促銷活動、集合和設計範本，而不需要登入Adobe Target。 [此處](https://developer.adobe.com/target/administer/recommendations-api/)提供所有Recommendations API的完整清單，但此處提供摘要以供參考。

| 資源 | 詳細資料 |
| --- | --- |
| [集合](https://developer.adobe.com/target/administer/recommendations-api/#tag/Collections) | 清單、建立、取得、編輯和刪除集合。 |
| [標準](https://developer.adobe.com/target/administer/recommendations-api/#tag/Criteria) | 列出並取得條件。 |
| [設計](https://developer.adobe.com/target/administer/recommendations-api/#tag/Designs) | 列出、建立、取得、編輯、刪除及驗證設計。 |
| [個實體](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities) | 儲存、刪除和取得實體。 |
| [促銷活動](https://developer.adobe.com/target/administer/recommendations-api/#tag/Promotions) | 列出、建立、取得、編輯和刪除促銷活動。 |
| [類別條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Category-Criteria) | 清單、建立、取得、編輯和刪除類別條件。 |
| [自訂條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Custom-Criteria) | 列出、建立、取得、編輯和刪除自訂條件。 |
| [專案條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Item-Criteria) | 列出、建立、取得、編輯和刪除專案條件。 |
| [人氣條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Popularity-Criteria) | 列出、建立、取得、編輯和刪除熱門程度條件。 |
| [設定檔屬性條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Profile-Attribute-Criteria) | 列出、建立、取得、編輯和刪除設定檔屬性條件。 |
| [最近的條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Recent-Criteria) | 列出、建立、取得、編輯和刪除最近使用的條件。 |
| [序列條件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Sequence-Criteria) | 清單、建立、取得、編輯和刪除序列條件。 |

## 參考檔案

* [Adobe Target Delivery API檔案](/help/dev/implement/delivery-api/overview.md)
* [將推薦與電子郵件整合](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/integrating-recs-email.html?lang=zh-Hant)

## 摘要與評論

恭喜！完成本指南後，您已瞭解如何：
* [使用Recommendations API管理目錄](manage-catalog.md)
* [使用Recommendations API管理自訂條件](manage-custom-criteria.md)
* [搭配建議使用傳送API](fetch-recs-server-side-delivery-api.md)

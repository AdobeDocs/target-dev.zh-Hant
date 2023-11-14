---
title: 如何使用API管理您的Recommendations目錄
description: 使用Adobe Target API建立、更新、儲存、取得及刪除Recommendations目錄中的實體所需的步驟。
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: aea82607-cde4-456a-8dfb-2967badce455
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 1%

---

# 使用API管理您的Recommendations目錄

同時確保您符合 [使用Recommendations API的需求](/help/dev/before-administer/recs-api/overview.md#prerequisites)，您已瞭解如何 [產生存取權杖](/help/dev/before-administer/configure-authentication.md) 使用JWT驗證流程以使用 [!DNL Adobe Target] 管理API (位於 [Adobe Developer Console](https://developer.adobe.com/console/home).

您現在可以使用 [RECOMMENDATIONS API](https://developer.adobe.com/target/administer/recommendations-api/) 新增、更新或刪除建議目錄中的專案。 與其他Adobe Target Admin API一樣，Recommendations API需要驗證。

>[!NOTE]
>
>傳送 **[!UICONTROL IMS：JWT透過使用者權杖產生+驗證]** 要求重新整理您的存取Token以進行驗證，因為它會在24小時後過期。 另請參閱 [設定Adobe API驗證](../configure-authentication.md) 以取得指示。

![JWT3ff](assets/configure-io-target-jwt3ff.png)

繼續進行之前，請先取得 [Recommendations Postman集合](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman).

## 使用儲存實體API建立和更新專案

若要使用API （而非CSV產品摘要或產品頁面上引發的Target請求）填入您的Recommendations產品資料庫，請使用 [儲存實體API](https://developer.adobe.com/target/administer/recommendations-api/#operation/saveEntities). 此請求會在單一Target環境中新增或更新專案。 語法為：

```
POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities
```

例如，「儲存實體」可用於在達到某些臨界值時（例如存貨或價格的臨界值）更新專案，以標示這些專案並防止它們被建議。

1. 瀏覽至 **[!UICONTROL Target]** > **[!UICONTROL 設定]** > **[!UICONTROL 主機]** > **[!UICONTROL 控制環境]** 以取得您想要新增或更新專案的「目標環境ID」。

   ![SaveEntities1](assets/SaveEntities01.png)

1. 驗證 `TENANT_ID` 和 `API_KEY` 參考先前建立的Postman環境變數。 使用下圖來比較。 如有必要，請修改API請求中的標題和路徑，使其與以下影像中的標題和路徑相符。

   ![SaveEntities3](assets/SaveEntities03.png)

1. 輸入您的JSON為 **原始** 中的程式碼 **內文**. 別忘了使用指定您的環境ID `environment` 變數中。 （在以下範例中，環境ID為6781。）

   ![SaveEntities4.png](assets/SaveEntities04.png)

   以下是將entity.id kit2001與Toaster Oven產品的關聯實體值新增到環境6781中的範例JSON。

   ```
       {
       "entities": [{
               "name": "Toaster Oven",
               "id": "kit2001",
               "environment": 6781,
               "categories": [
                   "housewares:appliances"
               ],
               "attributes": {
                   "inventory": 77,
                   "margin": 23,
                   "message": "crashing helicopter",
                   "pageUrl": "www.foobar.foo.com/helicopter.html",
                   "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                  "value": 19.2
               }
           }]
       }
   ```

1. 按一下「**[!UICONTROL 傳送]**」。您應會收到下列回應。

   ![SaveEntities5.png](assets/SaveEntities05.png)

   JSON物件可以調整規模，以傳送多個產品。 例如，此JSON會指定兩個實體。

   ```
       {
           "entities": [{
                   "name": "Toaster Oven",
                   "id": "kit2001",
                   "environment": 6781,
                   "categories": [
                       "housewares:appliances"
                   ],
                   "attributes": {
                       "inventory": 89,
                       "margin": 11,
                       "message": "Toaster Oven",
                       "pageUrl": "www.foobar.foo.com/helicopter.html",
                       "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                       "value": 102.5
                   }
               },
               {
                   "name": "Blender",
                   "id": "kit2002",
                   "environment": 6781,
                   "categories": [
                       "housewares:appliances"
                   ],
                   "attributes": {
                       "inventory": 36,
                       "margin": 5,
                       "message": "Blender",
                       "pageUrl": "www.foobar.foo.com/helicopter.html",
                       "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                       "value": 54.5
                   }
               }
           ]
       }
   ```

1. 現在輪到你了！ 使用 **[!UICONTROL 儲存實體]** API可將下列專案新增至您的目錄。 使用上述範例JSON作為起點。 （您需要擴充JSON以包含其他實體。）

   ![SaveEntities6.png](assets/SaveEntities06.png)

看來最後兩個專案並不屬於。 讓我們使用 **[!UICONTROL 取得實體]** API，並在必要時使用 **[!UICONTROL 刪除實體]** API。

## 使用取得實體API取得專案詳細資料

若要擷取現有專案的詳細資訊，請使用 [取得實體API](https://developer.adobe.com/target/administer/recommendations-api/#operation/getEntity). 語法為：

```
GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities/[entity.id]
```

一次只能為單一實體擷取實體詳細資料。 您可以使用「取得實體」來確認如預期般在目錄中進行的更新，或稽核目錄的內容。

1. 在API請求中，使用變數指定實體ID `entityId`. 下列範例將傳回entityId=kit2004之實體的詳細資料。

   ![GetEntity1](assets/GetEntity1.png)

1. 驗證 `TENANT_ID` 和 `API_KEY` 參考先前建立的Postman環境變數。 使用下圖來比較。 如有必要，請修改API請求中的標題和路徑，使其與以下影像中的標題和路徑相符。

   ![GetEntity2](assets/GetEntity2.png)

1. 傳送要求。

   ![GetEntity3](assets/GetEntity3.png)
如果您收到錯誤訊息，指出找不到實體（如上述範例所示），請確認您要將請求提交至正確的Target環境。



   >[!NOTE]
   >
   >如果沒有明確指定環境，取得實體會嘗試從取得實體。 [預設環境](https://experienceleague.adobe.com/docs/target/using/administer/environments.html) 僅限。 如果您想從預設環境以外的任何環境提取，則必須指定環境ID。

1. 如有必要，請新增 `environmentId` 引數，然後重新傳送要求。

   ![GetEntity4](assets/GetEntity4.png)

1. 傳送另一個 **[!UICONTROL 取得實體]** 請求，這次檢查其entityId=kit2005的實體。

   ![GetEntity5](assets/GetEntity5.png)

假設您決定需要從目錄中移除這些實體。 讓我們使用 **[!UICONTROL 刪除實體]** API。

## 使用Delete Entities API刪除專案

若要從目錄中移除專案，請使用 [刪除實體API](https://developer.adobe.com/target/administer/recommendations-api/#operation/deleteEntities). 語法為：

```
DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities?ids=[comma-delimited-entity-ids]&environment=[environmentId]
```

>[!WARNING]
>
>刪除實體API會刪除您指定的ID所參考的實體。 如果未提供實體ID，則會刪除指定環境中的所有實體。 如果未指定環境ID，實體將從所有環境中刪除。 請謹慎使用！

1. 瀏覽至 **[!UICONTROL Target]** > **[!UICONTROL 設定]** > **[!UICONTROL 主機]** > **[!UICONTROL 環境]** 以取得您要刪除專案的「目標環境ID」。

   ![DeleteEntities1](assets/SaveEntities01.png)

1. 在API要求中，使用語法指定您要刪除之實體的實體ID `&ids=[comma-delimited-entity-ids]` （一個查詢引數）。 刪除多個實體時，請使用逗號區隔ID。

   ![DeleteEntities2](assets/DeleteEntities2.png)

1. 使用語法指定環境ID `&environment=[environmentId]`，否則會刪除所有環境中的實體。

   ![DeleteEntities3](assets/DeleteEntities3.png)

1. 驗證 `TENANT_ID` 和 `API_KEY` 參考先前建立的Postman環境變數。 使用下圖來比較。 如有必要，請修改API請求中的標題和路徑，使其與以下影像中的標題和路徑相符。

   ![DeleteEntities4](assets/DeleteEntities4.png)

1. 傳送要求。

   ![DeleteEntities5](assets/DeleteEntities5.png)

1. 使用驗證您的結果 **[!UICONTROL 取得實體]**，現在應表示找不到已刪除的實體。

   ![DeleteEntities6](assets/DeleteEntities6.png)

   ![DeleteEntities6](assets/DeleteEntities7.png)

恭喜！您現在可以使用Recommendations API來建立、更新、刪除及取得目錄中實體的詳細資訊。 在下一節中，您將瞭解如何管理自訂條件。

&lt;!— [下堂課「管理自訂條件」>](manage-custom-criteria.md) —>

---
title: Adobe Target API概觀
description: 不同Adobe Target API的概觀，包括傳送API、報表API、管理員API、設定檔API、建議API，以及Postman集合的連結。
exl-id: bf886103-36af-4061-b8be-2fe645f45ff3
feature: APIs/SDKs
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 1%

---

# Target API總覽

本文主要說明不同的Target API，然後再著重說明「管理員」和「設定檔API」的特定需求。 如果您想透過UI管理Target，請參閱 [的管理區段 *Adobe Target商業使用手冊*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en).

## API型別

Adobe Target API是支援Adobe Target產品(例如Adobe Recommendations)的API集合。 這些API可讓您建立資料豐富的使用者介面，以便用於操作和整合資料。

Adobe Target API可依型別分組：管理員、設定檔、傳送和報告。

>[!NOTE]
>
>管理員API和設定檔API通常是整體參照（「管理員和設定檔API」），但也可能單獨參照（「管理員API」和「設定檔API」）。 Recommendations API是Target Admin API的特定實作。

| API型別 | 讓您能夠執行的動作 | 下載連 | 其他實用連結 |
| --- | --- | --- |--- |
| [管理](../administer/admin-api/admin-api-overview-new.md) | 建立、修改和刪除活動、對象、選件和其他物件(包括Recommendations實體、條件、設計等)。 Recommendations API是一種Admin API。) | <UL><li>[Target管理員API Postman集合](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[Recommendations API Postman集合](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman)</li></UL> | [使用Recommendations API](../before-administer/recs-api/overview.md) |
| 個人資料 | 擷取及修改儲存在Adobe Target中的使用者設定檔。 | [Target設定檔API Postman集合](https://developers.adobetarget.com/api/#profiles) |  |
| [傳送](../implement/delivery-api/overview.md) | 從Target擷取最佳化和個人化的內容，以傳送給一般使用者。 | [Target傳送API Postman集合](/help/dev/before-implement/delivery-api-overview/getting-started.md#postman) |  |
| [報告](../administer/admin-api/admin-api-overview-new.md) | 匯出活動結果和其他報告結果。 | 報告API包含在 [Target管理員API Postman集合](https://developers.adobetarget.com/api/#admin-postman-collection). |  |
| [模型](../administer/models-api/models-api-overview.md) | 管理您要Target從其機器學習模型中排除的功能清單（「封鎖清單」）。 模型API是一種Admin API，但由於對無法透過UI存取的物件（封鎖清單）執行的獨特操作，因此此處單獨列出模型API。 |  |  |

## API 的各項差異

Target Admin API (包括Recommendations API)與Target Delivery API之間有重要的差異：

* 管理員API可讓您設定Target的各個層面，您也可以在Target UI中設定。 模型API是此作法的例外，其可讓您設定UI中未提供的Target功能。 無論如何， **所有管理員API都需要驗證。**

* 傳送API可讓您擷取內容。 傳送API不需要驗證。

若要使用Target Admin API，您必須使用 [Adobe Developer Console](https://developer.adobe.com/console/home). 如需詳細資訊，請參閱 [如何設定驗證](../before-administer/configure-authentication.md).

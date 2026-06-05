---
keywords: 實作， api，設定檔，設定檔api設定，驗證token
description: 瞭解如何透過 [!DNL Adobe Target] API設定批次更新的驗證，並產生設定檔驗證Token。
title: 如何使用設定檔API設定來啟用或停用批次更新？
feature: APIs/SDKs
exl-id: 968f33d0-296b-4248-8c9a-8e6f3077bdfa
TQID: https://experienceleague.adobe.com/-KYSphaCrm0ICK7g92v9x-uK--nwirs4-DWBR3G5rTM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 31%

---

# 輪廓 API 設定

啟用或停用透過[!DNL Adobe Target] API批次更新的驗證，並產生設定檔驗證Token。

[!DNL Adobe Target]會為每個個別使用者建立和維護設定檔。 此設定檔儲存在[!DNL Target]邊緣叢集上，並在每次造訪後即時更新。 您也可以透過API個別或大量更新設定檔。

為了提高安全性，您可以要求「大量更新 API」呼叫必須在要求的標頭中傳送有效的存取 Token。

**若要要求驗證，並使用[!DNL Target] UI產生存取權杖：**

1. 按一下&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]**。
1. 在&#x200B;**[!UICONTROL 設定檔API]**&#x200B;底下，將&#x200B;**[!UICONTROL 需要驗證]**&#x200B;切換至啟用或停用的位置。

   ![替代影像](assets/profile_api_settings.png)

1. （視條件而定）如果您已啟用驗證需求，請按一下&#x200B;**[!UICONTROL 產生新的設定檔驗證Token]**。

   ![替代影像](assets/profile_api_settings_2.png)

   權杖會根據「到期時間」方塊中列出的時間而到期。

   您必須具備下列其中一個使用者權限才能產生驗證權杖：

   * 管理員角色或至少擁有核准者許可權

     如需Target Standard客戶的詳細資訊，請參閱&#x200B;*使用者*&#x200B;中的[指定角色和許可權](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html#roles-permissions)。 如需有關 [!DNL Target Premium] 客戶的詳細資訊，請參閱[設定企業權限](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html)。

   * 工作區/產品設定檔層級的管理員角色

     工作區僅適用於 [!DNL Target Premium] 客戶。 如需詳細資訊，請參閱[企業使用者權限](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html)。

   * [!DNL Adobe Target] 產品層級的管理員權限 (Sysadmin 權限)

您也可以透過 API 來產生輪廓驗證 Token。 如需詳細資訊，請參閱[Adobe Target管理及設定檔API指南](../../administer/admin-api/admin-api-overview-new.md)中的「設定檔」。

1. 複製權杖，並將其加入請求標頭的格式： &quot;Authorization&quot; ： &quot;Bearer&quot;。

1. 按一下&#x200B;**[!UICONTROL 產生新的設定檔驗證Token]**，視需要重新產生Token。

>[!WARNING]
>
>重設此 Token 會造成使用目前 Token 的 API 呼叫失敗。 這需要更新任何使用此 Token 的指令碼或應用程式。

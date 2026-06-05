---
keywords: 全域mbox，自訂全域mbox，編輯at.js， at.js，實作at.js
description: 瞭解如何在 [!DNL Adobe Target]中的[!UICONTROL 管理]-[!UICONTROL 實作]頁面上自訂at.js的全域mbox。
title: 如何自訂全域mbox？
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
TQID: https://experienceleague.adobe.com/MtbjwpKrZ-WmBnE5tBY74oJgQVB-zPLrCuFDrFshkGo
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 227
ht-degree: 16%

---

# 自訂全域 mbox

可協助您為at.js自訂[!DNL Adobe Target]全域mbox的資訊。

1. 按一下&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]**。

1. 停用&#x200B;**[!UICONTROL 已啟用頁面載入（自動建立全域mbox）]**，然後新增您要用來從[!DNL Target]傳送活動的自訂全域mbox名稱。

>[!WARNING]
>
>當您選取其他全域mbox時，變更會自動儲存。

此自訂全域 mbox 也用於點擊追蹤。

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. 在您的網站上實作at.js資料庫。

   如需詳細資訊，請參閱[如何部署at.js](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md)。

1. 計算您的發行轉變所需的時間。

   當您準備好要[!DNL Target]開始將您的全域mbox用於未來所有活動時，您可以繼續進行此步驟。

   更新自訂全域 mbox 的名稱以符合以上的步驟 2 中使用的名稱。


>[!WARNING]
>
>您帳戶中的所有活動都會與此mbox同步。 確認您的網站上存在全域mbox，讓活動可繼續運作。 請務必編輯並重新儲存使用與此mbox同步的[!UICONTROL 視覺化體驗撰寫器] (VEC)建立的受影響活動。 不需要重新儲存在[!UICONTROL 表單式體驗撰寫器]中或透過API建立的活動。

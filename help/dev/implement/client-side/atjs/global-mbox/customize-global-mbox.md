---
keywords: 全域mbox，自訂全域mbox，編輯at.js， at.js，實作at.js
description: 瞭解如何在 [!DNL Adobe Target]的[!UICONTROL Administration]-[!UICONTROL Implementation]頁面上自訂at.js的全域mbox。
title: 如何自訂全域mbox？
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 18%

---

# 自訂全域 mbox

可協助您為at.js自訂[!DNL Adobe Target]全域mbox的資訊。

1. 按一下&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]**。

1. 停用&#x200B;**[!UICONTROL Page load enabled (Auto create global mbox)]**，然後新增您要用來從[!DNL Target]傳送活動的自訂全域mbox的名稱。

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
>您帳戶中的所有活動都會與此mbox同步。 確認您的網站上存在全域mbox，讓活動可繼續運作。 請務必編輯並重新儲存使用與此mbox同步的[!UICONTROL Visual Experience Composer] (VEC)所建立的受影響活動。 不需要重新儲存在[!UICONTROL Form-Based Experience Composer]中或透過API建立的活動。

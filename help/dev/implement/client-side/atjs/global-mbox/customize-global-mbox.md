---
keywords: 全域mbox，自訂全域mbox，編輯at.js， at.js，實作at.js
description: 瞭解如何在上自訂適用於at.js的全域mbox [!UICONTROL 管理]-[!UICONTROL 實施] 頁面位置 [!DNL Adobe Target].
title: 如何自訂全域mbox？
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 17%

---

# 自訂全域 mbox

可協助您自訂的資訊 [!DNL Adobe Target] 適用於at.js的全域mbox。

1. 按一下「**[!UICONTROL 管理]** > 「**[!UICONTROL 實施]**」。

1. 停用 **[!UICONTROL 已啟用頁面載入（自動建立全域mbox）]**，然後新增您想要用來傳送活動之自訂全域mbox的名稱 [!DNL Target].

>[!WARNING]
>
>當您選取其他全域mbox時，變更會自動儲存。

此自訂全域 mbox 也用於點擊追蹤。

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. 在您的網站上實作at.js資料庫。

   另請參閱 [如何部署at.js](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md) 以取得詳細資訊。

1. 計算您的發行轉變所需的時間。

   當您準備就緒時 [!DNL Target] 若要在日後開始針對所有活動使用您的全域mbox，您可以繼續進行此步驟。

   更新自訂全域 mbox 的名稱以符合以上的步驟 2 中使用的名稱。


>[!WARNING]
>
>您帳戶中的所有活動都會與此mbox同步。 確認您的網站上存在全域mbox，讓活動可繼續運作。 請務必編輯並重新儲存使用建立的受影響活動。 [!UICONTROL 視覺化體驗撰寫器] (VEC)與此mbox同步。 不需要重新儲存在中建立的活動 [!UICONTROL 表單式體驗撰寫器] 或透過API。

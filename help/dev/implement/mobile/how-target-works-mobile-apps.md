---
description: 瞭解如何使用 [!DNL Adobe Mobile SDK] 向行動應用程式訪客顯示最佳體驗。
title: 如何 [!DNL Target] 在行動應用程式中工作？
feature: Implement Mobile
exl-id: 33001f01-fde6-48cb-ac02-d1a632b2150d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 34%

---

# 如何 [!DNL Target] 適用於行動應用程式

此 [!DNL Adobe Mobile SDK] 聯絡 [!DNL Target] 伺服器取得內容以及其他資料點，向使用者顯示正確的體驗。

>[!IMPORTANT]
>
>支援 [!DNL Adobe Mobile] 版本4。*x* SDK已於2021年8月31日結束，不建議再用於 [!DNL Adobe Target] 行動使用者。
>
>此 [適用於行動應用程式的Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} 是推薦的供電解決方案 [!DNL Adobe Experience Cloud] 行動應用程式中的解決方案和服務。

## [!DNL Target] 位置和成功量度

*目標位置*&#x200B;又稱為 mbox。應用程式中識別的位置可供測試或個人化 (例如，主畫面的歡迎訊息)。測試建立程序期間會識別這些位置。

A *[成功量度](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)*&#x200B;是使用者執行的動作，可指出特定的活動是否成功 (例如，註冊、購物、訂票等)。

![替代影像](assets/mobile-target-location.png)

* **[!DNL Target]位置：** 顯示在註冊按鈕下方的內容。

  此特定使用者在下午 6 點以前免運費。此位置可跨多個位置重複使用 [!DNL Target] 執行A/B測試和個人化的活動。

* **成功量度：** 使用者點選註冊按鈕時所執行的動作。

**瞭解如何 [!DNL Target] 在SDK中運作**

![替代影像](assets/how-target-mobile-works.png)

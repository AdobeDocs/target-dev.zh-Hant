---
keywords: 實施，實施， adobe launch， launch，競爭，重新導向， experienceplatform launch，platform launch，標籤， adobe platform，實施2
description: 瞭解如何使用 [!DNL Adobe Experience Platform] （實作Target的偏好方法）實作 [!DNL Adobe Target] at.js資料庫。
title: 如何使用 [!DNL Adobe Experience Platform]實作 [!DNL Target] ？
feature: Implement Server-side
exl-id: 0a325871-194a-479c-a3bf-294e3dde3e9a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 4%

---

# 使用[!DNL Adobe Experience Platform]實作[!DNL Target]

[!DNL Adobe Experience Platform]中的標籤是新一代[!DNL Adobe]的標籤管理功能。 標籤可讓客戶透過簡單的方式部署及管理必要的分析、行銷及廣告標籤功能，以便支援相關客戶體驗。

>[!NOTE]
>
>Adobe Experience Platform Launch已經過品牌重塑，現在是[!DNL Adobe Experience Platform]中的一套資料彙集技術。 因此，所有產品檔案中出現了幾項術語變更。 請參閱下列[檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?lang=zh-Hant&)，以取得術語變更的彙總參考資料。

下表列出您可取得詳細資訊的各種來源：

| 資源 | 詳細資料 |
|--- |--- |
| [新增Adobe Target](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/implement-solutions/target.html?lang=zh-Hant#implement-solutions) | 本教學課程提供逐步指示，說明如何在含有[!DNL Adobe Experience Platform]標籤的網站中實作[!DNL Target]。 主題包括新增 at.js JavaScript 資料庫、觸發全域 mbox、新增參數以及與其他解決方案整合。本文是大規模教學課程的一部分，說明如何實作Adobe Experience Platform和其他Adobe Experience Cloud解決方案。 |
| [快速入門手冊](https://experienceleague.adobe.com/docs/experience-platform/tags/get-started/quick-start.html?lang=zh-Hant) | 關於部署及管理提供客戶體驗必需的相關分析、行銷和廣告標籤資訊。 |
| [Adobe [!DNL Target] 延伸總覽](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/target/overview.html?lang=zh-Hant) | 有關使用[!DNL Adobe Experience Platform]實作[!DNL Target]的資訊。 |

## 使用[!DNL Target]擴充功能實作at.js的優點

下列優點只有在您使用[!DNL Adobe Experience Platform]中的標籤來實作at.js時才適用。 因此，Adobe強烈建議您在[!DNL Adobe Experience Platform]中使用標籤，而不要手動實作at.js。

* **解決[!DNL Adobe Analytics]與[!DNL Target]競爭條件：**&#x200B;由於可以在[!DNL Target]呼叫之前觸發[!DNL Analytics]呼叫，因此[!DNL Target]呼叫不會聯結[!DNL Analytics]呼叫。 此排序可能會導致資料不正確。 [!DNL Target]延伸模組會確保[!DNL Analytics]指標呼叫等待[!DNL Target]呼叫完成，無論是否成功。 使用[!DNL Adobe Experience Platform]中的標籤可解決客戶在手動實作時可能遇到的資料不一致問題。

  >[!NOTE]
  >
  >在[!DNL Adobe Analytics]擴充功能中使用「傳送信標」動作，讓[!DNL Analytics]呼叫等待[!DNL Target]呼叫。 如果您使用自訂程式碼直接呼叫`s.t()`或`s.tl()`，[!DNL Analytics]呼叫不會等到[!DNL Target]呼叫完成。

* **防止不正確的重新導向選件處理：**&#x200B;如果頁面上有[!DNL Target]和[!DNL Analytics]，而且有Target執行的重新導向選件，可能會發生[!DNL Analytics]追蹤器觸發不應觸發之要求的情況（因為系統正在將使用者重新導向至不同的URL）。 如果您透過[!DNL Adobe Experience Platform]中的標籤實作[!DNL Target]和[!DNL Analytics]，則不會遇到此問題。 使用[!DNL Adobe Experience Platform]中的標籤，[!DNL Target]指示[!DNL Analytics]中止[!DNL Analytics]指標要求。

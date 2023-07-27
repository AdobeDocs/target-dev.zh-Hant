---
keywords: 實施，實施， adobe launch， launch，競爭，重新導向， experienceplatform launch，platform launch，標籤， adobe platform，實施2
description: 瞭解如何實作 [!DNL Adobe Target] at.js資料庫使用 [!DNL Adobe Experience Platform]，實作Target的偏好方法。
title: 如何實作 [!DNL Target] 使用 [!DNL Adobe Experience Platform]？
feature: Implement Server-side
exl-id: 0a325871-194a-479c-a3bf-294e3dde3e9a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 8%

---

# 使用 [!DNL Target] 實作 [!DNL Adobe Experience Platform]

中的標籤 [!DNL Adobe Experience Platform] 是推出的新一代標籤管理功能 [!DNL Adobe]. 標籤可讓客戶透過簡單的方式部署及管理必要的分析、行銷及廣告標籤功能，以便支援相關客戶體驗。

>[!NOTE]
>
>Adobe Experience Platform Launch已經過品牌重塑，現在是 [!DNL Adobe Experience Platform]. 因此，所有產品文件中出現了幾項術語變更。請參閱下列內容 [檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?) 以取得術語變更的彙整參考資料。

下表列出您可取得詳細資訊的各種來源：

| 資源 | 詳細資料 |
|--- |--- |
| [新增Adobe Target](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/implement-solutions/target.html#implement-solutions) | 本教學課程提供實作的逐步指示 [!DNL Target] 在網站中標籤於 [!DNL Adobe Experience Platform]. 主題包括新增 at.js JavaScript 資料庫、觸發全域 mbox、新增參數以及與其他解決方案整合。本文是大規模教學課程的一部分，說明如何實作Adobe Experience Platform和其他Adobe Experience Cloud解決方案。 |
| [快速入門手冊](https://experienceleague.adobe.com/docs/experience-platform/tags/get-started/quick-start.html) | 關於部署及管理提供客戶體驗必需的相關分析、行銷和廣告標籤資訊。 |
| [Adobe [!DNL Target] 擴充功能概觀](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/target/overview.html) | 實作的相關資訊 [!DNL Target] 使用 [!DNL Adobe Experience Platform]. |

## 使用實作at.js的優點 [!DNL Target] 副檔名

下列優點僅適用於您使用的標籤 [!DNL Adobe Experience Platform] 實作at.js。 因此，Adobe強烈建議您使用 [!DNL Adobe Experience Platform] 而不需手動實作at.js。

* **解決 [!DNL Adobe Analytics] 和 [!DNL Target] 競爭條件：** 因為 [!DNL Analytics] 呼叫可在 [!DNL Target] 呼叫， [!DNL Target] 呼叫未連結至 [!DNL Analytics] 呼叫。 此排序可能會導致資料不正確。 此 [!DNL Target] 擴充功能可確保 [!DNL Analytics] 信標呼叫會等待 [!DNL Target] 呼叫完成，無論是否成功。 使用中的標籤 [!DNL Adobe Experience Platform] 解決客戶手動實施時可能遇到的資料不一致問題。

  >[!NOTE]
  >
  >請在中使用「傳送信標」動作 [!DNL Adobe Analytics] 擴充功能可讓 [!DNL Analytics] 呼叫等待 [!DNL Target] 呼叫。 如果您直接呼叫 `s.t()` 或 `s.tl()` 使用自訂程式碼， [!DNL Analytics] 來電不會等到 [!DNL Target] 呼叫完成。

* **防止不正確的重新導向選件處理：** 如果您有 [!DNL Target] 和 [!DNL Analytics] 在頁面上，而且有Target執行的重新導向選件，您可能會遇到 [!DNL Analytics] 追蹤器會觸發不應觸發的請求（因為系統正在將使用者重新導向至不同的URL）。 若您實施 [!DNL Target] 和 [!DNL Analytics] 透過中的標籤 [!DNL Adobe Experience Platform]，您不會遇到此問題。 使用中的標籤 [!DNL Adobe Experience Platform]， [!DNL Target] 指示 [!DNL Analytics] 中止 [!DNL Analytics] 信標要求。

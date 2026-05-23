---
keywords: 全域mbox，實作at.js
description: 瞭解 [!DNL Adobe Target], a name used to refer to the single server call made at the top of each web page in your [!DNL Target] 實作中的全域mbox。
title: 什麼是全域mbox？
feature: at.js
exl-id: 572c1dc6-5cdd-427a-9458-e5ec49990cf8
TQID: https://experienceleague.adobe.com/MXEGvHHY8tFMfS6bMHcZYaG9mZtOWEkuODKH24JifZI
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
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 201
ht-degree: 61%

---

# 瞭解全域 mbox

關於全域mbox的資訊，此名稱用來指稱您[!DNL Adobe Target]實作中每個網頁頂端進行的單一伺服器呼叫。

依預設，全域 mbox 的名稱為 `target-global-mbox`。 必要的話，可以為您的帳戶來重新命名它。

一般 mbox (非全域 mbox) 和全域 mbox 之間有幾項差異，包括:

| 一般 mbox | 全域 mbox |
|--- |--- |
| 一般 mbox 通常會以 `<DIV>` 標籤包住內容。 | 全域 mbox 為「空白」，不會包住任何內容。 |
| 只來自一個活動的內容可以透過一般 mbox 來傳送。 | 來自多個活動的內容可以透過傳給全域 mbox 的一個回應來傳送。 |

如果透過全域mbox或多個一般mbox傳遞多個活動，Target [會決定傳遞活動（或多個活動）至網頁的優先順序](https://experienceleague.adobe.com/docs/target/using/activities/priority.html?lang=zh-Hant)。

使用 [!DNL Target] 函式，可將其他頁面層級資料連同全域 mbox 一起傳送至 `[!UICONTROL targetPageParams]`。 這類似於 mbox 參數功能。 如需詳細資訊，請參閱[將參數傳遞至全域 mbox](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)。

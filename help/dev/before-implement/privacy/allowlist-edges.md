---
keywords: 實作，實施，白名單，白名單，允許清單，允許清單，邊緣， edges， $9
description: 檢視主機清單，協助您將 [!DNL Adobe Target] 個邊緣（地理上分散的服務節點，可確保使用者的最佳回應時間）列入允許清單。
title: 我要如何將 [!DNL Target] Edge節點加入允許清單？
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
TQID: https://experienceleague.adobe.com/-XCVJpuvQ1xV9vQBZbomDKU3F-60b5FS-LU8lIBp4GQ
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: a94ced60-8199-4549-b453-ede2acb4101e
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080bid: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 373
ht-degree: 0%

---

# 允許列出[!DNL Target]個邊緣節點

協助您將[!DNL Adobe Target]邊緣列入允許清單的資訊和最新的主機清單。

Edge是按地理區域分配服務的架構，可確保請求內容的最終使用者獲得最佳回應時間，無論他們身在何處。 每個邊緣節點都具備回應使用者內容請求及追蹤該請求之分析資料所需的所有資訊。 使用者請求會路由至最近的邊緣節點。 如需詳細資訊，請參閱[邊緣網路](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934)。

如有需要，您可以允許列出[!DNL Target]個邊緣節點。

>[!IMPORTANT]
>
>除了允許列出[!DNL Target]個邊緣和[!DNL Target]個邊緣IP位址的網路位址轉譯(NAT) IP位址，您也應該允許列出所有[!DNL Adobe Analytics]個IP位址區塊。
>
>如需詳細資訊，請參閱&#x200B;*Adobe Analytics技術說明*&#x200B;檔案中的[所有Adobe Analytics IP位址區塊](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank}。
>
>正在更新[!DNL Adobe Target]基礎結構，而想要加入允許清單位址的客戶必須使用這兩組IP。 若未這麼做，將會影響使用伺服器端或混合實作的客戶，這些實作中用於擷取體驗的Target API呼叫源自於設定為使用允許清單之防火牆後的網路內。

為確保透過[!DNL Experience Edge Connector]對[!DNL Target]的存取不中斷，客戶可以更新其網路設定，以允許流量流向Proxy服務。

## Proxy服務概述

* **服務端點**： `https://tnt-web-proxy.adobe.io`。
* **基礎架構**：裝載於[!DNL Adobe] Ethos平台。
* **注意**：此服務使用以延遲為基礎的DNS路由，不依賴靜態IP位址。

## CNAME目標

Proxy服務會使用CNAME記錄，以動態方式將流量路由至多個區域。 這些是目前的目標：

| Edge位置 | 輸出IP位址 |
| --- | --- |
| 地區 | CNAME目標 |
| 歐洲(eu-west-1) | `ethos.pub.ethos11-prod-nld2.ethos.adobe.net` |
| 美國東部(us-east-2) | `ethos.pub.ethos11-prod-va7.ethos.adobe.net` |
| 美國東部(us-east-1) | `ethos.pub.ethos11-prod-aus5.ethos.adobe.net` |

## 建議的允許清單專案

為確保連線可靠，請允許列出下列主機名稱：

* `ethos.pub.ethos11-prod-nld2.ethos.adobe.net`
* `ethos.pub.ethos11-prod-va7.ethos.adobe.net`
* `ethos.pub.ethos11-prod-aus5.ethos.adobe.net`

## 可選：IP探索

如果您的網路原則需要以IP為基礎的允許清單，您可以使用此工具檢視與Proxy服務相關聯的目前公用IP位址：

* `DNSChecker – A Record Lookup for tnt-web-proxy.adobe.io`
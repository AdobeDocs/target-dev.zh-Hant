---
keywords: 實作，實施，白名單，白名單，允許清單，允許清單，邊緣， edges， $9
description: 檢視主機清單，協助您加入允許清單 [!DNL Adobe Target] edges （地理上分散的服務節點，確保使用者的最佳回應時間）。
title: 如何加入允許清單 [!DNL Target] 邊緣節點？
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 49b6572c0d414ab304712691c97794bb0b1e3781
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# 允許清單 [!DNL Target] 邊緣節點

協助您加入允許清單的資訊和最新的主機清單 [!DNL Adobe Target] 邊緣。

Edge是按地理區域分配服務的架構，可確保請求內容的最終使用者獲得最佳回應時間，無論他們身在何處。 每個邊緣節點都具備回應使用者內容請求及追蹤該請求之分析資料所需的所有資訊。 使用者請求會路由至最近的邊緣節點。 如需詳細資訊，請參閱 [邊緣網路](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934).

您可以加入允許清單 [!DNL Target] 邊緣節點（如有需要）。

>[!IMPORTANT]
>
>除了允許列出網路位址轉譯(NAT) IP位址 [!DNL Target] 邊緣和 [!DNL Target] 文章中討論的邊緣IP位址，您也應該將所有列入允許清單 [!DNL Adobe Analytics] ip位址區塊。
>
>如需詳細資訊，請參閱 [所有Adobe Analytics IP位址區塊](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank} 在 *Adobe Analytics技術備忘稿* 檔案。
>
>[!DNL Adobe Target] 基礎結構正在更新，想要加入允許清單位址的客戶必須使用這兩組IP。 若未這麼做，將會影響使用伺服器端或混合實作的客戶，這些實作中用於擷取體驗的Target API呼叫源自於設定為使用允許清單之防火牆後的網路內。
>
>全部Edge4 *x* 下表所列地址預計於2023年8月9日更新。

## 網路位址轉譯(NAT) IP位址 [!DNL Target] 邊緣

的輸出IP位址清單 [!DNL Target] 邊緣。 如果您打算將這些IP加入允許清單 [!DNL Target] 請洽詢您的服務。

| 邊緣位置 | 輸出IP位址 |
| --- | --- |
| Edge41 （孟買） | 3.6.2.221<br />13.235.112.4 <br />52.66.66.192 |
| Edge42 （東京） | 52.69.55.232<br />43.206.61.43 <br />13.113.73.214 |
| Edge44 （美國東岸） | 54.164.192.223<br />52.86.86.203 <br />54.88.167.98 |
| Edge45 （美國西海岸） | 52.40.124.129<br />54.148.219.69 <br />54.189.208.212 |
| Edge46 （雪梨） | 54.253.144.4<br />54.66.198.142 <br />13.211.218.51 |
| Edge47 （愛爾蘭） | 52.208.136.136<br />54.170.28.19 <br />99.80.111.82 |
| Edge48 （新加坡） | 3.1.141.36<br />18.143.112.116 <br />52.76.61.44 |

## [!DNL Target] 邊緣IP位址

的IP位址清單 [!DNL Target] 邊緣。 如果您想要對進行API呼叫，請將這些IP加入允許清單 [!DNL Target] 邊緣。

此清單會經常變更，因為負載平衡器會根據流量設定檔進行放大和縮小。

| 邊緣位置 | 網域 | IP 位址 |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br />(其中CLIENTCODE是您的 [!DNL Target] 使用者端識別碼) |  |
| Edge41 （孟買） | `mboxedge41.tt.omtrdc.net` | 15.206.104.6<br />3.109.14.178 <br />13.234.139.131 |
| Edge42 （東京） | `mboxedge42.tt.omtrdc.net` | 52.194.84.34<br />3.115.158.39 <br />18.180.123.21 |
| Edge44 （美國東岸） | `mboxedge44.tt.omtrdc.net` | 54.205.210.54<br />23.20.189.8 <br />35.169.173.155 |
| Edge45 （美國西海岸） | `mboxedge45.tt.omtrdc.net` | 35.161.163.45<br />44.230.114.101 <br />35.161.120.22 |
| Edge46 （雪梨） | `mboxedge46.tt.omtrdc.net` | 3.104.142.61<br />52.62.4.152 <br />54.253.105.140 |
| Edge47 （愛爾蘭） | `mboxedge47.tt.omtrdc.net` | 18.203.168.186<br />54.228.83.91 <br />54.217.181.83 |
| Edge48 （新加坡） | `mboxedge48.tt.omtrdc.net` | 54.179.6.70<br />13.215.150.94 <br />18.136.47.70 |

當負載平衡器偵測到流量設定檔中的變更時，它會放大或縮小。 根據偵測到的變更，Elastic Load Balancing進行縮放所需的時間範圍為1到7分鐘。 當負載平衡器縮放時，會以新的IP位址清單來更新DNS記錄。 為確保您能利用增加的容量，Elastic Load Balancing會在60秒的DNS記錄上使用TTL設定。

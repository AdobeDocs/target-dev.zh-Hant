---
keywords: 實作，實施，白名單，白名單，允許清單，允許清單，邊緣， edges， $9
description: 檢視主機清單，協助您將 [!DNL Adobe Target] 個邊緣（地理上分散的服務節點，可確保使用者的最佳回應時間）列入允許清單。
title: 我要如何將 [!DNL Target] Edge節點加入允許清單？
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 1583cfe8ea1009fd1df5c5a0e5f3f95f72daf2b9
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# 允許列出[!DNL Target]個邊緣節點

協助您將[!DNL Adobe Target]邊緣列入允許清單的資訊和最新的主機清單。

Edge是按地理區域分配服務的架構，可確保請求內容的最終使用者獲得最佳回應時間，無論他們身在何處。 每個邊緣節點都具備回應使用者內容請求及追蹤該請求之分析資料所需的所有資訊。 使用者請求會路由至最近的邊緣節點。 如需詳細資訊，請參閱[邊緣網路](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html?lang=zh-Hant#concept_0AE2ED8E9DE64288A8B30FCBF1040934)。

如有需要，您可以允許列出[!DNL Target]個邊緣節點。

>[!IMPORTANT]
>
>除了允許列出[!DNL Target]個邊緣和[!DNL Target]個邊緣IP位址的網路位址轉譯(NAT) IP位址，您也應該允許列出所有[!DNL Adobe Analytics]個IP位址區塊。
>
>如需詳細資訊，請參閱&#x200B;*Adobe Analytics技術說明*&#x200B;檔案中的[所有Adobe Analytics IP位址區塊](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=zh-Hant#all-adobe-analytics-ip-address-blocks){target=_blank}。
>
>正在更新[!DNL Adobe Target]基礎結構，而想要加入允許清單位址的客戶必須使用這兩組IP。 若未這麼做，將會影響使用伺服器端或混合實作的客戶，這些實作中用於擷取體驗的Target API呼叫源自於設定為使用允許清單之防火牆後的網路內。
>
>下列兩個表格中所列的所有Edge4 *x*&#x200B;位址都排程在2023年8月9日更新。

## [!DNL Target]個邊緣的網路位址轉譯(NAT) IP位址

[!DNL Target]個邊緣的輸出IP位址清單。 如果您打算讓[!DNL Target]聯絡您的服務，請將這些IP加入允許清單。

| Edge位置 | 輸出IP位址 |
| --- | --- |
| Edge41 （孟買） | 3.6.2.221<P>13.235.112.4 <P>52.66.66.192 |
| Edge42 （東京） | 52.69.55.232<P>43.206.61.43 <P>13.113.73.214 |
| Edge44 （美國東岸） | 54.164.192.223<P>52.86.86.203 <P>54.88.167.98 |
| Edge45 （美國西海岸） | 52.40.124.129<P>54.148.219.69 <P>54.189.208.212 |
| Edge46 （雪梨） | 54.253.144.4<P>54.66.198.142 <P>13.211.218.51 |
| Edge47 （愛爾蘭） | 52.208.136.136<P>54.170.28.19 <P>99.80.111.82 |
| Edge48 （新加坡） | 3.1.141.36<P>18.143.112.116 <P>52.76.61.44 |

## [!DNL Target]個邊緣IP位址

[!DNL Target]個邊緣的IP位址清單。 如果您想要對[!DNL Target]邊緣進行API呼叫，請將這些IP加入允許清單。

此清單會經常變更，因為負載平衡器會根據流量設定檔進行放大和縮小。

| Edge位置 | 網域 | IP 位址 |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br /> （其中CLIENTCODE是您的[!DNL Target]使用者端ID） |  |
| Edge41 （孟買） | `mboxedge41.tt.omtrdc.net` | 3.6.2.221<P>52.66.66.192<P>13.235.112.4 |
| Edge42 （東京） | `mboxedge42.tt.omtrdc.net` | 43.206.61.43<P>13.113.73.214<P>52.69.55.232 |
| Edge44 （美國東岸） | `mboxedge44.tt.omtrdc.net` | 54.88.167.98<P>54.164.192.223<P>52.86.86.203 |
| Edge45 （美國西海岸） | `mboxedge45.tt.omtrdc.net` | 52.40.124.129<P>54.148.219.69<P>54.189.208.212 |
| Edge46 （雪梨） | `mboxedge46.tt.omtrdc.net` | 54.66.198.142<P>54.253.144.4<P>13.211.218.51 |
| Edge47 （愛爾蘭） | `mboxedge47.tt.omtrdc.net` | 54.170.28.19<P>52.208.136.136<P>99.80.111.82 |
| Edge48 （新加坡） | `mboxedge48.tt.omtrdc.net` | 52.76.61.44<P>3.1.141.36<P>18.143.112.116 |

當負載平衡器偵測到流量設定檔中的變更時，它會放大或縮小。 根據偵測到的變更，Elastic Load Balancing進行縮放所需的時間範圍為1到7分鐘。 當負載平衡器縮放時，會以新的IP位址清單來更新DNS記錄。 為確保您能利用增加的容量，Elastic Load Balancing會在60秒的DNS記錄上使用TTL設定。

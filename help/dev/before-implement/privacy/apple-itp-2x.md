---
keywords: apple， ITP，智慧型追蹤預防， experience cloud id， ecid， itp
description: 瞭解 [!DNL Adobe Target] 以及旨在保護Safari使用者隱私的Apple智慧型追蹤預防(ITP)計畫的影響。
title: 如何 [!DNL Target] 處理Apple ITP支援？
feature: Privacy & Security
exl-id: 6deee03b-df86-4d0d-999c-b11855ddfda5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 32%

---

# Apple 智慧型追蹤預防 (ITP) 2.x

智慧型追蹤預防(ITP)是Apple保護Safari使用者隱私權的計畫。 ITP 是在 2017 年首次發行，將目標鎖定於協力廠商 Cookie 的使用方式。事實上，Apple 完全封鎖了協力廠商 Cookie，進而對廣告技術和行銷技術公司造成了嚴重的問題，因為協力廠商 Cookie 通常是用來追蹤訪客及收集訪客資料。現在，Apple 正在努力朝向針對 Safari 內第一方 Cookie 的使用方式設定限制。

這些 ITP 版本包含下列限制:

| 版本 | 詳細資料 |
| --- | --- |
| [ITP 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/) | 針對使用 `document.cookie` API 放在瀏覽器上的用戶端 Cookie 設定七天過期的上限。<br />發行日期: 2019 年 2 月 21 日。 |
| [ITP 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/) | 將七天過期的上限大幅縮短為一天。<br />發行日期: 2019 年 4 月 24 日。 |
| [ITP 2.3](https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/) | 省略了數個因應措施，例如採用localStorage或使用JavaScript `Document.referrer property`.<br />發行日期： 2019年9月23日。<br />ITP的CNAME遮蔽防禦功能已在Safari 14、macOS Big Sur、Catalina、Mojave、iOS 14和iPadOS 14中發行。 由第三方CNAME遮罩的HTTP回應建立的所有Cookie都會設定為在7天後過期。<br />2020年11月12日宣佈。 |

## 這對我as a有何影響 [!DNL Target] 客戶？

Target提供JavaScript程式庫讓您在頁面上部署，以便 [!DNL Target] 可為訪客提供即時個人化內容。 共有三種 [!DNL Target] JavaScript資料庫at.js 1.*x*， at.js 2.*x*，則 [!DNL Adobe Experience Cloud Web SDK] 可放置使用者端 [!DNL Target] 訪客瀏覽器上的Cookie，透過 `document.cookie` API。 因此， [!DNL Target] Cookie會受到Apple ITP 2.1、2.2和2.3的影響，且將在七天後過期（若是ITP 2.1）以及在一天後過期（若是ITP 2.2和ITP 2.3）。

Apple ITP 2.x影響 [!DNL Target] 區域內：

| 影響 | 詳細資料 |
| --- | --- |
| 獨特訪客計數可能會增加 | 由於過期時間設為七天（若是ITP 2.1）和一天（若是ITP 2.2和ITP 2.3），您可能會看到來自Safari瀏覽器的不重複訪客增加。 如果訪客在七天後(ITP 2.1)或一天後（ITP 2.2和ITP 2.3）重新造訪網域， [!DNL Target] 強制置入新的 [!DNL Target] 網域上的Cookie來取代過期的Cookie。 即使是相同的使用者，新的 [!DNL Target] Cookie 會將其轉譯為新的獨特訪客。 |
| 縮短 [!DNL Target] 活動的回顧期 | [!DNL Target] 活動的訪客設定檔可能已針對決策功能縮短回顧期間。系統會運用 [!DNL Target] Cookie 來識別訪客，並針對個人化儲存使用者設定檔屬性。有鑑於此 [!DNL Target] Safari中的Cookie可在七天(ITP 2.1)或一天（ITP 2.2和2.3）後過期，此為繫結至已清除的使用者設定檔資料 [!DNL Target] Cookie無法用於決策。 |
| 基於 3rdPartyID 的設定檔指令碼 | 由於過期時間設為七天（若是ITP 2.1）和一天（若是ITP 2.2和ITP 2.3）， [設定檔指令碼](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html) 根據3rdPartyID Cookie將在過期時停止運作。 |
| iOS 裝置內的 QA/Preview URL | 由於過期時間設為七天（若是ITP 2.1）和一天（若是ITP 2.2和ITP 2.3）， [QA/預覽URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) 過期時將會停止運作，因為URL是以3rdPartyID Cookie為基礎。 |

## 我目前的 [!DNL Target] 實施是否會受到影響?

Experience Cloud如果您除了使用 [!DNL Target] JavaScript程式庫時，您的實施會以本文列出的方式受到影響： [Safari ITP 2.1對Adobe Experience Cloud和Experience Platform客戶的影響](https://medium.com/adobetech/safari-itp-2-1-impact-on-adobe-experience-cloud-customers-9439cecb55ac).

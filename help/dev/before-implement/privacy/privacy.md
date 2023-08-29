---
keywords: 隱私， ip位址，地域劃分，選擇退出，選擇退出，資料隱私權，政府法規，法規， gdpr， ccpa，隱私，個人識別資訊， PII
description: 瞭解如何 [!DNL Adobe Target] 遵守適用的資料隱私權法律，包括收集和處理IP位址、PII和選擇退出指示。
title: Target如何處理隱私權問題，包括PII？
feature: Privacy & Security
exl-id: 4330e034-2483-4a25-9c87-48dbef6fc9de
source-git-commit: d9ac5bab3a09cf49b2178a62c06eebe733b9048d
workflow-type: tm+mt
source-wordcount: '865'
ht-degree: 39%

---

# 隱私權

[!DNL Adobe Target] 已啟用程序和設定，好讓您在遵守適用的資料隱私法的情況下使用 [!DNL Target]。

## 收集IP位址和個人識別資訊(PII)

您網站訪客的 IP 位址將傳輸到 Adobe 資料處理中心 (DPC)。根據訪客的網路設定，IP位址不一定代表訪客電腦的IP位址。 例如，該 IP 位址可能是網路位址轉譯 (NAT) 防火牆、HTTP Proxy 或內部閘道的外部 IP 位址。

>[!IMPORTANT]
>
>[!DNL Target] 不會儲存使用者的任何IP位址或任何個人識別資訊(PII)。 IP位址僅由以下人員使用： [!DNL Target] 工作階段期間（記憶體中，永不儲存）。

## 取代 IP 位址的最後一個八位元

Adobe已開發「設計的隱私權」設定，使用者可將其啟用以進行Adobe [!DNL Target]. 啟用時，Adobe [!DNL Target] 在收集IP位址時，立即模糊化IP位址的最後八位元（最後一部分）。 在對 IP 位址進行任何處理前 (包括選用的 IP 位址地理查閱)，就會執行這種匿名化作業。

啟用此功能時，IP 位址的匿名性已足夠，不再能識別為個人資訊。因此， [!DNL Target] 若相關國家不允許收集個人資訊，可用於符合資料隱私權法規的情況。 IP 位址模糊化可能會顯著影響城市層級資訊的取得。地區和國家層級資訊則只會受到輕微影響。

下列設定可在 [!DNL Target] UI，瀏覽至 **[!UICONTROL 管理]** > **[!UICONTROL 實施]**：

* [!UICONTROL 最後一個八位元模糊化]： [!DNL Target] 隱藏IP位址的最後八位元。
* [!UICONTROL 整個IP模糊化]： [!DNL Target] 隱藏整個IP位址。
* [!UICONTROL 無]： [!DNL Target] 不會隱藏IP位址的任何部分。

  ![obfuscate-ip-options](assets/obfuscate-ip.png)

[!DNL Target] 會收到完整IP位址並根據指定加以模糊化（如果設定為最後一個八位元或整個IP）。 [!DNL Target] 然後只會在目前工作階段期間將模糊化的IP位址保留在記憶體中。

### 使用時資料流層級IP模糊化 [!DNL Adobe Experience Platform Web SDK]

使用時 [!DNL Platform Web SDK] （23.4版或更新版本），資料流層級IP模糊化設定的優先順序高於中設定的任何IP模糊化選項。 [!DNL Target]. 例如，如果資料流層級IP模糊化選項設為 [!UICONTROL 完整] 和 [!DNL Target] IP模糊化選項設為 [!UICONTROL 最後一個八位元模糊化]， [!DNL Target] 接收完全模糊化的IP。 因為中的IP模糊化 [!DNL Target] 發生在地理位置查詢之前，資料流層級IP模糊化設定沒有影響。

在資料流層級設定IP模糊化後，您的資料透過Edge Network，請求會進入 [!DNL Target] 和 [!DNL Adobe Audience Manager] (AAM)僅包含模糊化的IP，而任何以使用者端IP為基礎的邏輯都會受到資料流層級IP模糊化選項的影響。 中設定的任何IP模糊化 [!DNL Target] 或AAM套用至已模糊化的IP。

如需詳細資訊，請參閱 [!UICONTROL IP模糊化] 在 [設定資料串流](https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html){target=_blank} 在 *[!DNL Adobe Experience Platfrom]Datastreams指南*.

## 地域劃分

如果您啟用取代IP位址的最後八位元，則可以使用中的報告分析IP位址的其餘值 [!DNL Target]. 如果IP位址的最後八位元尚未模糊化，則可以在中分析完整的IP位址 [!DNL Target]. 您可以使用 GeoSegmentation 功能，依地理區域來安排訪客位置。地域劃分資料的詳細程度只到城市層級或郵遞區號層級，不到個人層級。

如果 IP 位址完全模糊，則無法使用 GeoSegmentation 和地理位置定位。

## 選擇退出連結

您可以在您的網站中新增選擇退出連結，讓訪客能夠選擇退出所有 計數及內容傳遞。

1. 將下列連結新增至您的網站:

   `<a href="https://clientcode.tt.omtrdc.net/optout"> Your Opt Out Language Here</a>`

1. （視條件而定）如果您正在使用CNAME，連結應該包含「client=`clientcode` 引數，例如：
   `https://my.cname.domain/optout?client=clientcode`。

1. 將 `clientcode` 替換為您的用戶端代碼，然後新增要連結至選擇退出 URL 的文字或影像。

訪客只要按一下此連結，就不會包含在任何從該訪客的瀏覽作業呼叫的 mbox 中，直到訪客刪除本身的 Cookie 或事隔滿兩年 (以先發生者為準) 為止。其原理是在 `disableClient` 中為訪客設定一個名稱為 `clientcode.tt.omtrdc.net` 的 Cookie。

即使您使用第一方 Cookie 實作，所提供的選擇退出還是透過第三方 Cookie 來設定。如果使用者端僅使用第一方Cookie， [!DNL Target] 檢查是否已設定選擇退出Cookie。

## 隱私權與資料保護規範

另請參閱 [隱私權與資料保護規範](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) 有關歐盟一般資料保護規範(GDPR)、加州消費者隱私保護法(CCPA)和其他國際隱私規定的資訊，以及這些規定對您的組織和 [!DNL Target].

## 功能使用情況資料的收集

收集個別功能使用情況資料以供內部Adobe使用，以識別是否 [!DNL Target] 功能正在按預期執行，或用於識別未被充分利用的功能。 收集各種延遲測量結果來協助解決效能問題。 不會收集個人資料。

您可以在用戶端初始化選項中將 `telemetryEnabled` 設定為 false 來選擇退出我們 SDK 中的使用資料報告。 如需詳細資訊，請參閱 [targetGlobalSettings 中的 telemetryEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled)。

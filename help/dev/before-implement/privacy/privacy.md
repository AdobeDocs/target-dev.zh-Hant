---
keywords: 隱私， ip位址，地域劃分，選擇退出，選擇退出，資料隱私權，政府法規，法規， gdpr， ccpa，隱私，個人識別資訊， PII
description: 瞭解 [!DNL Adobe Target] 如何遵守適用的資料隱私法，包括IP位址、PII和選擇退出指示的收集和處理。
title: Target如何處理隱私權問題，包括PII？
feature: Privacy & Security
exl-id: 4330e034-2483-4a25-9c87-48dbef6fc9de
source-git-commit: 88bde40aa6dfb96e1d53e4db6ba5547d38dbbb99
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 43%

---

# 隱私權

[!DNL Adobe Target] 已啟用程序和設定，好讓您在遵守適用的資料隱私法的情況下使用 [!DNL Target]。

## 收集IP位址和個人識別資訊(PII)

您網站訪客的 IP 位址將傳輸到 Adobe 資料處理中心 (DPC)。根據訪客的網路設定，IP位址不一定代表訪客電腦的IP位址。 例如，該 IP 位址可能是網路位址轉譯 (NAT) 防火牆、HTTP Proxy 或內部閘道的外部 IP 位址。

>[!IMPORTANT]
>
>[!DNL Target]不會儲存使用者的任何IP位址或任何個人識別資訊(PII)。 IP位址僅由[!DNL Target]在工作階段期間使用（記憶體中，永不保留）。

## 取代 IP 位址的最後一個八位元

Adobe已開發使用者可為Adobe[!DNL Target]啟用的「設計隱私權」設定。 啟用時，Adobe[!DNL Target]會立即在收集IP位址時模糊化IP位址的最後八位元（最後一部分）。 在對 IP 位址進行任何處理前 (包括選用的 IP 位址地理查閱)，就會執行這種匿名化作業。

啟用此功能時，IP 位址的匿名性已足夠，不再能識別為個人資訊。因此，在不允許收集個人資訊的國家/地區，[!DNL Target]可用於遵守資料隱私權法律。 IP 位址模糊化可能會顯著影響城市層級資訊的取得。地區和國家層級資訊則只會受到輕微影響。

導覽至&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]**，即可在[!DNL Target] UI中使用下列設定：

* [!UICONTROL Last octet obfuscation]： [!DNL Target]會隱藏IP位址的最後一個八位元。
* [!UICONTROL Entire IP obfuscation]： [!DNL Target]會隱藏整個IP位址。
* [!UICONTROL None]： [!DNL Target]不會隱藏IP位址的任何部分。

  ![obfuscate-ip-options](assets/obfuscate-ip.png)

[!DNL Target]會收到完整IP位址並根據指定加以模糊化（如果設定為最後一個八位元或整個IP）。 然後[!DNL Target]只會在目前工作階段期間將模糊化的IP位址保留在記憶體中。

### 使用[!DNL Adobe Experience Platform Web SDK]時資料流層級IP模糊化 {#aep}

使用[!DNL Platform Web SDK] （23.4版或更新版本）時，資料流層級IP模糊化設定的優先順序高於[!DNL Target]中設定的任何IP模糊化選項。 例如，如果資料流層級IP模糊化選項設為[!UICONTROL Full]，而[!DNL Target] IP模糊化選項設為[!UICONTROL Last octet obfuscation]，則[!DNL Target]會收到完全模糊化的IP。

如需詳細資訊，請參閱&#x200B;*[!DNL Adobe Experience Platfrom]資料串流指南*&#x200B;中[設定資料串流](https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=zh-Hant){target=_blank}中的[!UICONTROL IP Obfuscation]。

## 地域劃分

如果啟用取代IP位址的最後八位元，可以使用[!DNL Target]中的報告分析IP位址的其餘值。 如果IP位址的最後八位元尚未模糊化，則可在[!DNL Target]中分析完整IP位址。 您可以使用 GeoSegmentation 功能，依地理區域來安排訪客位置。地域劃分資料的詳細程度只到城市層級或郵遞區號層級，不到個人層級。

如果 IP 位址完全模糊，則無法使用 GeoSegmentation 和地理位置定位。

## 選擇退出連結

您可以在您的網站中新增選擇退出連結，讓訪客能夠選擇退出所有 計數及內容傳遞。

1. 將下列連結新增至您的網站:

   `<a href="https://clientcode.tt.omtrdc.net/optout"> Your Opt Out Language Here</a>`

1. （視條件而定）如果您正在使用CNAME，連結應該包含「client=`clientcode`引數，例如：
   `https://my.cname.domain/optout?client=clientcode`。

1. 將 `clientcode` 替換為您的用戶端代碼，然後新增要連結至選擇退出 URL 的文字或影像。

訪客只要按一下此連結，就不會包含在任何從該訪客的瀏覽作業呼叫的 mbox 中，直到訪客刪除本身的 Cookie 或事隔滿兩年 (以先發生者為準) 為止。其原理是在 `disableClient` 中為訪客設定一個名稱為 `clientcode.tt.omtrdc.net` 的 Cookie。

即使您使用第一方 Cookie 實作，所提供的選擇退出還是透過第三方 Cookie 來設定。如果使用者端僅使用第一方Cookie，[!DNL Target]會檢查是否設定了選擇退出Cookie。

## 隱私權與資料保護規範

請參閱[隱私權與資料保護規範](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)，以取得有關歐盟一般資料保護規範(GDPR)、加州消費者隱私保護法(CCPA)及其他國際隱私規定的資訊，以及這些規定對貴組織和[!DNL Target]的影響。

## 功能使用情況資料的收集

收集個別功能使用情況資料以供內部Adobe之用，以識別[!DNL Target]功能是否如預期執行，或識別未被充分利用的功能。 收集各種延遲測量結果來協助解決效能問題。 不會收集個人資料。

您可以在用戶端初始化選項中將 `telemetryEnabled` 設定為 false 來選擇退出我們 SDK 中的使用資料報告。 如需詳細資訊，請參閱 [targetGlobalSettings 中的 telemetryEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled)。

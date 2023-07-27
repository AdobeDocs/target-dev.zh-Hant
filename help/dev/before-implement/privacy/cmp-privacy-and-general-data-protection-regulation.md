---
keywords: gdpr， eu，歐盟，隱私權，常見問題集，常見問題集，加州消費者隱私保護法， ccpa，隱私權，資料保護，選擇退出，選擇退出，政府，法規， gdpr5， gdpr6， gdpr7， gdpr8， gdpr9， eu0， eu1， eu2， eu3， eu4， eu5
description: 瞭解Target和歐盟一般資料保護規範(GDPR)、加州消費者隱私保護法(CCPA)及其他隱私規定。
title: Target如何處理隱私權與資料保護規範？
feature: Privacy & Security
exl-id: 40bac3c5-8e6f-4a90-ac0c-eddce1dbe6c0
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '2374'
ht-degree: 64%

---

# 隱私權與資料保護規範

有關歐盟一般資料保護規範 (GDPR)、加州消費者隱私保護法 (CCPA) 及其他國際隱私規定的資訊。 瞭解這些規範如何影響您的組織和Adobe Target。

## 隱私權與一般資料保護規範 (GDPR) 總覽

自 2018 年 5 月 25 日起，歐盟的 GDPR 已正式生效。 如需有關此規範對您有何意義的詳細資訊，請參閱 [GDPR 和您的企業](https://business.adobe.com/privacy/general-data-protection-regulation.html)。

當 Adobe 提供軟體和服務給企業時，Adobe 為了提供這些服務，會以資料處理者的角色處理和儲存任何個人資料。身為資料處理者，Adobe 會根據貴公司的權限和指示 (例如，依照您與 Adobe 的合約規定) 處理個人資料。

身為資料控管者，您可以決定 Adobe 代表您處理和儲存的個人資料。如果使用 Adobe Experience Cloud 解決方案，則 Adobe 可能會根據您使用的解決方案，以及您選擇傳送到 Adobe Experience Cloud 帳戶的資訊，為您託管個人資料。如需範例的詳細清單，請參閱 [Adobe Experience Cloud 隱私權](https://www.adobe.com/privacy/experience-cloud.html#collect)。

Adobe Experience Cloud為資料控管者提供GDPR完備的API，可讓資料控管者完成下列工作：

* 存取儲存在 Target 中的資料主體資訊
* 刪除儲存在 Target 中的資料主體資訊

如需詳細資訊，請參閱：

* [AdobePrivacy Service概觀](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html)
* [Privacy Service API指南](https://experienceleague.adobe.com/docs/experience-platform/privacy/api/overview.html)
* [Privacy Service UI總覽](https://experienceleague.adobe.com/docs/experience-platform/privacy/ui/overview.html)

## 加州消費者隱私保護法 (CCPA) 總覽

加州消費者隱私保護法 (CCPA) 為加州消費者提供關於其個人資訊的新權利，並對在加州執行業務的某些實體施加資料保護責任。CCPA 已於 2020 年 1 月 1 日生效。

整體來說，此法律賦予加州人多項重要權利，包括下列權利:

* 要求資訊 (資料存取)
* 選擇退出個人資訊銷售 (這是一項定義非常廣泛的權利，可選擇退出與第三方分享資訊)
* 要求刪除個人資訊
* 在個人資訊已公開或銷售時收到通知

如果您去年忙著為歐洲的隱私權法律(GDPR)做好準備，您可能很熟悉這裡的其中一些權利，您已完成的許多工作可以另作他用。

>[!NOTE]
>
>存取和刪除適用於 CCPA 的資料會遵循與 GDPR 相同的流程。

## Adobe Target和Adobe Experience Platform選擇加入

Target透過Adobe Experience Platform中的標籤支援選擇加入功能，可協助支援您的同意管理策略。 選擇加入功能可讓客戶控制觸發 Target 標記的方法和時機。也可選擇透過Adobe Experience Platform預先核准Target標籤。 若要啟用在Target at.js資料庫中使用選擇加入的功能，您應使用 `targetGlobalSettings` 並新增 `optinEnabled=true` 設定。 在Adobe Experience Platform中，以擴充功能安裝檢視，從GDPR選擇加入下拉式清單中選取「啟用」。 另請參閱 [使用Adobe Experience Platform實作Target](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 以取得更多詳細資料。

下列程式碼片段會向您示範如何啟用 `optinEnabled=true` 設定:

```
window.targetGlobalSettings = {
  optinEnabled: true
};
```

>[!NOTE]
>
>at.js 1.7.0 版和 at.js 2.1.0 或更新版本可支援選擇加入功能。at.js 2.0.0 版和 2.0.1 版不支援選擇加入。

建議使用Adobe Experience Platform管理選擇加入。 Adobe Experience Platform中有更精細的控制功能，可在Target引發前隱藏選取的頁面元素，可能利於用於知情同意策略的一部分。

使用「選擇加入」時，該考慮三種情況:

1. **Target標籤會透過Adobe Experience Platform預先核准（或資料主體先前核准的Target）：** Target標籤不會為同意保留，並會如預期運作。
1. **Target 標記「不會」預先核准且 `bodyHidingEnabled` 為「FALSE」:** Target 標記僅在向客戶取得同意後觸發。取得同意前，僅可使用預設內容。收到同意後，系統會呼叫 Target，資料主體 (訪客) 即可使用個人化內容。由於知情同意前只能使用預設內容，因此使用適當的策略很重要，例如蓋住頁面任何部分的啟動顯示畫面或可個人化的內容。 此程序可確保資料主體 (訪客) 的體驗保持一致。
1. **Target 標記「不會」預先核准且 `bodyHidingEnabled` 為「TRUE」:** Target 標記僅在向客戶取得同意後觸發。取得同意前，僅可使用預設內容。但由於 `bodyHidingEnabled` 設為 True，`bodyHiddenStyle` 會指定在觸發 Target 標記前隱藏的頁面內容 (或資料主體拒絕選擇加入，而顯示預設內容)。根據預設，`bodyHiddenStyle` 設定為 `body { opacity:0;}`，這會隱藏 HTML 內文標記。 Adobe 建議的頁面設定如下，以便將頁面內容放入一個容器，並將知情同意管理程式對話框放入另一個容器，即可隱藏頁面的整個內文，而不是知情同意管理程式對話框。 這項設定會將 Target 設定為只隱藏頁面內容容器。請參閱 [Privacy Service 總覽](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?)。

   情況 3 的建議頁面設定如下:

   ```
   <html> 
   <head> 
   //visitor, at.js 
   </head> 
   
   <body> 
   <div id = "consentManagerDialog"> 
   
   //consent manager html dialog goes here 
   </div> 
   
   <div id="pageContent"> 
   // page content goes here 
   </div> 
   
   </body> 
   </html> 
   ```

   假設 `bodyHiddenStyle` 如下:

   ```
   #pageContent { opacity:0;}
   ```

## 隱私權與資料保護規範常見問題集

歐盟一般資料保護規範 (GDPR)、加州消費者隱私保護法 (CCPA) 及 Target 專用之其他國際隱私權要求的相關常見問題集。

### 針對這些規範採取什麼的Adobe政策？

身為資料處理者，Adobe已符合相關規範或致力於履行我們的義務。 Adobe 在設計上擁有經過認證的安全性與隱私權控制的堅實基礎，並已於 2018 年 5 月的截止日期前對產品做了強化。 企業客戶有責任實作這些增強功能，並更新任何必要的政策和程序。

### 我們公司身為資料控管者，必須對每個使用的Adobe Experience Cloud解決方案提交GDPR或CCPA要求嗎？

不需要，Adobe會以集中的方式協助資料控管者符合其GDPR和CCPA規定。 資料控管者無需直接處理每個解決方案。

Experience Cloud解決方案（包括Target）的所有GDPR和CCPA要求，都是透過目前稱為GDPR API的中央AdobeAPI來執行。 此API接著會完成資料控管者的Experience Cloud解決方案套件上的要求。

### Adobe會讓客戶刪除哪些資訊來回應資料主體/使用者要求？

有關 Target 中的個別訪客資訊會包含在 Target 訪客設定檔中。Target可讓客戶刪除與其訪客設定檔中之ID相關的所有資料。 如需Target儲存的設定檔資料範例，請參閱 [訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html).

不會識別特定個人的彙總或匿名資料 (例如報告資料)，或與特定個人不相關的資料 (例如內容資料)，則不在使用者刪除要求的範圍之內。

長達 90 天未使用的 Target 訪客設定檔會依預設刪除，您無需採取任何動作。

### 哪些ID受到支援，並且可協助客戶完成針對Target的GDPR或CCPA存取及刪除要求？

Target 支援下列 ID 類型，以便找出客戶設定檔:

| 使用者 ID | 命名空間 ID 類型 | 命名空間 ID | 定義 |
|--- |--- |--- |--- |
| Experience Cloud ID (ECID) | Standard | 4 | Adobe Experience Cloud ID，先前稱為訪客 ID 或 Experience Cloud ID。 您可以使用 JavaScript API 找到此 ID (請參閱下方的詳細資料)。 |
| TnT ID/Cookie ID(TNTID) | Standard | 9 | 在訪客的瀏覽器中設為Cookie的目標識別碼。 您可以使用 JavaScript API 找到此 ID (請參閱下方的詳細資料)。 |
| 第三方 ID/CRM ID(THIRDPARTYID) | Target 專用 | 不適用 | 此情況為您向 Target 提供您的 CRM 或客戶的其他唯一識別碼資訊。 |

>[!NOTE]
>
>雖然Target對第一方和第三方跨網域Cookie均有支援，但只建議將第一方Target Cookie用於GDPR和CCPA。

###  Target 如何處理同意管理?

GDPR 和 CCPA 不會改變您必須取得同意的時間，而是改變您取得同意的方式。 每個客戶的知情同意策略會依據其資料收集和使用做法以及其隱私權政策而定。 不支援GDPR和CCPA的同意管理，也不應透過Target達成。

Adobe 目前並未提供同意管理解決方案，但市場中已開發各種工具，可有效應付新需求中的部分內容。如需一般隱私工具的詳細資訊，包括知情同意管理程式，請參閱&#x200B;*國際隱私權專家協會 (iaap)* 網站上的 [2017 年隱私技術廠商報告](https://iapp.org/media/pdf/resource_center/Tech-Vendor-Directory-1.4.1-electronic.pdf)。

Target透過Adobe Experience Platform支援選擇加入功能，可支援您的同意管理策略。 選擇加入功能可讓客戶控制觸發 Target 標記的方法和時機。也可選擇透過Adobe Experience Platform預先核准Target標籤。 建議使用Adobe Experience Platform管理選擇加入。 Adobe Experience Platform中有更精細的控制功能，可在Target引發前隱藏選取的頁面元素，可能利於用於知情同意策略的一部分。

如需有關GDPR、CCPA和Adobe Experience Platform的詳細資訊，請參閱 [Adobe隱私權JavaScript程式庫和GDPR](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?). 另請參閱上面的 *Adobe Target 和 Adobe Experience Platform 選擇加入*&#x200B;一節。

### `AdobePrivacy.js` 會將資訊提交至 GDPR API 嗎？

AdobePrivacy.js *不會*&#x200B;將此資訊提交至 API。此動作必須由客戶來執行。本程式庫僅會提供儲存在該特定訪客所使用之瀏覽器中的 ID。

### `removeIdentities` 會移除哪些內容？

`removeIdentities`*僅會*&#x200B;從瀏覽器移除身分資料，而此動作完全取決於 Adobe 解決方案是否已實作此程式碼。

例如，Target會刪除儲存其ID的Cookie，但Adobe Audience Manager (AAM)不會刪除儲存於第三方Cookie中的Demdex ID。

### Target GDPR或CCPA要求中必須包含哪些資訊？

除了中央Privacy Service的需求，Target接受的有效GDPR或CCPA訊息包含：

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            "namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            "namespace":"TNTID", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"THIRDPARTYID", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
}
```

### 透過 GDPR API，我應該可以得到哪些回應類型?  

| 要求狀態 | Target 回應訊息 | 藍本 |
|--- |--- |--- |
| 正在處理 | 正在處理 | Target 已收到 GDPR 或 CCPA 要求，且正在處理中。 |
| 完成 | 不適用 - 公司環境不適用 | GDPR 或 CCPA 要求中的 IMS ID 未對應到任何 Target 用戶端。<br />某些公司有多個 IMS ID。 在布建Target的地方提交IMS ID。 |
| 完成 | 不適用 - 使用者環境不適用 | Target 設定檔存放區中沒有在 GDPR 或 CCPA 要求中提供的特定訪客或資料主體的 ID。<br />如果您嘗試提交Target不支援的名稱空間ID型別（請參閱上面所述的支援的ID），也會傳回這樣的結果。 |
| 錯誤 | 錯誤訊息 (詳細資訊會視錯誤類型而定) | 擷取或刪除要求的資料主體個人資料時發生錯誤。<br />因應存取要求上傳至 Azure 時發生錯誤。 |

### Target 針對存取要求傳送至 GDPR API 的回應是什麼?

存取資料要求的回應包含特定訪客的 Target 設定檔摘要。此傳回內容會傳送至Experience CloudGDPR API，接著傳送回應給資料控管者。

Target 存取 API 回應的範例看起來可能會像這樣:

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            ~"namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            ~"namespace":"tntId", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"thirdPartyId", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
} 
```

| 欄位 | 說明 |
|--- |--- |
| jobId | 表示中央 GDPR API 中的 GDPR 或 CCPA 工作 ID。 |
| imsOrgID | 為您的公司提供唯一識別碼。 |
| namespace | 亦稱為資料來源。請參閱「哪些 ID 受到支援，並且可協助客戶完成針對 Target 的 GDPR 或 CCPA 存取及刪除要求?」GDPR 存取及刪除要求?」 |
| type | 您要求 GDPR 或 CCPA 資料存取權限的 ID 類型。Target接受多種ID型別，部分為標準型，有些則為Target專用型。 請參閱「哪些 ID 受到支援，並且可協助客戶完成針對 Target 的 GDPR 或 CCPA 存取及刪除要求?」GDPR 存取及刪除要求?」 |
| value | namespace/資料來源的 ID。請參閱「哪些 ID 受到支援，並且可協助客戶完成針對 Target 的 GDPR 或 CCPA 存取及刪除要求?」，以瞭解接受的值。 |
| 整合程式碼 | 整合程式碼為易記的資料來源名稱，比起使用資料來源 ID，可協助您更輕鬆地追蹤資料來源。 |

當有多個值可用來識別個人資料時，每個有效的識別碼都會有一個個人資料檔案。一個或多個設定檔會透過GDPR中央API，以Target設定檔JSON回應格式傳送至集中的GDPR Azure Blob。

Target 設定檔 JSON 的範例看起來可能像下面的例子:

```
{"profileAttributes": 
 
"Sample_Parameter":{"value":"Gold Loyalty Status","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"user.ReturnTimeOfDay":{"value":"44.0","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"firstSessionStart":{"value":"1523497450602","modifiedAt":"2018-04-11T21:44:10.000-04:00"}, 
 
"user.sessionCountScript":{"value":"1","modifiedAt":"2018-04-11T21:44:14.000-04:00"} 
   } 
} 
```

下表包含說明性的個人資料 JSON 欄位描述:

| 欄位 | 說明 |
|--- |--- |
| Sample_Parameter | Target 設定檔中的許多資訊都是由資料控管者上傳或直接提供。在此範例中，參數是使用「設定檔更新 API」上傳到 Target 設定檔中。如需詳細資訊，請參閱[將資料傳入 Target 的方法](/help/dev/before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md)。 |
| user.ReturnTimeOfDay | 此標準欄位包含使用者最近回訪的時間。 |
| firstSessionStart | 此標準欄位包含使用者首次工作階段開始的時間。 |
| user.sessionCountScript | Target 設定檔中的許多資訊都是由資料控管者上傳或直接提供。在此範例中，個人資料指令碼會遞增此訪客對資料控制方網站執行工作階段的次數。 如需詳細資訊，請參閱[設定檔指令碼屬性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html)。 |

>[!NOTE]
>
>此程式碼範例是說明用的簡短版Target設定檔JSON。 Target 設定檔的許多欄位並非標準欄位。傳回的內容取決於特定訪客設定檔中的資訊。

### Target 支援 IP 模糊化功能嗎?  

如果您選擇使用作為GDPR或CCPA實作策略，Target可支援IP模糊化功能。 如需詳細資訊，請參閱[隱私權](privacy.md/#replacement-of-last-octet-of-ip-addresses)。

### 我應該採取某個行動來避免我的資料被分享或賣給第三方嗎？

Target不允許客戶直接從Target分享或銷售資料給第三方，因此Target不需選擇退出銷售。

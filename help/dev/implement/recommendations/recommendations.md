---
keywords: Recommendations，設定，偏好設定，垂直產業，篩選不相容的條件，預設主機群組，縮圖基底url， recommendations api token， $9
description: 瞭解如何在 [!DNL Adobe Target]中實作[!UICONTROL Recommendations]活動。
title: 如何實作[!UICONTROL Recommendations]活動？
feature: Recommendations
exl-id: af1e8b60-6dbb-451b-aa4f-e167d1800d1c
source-git-commit: 777c8b9aeb866e1b20ec24e85532a99f57d1fe72
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 25%

---

# 計畫和實作[!UICONTROL Recommendations]

協助您計畫和實作[!DNL Adobe Target Recommendations]的資訊。

>[!NOTE]
>
>除了本文章之外，[Adobe Target商業從業者指南](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=zh-Hant){target=_blank}也包含有關[Target Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=zh-Hant){target=_blank}的深入資訊。

在[!DNL Adobe Target]中設定第一個[!UICONTROL Recommendations]活動之前，請先完成下列步驟：

1. [在您要用來擷取使用者行為並傳遞建議的網頁和行動應用程式介面上實作[!UICONTROL Target]](#implement-target)。
1. [設定您想要推薦給使用者的產品或內容的[!UICONTROL Recommendations]目錄](#set-up-your-recommendations-catalog)。
1. [將行為資訊和內容](#pass-behavioral-information-and-context)傳遞給[!DNL Target Recommendations]，讓其提供個人化建議。
1. [設定全域排除專案](#configure-global-exclusions)。
1. [設定[!UICONTROL Recommendations]設定](#configure-recommendations-settings)。
1. （選用） [使用Admin API管理[!UICONTROL Recommendations]](#administer-recommendations-using-admin-apis)。

## 1.實作[!UICONTROL Target]

[!DNL Target Recommendations]需要您實作Adobe Experience Platform Web SDK或at.js 0.9.2 （或更新版本）。 如需詳細資訊，請參閱[[!UICONTROL Target]使用者端實作指南](../client-side/overview.md)。

## 2.設定您的[!UICONTROL Recommendations]目錄

若要提供高品質的建議，[!UICONTROL Target]必須瞭解您想要建議的產品或內容。 目錄通常包含三種關於建議專案的資訊。 假設您正在推薦電影。 包含下列專案：

1. 您要向收到建議的使用者顯示的資料。例如，您可以顯示影片名稱以及影片海報縮圖影像的URL。
1. 適合用於套用行銷和推銷控制的資料。例如，您可以顯示電影分級，這樣就不會建議NC-17電影。
1. 適合用於判斷專案與其他專案之相似度的資料。 例如，您可以顯示電影的型別以及電影的導演。

[!UICONTROL Target]提供多個整合選項來填入您的目錄。 這些選項可結合使用，以更新目錄中的不同專案，或更新不同頻率上的不同專案屬性。

| 方法 | 內容 | 使用時機 | 其他資訊 |
| --- | --- | --- | --- |
| 目錄摘要 | 排程每天上傳和擷取摘要(CSV、Google產品XML或Analytics產品分類)。 | 用於一次傳送多個專案的相關資訊。 用於傳送不常變更的資訊。 | 請參閱[摘要](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=zh-Hant)。 |
| 實體API | 呼叫API以傳送單一專案的最新更新。 | 用於一次傳送一個專案的更新。 用於傳送經常變更的資訊（例如價格、存貨/存貨層次）。 | 請參閱[Entities API開發人員檔案](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities)。 |
| 傳遞頁面上的更新 | 使用頁面上的JavaScript或使用傳送API，傳送單一專案的最新更新。 | 用於一次傳送一個專案的更新。 用於傳送經常變更的資訊（例如價格、存貨/存貨層次）。 | 請參閱下方的[專案檢視/產品頁面](#item-views-or-product-pages)。 |

大部分客戶至少應實作一個摘要。 然後，您可以選擇使用Entities API或頁面上的方法，以經常變更的屬性或專案更新來補充您的摘要。

## 3.傳遞行為資訊和內容

您應該傳遞給[!UICONTROL Target]的行為資訊和內容取決於訪客正在採取的動作，這通常與訪客正在互動的頁面型別有關。

### 專案檢視或產品頁面

在訪客檢視單一專案的頁面上（例如產品詳細資料頁面），您應傳遞訪客正在檢視專案的身分。 您也應該傳遞訪客正在檢視之專案的最精細類別，以允許對目前類別篩選建議。

您也可以在產品頁面本身上傳遞某些快速變更的屬性。 例如，您可以傳遞價格(`value`)和存貨/存貨量。

#### 傳遞價格與存貨

```js {line-numbers="true"}
<script type="text/javascript">
function targetPageParams() { 
   return { 
      "entity": { 
         "id": "32323", 
         "categoryId": "running-shoes", 
         "value": 119.99, 
         "inventory": 329 
      } 
   } 
}
</script>
```

### 類別檢視/類別頁面

在類別頁面上，您可能會想要將建議限制在該類別中的產品或內容。 若要這麼做，請確定您傳遞目前檢視類別的身分。

#### 傳遞目前類別

```js {line-numbers="true"}
function targetPageParams() { 
   return { 
      "entity": { 
         "categoryId": "running-shoes" 
      } 
   } 
}
```

### 購物卡新增/購物卡檢視/結帳頁面

在購物車頁面上，您可以根據訪客目前購物車的內容來建議專案。 若要這麼做，請使用特殊引數`cartIds`傳遞訪客目前購物車中所有專案的ID。

#### 傳遞目前購物車中的專案

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

如需有關購物車型推薦的詳細資訊，請參閱&#x200B;*[!DNL Adobe Target]商業從業者指南*&#x200B;中的[購物車型](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=zh-Hant#cart-based)。

### 排除已經在訪客購物車中的項目

在整個網站的頁面上，您可以從建議中排除某些專案。 例如，您可能不想建議訪客目前購物車中已有的專案。 若要這麼做，請使用特殊引數`excludedIds`傳遞您要排除的所有專案識別碼。

#### 傳遞要排除的專案

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### 購買/訂單確認頁面

發生購買事件時，請傳遞購買專案的身分。 請參閱[如何部署at.js >實作[!UICONTROL Target]而不使用標籤管理員](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)文章中的[追蹤轉換](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)。

## 4.設定全域排除

排除您絕不建議給訪客使用的全域層級任何專案。 請參閱&#x200B;*[!DNL Adobe Target]商業從業者指南*&#x200B;中的[排除專案](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/exclusions.html?lang=zh-Hant)。

## 5.設定[!UICONTROL Recommendations]設定

使用設定來管理您的 [!UICONTROL Recommendations] 實作。

若要存取&#x200B;**[!UICONTROL Recommendations Settings]**&#x200B;選項，請在[!DNL Adobe Experience Cloud]中開啟Target，然後按一下&#x200B;**[!UICONTROL Recommendations]** > **[!UICONTROL Settings]**。

![Recommendations設定頁面](/help/dev/implement/recommendations/assets/recs_settings.png)

可使用下列選項: 

| 設定 | 說明 |
|--- |--- |
| 自訂全域 Mbox | (可選) 指定用來提供 [!UICONTROL Target] 活動的自訂全域 mbox。根據預設，[!UICONTROL Target]使用的全域mbox會用於[!UICONTROL Recommendations]。<P>注意：此選項是在[!UICONTROL Target] **[!UICONTROL Administration]**&#x200B;頁面上設定。 開啟[!UICONTROL Target]，然後按一下&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Visual Experience Composer]**。 |
| 垂直產業 | 行業別用於協助將建議條件分類。此資訊可協助您的團隊成員找到適合特定頁面的條件，例如最適合購物車頁面或媒體頁面的條件。 |
| 篩選不相容的條件 | 啟用此選項只會顯示讓所選頁面傳遞必要資料的條件。不是每個條件都能在每個頁面上正確執行。 頁面或mbox必須傳入`entity.id`或`entity.categoryId`，目前專案/目前類別建議才能相容。 一般來說，最好只顯示相容的條件。不過，如果您要讓活動可以使用不相容的條件，請取消勾選此選項。<P>如果您使用標記管理解決方案，建議您停用此選項。<P>如需此選項的詳細資訊，請參閱&#x200B;*[!DNL Adobe Target]商業從業者指南*&#x200B;中的[[!UICONTROL Recommendations]常見問題集](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=zh-Hant)。 |
| 預設主機群組 | 選取您的預設主機群組。<P>主機群組可按不同用途，用來區隔目錄中的可用項目。例如，您可以將主機群組用於開發和生產環境、不同品牌或不同地理位置。依照預設，「目錄搜尋」、「集合」和「排除項目」中的預覽結果是根據預設主機群組所產生。(您也可以使用「環境」篩選器，選取不同的主機群組來預覽結果。)依照預設，除非在建立或更新項目時指定環境 ID，否則新增的項目可在所有主機群組中使用。提供的建議取決於要求中指定的主機群組。<P>如果沒有看見您的產品，請確定您使用正確的主機群組。例如，假設您將建議設定為使用測試環境，並將主機群組設為「測試」，則可能需要在測試環境中重建集合，才會顯示產品。若要查看每個環境中可用的產品，請對每個環境使用「目錄搜尋」。您也可以針對選取的環境（主機群組），預覽[!UICONTROL Recommendations]集合和排除專案的內容。<P>**注意：**&#x200B;變更選取的環境後，您必須按一下[搜尋]以更新傳回的結果。<P> **[!UICONTROL The Environment]**&#x200B;篩選器可在Target UI中的以下位置使用：<ul><li>目錄搜尋(**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**)</li><li>建立集合對話方塊(**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create New]**)</li><li>更新集合對話方塊(**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**)</li><li>建立排除專案對話方塊(**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create New]**)</li><li>更新排除專案對話方塊(**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**)</li></ul>如需詳細資訊，請參閱&#x200B;*[!DNL Adobe Target]商業從業者指南*&#x200B;中的[主機](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=zh-Hant)。 |
| 縮圖基底 URL | 設定產品目錄的基底 URL 可讓您在傳入縮圖 URL 時，使用相對 URL 來指定產品的縮圖。<P>例如：<P>`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`<P>設定相對於縮圖基底 URL 的 URL。 |
| [!UICONTROL Recommendations] API Token | 在[!UICONTROL Recommendations] API呼叫（例如下載API）中使用此權杖。 |

## 6. （選用）使用Admin API管理[!UICONTROL Recommendations]

請參閱[使用[!UICONTROL Recommendations] API](../../before-administer/recs-api/overview.md)實作指南，瞭解如何設定和使用[!UICONTROL Recommendations]的[!UICONTROL Target]管理和傳遞API。

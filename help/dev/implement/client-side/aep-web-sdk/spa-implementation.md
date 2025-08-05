---
title: '[!DNL Adobe Experience Platform Web SDK]的單頁應用程式實作'
description: 瞭解如何使用 [!DNL Adobe Experience Platform Web SDK]建立 [!DNL Target]的單頁應用程式(SPA)實作。
keywords: target；adobe target；xdm檢視；檢視；單頁應用程式；SPA；SPA生命週期；使用者端；AB測試；AB；體驗鎖定目標；XT；VEC
feature: AEP Web SDK
source-git-commit: 9a2c35b2d150638fbda00be866f84d2a6faa4300
workflow-type: tm+mt
source-wordcount: '1680'
ht-degree: 2%

---


# 實作單頁應用程式

[!DNL Adobe Experience Platform Web SDK]提供豐富的功能，讓貴公司能以新世代使用者端技術(例如單頁應用程式(SPA))為基礎進行個人化。

傳統網站使用「頁面至頁面」導覽模型（又稱為「多頁應用程式」）。 在這些網站中，設計與URL緊密連結，而在頁面之間移動需要完整頁面載入。

單頁應用程式等新式Web應用程式已改為採用可加快瀏覽器UI呈現速度的模型，且通常與頁面重新載入無關。 這些體驗會由客戶互動觸發，例如捲動、點按和游標移動。 隨著現代網路環境的不斷演化，傳統的一般事件（例如頁面載入）與部署個人化和實驗不再具有相關性。

![與傳統頁面生命週期比較，顯示SPA生命週期的圖表。](/help/dev/implement/client-side/aep-web-sdk/assets/spa-vs-traditional-lifecycle.png)

## SPA [!DNL Experience Platform Web SDK]的優點

以下是為您的單頁應用程式使用[!DNL Adobe Experience Platform Web SDK]的優點：

* 可在頁面載入時快取所有產品建議，以減少對單一伺服器呼叫發出的多個伺服器呼叫。
* 改善網站上的使用者體驗，因為選件會透過快取立即顯示，而不會出現傳統伺服器呼叫造成的延遲時間。
* 一行程式碼和一次性開發人員設定可讓行銷人員透過SPA上的[!UICONTROL A/B Test] (VEC)建立及執行[!UICONTROL Experience Targeting]和[!UICONTROL Visual Experience Composer] (XT)活動。

## xdm檢視和單頁應用程式

適用於SPA的[!UICONTROL Adobe Target] VEC利用了名為[!UICONTROL Views]的概念：視覺化元素的邏輯群組，這些元素共同構成SPA體驗。 因此，單頁應用程式可視為根據使用者互動轉換檢視，而不是轉換URL。 [!UICONTROL View]通常可以代表整個網站或網站內的分組視覺元素。

為了進一步說明檢視是什麼，下列範例使用在[!DNL React]中實作的假想線上電子商務網站來探索範例[!UICONTROL Views]。

導覽至首頁後，主圖影像會宣傳復活節特賣以及網站上提供的最新產品。 在這種情況下，可以為整個主畫面定義[!UICONTROL View]。 此[!UICONTROL View]可以簡單地稱為「home」。

![瀏覽器視窗中單頁應用程式的範例影像。](/help/dev/implement/client-side/aep-web-sdk/assets/example-views.png)

當客戶對該企業所銷售的產品越來越感興趣時，他們決定按一下&#x200B;**產品**&#x200B;連結。 與主網站類似，產品網站的整體可定義為[!UICONTROL View]。 此[!UICONTROL View]可命名為「products-all」。

![瀏覽器視窗中單頁應用程式的範例影像，其中顯示所有產品。](/help/dev/implement/client-side/aep-web-sdk/assets/example-products-all.png)

因為[!UICONTROL View]可以定義為整個網站或網站上的一組視覺元素。 產品網站上顯示的四個產品可分組並視為[!UICONTROL View]。 此檢視可命名為「products」。

![瀏覽器視窗中單頁應用程式的範例影像，顯示範例產品。](/help/dev/implement/client-side/aep-web-sdk/assets/example-products.png)

當客戶決定按一下「**載入更多**」按鈕來探索網站上更多產品時，在此情況下，網站URL不會變更。 不過，您可以在此處建立[!UICONTROL View]，以僅代表顯示的第二列產品。 [!UICONTROL View]名稱可以是&quot;products-page-2&quot;。

![瀏覽器視窗中單頁應用程式的範例影像，其他頁面上會顯示範例產品。](/help/dev/implement/client-side/aep-web-sdk/assets/example-load-more.png)

客戶決定從網站購買一些產品，然後進入結帳畫面。 在結帳網站上，客戶可選擇一般配送或快捷配送。 [!UICONTROL View]可以是網站上的任何一組視覺元素，因此可以為傳遞偏好設定建立[!UICONTROL View]，並稱為「傳遞偏好設定」。

![瀏覽器視窗中單頁應用程式簽出頁面的範例影像。](/help/dev/implement/client-side/aep-web-sdk/assets/example-check-out.png)

[!UICONTROL Views]的概念可以延伸至比此案例更遠的地方。 這些案例只是可在網站上定義的一些[!UICONTROL Views]範例。

## 正在實作[!UICONTROL XDM Views]

在[!UICONTROL XDM Views]中可運用[!DNL Target]，讓行銷人員透過[!UICONTROL Visual Experience Composer]在SPA上執行A/B和XT測試。 若要這麼做，必須執行下列步驟，以完成一次性開發人員設定：

1. 安裝[Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/web-sdk/install/overview)。
2. 決定單頁應用程式中要個人化的所有[!UICONTROL XDM Views]。
3. 定義[!UICONTROL XDM Views]後，若要傳遞A/B或XT VEC活動，請在您的單頁應用程式中實作`sendEvent()`函式，並將`renderDecisions`設為`true`以及對應的[!UICONTROL XDM View]。 [!UICONTROL XDM View]必須在`xdm.web.webPageDetails.viewName`中傳遞。 此步驟可讓行銷人員運用[!UICONTROL Visual Experience Composer]為這些XDM啟動A/B和XT測試。

   ```javascript
   alloy("sendEvent", { 
     "renderDecisions": true, 
     "xdm": { 
       "web": { 
         "webPageDetails": { 
         "viewName":"home" 
         }
       } 
     } 
   });
   ```

>[!NOTE]
>
>在第一個`sendEvent()`呼叫中，所有應呈現給使用者的[!UICONTROL XDM Views]都會被擷取並快取。 後續的`sendEvent()`呼叫（已傳入[!UICONTROL XDM Views]）會從快取讀取並轉譯，而不需要伺服器呼叫。

## `sendEvent()`函式範例

本節概述三個範例，說明如何在React中叫用假設性的電子商務SPA的`sendEvent()`函式。

### 範例1：A/B測試首頁

行銷團隊想要在整個首頁上執行A/B測試。

![瀏覽器視窗中單頁應用程式的範例影像。](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-1.png)

若要在整個主網站上執行A/B測試，必須在將XDM `sendEvent()`設為`viewName`的情況下叫用`home`：

```jsx
function onViewChange() { 
  
  var viewName = window.location.hash; // or use window.location.pathName if router works on path and not hash 

  viewName = viewName || 'home'; // view name cannot be empty 

  // Sanitize viewName to get rid of any trailing symbols derived from URL 

  if (viewName.startsWith('#') || viewName.startsWith('/')) { 
    viewName = viewName.substr(1); 
  }
   
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName":"home" 
        } 
      } 
    }
  }); 
} 

// react router v4 

const history = syncHistoryWithStore(createBrowserHistory(), store); 

history.listen(onViewChange); 

// react router v3 

<Router history={hashHistory} onUpdate={onViewChange} > 
```

### 範例2：個人化產品

行銷團隊想要在使用者按一下&#x200B;**載入更多**&#x200B;後，將價格標籤顏色變更為紅色，以個人化第二列產品。

![瀏覽器視窗中單頁應用程式的範例影像，顯示個人化優惠。](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-2.png)

```jsx
function onViewChange(viewName) { 

  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName
        }
      } 
    } 
  }); 
} 

class Products extends Component { 
  
  render() { 
    return ( 
      <button type="button" onClick={this.handleLoadMoreClicked}>Load more</button> 
    ); 
  } 

  handleLoadMoreClicked() { 
    var page = this.state.page + 1; // assuming page number is derived from component's state 
    this.setState({page: page}); 
    onViewChange('PRODUCTS-PAGE-' + page); 
  } 

} 
```

### 範例3：A/B測試傳送偏好設定

行銷團隊想要執行A/B測試，以檢視在選取&#x200B;**快捷配送**&#x200B;時，按鈕的顏色是否從藍色變更為紅色。 團隊認為此變更可提升轉換率（而非讓兩個傳送選項的按鈕均保持藍色）。

![瀏覽器視窗中單頁應用程式的範例影像（含A/B測試）。](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-3.png)

若要根據選取的傳遞偏好設定個人化網站內容，可以為每個傳遞偏好設定建立[!UICONTROL View]。 選取&#x200B;**一般傳遞**&#x200B;時，可將[!UICONTROL View]命名為「checkout-normal」。 如果選取&#x200B;**快速運送**，則可將[!UICONTROL View]命名為「checkout-express」。

```jsx
function onViewChange(viewName) { 
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName 
        }
      }
    }
  }); 
} 

class Checkout extends Component { 

  render() { 
    return ( 
      <div onChange={this.onDeliveryPreferenceChanged}> 
        <label> 
          <input type="radio" id="normal" name="deliveryPreference" value={"Normal Delivery"} defaultChecked={true}/> 
          <span> Normal Delivery (7-10 business days)</span> 
        </label> 
        <label> 
          <input type="radio" id="express" name="deliveryPreference" value={"Express Delivery"}/> 
          <span> Express Delivery* (2-3 business days)</span> 
        </label> 
      </div> 
    ); 
  } 

  onDeliveryPreferenceChanged(evt) { 
    var selectedPreferenceValue = evt.target.value; 
    onViewChange(selectedPreferenceValue); 
  } 

} 
```

## 針對SPA使用[!UICONTROL Visual Experience Composer]

當您完成定義[!UICONTROL XDM Views]並實作`sendEvent()`，且傳入了那些[!UICONTROL XDM Views]時，VEC就能夠偵測這些[!UICONTROL Views]，並允許使用者建立A/B或XT活動的動作或修改。

>[!NOTE]
>
>若要將VEC用於SPA，您必須安裝並啟動[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)或[Chrome VEC Helper擴充功能](https://experienceleague.adobe.com/zh-hant/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension)。

### [!UICONTROL Modifications]面板

[!UICONTROL Modifications]面板擷取為特定[!UICONTROL View]建立的動作。 [!UICONTROL View]的所有動作都會分組在該[!UICONTROL View]下。

### 動作

按一下動作會醒目顯示套用此動作之網站上的元素。 在[!UICONTROL View]下建立的每個VEC動作都有下列圖示： **資訊**、**編輯**、**複製**、**移動**&#x200B;和&#x200B;**刪除**。 下表會詳細說明這些圖示。

| 圖示 | 說明 |
|---|---|
| 資訊 | 顯示此動作的詳細資料。 |
| 編輯 | 可讓您直接編輯該動作的屬性。 |
| 原地複製 | 將動作原地複製至一或多個存在於[!UICONTROL Views]面板上的[!UICONTROL Modifications]，或原地複製至一或多個您已在VEC中瀏覽及導覽到的[!UICONTROL Views]。 動作不一定存在於[!UICONTROL Modifications]面板中。<br/><br/>**注意：**&#x200B;執行原地復製作業後，您必須透過[!UICONTROL View]導覽至VEC中的[!UICONTROL Browse]，才能檢視該原地複製動作的作業是否有效。 如果無法將動作套用至[!UICONTROL View]，您會看到錯誤。 |
| 移動 | 將動作移動到[!UICONTROL Page Load Event]或[!UICONTROL View]面板中已存在的任何其他[!UICONTROL Modifications]。<br/><br/>**頁面載入事件：**&#x200B;任何對應至頁面載入事件的動作，都會套用到網頁應用程式的初始頁面載入上。 <br/><br/>**注意：**&#x200B;執行移動作業後，您必須透過[!UICONTROL View]導覽至VEC中的[!UICONTROL Browse]，以檢視移動作業是否有效。 如果無法將動作套用至[!UICONTROL View]，請參閱錯誤。 |
| 刪除 | 刪除動作。 |

## 使用適用於SPA的VEC範例

本節概述使用[!UICONTROL Visual Experience Composer]建立A/B或XT活動之動作和修改的三個範例。

### 範例1：更新「首頁」檢視

本文稍早前，已針對整個首頁網站定義名為「home」的[!UICONTROL View]。 現在，行銷團隊想要以下列方式更新「首頁」檢視：

* 將&#x200B;**加入購物車**&#x200B;和&#x200B;**類似**&#x200B;按鈕變更為較淺的藍色。 此變更應在頁面載入期間發生，因為它涉及變更標題的元件。
* 將&#x200B;**2026年最新產品**&#x200B;標籤變更為&#x200B;**2026年最暢銷產品**，並將文字顏色變更為紫色。

若要在VEC中進行這些更新，請選取&#x200B;**撰寫**，並將這些變更套用至「首頁」檢視。

![視覺化體驗撰寫器範例頁面。](/help/dev/implement/client-side/aep-web-sdk/assets/vec-home.png)

### 範例2：變更產品標籤

針對「products-page-2」[!UICONTROL View]，行銷團隊想要將&#x200B;**Price**&#x200B;標籤變更為&#x200B;**Sale Price**，並將標籤顏色變更為紅色。

若要在VEC中進行這些更新，需執行下列步驟：

1. 在VEC中選取&#x200B;**瀏覽**。
2. 在網站頂端導覽列中選取&#x200B;**產品**。
3. 選取&#x200B;**載入更多**&#x200B;一次，以檢視產品的第二列。
4. 在VEC中選取&#x200B;**撰寫**。
5. 套用動作以將文字標籤變更為&#x200B;**促銷價**，且顏色為紅色。

![具有產品標籤的視覺化體驗撰寫器範例頁面。](/help/dev/implement/client-side/aep-web-sdk/assets/vec-products-page-2.png)

### 範例3：個人化傳送偏好設定樣式

[!UICONTROL Views]可以在精細層次定義，例如單選按鈕的狀態或選項。 本文前面的[!UICONTROL Views]是針對傳遞偏好設定、「checkout-normal」和「checkout-express」定義的。 行銷團隊想要將「checkout-express」檢視的按鈕顏色變更為紅色。

若要在VEC中進行這些更新，需執行下列步驟：

1. 在VEC中選取&#x200B;**瀏覽**。
2. 將產品新增至網站上的購物車。
3. 選取網站右上角的購物車圖示。
4. 選取&#x200B;**結帳您的訂單**。
5. 選取&#x200B;**傳遞偏好設定**&#x200B;下的&#x200B;**快速傳遞**&#x200B;選項按鈕。
6. 在VEC中選取&#x200B;**撰寫**。
7. 將&#x200B;**付款**&#x200B;按鈕顏色變更為紅色。

>[!NOTE]
>
>在選取[!UICONTROL View]快速運送[!UICONTROL Modifications]選項按鈕之前，「checkout-express」**不會出現在**&#x200B;面板中。 這是因為選取`sendEvent()`快速運送&#x200B;**選項按鈕時會執行**&#x200B;函式，因此在選取選項按鈕之前，VEC不會察覺「checkout-express」[!UICONTROL View]。

![視覺化體驗撰寫器顯示傳遞偏好設定選擇器。](/help/dev/implement/client-side/aep-web-sdk/assets/vec-delivery-preference.png)

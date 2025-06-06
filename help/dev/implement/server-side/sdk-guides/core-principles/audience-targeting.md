---
title: 對象目標定位
description: 受眾可用於鎖定您的實驗和個人化活動。 [!DNL Adobe Target] 現成可支援多種強大的對象目標定位功能。
exl-id: df1bd856-e848-452c-90a0-abf29e7a2313
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 22%

---

# 對象目標定位

## 總覽

受眾可用於鎖定您的實驗和個人化活動。 [!DNL Adobe Target]現成可支援多種強大的受眾目標定位功能。 下列屬性適用於[對象目標定位](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/create-audience.html?lang=zh-Hant)：

### [!DNL Target]資料庫

如需詳細資訊，請參閱[[!DNL Target] 資料庫](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-library.html?lang=zh-Hant)。
&#x200B;URL
* 從Bing反向連結
* Chrome瀏覽器
* Firefox瀏覽器
* 從Google反向連結
* Internet Explorer
* Linux作業系統
* Mac作業系統
* 新訪客
* 再度訪問的訪客
* Safari瀏覽器
* 平板電腦裝置
* Windows作業系統
* 從Yahoo反向連結

### 地理

如需詳細資訊，請參閱[地理](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=zh-Hant)。
「&#x200B;&#x200B;」
* 國家/地區
* 狀態
* 城市
* 郵遞區號
* 緯度
* 經度
* DMA
* 行動電信業者

### 網路

如需詳細資訊，請參閱[網路](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=zh-Hant)。

* ISP
* 網域名稱
* 連線速度

### 行動

如需詳細資訊，請參閱[行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=zh-Hant)。

* 裝置行銷名稱
* 裝置型號
* 裝置供應商
* 是行動裝置
* 是行動電話
* 是平板電腦
* 作業系統
* 螢幕高度 (px)
* 螢幕寬度 (px)

### 自訂

如需詳細資訊，請參閱[自訂引數](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=zh-Hant)。

* 任何索引鍵/值組

### 作業系統

如需詳細資訊，請參閱[作業系統](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=zh-Hant)。

* Linux
* Macintosh
* Windows

### 網頁

如需詳細資訊，請參閱[網站頁面](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=zh-Hant)。

* 目前頁面
* 上一頁
* 登陸頁面
* HTTP 標題

### 瀏覽器

如需詳細資訊，請參閱[瀏覽器](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=zh-Hant)。

* 類型
* 語言
* 版本

### 訪客設定檔

如需詳細資訊，請參閱[訪客設定檔](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=zh-Hant)。

* 任何索引鍵/值組，持續存在

### 流量來源

如需詳細資訊，請參閱[流量來源](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=zh-Hant)。

* 來自 Baidu
* 從 Bing
* 從 Google
* 從 Yahoo
* 引用登陸頁面: URL
* 引用登陸頁面: 網域
* 引用登陸頁面: 查詢

### 時間段

如需詳細資訊，請參閱[時間範圍](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=zh-Hant)。

* 開始日期/結束日期

## 使用者端提示

[!DNL Adobe Target]需要使用者端提示才能正確劃分瀏覽器、作業系統和行動對象屬性，以及某些設定檔指令碼例項。 如需詳細背景資訊，請參閱[使用者代理程式和使用者端提示](../../../client-side/atjs/user-agent-and-client-hints.md)。

### 如何將使用者端提示傳遞至[!DNL Adobe Target]

從Node.js SDK v2.4.0和Java SDK v2.3.0開始，使用者端提示可以透過`getOffers()`呼叫傳送至[!DNL Target]。 `request.context`物件上應包含使用者端提示和使用者代理。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
targetClient.getOffers({ 
    request: { 
        context: { 
            channel: "mobile" 
            userAgent: "Mozilla/5.0 (Linux; Android 12; Pixel 4a) AppleWebKit/537.36 (KHTML, like Gecko) Mobile Safari/537.36", 
            clientHints: { 
                mobile: "true", 
                platform: "Linux", 
                platformVersion: "12.1", 
                model: "Pixel 4a", 
                browserUAWithMajorVersion: "\"Not A;Brand\";v=\"98\", \"Chromium\";v=\"98\", \"Google Chrome\";v=\"98\"", 
                browserUAWithFullVersion: "\" Not A;Brand\";v=\"98.0.0.0\", \"Chromium\";v=\"98.0.4844.83\", \"Google Chrome\";v=\"98.0.4758.101\"", 
                bitness: "64", 
                architecture: "x86" 
            } 
        }, 
        execute: { 
            mboxes: [{ 
                name: "home", 
                index: 1 
            }] 
        } 
    } 
});
```

>[!TAB Java SDK]

```javascript {line-numbers="true"}
import com.adobe.target.delivery.v1.model.ClientHints; 
import com.adobe.target.delivery.v1.model.Context; 
import com.adobe.target.delivery.v1.model.ExecuteRequest; 
import com.adobe.target.edge.client.model.TargetDeliveryRequest; 

 
ClientHints clientHints = new ClientHints(); 
clientHints.setMobile(true); 
clientHints.setPlatform("macOS"); 
clientHints.setArchitecture("x86"); 
clientHints.setPlatformVersion("11.3.1"); 
clientHints.setBrowserUAWithMajorVersion( 
  "\" Not A;Brand\";v=\"99\", \"Chromium\";v=\"99\", \"Google Chrome\";v=\"99\""); 
String userAgent = 
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36"; 

 
TargetDeliveryRequest request = TargetDeliveryRequest.builder() 
        .execute(new ExecuteRequest().pageLoad(pageLoad)) 
        .context(new Context().clientHints(clientHints).userAgent(userAgent)) 
        .build(); 
```

>[!ENDTABS]

## 裝置上決策

下表指出裝置上決策支援或不支援的對象規則。

| 對象規則 | 裝置上決策 |
| --- | --- |
| [地理](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=zh-Hant) | 是 |
| [網路](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=zh-Hant) | 否 |
| [行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=zh-Hant) | 否 |
| [自訂引數](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=zh-Hant) | 是 |
| [作業系統 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=zh-Hant) | 是 |
| [網頁](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=zh-Hant) | 是 |
| [瀏覽器](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=zh-Hant) | 是 |
| [訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=zh-Hant) | 否 |
| [流量來源](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=zh-Hant) | 否 |
| [時間範圍](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=zh-Hant) | 是 |
| [Experience Cloud對象](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=zh-Hant) (來自Adobe Audience Manager、Adobe Analytics和Adobe Experience Manager的對象 | 否 |

### 裝置上決策的地理定位

為了維持具有地理型對象的裝置上決策活動幾乎零延遲，Adobe建議您在`getOffers`呼叫中自行提供地理值。 若要這麼做，請在要求的`Context`中設定`Geo`物件。 這表示您的伺服器需要一種方式來確定每個一般使用者的位置。 例如，您的伺服器可能會使用您設定的服務，執行IP對地理位置的查詢。 某些託管提供者(例如Google Cloud)會透過每個`HttpServletRequest`中的自訂標頭提供此功能。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        context: {
            geo: {
                city: "SAN FRANCISCO",
                countryCode: "US",
                stateCode: "CA",
                latitude: 37.75,
                longitude: -122.4
            }
        },
        execute: {
            pageLoad: {}
        }
    }
})
```

>[!TAB Java SDK]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(ipToGeoLookup(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

    public static Geo ipToGeoLookup(String ip) {
        GeoResult geoResult = geoLookupService.lookup(ip);
        return new Geo()
            .city(geoResult.getCity())
            .stateCode(geoResult.getStateCode())
            .countryCode(geoResult.getCountryCode());
    }
}
```

>[!ENDTABS]

不過，如果您無法在伺服器上執行IP對地理的查詢，但您仍想要對包含地理型受眾的`getOffers`要求執行裝置上決策，則也支援此功能。 此方法的缺點在於它將使用遠端IP對地理的查閱，這會增加每個`getOffers`呼叫的延遲。 此延遲應低於遠端`getOffers`呼叫，因為它點選了位於伺服器附近的CDN。 您必須&#x200B;**僅**&#x200B;在請求`Context`的`Geo`物件中提供`ipAddress`欄位，SDK才能擷取您使用者IP位址的地理位置。 如果提供除了`ipAddress`之外的任何其他欄位，[!DNL Target] SDK將不會擷取地理位置中繼資料以進行解析。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        context: {
            geo: {
                ipAddress: "127.0.0.1"
            }
        },
        execute: {
            pageLoad: {}
        }
    }
})
```

>[!TAB Java SDK]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(new Geo().ipAddress(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

}
```

>[!ENDTABS]

## 伺服器端決策

下表指出哪些對象規則支援或不支援伺服器端決策。

| 對象規則 | 伺服器端決策 |
| --- | --- |
| [地理](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=zh-Hant) | 是 |
| [網路](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=zh-Hant) | 是 |
| [行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=zh-Hant) | 是 |
| [自訂引數](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=zh-Hant) | 是 |
| [作業系統 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=zh-Hant) | 是 |
| [網頁](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=zh-Hant) | 是 |
| [瀏覽器](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=zh-Hant) | 是 |
| [訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=zh-Hant) | 是 |
| [流量來源](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=zh-Hant) | 是 |
| [時間範圍](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=zh-Hant) | 是 |
| [Experience Cloud對象](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=zh-Hant) (來自Adobe Audience Manager、Adobe Analytics和Adobe Experience Manager的對象 | 是 |

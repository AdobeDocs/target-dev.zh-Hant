---
keywords: 客戶服務； CNAME；憑證程式；正式名稱； Cookie；憑證； AMC； Adobe Managed憑證； Digicert；網域控制驗證； DCV
description: 與 [!DNL Adobe] 客戶服務合作，在 [!DNL Adobe Target] 中實作CNAME （規範名稱）支援，以處理廣告封鎖問題。
title: 如何在Target中使用CNAME？
feature: Privacy & Security
role: Developer
exl-id: bf533771-6d46-48ba-964c-3ad9ce9f7352
TQID: https://experienceleague.adobe.com/gTS60hypD2WGc2fJh-sUkq2-pkzt2KnM4CzSQ050L40
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1313
ht-degree: 1%

---

# CNAME和[!DNL Target]

使用[!DNL Adobe] Client Care在[!DNL Adobe Target]中實作CNAME （規範名稱）支援的說明。 使用CNAME來處理廣告封鎖問題或ITP相關（智慧型追蹤預防） Cookie政策。 使用CNAME時，會呼叫客戶擁有的網域，而非[!DNL Adobe]擁有的網域。

## 在[!DNL Target]中要求CNAME支援

1. 決定您的SSL憑證所需的主機名稱清單（請參閱底下的常見問題集）。
1. [填寫此表單](/help/dev/implement/assets/FPC_Request_Form.xlsx)，並在您[開啟要求CNAME支援的 [!DNL Adobe] 客戶服務票證](https://experienceleague.adobe.com/en/docs/target/using/cmp-resources-and-contact-information#reference_ACA3391A00EF467B87930A450050077C)時加入它：

   * [!DNL Adobe Target]使用者端代碼：
   * SSL憑證主機名稱（範例： `target.example.com target.example.org`）：
   * SSL憑證購買者（[!DNL Adobe]強烈建議使用，請參閱常見問題集）： Adobe/customer
   * 如果客戶購買憑證（也稱為「自帶憑證」，BYOC），請填寫以下其他詳細資料：

      * 憑證組織（範例：範例Company Inc）：
      * 憑證組織單位（選用，例如：行銷）：
      * 憑證國家/地區（範例：美國）：
      * 憑證州/地區（範例：加州）：
      * 憑證城市（範例：聖荷西）：

1. 對於每個主機名稱要求，Adobe會建立實作，並傳回CNAME記錄名稱供您建立，該名稱將包含尾碼為`tt.omtrdc.net`的隨機字串

   例如，如果您要求`target.example.com`，我們會以`abcdefgh.tt.omtrdc.net`的格式傳回CNAME。 您的DNS CNAME記錄應該類似於：

   ```
   target.example.com.  IN  CNAME  abcdefgh.tt.omtrdc.net.
   ```

   >[!IMPORTANT]
   >
   >[!DNL Adobe]的憑證授權單位DigiCert必須等到此步驟完成才能核發憑證。 因此，在此步驟完成之前，[!DNL Adobe]無法完成您的CNAME實作要求。

1. 如果[!DNL Adobe]正在購買憑證，[!DNL Adobe]會與DigiCert合作，在[!DNL Adobe]的生產伺服器上購買並部署您的憑證。

   如果客戶正在購買憑證(BYOC)，[!DNL Adobe]客戶服務會將憑證簽署要求(CSR)傳送給您。 透過您選擇的憑證授權單位購買憑證時，請使用CSR 。 發行憑證後，請將憑證復本及任何中繼憑證傳送至[!DNL Adobe] Client Care以進行部署。

   當您的實作準備就緒時，[!DNL Adobe] Client Care會通知您。

1. 將`serverDomain` （[檔案](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverDomain)）更新為新的CNAME主機名稱，並在您的at.js設定中將`overrideMboxEdgeServer`設定為`false` （[檔案](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver)）。

## 常見問題集

下列資訊回答有關在[!DNL Target]中請求和實作CNAME支援的常見問題：

### 我可以提供我自己的憑證（自備憑證或BYOC）嗎？

您可以提供自己的憑證。 但是，[!DNL Adobe]強烈建議不要使用此作法。 如果[!DNL Adobe]購買並控制憑證，則[!DNL Adobe]和您都能更輕鬆地管理SSL憑證生命週期。 SSL憑證存留期只會變得更短（請參閱下一個有關憑證存留期的區段）。 因此，[!DNL Adobe] Client Care每次都必須與您連絡，才能及時取得新憑證。 當憑證存留期將減少到只有47天時，這項工作將變成一項挑戰。 憑證過期時，因為瀏覽器拒絕連線，所以您的[!DNL Target]實作已受到危害。

>[!IMPORTANT]
>
>如果您要求[!DNL Target]自帶憑證CNAME實作，您有責任在每次到期時提供更新的憑證給[!DNL Adobe]客戶服務。 允許您的CNAME憑證在[!DNL Adobe]部署更新的憑證之前到期，會導致您特定[!DNL Target]實作的中斷。

### 我的新SSL憑證要多久才會過期？

作為憑證授權單位主要計畫的一部分，所有憑證存留期範圍將會縮短。 針對[!DNL Adobe]的憑證提供者DigiCert，將套用下列排程：

直到2026年3月15日，TLS憑證的存留期上限為398天。
自2026年3月15日起，TLS憑證的存留期上限為200天。
自2027年3月15日起，TLS憑證的生命週期上限為100天。
自2029年3月15日起，TLS憑證的存留期上限為47天。
如需詳細資訊，請參閱[DigiCert關於縮短憑證存留時間的文章](https://www.digicert.com/blog/tls-certificate-lifetimes-will-officially-reduce-to-47-days)

### 我應該選擇哪些主機名稱？ 每個網域應該選擇幾個主機名稱？

[!DNL Target] CNAME實作只需要SSL憑證和客戶DNS中每個網域一個主機名稱。 [!DNL Adobe]建議每個網域使用一個主機名稱。 有些客戶出於其自身目的（例如在測試環境中測試）而要求每個網域有更多主機名稱，此做法受到支援。

大多數客戶選擇類似`target.example.com`的主機名稱。 [!DNL Adobe]建議您遵循此作法，但最終選擇權歸您。 請勿要求現有DNS記錄的主機名稱。 這樣做會造成衝突，並延遲解決[!DNL Target] CNAME要求的時間。

### 我已有[!DNL Adobe Analytics]的CNAME實作，可以使用相同的憑證或主機名稱嗎？

否，[!DNL Target]需要個別的主機名稱和憑證。

### 我目前的[!DNL Target]實作是否會受到ITP 2.x影響？

Apple智慧型追蹤預防(ITP) 2.3版匯入了CNAME遮蔽緩解功能，此功能可偵測[!DNL Adobe Target] CNAME實作，並將Cookie的有效期縮短為七天。 目前[!DNL Target]沒有ITP CNAME遮罩緩解的因應措施。 如需有關ITP的詳細資訊，請參閱[Apple智慧型追蹤預防(ITP) 2.x](/help/dev/before-implement/privacy/apple-itp-2x.md)。

### 部署CNAME實作時，可能會發生哪些服務中斷？

部署憑證時沒有服務中斷（包括憑證續約）。

不過，當您將[!DNL Target]實作程式碼（ at.js中的`serverDomain`）中的主機名稱變更為新的CNAME主機名稱(`target.example.com`)後，網頁瀏覽器會將回頭的訪客視為新訪客。 回訪訪客的設定檔資料遺失，因為舊主機名稱(`clientcode.tt.omtrdc.net`)下的先前Cookie無法存取。 由於瀏覽器安全模式的緣故，無法存取先前的Cookie。 這種中斷只會在初次切換至新CNAME時發生。 憑證更新不會產生相同的效果，因為主機名稱不會變更。

### 我的CNAME實作使用什麼金鑰型別和憑證簽章演演算法？

所有憑證預設為RSA SHA-256，金鑰則為RSA 2048位元。 應透過客戶服務明確要求大於2048位元的金鑰大小。

### 如何驗證我的CNAME實作準備好進行流量？

使用以下命令集（在macOS或Linux命令列終端機中，使用bash和curl >=7.49）：

1. 將此Bash函式複製並貼到您的終端機中，或將函式貼到您的Bash啟動指令碼檔案中（通常是`~/.bash_profile`或`~/.bashrc`），以便該函式可在終端機工作階段中使用：

```
function adobeTargetCnameValidation {
  local hostname="$1"
  
  if [ -z "$hostname" ]; then
    echo "ERROR: no hostname specified"
    return 1
  fi
  
  local service="Adobe Target CNAME implementation"
  local edges="41 42 44 45 46 47 48"
  local edgeDomain="tt.omtrdc.net"
  local edgeFormat="mboxedge%d%s.$edgeDomain"
  local poolDomain="pool.data.adobedc.net"
  local shards=5
  local shardsFoundCount=0
  local shardsFound=""
  local shardsFoundOutput=""
  local curlRegex="subject:.*CN=|expire date:|issuer:"
  local curlValidation="SSL certificate verify ok"
  local curlResponseValidation='"OK"'
  local curlEndpoint="/uptime?mboxClient=uptime3"
  local url="https://$hostname$curlEndpoint"
  local sslShopperUrl="https://www.sslshopper.com/ssl-checker.html#hostname=$hostname"
  local success="✅"
  local failure="🚫"
  local info="🔎"
  local rule="="
  local horizontalRule="$(seq ${COLUMNS:-30} | xargs printf "$rule%.0s")"
  local miniRule="$(seq 5 | xargs printf "$rule%.0s")"
  local curlVersion="$(curl --version | head -1 | cut -d' ' -f2)"
  local curlVersionRequired=7.49
  local edgeCount="$(wc -w <<< "$edges" | tr -d ' ')"
  local cnameExists=""
  local endToEndTestSucceeded=""
  
  for region in IRL1 IND1 SIN OR SYD VA TYO; do
    local currShard="${region}-${poolDomain}"
    local curlResult="$(curl -vsm20 --connect-to "$hostname:443:$currShard:443" "$url" 2>&1)"
    
    if grep -q "$curlValidation" <<< "$curlResult"; then
      shardsFound+=" $currShard"
      
      if grep -q "$curlResponseValidation" <<< "$curlResult"; then
        shardsFoundCount=$((shardsFoundCount+1))
        shardsFoundOutput+="\n\n$miniRule $success $hostname [edge shard: $currShard] $miniRule\n"
      else
        shardsFoundOutput+="\n\n$miniRule $failure $hostname [edge shard: $currShard] $miniRule\n"
      fi
      
      shardsFoundOutput+="$(grep -E "$curlRegex" <<< "$curlResult" | sort)"
      
      if ! grep -q "$curlResponseValidation" <<< "$curlResult"; then
        shardsFoundOutput+="\nERROR: unexpected HTTP response from this shard using $url"
      fi
    fi
  done
  
  echo
  echo "$horizontalRule"
  echo
  echo "$service validation for hostname $hostname:"
  
  local dnsOutput="$(dig -t CNAME +short "$hostname" 2>&1)"
  if grep -qFi ".$edgeDomain" <<< "$dnsOutput"; then
    echo "$success $hostname passes DNS CNAME validation"
    cnameExists=true
  else
    echo -n "$failure $hostname FAILED DNS CNAME validation -- "
    if [ -n "$dnsOutput" ]; then
      echo -e "$dnsOutput is not in the subdomain $edgeDomain"
    else
      echo "required DNS CNAME record pointing to <target-client-code>.$edgeDomain not found"
    fi
  fi
  
  for region in IRL1 IND1 SIN OR SYD VA TYO; do
    local curlResult="$(curl -vsm20 --connect-to "$hostname:443:${region}-pool.data.adobedc.net:443" "https://$hostname$curlEndpoint" 2>&1)"
    
    if grep -q "$curlValidation" <<< "$curlResult"; then
      if grep -q "$curlResponseValidation" <<< "$curlResult"; then
        echo -en "$success $hostname passes TLS and HTTP response validation for region $region"
        if [ -n "$cnameExists" ]; then
          echo
        else
          echo " -- the DNS CNAME is not pointing to the correct subdomain for ${service}s with Adobe-managed certificates" \
            "(bring-your-own-certificate implementations don't have this requirement), but this test passes as configured"
        fi
        endToEndTestSucceeded=true
      else
        echo -n "$failure $hostname FAILED HTTP response validation for region $region --" \
          "unexpected response from $url -- "
        if [ -n "$cnameExists" ]; then
          echo "DNS is NOT pointing to the correct shard, notify Adobe Client Care"
        else
          echo "the required DNS CNAME record is missing, see above"
        fi
      fi
    else
      echo -n "$failure $hostname FAILED TLS validation for region $region -- "
      if [ -n "$cnameExists" ]; then
        echo "DNS is likely NOT pointing to the correct shard or there's a validation issue with the certificate or" \
          "protocols, see curl output below and optionally SSL Shopper ($sslShopperUrl):"
        echo ""
        echo "$horizontalRule"
        echo "$curlResult" | sed 's/^/    /g'
        echo "$horizontalRule"
        echo ""
      else
        echo "the required DNS CNAME record is missing, see above"
      fi
    fi
  done
  
  if [ "$shardsFoundCount" -ge "$edgeCount" ]; then
    echo -n "$success $hostname passes shard validation for the following $shardsFoundCount edge shards:"
    echo -e "$shardsFoundOutput"
    echo
    
    if [ -n "$cnameExists" ] && [ -n "$endToEndTestSucceeded" ]; then
      echo "$horizontalRule"
      echo ""
      echo "  For additional TLS/SSL validation, see SSL Shopper:"
      echo ""
      echo "    $info  $sslShopperUrl"
      echo ""
      echo "  To check DNS propagation around the world, see whatsmydns.net:"
      echo ""
      echo "    $info  DNS A records:     https://whatsmydns.net/#A/$hostname"
      echo "    $info  DNS CNAME record:  https://whatsmydns.net/#CNAME/$hostname"
    fi
  else
    echo -n "$failure $hostname FAILED shard validation -- shards found: $shardsFoundCount," \
      "expected: $edgeCount"
    echo ""
  fi
  
  echo
  echo "$horizontalRule"
  echo
}  
```

1. 貼上此命令（將`target.example.com`取代為您的主機名稱）：

   ```
   adobeTargetCnameValidation target.example.com
   ```

   如果實作準備就緒，您會看到如下所示的輸出。 重要的一點是，所有驗證狀態行都顯示`✅`而非`🚫`。 每個[!DNL Target]邊緣CNAME分片都應該顯示`CN=target.example.com`，這符合要求的憑證上的主要主機名稱（憑證上的其他SAN主機名稱不會列印在此輸出中）。

```
$ adobeTargetCnameValidation target.example.com

==========================================================

Adobe Target CNAME implementation validation for hostname target.example.com:
✅ target.example.com passes DNS CNAME validation
✅ target.example.com passes TLS and HTTP response validation for region IRL1
✅ target.example.com passes TLS and HTTP response validation for region IND1
✅ target.example.com passes TLS and HTTP response validation for region SIN
✅ target.example.com passes TLS and HTTP response validation for region OR
✅ target.example.com passes TLS and HTTP response validation for region SYD
✅ target.example.com passes TLS and HTTP response validation for region VA
✅ target.example.com passes TLS and HTTP response validation for region TYO
✅ target.example.com passes shard validation for the following 7 edge shards:

===== ✅ target.example.com [edge shard: IRL1-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: IND1-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: SIN-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: OR-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: SYD-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: VA-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: TYO-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

==========================================================  

For additional TLS/SSL validation, see SSL Shopper:

    🔎  https://www.sslshopper.com/ssl-checker.html#hostname=target.example.com  

To check DNS propagation around the world, see whatsmydns.net:

    🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
    🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com 
```

>[!NOTE]
>
>如果此驗證命令在DNS驗證時失敗，但您已進行必要的DNS變更，則可能需要等待DNS更新完全傳播。 DNS記錄具有相關的[TTL （存留時間）](https://en.wikipedia.org/wiki/Time_to_live#DNS_records)，它指定這些記錄的DNS回覆的快取到期時間。 因此，您可能需要至少等待TTL的時間。 您可以使用`dig target.example.com`命令或[G Suite Toolbox](https://toolbox.googleapps.com/apps/dig/#CNAME)來查詢您特定的TTL。 若要檢查全球的DNS傳播，請參閱[whatsmydns.net](https://whatsmydns.net/#CNAME)。

### 如何使用具有 CNAME 的退出連結

如果您使用CNAME，選擇退出連結應該包含「client=`clientcode`引數，例如：
`https://my.cname.domain/optout?client=clientcode`.

將`clientcode`取代為您的使用者端代碼，然後新增要連結至[選擇退出URL](/help/dev/before-implement/privacy/privacy.md)的文字或影像。

## 已知限制

* 有CNAME和at.js 1.x時，QA模式沒有粘性，因為它是以協力廠商Cookie為基礎。 因應措施是將預覽引數新增到您導覽到的每個URL中。 有CNAME和at.js 2.x時，QA模式會很粘滯。
* 使用CNAME時，[!DNL Target]呼叫的Cookie標頭大小更有可能增加。 [!DNL Adobe]建議將Cookie大小保持在8 KB以下。

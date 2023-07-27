---
keywords: 客戶服務， cname，憑證程式，規範名稱， cookie，憑證， amc， adobe managed certificate， digicert，網域控制驗證， dcv，客戶服務2
description: 使用 [!UICONTROL Adobe客戶服務] 若要在中實作CNAME （正式名稱）支援 [!DNL Adobe Target] 以處理廣告封鎖問題。
title: 如何在Target中使用CNAME？
feature: Privacy & Security
exl-id: 5709df5b-6c21-4fea-b413-ca2e4912d6cb
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1193'
ht-degree: 1%

---

# CNAME和Target

使用說明 [!DNL Adobe Client Care] 若要在中實作CNAME （正式名稱）支援 [!DNL Adobe Target]. 使用CNAME來處理廣告封鎖問題或ITP相關（智慧型追蹤預防） Cookie政策。 透過CNAME，會呼叫客戶擁有的網域，而不是Adobe擁有的網域。

## 請求Target中的CNAME支援

1. 決定您的SSL憑證所需的主機名稱清單（請參閱底下的常見問題集）。

1. 針對每個主機名稱，在您的DNS中建立指向您的一般CNAME記錄 [!DNL Target] 主機名稱 `clientcode.tt.omtrdc.net`.

   例如，如果您的使用者端代碼是&quot;cnamecustomer&quot;，而您建議的主機名稱是 `target.example.com`，您的DNS CNAME記錄看起來類似於：

   ```
   target.example.com.  IN  CNAME  cnamecustomer.tt.omtrdc.net.
   ```

   >[!WARNING]
   >
   >在此步驟完成之前，Adobe的憑證授權單位DigiCert無法核發憑證。 因此，在此步驟完成之前，Adobe無法完成您的CNAME實作請求。

1. [填寫此表單](assets/FPC_Request_Form.xlsx) 並在您選擇加入時 [開啟請求CNAME支援的Adobe客戶服務票證](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?#reference_ACA3391A00EF467B87930A450050077C)：

   * [!DNL Adobe Target] 客戶代碼:
   * SSL憑證主機名稱(範例： `target.example.com target.example.org`)：
   * SSL憑證購買者(強烈建議Adobe，請參閱常見問題集)：Adobe/客戶
   * 如果客戶購買憑證（也稱為「自帶憑證」，BYOC），請填寫以下其他詳細資料：
      * 憑證組織（範例：範例Company Inc）：
      * 憑證組織單位（選用，例如：行銷）：
      * 憑證國家/地區（範例：美國）：
      * 憑證州/地區（範例：加州）：
      * 憑證城市（範例：聖荷西）：

1. 如果Adobe正在購買憑證，Adobe會與DigiCert合作來購買憑證，並將憑證部署在Adobe的生產伺服器上。

   如果客戶正在購買憑證(BYOC)，Adobe客戶服務會將憑證簽署要求(CSR)傳送給您。 透過您選擇的憑證授權單位購買憑證時，請使用CSR 。 核發憑證後，傳送憑證復本及任何中繼憑證給Adobe客戶服務進行部署。

   當您的實作就緒時，Adobe Client Care會通知您。

1. 更新 `serverDomain` [檔案](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverdomain) 至新的CNAME主機名稱並設定 `overrideMboxEdgeServer` 至 `false` [檔案](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver) （在您的at.js設定中）。

## 常見問題集

下列資訊回答有關在Target中請求和實作CNAME支援的常見問題：

### 我可以提供我自己的憑證（自備憑證或BYOC）嗎？

您可以提供自己的憑證。 不過，Adobe不建議使用此作法。 如果Adobe購買並控制憑證，則Adobe和您都能更輕鬆地管理SSL憑證生命週期。 SSL憑證必須每年續約。 因此，Adobe客戶服務必須每年與您聯絡，以及時取得新憑證。 有些客戶可能難以及時產生更新的憑證。 您的 [!DNL Target] 因為瀏覽器拒絕連線，憑證過期時危及實作。

>[!WARNING]
>
>如果您要求 [!DNL Target] 自備憑證CNAME實作，您每年都有責任向Adobe Client Care提供更新的憑證。 允許您的CNAME憑證在Adobe部署更新的憑證之前過期，會導致您的特定中斷 [!DNL Target] 實作。

### 我的新SSL憑證要多久才會過期？

所有Adobe購買的憑證有效期為一年。 另請參閱 [DigiCert關於1年憑證的文章](https://www.digicert.com/blog/position-on-1-year-certificates) 以取得詳細資訊。

### 我應該選擇哪些主機名稱？ 每個網域應該選擇幾個主機名稱？

Target CNAME實作只需要SSL憑證和客戶的DNS中每個網域有一個主機名稱。 Adobe建議每個網域使用一個主機名稱。 有些客戶出於其自身目的（例如在測試環境中測試）而要求每個網域有更多主機名稱，此做法受到支援。

大部分的客戶都會選擇類似的主機名稱 `target.example.com`. Adobe建議您遵循此作法，但最終還是由您來決定。 請勿要求現有DNS記錄的主機名稱。 這樣做會造成衝突，並延遲解決問題的時間 [!DNL Target] CNAME要求。

### 我已經有Adobe Analytics的CNAME實作，可以使用相同的憑證或主機名稱嗎？

否， [!DNL Target] 需要個別的主機名稱和憑證。

### 我目前的實施是否為 [!DNL Target] 是否受ITP 2.x影響？

Apple智慧型追蹤預防(ITP) 2.3版推出其CNAME遮蔽緩解功能，此功能可偵測 [!DNL Target] CNAME實施並將Cookie的有效期縮短為七天。 目前 [!DNL Target] 沒有因應ITP之CNAME遮蓋緩解的方法。 如需有關ITP的詳細資訊，請參閱 [Apple智慧型追蹤預防(ITP) 2.x](../before-implement/privacy/apple-itp-2x.md).

### 部署CNAME實作時，可能會發生哪些服務中斷？

部署憑證時沒有服務中斷（包括憑證續約）。

不過，當您變更主機名稱后， [!DNL Target] 實作程式碼(`serverDomain` （在at.js中）至新的CNAME主機名稱(`target.example.com`)，網頁瀏覽器會將回訪訪客視為新訪客。 回訪訪客的設定檔資料會遺失，因為舊主機名稱(`clientcode.tt.omtrdc.net`)。 由於瀏覽器安全模式的緣故，無法存取先前的Cookie。 這種中斷只會在初次切換至新CNAME時發生。 憑證更新不會產生相同的效果，因為主機名稱不會變更。

### 我的CNAME實作使用什麼金鑰型別和憑證簽章演演算法？

所有憑證預設為RSA SHA-256，金鑰則為RSA 2048位元。 目前不支援大於2048位元的金鑰大小。

### 如何驗證我的CNAME實作準備好進行流量？

使用以下命令集(在macOS或Linux命令列終端機中，使用bash和curl >=7.49)：

1. 將此bash函式複製並貼到您的終端機中，或將函式貼到bash啟動指令碼檔案中(通常是 `~/.bash_profile` 或 `~/.bashrc`)，因此函式可用於各終端機工作階段：

   ```
   function adobeTargetCnameValidation {
     local hostname="$1"
     if [ -z "$hostname" ]; then
       echo "ERROR: no hostname specified"
       return 1
     fi
   
     local service="Adobe Target CNAME implementation"
     local edges="31 32 34 35 36 37 38"
     local edgeDomain="tt.omtrdc.net"
     local edgeFormat="mboxedge%d%s.$edgeDomain"
     local shardFormat="-alb%02d"
     local shards=5
     local shardsFoundCount=0
     local shardsFound
     local shardsFoundOutput
     local curlRegex="subject:.*CN=|expire date:|issuer:"
     local curlValidation="SSL certificate verify ok"
     local curlResponseValidation='"OK"'
     local curlEndpoint="/uptime?mboxClient=uptime3"
     local url="https://$hostname$curlEndpoint"
     local sslLabsUrl="https://ssllabs.com/ssltest/analyze.html?hideResults=on&latest&d=$hostname"
     local success="✅"
     local failure="🚫"
     local info="🔎"
     local rule="="
     local horizontalRule="$(seq ${COLUMNS:-30} | xargs printf "$rule%.0s")"
     local miniRule="$(seq 5 | xargs printf "$rule%.0s")"
     local curlVersion="$(curl --version | head -1 | cut -d' ' -f2 )"
     local curlVersionRequired=">=7.49"
     local edgeCount="$(wc -w <<< "$edges" | tr -d ' ')"
     local edge
     local shard
     local currEdgeShard
     local dnsOutput
     local cnameExists
     local endToEndTestSucceeded
     local curlResult
   
     for shard in $(seq $shards); do
       if [ "$shardsFoundCount" -eq 0 ]; then
         for edge in $edges; do
           if [ "$shard" -eq 1 ]; then
             currEdgeShard="$(printf "$edgeFormat" "$edge" "")"
           else
             currEdgeShard="$(
               printf "$edgeFormat" "$edge" "$(
                 printf -- "$shardFormat" "$shard"
               )"
             )"
           fi
           curlResult="$(curl -vsm20 --connect-to "$hostname:443:$currEdgeShard:443" "$url" 2>&1)"
           if grep -q "$curlValidation" <<< "$curlResult"; then
             shardsFound+=" $currEdgeShard"
             if grep -q "$curlResponseValidation" <<< "$curlResult"; then
               shardsFoundCount=$((shardsFoundCount+1))
               shardsFoundOutput+="\n\n$miniRule $success $hostname [edge shard: $currEdgeShard] $miniRule\n"
             else
               shardsFoundOutput+="\n\n$miniRule $failure $hostname [edge shard: $currEdgeShard] $miniRule\n"
             fi
             shardsFoundOutput+="$(grep -E "$curlRegex" <<< "$curlResult" | sort)"
             if ! grep -q "$curlResponseValidation" <<< "$curlResult"; then
               shardsFoundOutput+="\nERROR: unexpected HTTP response from this shard using $url"
             fi
           fi
         done
       fi
     done
   
     echo
     echo "$horizontalRule"
     echo
     echo "$service validation for hostname $hostname:"
     dnsOutput="$(dig -t CNAME +short "$hostname" 2>&1)"
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
   
     curlResult="$(curl -vsm20 "$url" 2>&1)"
     if grep -q "$curlValidation" <<< "$curlResult"; then
       if grep -q "$curlResponseValidation" <<< "$curlResult"; then
         echo -en "$success $hostname passes TLS and HTTP response validation"
         if [ -n "$cnameExists" ]; then
           echo
         else
           echo " -- the DNS CNAME is not pointing to the correct subdomain for ${service}s with Adobe-managed certificates" \
             "(bring-your-own-certificate implementations don't have this requirement), but this test passes as configured"
         fi
         endToEndTestSucceeded=true
       else
         echo -n "$failure $hostname FAILED HTTP response validation --" \
           "unexpected response from $url -- "
         if [ -n "$cnameExists" ]; then
           echo "DNS is NOT pointing to the correct shard, notify Adobe Client Care"
         else
           echo "the required DNS CNAME record is missing, see above"
         fi
       fi
     else
   
       echo -n "$failure $hostname FAILED TLS validation -- "
       if [ -n "$cnameExists" ]; then
         echo "DNS is likely NOT pointing to the correct shard or there's a validation issue with the certificate or" \
           "protocols, see curl output below and optionally SSL Labs ($sslLabsUrl):"
         echo ""
         echo "$horizontalRule"
         echo "$curlResult" | sed 's/^/    /g'
         echo "$horizontalRule"
         echo ""
       else
         echo "the required DNS CNAME record is missing, see above"
       fi
     fi
   
     if [ "$shardsFoundCount" -ge "$edgeCount" ]; then
       echo -n "$success $hostname passes shard validation for the following $shardsFoundCount edge shards:"
       echo -e "$shardsFoundOutput"
       echo
   
       if [ -n "$cnameExists" ] && [ -n "$endToEndTestSucceeded" ]; then
         echo "$horizontalRule"
         echo ""
         echo "  For additional TLS/SSL validation, including detailed browser/client support,"
         echo "  see SSL Labs (click the first IP address if prompted):"
         echo ""
         echo "    $info  $sslLabsUrl"
         echo ""
         echo "  To check DNS propagation around the world, see whatsmydns.net:"
         echo ""
         echo "    $info  DNS A records:     https://whatsmydns.net/#A/$hostname"
         echo "    $info  DNS CNAME record:  https://whatsmydns.net/#CNAME/$hostname"
       fi
     else
       echo -n "$failure $hostname FAILED shard validation -- shards found: $shardsFoundCount," \
         "expected: $edgeCount"
       if bc -l <<< "$(cut -d. -f1,2 <<< "$curlVersion") $curlVersionRequired" 2>/dev/null | grep -q 0; then
         echo -n " -- insufficient curl version installed: $curlVersion, but this script requires curl version" \
           "$curlVersionRequired because it uses the curl --connect-to flag to bypass DNS and directly test" \
           "each Adobe Target edge shards' SNI confirguation for $hostname"
       fi
       if [ -n "$shardsFoundOutput" ]; then
         echo -e ":\n$shardsFoundOutput"
       fi
       echo
     fi
     echo
     echo "$horizontalRule"
     echo
   }
   ```

1. 貼上此命令(取代 `target.example.com` 主機名稱)：

   ```
   adobeTargetCnameValidation target.example.com
   ```

   如果實作準備就緒，您會看到如下所示的輸出。 重要部分是所有驗證狀態行都會顯示 `✅` 而非 `🚫`. 每個Target邊緣CNAME分割槽應顯示 `CN=target.example.com`，它會符合要求的憑證上的主要主機名稱（憑證上的其他SAN主機名稱不會列印在此輸出中）。

   ```
   $ adobeTargetCnameValidation target.example.com
   
   ==========================================================
   
   Adobe Target CNAME implementation validation for hostname target.example.com:
   ✅ target.example.com passes DNS CNAME validation
   ✅ target.example.com passes TLS and HTTP response validation
   ✅ target.example.com passes shard validation for the following 7 edge shards:
   
   ===== ✅ target.example.com [edge shard: mboxedge31-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge32-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge34-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge35-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge36-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge37-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge38-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ==========================================================
   
     For additional TLS/SSL validation, including detailed browser/client support,
     see SSL Labs (click the first IP address if prompted):
   
       🔎  https://ssllabs.com/ssltest/analyze.html?hideResults=on&latest&d=target.example.com
   
     To check DNS propagation around the world, see whatsmydns.net:
   
       🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
       🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com
   
   ==========================================================
   ```

>[!NOTE]
>
>如果此驗證命令在DNS驗證時失敗，但您已進行必要的DNS變更，則可能需要等待DNS更新完全傳播。 DNS記錄有一個關聯的 [TTL （存留時間）](https://en.wikipedia.org/wiki/Time_to_live#DNS_records) 指定這些記錄之DNS回覆的快取到期時間。 因此，您可能需要至少等待TTL的時間。 您可以使用 `dig target.example.com` 命令或 [G Suite Toolbox](https://toolbox.googleapps.com/apps/dig/#CNAME) 以查詢您特定的TTL。 若要檢查全球的DNS傳播，請參閱 [whatsmydns.net](https://whatsmydns.net/#CNAME).

### 如何使用具有 CNAME 的退出連結

如果您使用CNAME，選擇退出連結應包含「client=`clientcode` 引數，例如：
`https://my.cname.domain/optout?client=clientcode`.

取代 `clientcode` 輸入您的使用者端代碼，然後新增要連結至 [選擇退出URL](privacy/privacy.md).

## 已知限制

* 有CNAME和at.js 1.x時，QA模式沒有粘性，因為它是以協力廠商Cookie為基礎。 因應措施是將預覽引數新增到您導覽到的每個URL中。 有CNAME和at.js 2.x時，QA模式會很粘滯。
* 使用CNAME時，的Cookie標頭大小更有可能為 [!DNL Target] 呼叫數會增加。 Adobe建議將Cookie大小維持在8 KB以下。

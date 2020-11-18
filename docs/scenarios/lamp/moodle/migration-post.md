---
title: 如何在 Moodle 遷移之後追蹤
description: 瞭解如何在 Moodle 遷移之後進行後續追蹤。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 8594919772cfccf3dd02769a14d62f64cb1ad995
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714490"
---
# <a name="how-to-follow-up-after-a-moodle-migration"></a>如何在 Moodle 遷移之後追蹤

## <a name="post-migration-tasks"></a>移轉後工作

遷移後工作是最終的應用程式設定，包括設定記錄目的地、SSL 憑證，以及排程的工作/cron 作業。 此範圍的工作包括：

- 設定應用程式。
- 正在更新虛擬機器擴展集實例 (s) 中的記錄檔路徑。
- 正在重新開機虛擬機器擴展集實例 (s 中的伺服器) 。
- 正在更新憑證。
- 正在更新憑證位置。
- 正在更新 HTML 本機複本。
- 重新開機 PHP 和 nginx 伺服器。
- 將 DNS 名稱對應至 Azure Load Balancer IP。

## <a name="controller-virtual-machine-scale-set"></a>控制器虛擬機器擴展集

### <a name="log-paths"></a>記錄檔路徑

內部部署可能會有不同的記錄路徑位置，需要以 Azure 記錄路徑更新。 例如，/var/log/syslogs/moodle/access.log 和/var/log/syslogs/moodle/error.log。

- 更新記錄檔的位置。 此命令會開啟設定檔：

  ```bash
  nano /etc/nginx/nginx.conf
  ```

- 變更記錄檔路徑位置。

- 尋找 `access_log` 並 `error_log` 更新記錄檔路徑。

- 按下 `CTRL+O` 以儲存並結束 `CTRL+X` 。

### <a name="restart-servers"></a>重新開機伺服器

重新開機 nginx 和 php-php-fpm 伺服器：

  ```bash
  sudo systemctl restart nginx
  sudo systemctl restart php<phpVersion>-fpm
  ```

## <a name="controller-virtual-machine"></a>控制器虛擬機器

### <a name="certificates"></a>憑證

- 若要存取憑證，請登入控制器虛擬機器。

- Moodle 應用程式的憑證位於/moodle/certs。

- 將 .crt 和金鑰檔案複製到/moodle/certs/。 檔案名應變更為 nginx 和 nginx，才能由設定的 nginx 伺服器辨識。 根據您的本機環境，您可以選擇使用公用程式 SCP 或 WinSCP 之類的工具，將這些檔案複製到控制器虛擬機器。

- 用來變更憑證名稱的命令。

  ```bash
  cd /path/to/certs/location
  mv /path/to/certs/location/*.key /moodle/certs/nginx.key
  mv /path/to/certs/location/*.crt /moodle/certs/nginx.crt
  ```

- 您也可以產生自我簽署的憑證，這只適用于測試。

  ```bash
  openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /moodle/certs/nginx.key \
   -out /moodle/certs/nginx.crt \
  -subj "/C=US/ST=WA/L=Redmond/O=IT/CN=mydomain.com"
  ```

- 建議將憑證檔案設為唯讀給擁有者，並將這些檔案設為擁有者 `www-data:www-data` 。

  ```bash
  chown www-data:www-data /moodle/certs/nginx.*
   chmod 400 /moodle/certs/nginx.*
  ```

- 若要更新憑證位置並開啟設定檔：

  ```bash
  nano /etc/nginx/sites-enabled/*.conf
  ```

- 若要變更憑證的路徑位置，請尋找 `ssl_certificate` 。 更新憑證的路徑，如下所示：

```bash
   /moodle/certs/moodle/certs/nginx.crt;
   /moodle/certs/nginx.key;
```

- 按下 `CTRL+O` 以儲存檔案並結束 `CTRL+X` 。

### <a name="update-the-local-html-copy"></a>更新本機 HTML 複本

Moodle html 網站 (`/moodle/html/moodle`) 內容的本機複本會在虛擬機器擴展集中建立 `/var/www/html/moodle` 。 只有在時間戳記有更新時，才會更新本機複本。 從控制器虛擬機器執行下列命令，以更新時間戳記。

  ```bash
  sudo -s
  /usr/local/bin/update_last_modified_time.moodle_on_azure.sh
  ```

- 每次執行上次修改時間戳記檔案中的腳本 (`/moodle/html/moodle/last_modified_time.moodle_on_azure`) 時， `/moodle/html/moodle` 會在本機複製 () 更新目錄內容 `/var/www/html` 。

### <a name="restart-servers"></a>重新開機伺服器

- 重新開機 `nginx` 和 `php-fpm` 伺服器。

  ```bash
  sudo systemctl restart nginx
  sudo systemctl restart php<phpVersion>-fpm
  ```

### <a name="map-the-dns-name-to-the-azure-load-balancer-ip"></a>將 DNS 名稱對應至 Azure Load Balancer IP

- Azure Load Balancer IP 的 DNS 名稱對應必須在主機服務提供者層級進行。

- 停用 Moodle 網站上的 **維護模式** 。

- 在控制器虛擬機器中執行下列命令：

  ```bash
  sudo /usr/bin/php admin/cli/maintenance.php --disable
  ```

- 若要檢查 Moodle 網站的狀態，請執行下列命令：

  ```bash
  sudo /usr/bin/php admin/cli/maintenance.php
  ```

- 點擊 DNS 名稱以取得遷移的 Moodle 網頁。

## <a name="frequently-asked-questions-and-troubleshooting"></a>常見問題和疑難排解

1. 錯誤：資料庫連接失敗：針對 _資料庫連接失敗_ 之類的錯誤，或 _無法連接到您指定的資料庫_，可能的原因和解決方案如下：

    - 未安裝或正在執行您的資料庫伺服器。 若要檢查 MySQL 是否有此情況，請嘗試輸入下列命令：

      ```bash
      $telnet database_host_name 3306
      ```

    - 您應該會取得不需要的回應，其中包含 MySQL 伺服器的版本號碼。

    - 如果您嘗試在不同的埠上執行兩個 Moodle 實例，請在設定中使用主機 (不是 localhost) 的 IP 位址 `$CFG->dbhost` ; 例如 `$CFG->dbhost = 127.0.0.1:3308` 。

    - 您尚未建立 Moodle 資料庫或指派具有正確許可權的使用者來存取它。

    - Moodle 資料庫設定不正確。 Moodle 設定檔中的資料庫名稱、資料庫使用者或資料庫使用者密碼 `config.php` 不正確。

    - 檢查您的 MySQL 使用者名稱或密碼中沒有任何單引號或非字母字母。

2. 錯誤： _500：內部伺服器錯誤_：發生此錯誤的可能原因有好幾個。 首先請檢查您的 web 伺服器錯誤記錄檔，這應該會有更完整的說明。 以下是一些已知的可能性：

    - 或檔案中有語法錯誤 `.htaccess` `httpd.conf` 。 指示詞的寫入方式會因您所使用的檔案而有所不同。 您可以使用下列命令來測試 nginx 檔中的設定錯誤：

      `nginx -t`

    - Web 服務器會以您自己的使用者名稱執行，而且所有檔案的最大許可權等級為755。 檢查是否已在 [控制台] 中為您的 Moodle 目錄設定此設定，或如果您有 shell 的存取權，請使用此命令：

      ```bash
      chmod -R 755 moodle
      ```

3. 錯誤： _403：禁止_。

    此錯誤表示 PHP memory_limit 值對 PHP 腳本而言不足。 Memory_limit 值是允許的記憶體大小， `64M` 在上述範例中 (67108864 個位元組/1024 = 65536 KB。 65536 KB/1024 = 64 MB) 。 您必須增加 PHP memory_limit 值，直到此訊息消失為止。 有兩種方法可以執行這項操作：

    方法1：針對託管安裝，您應該要求您的主機支援如何執行這項操作。 許多允許 `.htaccess` 的檔案。 如果有的話，請將下列這一行新增至您 `.htaccess` 的檔案，或在 Moodle 目錄中建立檔案（如果尚未存在的話）：

      ```bash
      php_value memory_limit <value>M
      Example: php_value memory_limit 40M
      ```

    方法2：如果您有自己的伺服器搭配 shell 存取，請編輯您的檔案 `php.ini` 。 簽入您的輸出，以確定它是正確的 `phpinfo` ：

      ```bash
      memory_limit <value>M
      Example: memory_limit 40M
      ```

    請記住，您需要重新開機您的 web 伺服器，才能讓變更 `php.ini` 生效。 替代方法是 `memory_limit` 使用 ' memory_limit 0 ' 命令停用。

4. 無法登入;卡在登入畫面上。

    如果您看到您的會話已超時，也可以套用此 _程式。請重新登入。_ 或偵測 _到會影響您登入會話的伺服器錯誤。請重新登入，或重新開機瀏覽器。_ 以下是您可以採取來解決此問題的可能原因和動作：

    - 首先，檢查您的主要系統管理員帳戶是否為另一個手動帳戶，也是問題所在。 如果您的使用者使用外部驗證方法 (例如，LDAP) ，則這可能是問題所在。 找出原因，並確認它已 Moodle，然後再繼續進行。

    - 檢查您的硬碟未滿、您的伺服器在共用主機上，而且您尚未達到您的磁碟空間配額。 這可防止建立新的會話，而且沒有人能夠登入。

    - 請仔細檢查您區域中的許可權 `moodledata` 。 網頁伺服器必須能夠寫入 `sessions` 子目錄。

5. 嚴重錯誤： _$CFG >dataroot 無法寫入。系統管理員必須修正目錄許可權！_ 正在結束。

    - 檢查 Moodle 和 moodledata 許可權是否為 www 資料：僅限 www 資料。 如果不是，請變更群組和擁有權許可權。 下列命令會更新許可權：

      ```bash
      sudo chown -R /moodle/moodledata
      ```

6. 找不到最上層課程。

    - 如果在您嘗試安裝 Moodle 後立即顯示，可能表示安裝未完成。 完整的安裝將會要求您提供系統管理員設定檔，並在完成之前將網站命名。 檢查您的記錄檔是否有錯誤，然後卸載資料庫以重新開機。 如果您使用 web 型安裝程式，請嘗試命令列1。

7. 登入時，登入連結不會變更。 我已登入，但無法自由導覽。 請確定您的設定中的 URL 與 `$CFG->wwwroot` 您實際用來存取網站的 URL 完全相同。

8. 上傳檔案時發生錯誤：

    - 如果您在上傳檔案時收到「找 _不_ 到檔案」錯誤，表示您的 web 伺服器上未啟用斜線引數。 請嘗試啟用。

    - 如果您的網頁伺服器不支援斜線引數，則可以在 Moodle 中停用這些引數，方法是 unticking Administration > Site Administration > Server > HTTP 的 [ **使用斜線引數** ] 核取方塊。

      > [!WARNING]
      > 停用斜線引數會導致 SCORM 套件無法運作，而且會顯示斜線引數警告！

9. 網站卡在 **維護模式** 中。 有時候 Moodle 會卡在 **維護模式** 中，而 _此網站正在進行維護的訊息，而且目前無法使用_ ，因為您嘗試將其關閉。 當您將 Moodle 置於 **維護模式** 時，它會 `maintenance.html` 在網站檔案的目錄中建立檔案 `moodledata/maintenance.html` 。 若要修正此問題，請嘗試下列方法：

    - 檢查 web 伺服器使用者是否有 moodledata 目錄的寫入權限。
    - 手動刪除檔案 `maintenance.html` 。

10. 何處可以找到記錄：

    - Syslog: 
      - 當有人存取頁面時，可能會產生錯誤或存取記錄。
      - 它們會在此位置進行捕捉： `/var/log/nginx/` 。

    - Cron 記錄：
      - Cron 作業將會執行，並會更新實例中的本機複本。
      - 路徑為 `/var/log/sitelogs/moodle/cron.log` 。

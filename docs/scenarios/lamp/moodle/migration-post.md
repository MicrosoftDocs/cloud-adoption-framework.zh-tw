---
title: 如何在 Moodle 遷移之後追蹤
description: 瞭解如何在 Moodle 遷移之後進行後續追蹤。 瞭解如何更新記錄檔路徑、重新開機伺服器，以及採取完成遷移所需的其他步驟。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/30/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 4975037dc30aa95bcb4bc58d69914970769e3183
ms.sourcegitcommit: 32e8e7a835a688eea602f2af1074aa926ab150c3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687739"
---
# <a name="how-to-follow-up-after-a-moodle-migration"></a>如何在 Moodle 遷移之後追蹤

## <a name="post-migration-tasks"></a>移轉後工作

在遷移 Moodle 之後，您必須負責進行某些遷移後工作，以完成您的設定。 這些工作包括：

- 正在更新虛擬機器擴展集實例中的記錄檔路徑。
- 正在重新開機虛擬機器擴展集實例中的伺服器。
- 正在更新憑證。
- 正在更新憑證位置。
- 正在更新 HTML 本機複本。
- 重新開機 PHP 和 nginx 伺服器。
- 將 DNS 名稱對應至 Azure Load Balancer IP 位址。

## <a name="controller-virtual-machine-scale-set"></a>控制器虛擬機器擴展集

請採取下列步驟來完成您的虛擬機器擴展集設定。 您將需要使用私人 IP 位址，以 SSH 連線到您的虛擬機器擴展集實例，如 [存取虛擬機器擴展集](azure-infra-config.md#access-the-virtual-machine-scale-set)所述。

### <a name="update-log-paths"></a>更新記錄檔路徑

您的內部部署環境和 Azure 可能會將記錄檔儲存在不同的位置。 例如，您可能需要更新這些記錄檔路徑：

- `/var/log/syslogs/moodle/access.log`
- `/var/log/syslogs/moodle/error.log`

請遵循下列步驟來更新記錄檔位置：

1. 輸入此命令以開啟設定檔：

   ```bash
   nano /etc/nginx/nginx.conf
   ```

2. 尋找 `access_log` 並 `error_log` 更新記錄檔路徑。

3. 按 CTRL + O 儲存變更，然後按 CTRL + X 關閉檔案。

### <a name="restart-servers"></a>重新開機伺服器

輸入下列命令以重新開機 nginx 和 php php-fpm 伺服器：

```bash
sudo systemctl restart nginx
sudo systemctl restart php<php version>-fpm
```

## <a name="controller-virtual-machine"></a>控制器虛擬機器

請採取下列步驟來完成控制器虛擬機器設定。

### <a name="update-security-certificates"></a>更新安全性憑證

1. 登入控制器虛擬機器。 您可以在資料夾中找到 Moodle 應用程式的憑證 `/moodle/certs` 。

1. 將和檔案複製 `.crt` `.key` 到 `/moodle/certs/` 。 分別將檔案名變更為 `nginx.crt` 和 `nginx.key` ，讓設定的 nginx 伺服器能夠辨識這些名稱。 如果您的本機環境支援 SCP 公用程式或工具（例如 WinSCP），您可以使用這些工具將這些檔案複製到控制器虛擬機器。 否則，請使用下列命令：

   ```bash
   cd /<path to certs location>
   mv /<path to certs location>/*.key /moodle/certs/nginx.key
   mv /<path to certs location>/*.crt /moodle/certs/nginx.crt
   ```

   若要複製檔案，您可以使用下列命令來產生自我簽署的憑證：

   ```bash
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
   -keyout /moodle/certs/nginx.key \
   -out /moodle/certs/nginx.crt \
   -subj "/C=US/ST=WA/L=Redmond/O=IT/CN=mydomain.com"
   ```

   您只能使用自我簽署憑證進行測試。

1. 建議的憑證檔案是由擁有者所擁有，而且對擁有者而言 `www-data:www-data` 是唯讀的。 輸入下列命令來進行這些變更：

   ```bash
   chown www-data:www-data /moodle/certs/nginx.*
   chmod 400 /moodle/certs/nginx.*
   ```

1. 採取下列步驟來更新憑證位置：

   1. 輸入此命令以開啟設定檔：

      ```bash
      nano /etc/nginx/sites-enabled/*.conf
      ```

   1. `ssl_certificate`在檔案中尋找。

   1. 將憑證的路徑取代為下列值：

      ```bash
      /moodle/certs/moodle/certs/nginx.crt;
      /moodle/certs/nginx.key;
      ```

    1. 按 CTRL + O 儲存您的變更，然後按 CTRL + X 關閉檔案。

### <a name="update-local-html-copy"></a>更新本機 HTML 複製

Moodle HTML 網站內容的本機複本 `/moodle/html/moodle` 是在此資料夾的虛擬機器擴展集中建立： `/var/www/html/moodle` 。 只有在時間戳記變更時，才會更新本機複本。 在控制器虛擬機器中輸入下列命令，以更新時間戳記：

```bash
sudo -s
/usr/local/bin/update_last_modified_time.moodle_on_azure.sh
```

上次修改時間戳記檔案 `/moodle/html/moodle/last_modified_time.moodle_on_azure` 包含腳本。 每次腳本執行時， `/moodle/html/moodle` 都會更新本機複本中的目錄內容 `/var/www/html` 。

### <a name="restart-servers"></a>重新開機伺服器

輸入下列命令以重新開機 `nginx` 和 `php-fpm` 伺服器：

```bash
sudo systemctl restart nginx
sudo systemctl restart php<php version>-fpm
```

### <a name="map-the-dns-name-to-the-azure-load-balancer-ip-address"></a>將 DNS 名稱對應至 Azure Load Balancer IP 位址

請在主機服務提供者層級遵循下列步驟，將 DNS 名稱對應至 Azure Load Balancer IP：

1. 在控制器虛擬機器中輸入下列命令，以關閉 Moodle 網站上的維護模式：


   ```bash
   sudo /usr/bin/php admin/cli/maintenance.php --disable
   ```

1. 輸入此命令以檢查 Moodle 網站的狀態：

   ```bash
   sudo /usr/bin/php admin/cli/maintenance.php
   ```

1. 移至 DNS 名稱，以查看遷移的 Moodle 網頁。

## <a name="frequently-asked-questions-and-troubleshooting"></a>常見問題和疑難排解

當您有關于 Moodle 遷移的問題時，請參閱下列資訊。 這些記錄檔也可協助您針對問題進行疑難排解：

- Syslog 檔案：

  - 每當使用者移至您的網頁時，系統就會產生錯誤記錄檔或存取記錄檔。
  - 您可以在此資料夾中找到它們： `/var/log/nginx/` 。

- Cron 記錄檔：
  - 當 cron 作業執行時，它會更新記錄檔的本機複本。
  - 您可以在此資料夾中找到此檔案： `/var/log/sitelogs/moodle/cron.log` 。

### <a name="database-connection-failure"></a>資料庫連接失敗

針對 *資料庫連接失敗* 或 *無法連接到您指定之資料庫* 的錯誤，以下是一些可能的原因和解決方案：

- 未安裝或正在執行您的資料庫伺服器。 若要在 MySQL 中檢查這個條件，請輸入下列命令：

  ```bash
  $telnet database_host_name 3306
  ```

  如果您的資料庫正在執行，您應該會收到包含 MySQL 伺服器版本號碼的回應。

- 未正確設定主機位址。 如果您在不同的埠上執行兩個 Moodle 實例，請在設定中使用主機的 IP 位址，而不是 `localhost` `$CFG->dbhost` 。 例如，使用：

  `$CFG->dbhost = 127.0.0.1:3308`

- 您尚未建立 Moodle 資料庫。 或者，您尚未指派適當的許可權來存取資料庫。 檢查資料庫和您授與的許可權。

- Moodle 資料庫設定不正確。 比方說，Moodle 設定檔中的資料庫名稱、使用者名稱或密碼 `config.php` 不正確。 請確定您的 MySQL 使用者名稱和密碼未包含單引號或非英數位元。

### <a name="internal-server-error"></a>內部伺服器錯誤

此錯誤有幾個可能的原因： *500：內部伺服器錯誤*。 一開始請先檢查您的 web 伺服器錯誤記錄檔，其中應該包含詳細說明。 以下是一些可能性：

- 或檔案中有語法錯誤 `.htaccess` `httpd.conf` 。 指示詞的正確語法會因您所使用的檔案而有所不同。 使用下列命令來測試 nginx 檔中的設定錯誤：

  ```bash
  `nginx -t`
  ```

- Web 服務器會以您自己的使用者名稱執行，而且存取權限不正確。 在此情況下，所有檔案都需要最大許可權等級755。 檢查是否已在 [控制台] 中為您的 Moodle 目錄設定此層級。 或者，如果您有 shell 的存取權，請使用此命令來設定層級：

  ```bash
  chmod -R 755 moodle
  ```

### <a name="memory-limit-error"></a>記憶體限制錯誤

當 *403：禁止* 的錯誤發生時，php `memory_limit` 值不夠大，無法用於 php 腳本。 此 `memory_limit` 值為允許的記憶體大小。 `memory_limit`以較小的數量增加 PHP 值，直到訊息消失為止。 使用下列其中一種方法：

- 若為託管安裝，請要求您的主機支援如何增加值。 許多環境會使用檔案 `.htaccess` 。 如果您的安裝已安裝，請在您的檔案中新增下列程式 `.htaccess` 程式碼：

  ```bash
  php_value memory_limit <value>M
  ```

  例如，若要將值增加為 40 mb，請輸入：

  `php_value memory_limit 40M`

  如果檔案不 `.htaccess` 存在，請在 Moodle 目錄中使用該行建立一個檔案。

- 如果您有自己的伺服器具有 shell 存取權，請編輯您的檔案 `php.ini` 。 然後重新開機您的 web 伺服器，以套用您在中所做的變更 `php.ini` 。 若要確定您已正確更新此值，請輸入下列命令：

  ```bash
  `phpinfo`
  ```

  的輸出 `phpinfo` 應該包含類似下面的行：

  ```bash
  memory_limit <value>M
  ```

  例如，它可能包含下列程式程式碼：

  `memory_limit 40M`

增加值的替代 `memory_limit` 方式是輸入下列命令來關閉記憶體限制：

  ```bash
memory_limit 0
```

### <a name="sign-in-errors"></a>登入錯誤

有時您無法登入，或看到下列其中一則訊息：

- *您的會話已超時。請重新登入。*

- *偵測到會影響您登入會話的伺服器錯誤。請重新登入，或重新開機瀏覽器。*

您的驗證方法可能有問題，特別是當您使用 LDAP 之類的外部方法來驗證使用者時。 嘗試登入另一個手動帳戶，例如您的主要系統管理員帳戶。 如果您無法登入，請檢查您的驗證。 如果您可以登入其他帳戶，以下是 Moodle 登入問題的可能原因和解決方案：

- 硬碟可能已滿。 在此情況下，Moodle 無法建立新的會話，且使用者無法登入。 檢查您的硬碟未滿、您的伺服器在共用主機上，而且您尚未達到您的磁碟空間配額。

- Web 服務器無法寫入 `sessions` 子目錄。 請仔細檢查您區域中的許可權 `moodledata` 。

### <a name="fatal-errors"></a>嚴重錯誤

如果您看到此錯誤，Moodle 和 moodledata 許可權可能不正確：*嚴重錯誤： $CFG->dataroot 無法寫入。系統管理員必須修正目錄許可權！* 正在結束。

檢查這些許可權是否 `www-data:www-data` 只有。 如果許可權位於不同層級，請使用此命令變更群組和擁有權許可權：

```bash
sudo chown -R /moodle/moodledata
```

### <a name="top-level-course-errors"></a>最高層級的課程錯誤

如果您在安裝 Moodle 後找不到最上層課程，則安裝可能尚未完成。 在完成安裝結束之前，Moodle 會要求您提供系統管理員設定檔，並提示您為網站命名。 如果缺少這些步驟，請檢查記錄檔中是否有錯誤。 然後重新開機資料庫。 如果您使用 web 型安裝程式，請使用命令列再次安裝 Moodle。

### <a name="navigation-errors"></a>流覽錯誤

如果您無法在登入後於 Moodle 中自由流覽，您的 URL 設定可能不正確。 請確定您設定中的 URL 與 `$CFG->wwwroot` 您用來存取網站的 URL 相同。

### <a name="file-upload-errors"></a>檔案上傳錯誤

如果您在上傳檔案時看到「找 *不* 到檔案」錯誤，則您的 web 伺服器可能未開啟斜線引數。

- 如果您的 web 伺服器支援斜線引數，請將其開啟。

- 如果您的網頁伺服器不支援斜線引數，請清除 **Administration** Site administration server HTTP 中的 [**使用斜線引數**] 核取方塊，在 Moodle 中將它們關閉  >    >    >  ****。 您可能會看到此訊息。

  > [!WARNING]
  > 停用斜線引數會導致 SCORM 套件無法運作，而且會顯示斜線引數警告！

### <a name="maintenance-mode-errors"></a>維護模式錯誤

當 Moodle 處於維護模式，而您嘗試離開該模式時，有時您會看到此訊息： *此網站正在進行維護，目前無法使用*。 當 `maintenance.html` Moodle 在 `moodledata` 進入維護模式時，資料夾中所建立的檔案有問題時，就會出現這種情況。 在此情況下，請執行下列步驟：

- 檢查 web 伺服器使用者是否在目錄中具有寫入權限 `moodledata` 。
- 手動刪除檔案 `maintenance.html` 。

## <a name="next-steps"></a>後續步驟

- [適用於 MySQL 的 Azure 資料庫文件](/azure/mysql/)
- [什麼是虛擬機器擴展集？](/azure/virtual-machine-scale-sets/overview)
- [儲存體帳戶概觀](/azure/storage/common/storage-account-overview)

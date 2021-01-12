---
title: Moodle 手動遷移步驟
description: 請遵循下列步驟，將 Moodle 內部部署備份封存匯入至 Azure 資源，並設定 Moodle 應用程式。
author: UmakanthOS
ms.author: brblanch
ms.date: 11/30/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: think-tank
ms.openlocfilehash: 729131cf4245dbda7c48e7067cf224fbff927c67
ms.sourcegitcommit: 32a958d1dd2d688cb112e9d1be1706bd1e59c505
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98123539"
---
# <a name="moodle-manual-migration-steps"></a>Moodle 手動遷移步驟

本文說明將內部部署 Moodle 封存遷移至 Azure 的步驟。 此 Moodle 封存的內容包含 Moodle 應用程式、相關的設定，以及來自內部部署 Moodle 部署的資料庫複本。 成功將內部部署備份匯入到 Azure 基礎結構之後，您就可以執行 Moodle 的設定更新。

開始此程式之前，請務必完成這些文章中的所有步驟：
- [如何準備進行 Moodle 遷移](migration-pre.md)
- [Moodle 遷移架構和範本](migration-arch.md)

在 Azure Resource Manager (ARM) 範本部署完成後，請登入 [Azure 入口網站](https://portal.azure.com/) ，然後移至您在部署程式中建立的資源群組。 檢查新建立之基礎結構資源的清單。 根據您用於部署的 ARM 範本，建立的資源看起來會與下圖類似。

![顯示在 Moodle 遷移資源群組中建立之基礎結構資源的螢幕擷取畫面。](images/resource-creation-overview.png)

## <a name="copy-the-moodle-archive"></a>複製 Moodle 封存

遷移程式的第一個步驟是將 Moodle 備份封存從 Azure Blob 儲存體複製到控制器虛擬機器 (VM) 以進行 Moodle 部署。 這是您在 [建立](migration-pre.md#create-an-archive)封存時所建立的相同封存。

### <a name="sign-in-to-the-controller-virtual-machine"></a>登入控制器虛擬機器

1. 使用免費的開放原始碼終端機模擬器或序列主控台工具（例如 [PuTTY](https://www.putty.org/) ）來登入控制器 VM。
   
1. 在 [ **PuTTY** 設定] 中，輸入控制器 VM 的公用 IP 位址作為 **主機名稱**。
   
1. 在左側導覽中，展開 [ **SSH**]。
   
   ![[PuTTY 設定] 頁面的螢幕擷取畫面。](images/putty-configuration.png)
   
1. 選取 [ **驗證**]，然後尋找您用來部署 Azure 基礎結構與 ARM 範本的 SSH 金鑰檔案。
   
1. 選取 [開啟]  。 針對 [使用者名稱]，輸入 **azureadmin**，因為它是在範本中硬式編碼。
   
   ![顯示 SSH 驗證設定的 [PuTTY 設定] 頁面螢幕擷取畫面。](images/putty-ssh-key.png)
   
如需 PuTTY 的詳細資訊，請參閱 [PuTTY 一般常見問題/疑難排解問題](https://documentation.help/PuTTY/faq.html)。

### <a name="download-and-install-azcopy-on-the-controller-vm"></a>在控制器 VM 上下載並安裝 AzCopy

在您登入控制器 VM 之後，請執行下列命令來安裝 AzCopy：

  ```bash
  sudo -s
  wget https://aka.ms/downloadazcopy-v10-linux
  tar -xvf downloadazcopy-v10-linux
  sudo rm /usr/bin/azcopy
  sudo cp ./azcopy_linux_amd64_*/azcopy /usr/bin/
  ```

### <a name="back-up-the-current-configuration"></a>備份目前的設定

開始匯入程式之前，建議您先備份預設或目前的設定。

1. 建立備份目錄：
   
   ```bash
   cd /home/azureadmin/
   mkdir -p backup
   mkdir -p backup/moodle
   mkdir -p backup/moodle/html
   ```
   
1. 建立 `moodle` 和目錄的備份 `moodledata` ：
   
   ```bash
   mv /moodle/html/moodle /home/azureadmin/backup/moodle/html/moodle
   mv /moodle/moodledata /home/azureadmin/backup/moodle/moodledata
   ```
   
### <a name="copy-the-moodle-archive-to-the-controller-vm"></a>將 Moodle 封存複製到控制器 VM

1. 執行下列命令，以將壓縮的 `storage.tar.gz` 備份檔案從 Azure Blob 儲存體下載至控制器 VM `/home/azureadmin/` 目錄：
   
   ```bash
   sudo -s
   cd /home/azureadmin/
   azcopy copy "https://<storageaccount>.blob.core.windows.net/<container>/<BlobDirectoryName><SAStoken>" "/home/azureadmin/storage.tar.gz"
   ```
   
   替代您自己的儲存體帳戶和 SAS 權杖值。 例如：
   
   `azcopy copy "https://onpremisesstorage.blob.core.windows.net/migration/storage.tar.gz?sv=2019-12-12&ss=" "/home/azureadmin/storage.tar.gz"`
   
1. 將壓縮檔案解壓縮至目錄。
   
   ```bash
   cd /home/azureadmin
   tar -zxvf storage.tar.gz
   ```
  
### <a name="import-moodle-files-to-azure"></a>將 Moodle 檔案匯入至 Azure

解壓縮之後，您可以在底下找到 `storage` 目錄 `home/azureadmin` 。 此 `storage` 目錄包含 `moodle` 、 `moodledata` 和設定目錄，以及資料庫備份檔案。 您可以在下列步驟中，將每個檔案和目錄複寫到目標位置：

1. 將 `moodle` 和 `moodledata` 目錄複寫到共用位置 `/moodle` 。
   
   ```bash
   cp -rf /home/azureadmin/storage/moodle /moodle/html/
   cp -rf /home/azureadmin/storage/moodledata /moodle/moodledata
   ```

## <a name="import-the-moodle-database-to-azure"></a>將 Moodle 資料庫匯入至 Azure

連接到適用於 MySQL 的 Azure 資料庫伺服器，並將內部部署 Moodle 資料庫封存匯入適用於 MySQL 的 Azure 資料庫。

### <a name="connect-to-the-mysql-server"></a>連線至 MySQL 伺服器

適用於 MySQL 的 Azure 資料庫實例會受到防火牆保護。 預設會拒絕伺服器的所有連線以及伺服器內的資料庫。 第一次連線到適用於 MySQL 的 Azure 資料庫之前，請將防火牆設定為允許控制器 VM 的公用 IP 位址或 IP 位址範圍的存取。

您可以使用 Azure 命令列 (Azure CLI) 或 Azure 入口網站來設定防火牆。

執行下列 Azure CLI 命令，並以您自己的預留位置值取代：

```azurecli
az mysql server firewall-rule create --resource-group <myresourcegroup> --server <mydemoserver> --name <AllowMyIP> --start-ip-address <192.168.0.1> --end-ip-address <192.168.0.1>
```

或者，在 Azure 入口網站中，從您已部署的 Moodle 基礎結構資源中選取適用於 MySQL 的 Azure 資料庫伺服器。 在 [伺服器] 頁面的左側導覽中，選取 [ **連接安全性**]。

您可以在這裡新增允許的 IP 位址和設定防火牆規則。 建立規則之後，請選取 [ **儲存** ]。

![適用於 MySQL 的 Azure 資料庫伺服器的 [連線安全性] 窗格螢幕擷取畫面。](images/database-connection-security.png)

您現在可以使用 [mysql](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) 命令列工具或 [mysql 工作臺](https://dev.mysql.com/doc/workbench/en/)來連接至 mysql 伺服器。

![[MySQL 工作臺設定新連線] 畫面的螢幕擷取畫面。](images/database-connection.png)

若要取得連線資訊，請移至您在 Azure 入口網站中的 MySQL 伺服器的 **[總覽** ] 頁面。 使用每個欄位旁的複製圖示來複製 **伺服器名稱** 和 **伺服器管理員登入名稱**。

例如，您的伺服器名稱可能是 `mydemoserver.mysql.database.azure.com` ，而伺服器管理員登入名稱可能是 `myadmin@mydemoserver` 。

您也需要密碼。 如果您需要重設密碼，請選取功能表列中的 [ **重設密碼** ]。

在下列各節中，請使用這些資料庫伺服器詳細資料。

### <a name="import-the-moodle-database-to-azure-database-for-mysql"></a>將 Moodle 資料庫匯入適用於 MySQL 的 Azure 資料庫

1. 建立 MySQL 資料庫，以將內部部署資料庫匯入：
   
   ```bash
   mysql -h $server_name -u $server_admin_login_name -p$admin_password -e "CREATE DATABASE $moodledbname CHARACTER SET utf8;"
   ```
   
1. 將正確的許可權指派給資料庫：
   
   ```bash
   mysql -h $server_name -u $server_admin_login_name -p$admin_password -e "GRANT ALL ON $moodledbname.* TO '$server_admin_login_name' IDENTIFIED BY '$admin_password';"
   ```
   
1. 匯入資料庫：
   
   ```bash
   mysql -h $server_name -u $server_admin_login_name -p$admin_password $moodledbname < /home/azureadmin/storage/database.sql
   ```

## <a name="update-configurations"></a>更新設定

將內部部署 Moodle 資料庫封存匯入適用於 MySQL 的 Azure 資料庫之後，請視需要更新控制器 VM 上的下列設定：

- 更新 Moodle 設定檔。
- 設定目錄許可權。
- 設定 PHP 和 nginx web 伺服器。
- 更新 DNS 名稱和其他變數。
- 安裝任何遺漏的 PHP 延伸模組。
- 確定控制器 VM 上的 web 伺服器實例已停止。
- 將配置檔案複製到共用位置，以複製到虛擬機器擴展集。

### <a name="update-the-moodle-config-file"></a>更新 Moodle 設定檔

更新 Moodle 設定檔中的資料庫詳細資料參數 `/moodle/config.php` 。

若要取得這項工作的 DNS 名稱：

1. 在 Azure 入口網站中，從您已部署的 Moodle 基礎結構資源中選取 **Load Balancer 公用 IP 位址** 。
   
1. 在 [ **總覽** ] 頁面上，選取 **DNS 名稱** 旁邊的複製圖示。
   
若要更新檔案 `config.php` ：

1. 在編輯器中輸入下列要編輯的命令 `config.php` `nano` ：
   
   ```bash
   cd /moodle/html/moodle/
   nano config.php
   ```
   
1. 使用您從 Azure 入口網站複製的值，更新檔案中的資料庫詳細資料：
   
   ```php
   $CFG->dbhost    = 'localhost';                // Change 'localhost' to the server name.
   $CFG->dbname    = 'moodle';                   // Change 'moodle' to the newly created database name.
   $CFG->dbuser    = 'root';                     // Change 'root' to the server admin login name.
   $CFG->dbpass    = 'password';                 // Change 'password' to the server admin login password.
   $CFG->wwwroot   = 'https://on-premises.com';  // Change 'on-premises' to the DNS name.
   $CFG->dataroot  = '/var/moodledata';          // Change the path to '/moodle/moodledata'.
   ```
   
1. 進行變更之後，請按 CTRL + O 儲存檔案，並按 CTRL + X 結束編輯器。

您可以將內部部署目錄儲存在 `dataroot` 任何位置。

### <a name="configure-directory-permissions"></a>設定目錄許可權

- 指派755和 www 資料擁有者：群組對目錄的許可權 `moodle` 。
  
  ```bash
  sudo chmod 755 /moodle/html/moodle sudo chown -R www-data:www-data /moodle/html/moodle
  ```
  
- 指派770和 www 資料擁有者：群組對目錄的許可權 `moodledata` 。
  
  ```bash
  sudo chmod 770 /moodle/moodledata sudo chown -R www-data:www-data /moodle/moodledata
  ```
  
### <a name="update-web-config-files"></a>更新 web 設定檔

備份和更新 nginx 檔案 `conf` ：

```bash
sudo mv /etc/nginx/sites-enabled/*.conf /home/azureadmin/backup/
cd /home/azureadmin/storage/configuration/
sudo cp -rf nginx/sites-enabled/*.conf /etc/nginx/sites-enabled/
```

備份和更新 PHP 檔案 `www.conf` ：

```bash
_PHPVER='/usr/bin/php -r "echo PHP_VERSION;" | /usr/bin/cut -c 1,2,3'
echo $_PHPVER
sudo mv /etc/php/$_PHPVER/fpm/pool.d/www.conf /home/azureadmin/backup/www.conf
sudo cp -rf /home/azureadmin/storage/configuration/php/$_PHPVER/fpm/pool.d/www.conf /etc/php/$_PHPVER/fpm/pool.d/
```

### <a name="update-nginx-configuration-variables"></a>更新 nginx 設定變數

將 Azure 雲端 DNS 名稱更新為內部部署 Moodle 應用程式的 DNS 名稱。

1. 開啟 nginx 設定檔：
   
   ```bash
   nano /etc/nginx/sites-enabled/*.conf
   ```
   
1. ARM 範本部署會將 nginx 伺服器設定為埠81。 如果檔案不是81，請將檔案中的更新 `SERVER_PORT` 為81。
   
1. 更新 `server_name` 。 例如，若為 `server_name on-premises.com` ，則 `on-premises.com` 使用 DNS 名稱更新。 在大部分情況下，DNS 名稱在遷移時不會變更。
   
1. 更新 HTML `root` 目錄位置。 例如，將 `root /var/www/html/moodle;` 更新為 `root /moodle/html/moodle;`。
   
   內部部署根目錄可以位於任何位置。
   
1. 進行變更之後，請按 CTRL + O 儲存檔案，並按 CTRL + X 結束。

### <a name="install-any-missing-php-extensions"></a>安裝任何遺漏的 PHP 延伸模組

ARM 部署範本會安裝下列 PHP 擴充功能： php-fpm、cli、捲曲、zip、梨、g.、dev、mcrypt、soap、json、redis、bcmath、gd、mysql、xmlrpc、國際、xml 和 bz2。 如果內部部署 Moodle 應用程式有任何不在控制器 VM 上的 PHP 延伸模組，您可以手動進行安裝。

若要取得內部部署應用程式中 PHP 延伸模組的清單，請執行：

```bash
php -m
```

若要安裝遺失的擴充功能，請執行：

```bash
sudo apt-get install -y php-<extension>
```

### <a name="ensure-the-web-server-instances-on-the-controller-vm-are-stopped"></a>確定控制器 VM 上的 web 伺服器實例已停止

1. 重新開機 web 伺服器。
   
   ```bash
   sudo systemctl restart nginx
   sudo systemctl restart php$_PHPVER-fpm
   ```
   
1. 停止 web 伺服器。
   
   ```bash
   sudo systemctl stop nginx
   sudo systemctl stop php$_PHPVER-fpm
   ```

當要求到達 Azure Load Balancer 時，它現在會重新導向至虛擬機器擴展集實例，而不會重新導向至控制器 VM。

### <a name="copy-configuration-files"></a>複製設定檔

將 PHP 和 web 伺服器配置檔案複製到共用位置，以供稍後複製到虛擬機器擴展集實例。

若要在共用位置建立設定檔案的目錄，請執行：

```bash
mkdir -p /moodle/config
mkdir -p /moodle/config/php
mkdir -p /moodle/config/nginx
```

若要將 PHP 和 web 伺服器配置檔案複製到共用目錄，請執行：

```bash
cp /etc/nginx/sites-enabled/* /moodle/config/nginx
cp /etc/php/$_PHPVER/fpm/pool.d/www.conf /moodle/config/php
```

## <a name="next-steps"></a>後續步驟

繼續 [設定 Moodle 控制器實例和背景工作角色節點](azure-infra-config.md) ，以進行 Moodle 遷移程式中的後續步驟。

---
title: 如何開始手動 Moodle 遷移
description: 瞭解如何開始手動 Moodle 遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 2fbaf59de2effb689d160aa22a41c51ed291b98e
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714487"
---
# <a name="how-to-start-a-manual-moodle-migration"></a>如何開始手動 Moodle 遷移

若要開始進行 Moodle 遷移，請在完成部署之後登入 [Azure](https://portal.azure.com/) 。 移至已建立的資源群組，並尋找所有建立的資源。 下圖將示範如何建立資源：

[資源概觀](./images/resource-creation-overview.png)

## <a name="controller-virtual-machine"></a>控制器虛擬機器

- 使用免費的開放原始碼終端機模擬器或序列主控台工具來登入控制器電腦。
- 複製控制器虛擬機器的公用 IP，以作為主機名稱使用。
- 在導覽面板中展開 [ **SSH** ]，選取 [ **驗證**]，然後尋找使用 Azure Resource Manager 範本部署 AZURE 基礎結構的 ssh 金鑰檔。
- 選取 [開啟]  。 系統會提示您輸入使用者名稱。 輸入 **azureadmin**，因為它是在範本中硬式編碼。

[PuTTY 登入頁面。](./images/putty-login.png)

[PuTTY 主要準則。](./images/putty-key-criteria.png)

- 流覽並選取 SSH 金鑰，然後按一下 [開啟] 按鈕。

流覽 [PuTTY 一般常見問題/疑難排解問題](https://documentation.help/PuTTY/faq.html) ，以深入瞭解 PuTTY。

登入之後，請執行下一組要遷移的命令。

## <a name="download-and-install-azcopy"></a>下載並安裝 AzCopy

執行：下列命令以安裝 AzCopy：

  ```bash
  sudo -s
  wget https://aka.ms/downloadazcopy-v10-linux
  tar -xvf downloadazcopy-v10-linux
  sudo rm /usr/bin/azcopy
  sudo cp ./azcopy_linux_amd64_*/azcopy /usr/bin/
  ```

## <a name="copy-the-backup"></a>複本備份

若要從 Azure Resource Manager 部署將備份封存複製到控制器虛擬機器實例：

- 將壓縮的備份檔案 (gz) 從 blob 儲存體下載到位於/home/azureadmin/位置的控制器虛擬機器。

  ```bash
  sudo -s
  cd /home/azureadmin/
  azcopy copy `https://storageaccount.blob.core.windows.net/container/BlobDirectoryName<SASToken>` `/home/azureadmin/`
  ```

  例如：`azcopy copy 'https://onpremisesstorage.blob.core.windows.net/migration/storage.tar.gz?sv=2019-12-12&ss=' /home/azureadmin/storage.tar.gz`

- 將壓縮的內容解壓縮至目錄。

  ```bash
  d /home/azureadmin
  ar -zxvf storage.tar.gz
  ```

## <a name="how-to-migrate-and-configure-a-moodle-application"></a>如何遷移及設定 Moodle 應用程式

在遷移之前，請先備份目前的設定。 備份目錄會解壓縮為 `storage/at/home/azureadmin` 。 此儲存體目錄包含 Moodle、moodledata 和設定目錄，以及資料庫備份檔案。 這些將會複製到所需的位置。

- 建立備份目錄。

  ```bash
  cd /home/azureadmin/
  mkdir -p backup
  mkdir -p backup/moodle
  mkdir -p backup/moodle/html
  ```

- 建立 Moodle 和 moodledata 目錄的備份。

  ```bash
  mv /moodle/html/moodle /home/azureadmin/backup/moodle/html/moodle
  mv /moodle/moodledata /home/azureadmin/backup/moodle/moodledata
  ```

- 將內部部署 Moodle 和 moodledata 目錄複寫到共用位置 (`/moodle`) 。

  ```bash
  cp -rf /home/azureadmin/storage/moodle /moodle/html/
  cp -rf /home/azureadmin/storage/moodledata /moodle/moodledata
  ```

- 將 Moodle 資料庫匯入至 Azure。 適用於 MySQL 的 Azure 資料庫實例會受到防火牆保護。 依預設，伺服器與其內部資料庫的所有連線皆會遭拒。 第一次連接到適用於 MySQL 的 Azure 資料庫之前，請先設定防火牆以新增用戶端電腦的公用網路 IP 位址或 IP 位址範圍。

  ```bash
  az mysql server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
  ```

- 選取新的 MySQL 伺服器，然後選取 [ **連接安全性**]。

![選取 [連接安全性]。](./images/database-connection-security.png)

- 您可以在這裡新增 IP 或設定防火牆規則。 建立規則之後，請選取 [ **儲存** ]。

您現在可以使用 mysql 命令列工具或 MySQL 工作臺工具來連接到伺服器。 若要取得連接資訊，請從 **MySQL Server 資源** 網頁複製 **伺服器名稱** 和 **伺服器管理員登入名稱**。 您可以選取每個欄位旁邊的 [複製] 按鈕來進行這項操作。

![設定新的連接。](images/database-connection.png)

例如，如果伺服器名稱是 `mydemoserver.mysql.database.azure.com` ，而伺服器管理員登入名稱為 `myadmin@mydemoserver` ：

- 匯入資料庫之前，請確定適用于 MySQL 的 Azure 資料庫詳細資料已就緒。
- 流覽至 Azure 入口網站，然後移至已建立的資源群組。
- 選取適用於 MySQL 的 Azure 資料庫 server 資源。
- 在 [總覽] 面板中，尋找適用於 MySQL 的 Azure 資料庫伺服器的詳細資料，例如伺服器名稱、伺服器管理員登入名稱。
- 按一下頁面頂端的 [重設密碼] 按鈕來重設密碼。

使用這些資料庫伺服器詳細資料來執行下列命令：

- 將內部部署資料庫匯入適用於 MySQL 的 Azure 資料庫。

- 建立資料庫以匯入內部部署資料庫。

  ```bash
  mysql -h $server_name -u $server_admin_login_name -p$admin_password -e "CREATE DATABASE $moodledbname CHARACTER SET utf8;"
  ```

- 將許可權指派給資料庫。

  ```bash
  mysql -h $server_name -u $server_admin_login_name -p$admin_password -e "GRANT ALL ON $moodledbname.* TO `$server_admin_login_name` IDENTIFIED BY `$admin_password`;"
  ```

- 匯入資料庫。

  ```bash
  mysql -h $server_name -u $server_admin_login_name -p$admin_password $moodledbname < /home/azureadmin/storage/database.sql
  ```

- 更新 Moodle 設定檔中的資料庫詳細資料， (/moodle/config.php) 。

更新 config.xml 中的參數。 請務必儲存這項工作的 DNS 名稱：

- 流覽至 Azure 入口網站，並尋找您的資源群組。

- 找出 **Load Balancer 的公用 IP**，並從 [ **總覽** ] 面板取得 DNS 名稱：

  >dbhost、dbname、>contoso\dbuser、dbpass、dataroot 和 wwwroot

  ```bash
  cd /moodle/html/moodle/
  nano config.php

- Update the database details, and save the file.

  For example:

  - $CFG->dbhost    = `localhost`;                - Change `localhost` to the server name.
  - $CFG->dbname    = `moodle`;                   - Change `moodle` to the newly created database name.
  - $CFG->dbuser    = `root`;                     - Change `root` to the server admin login name.
  - $CFG->dbpass    = `password`;                 - Change `password` to the server admin login password.
  - $CFG->wwwroot   = `https://on-premises.com`;  - Change `on-premises` to the DNS name.
  - $CFG->dataroot  = `/var/moodledata`;          - Change the path to `/moodle/moodledata`.

The on-premises `dataroot` directory can be stored at any location. After making the changes, save the file. Press `CTRL+O` to save and `CTRL+X` to exit.

Configure directory permissions.

- Set 755 and www-data owner:group permissions to the Moodle directory.

  ```bash
  sudo chmod 755 /moodle/html/moodle sudo chown -R www-data:www-data /moodle/html/moodle
   ```

- 設定770和 www-資料擁有者：群組 moodledata 目錄的許可權。

  ```bash
  sudo chmod 770 /moodle/moodledata sudo chown -R www-data:www-data /moodle/moodledata
  ```

- 更新 nginx 會議檔案。

  ```bash
  sudo mv /etc/nginx/sites-enabled/*.conf /home/azureadmin/backup/
  cd /home/azureadmin/storage/configuration/
  sudo cp -rf nginx/sites-enabled/*.conf /etc/nginx/sites-enabled/
  ```

- 更新 PHP 設定檔。

  ```bash
  _PHPVER=`/usr/bin/php -r "echo PHP_VERSION;" | /usr/bin/cut -c 1,2,3`
  echo $_PHPVER
  sudo mv /etc/php/$_PHPVER/fpm/pool.d/www.conf /home/azureadmin/backup/www.conf
  sudo cp -rf /home/azureadmin/storage/configuration/php/$_PHPVER/fpm/pool.d/www.conf /etc/php/$_PHPVER/fpm/pool.d/
  ```

- 安裝遺漏的 PHP 擴充功能。 Azure Resource Manager 範本會安裝下列 PHP 擴充功能： php-fpm、cli、捲曲、zip、梨、g.、dev、mcrypt、soap、json、redis、bcmath、gd、mysql、xmlrpc、國際、xml 和 bz2。 若要取得安裝在內部部署上的 PHP 延伸模組清單，請執行下列命令：

  ```bash
  php -m
  ```

- 如果內部部署的任何其他 PHP 延伸模組不存在於控制器虛擬機器中，則可以手動安裝。

  ```bash
  sudo apt-get install -y php-<extensionName>
  ```

- 更新 DNS 名稱和根目錄位置。 使用內部部署 DNS 名稱來更新 Azure 雲端 DNS 名稱。 此命令會開啟設定檔：

  ```bash
  nano /etc/nginx/sites-enabled/*.conf
  ```

- Azure Resource Manager 範本部署會在埠81上設定 nginx 伺服器。 如果伺服器埠不是81，請將其更新為81。

- 更新伺服器名稱。 例如，如果 server_name on-premises.com，請使用 DNS 名稱來更新 on-premises.com。 在大部分的情況下，DNS 在遷移時可能會維持不變。

- 更新 HTML 根目錄位置。 例如，如果 `root /var/www/html/moodle;` ，請將其更新為 `root /moodle/html/moodle;` 。

- 內部部署根目錄可以位於任何位置。

- 變更之後，請按下儲存檔案 `CTRL+O` 並結束 `CTRL+X` 。

- 重新開機 web 伺服器。

  ```bash
  sudo systemctl restart nginx
  sudo systemctl restart php$_PHPVER-fpm  
  ```

- 停止 web 伺服器。 當要求到達 Azure Load Balancer 時，系統會將其重新導向至虛擬機器擴展集實例，但不會重新導向至控制器虛擬機器。

  ```bash
  sudo systemctl stop nginx
  sudo systemctl stop php$_PHPVER-fpm  
  ```

## <a name="how-to-copy-configuration-files"></a>如何複製設定檔

將 PHP 和 web 伺服器配置檔案複製到共用位置。 設定檔可從共用位置複製到虛擬機器擴展集實例。

若要建立共用位置中的設定目錄：

```bash
mkdir -p /moodle/config
mkdir -p /moodle/config/php
mkdir -p /moodle/config/nginx
```

將 PHP 和 web 伺服器配置檔案複製到設定目錄：

```bash
cp /etc/nginx/sites-enabled/* /moodle/config/nginx
cp /etc/php/$_PHPVER/fpm/pool.d/www.conf /moodle/config/php
```

## <a name="next-steps"></a>後續步驟

如需有關 Moodle 遷移程式中下一個步驟的詳細資訊，請繼續進行 [ (Azure 基礎結構設定) 的 Moodle 控制器實例和背景工作節點 ](./azure-infra-config.md) 。

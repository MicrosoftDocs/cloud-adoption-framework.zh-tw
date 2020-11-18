---
title: 如何準備進行 Moodle 遷移
description: 瞭解如何準備 Moodle 遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 71990470e04f68f78b0bfffc2b1d837c7f0f29ad
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714530"
---
# <a name="how-to-prepare-for-a-moodle-migration"></a>如何準備進行 Moodle 遷移

## <a name="pre-migration-tasks"></a>預先遷移工作

將資料從內部部署匯出至 Azure 包含下列工作：

- 安裝 Azure CLI。
- 建立訂閱。
- 建立資源群組。
- 建立儲存體帳戶。
- 備份內部部署資料。
- 下載並安裝 AzCopy。
- 將封存的檔案複製到 Azure Blob。

## <a name="install-the-azure-cli"></a>安裝 Azure CLI

- 在內部部署基礎結構內部的主機上安裝 Azure CLI，以進行所有 Azure 相關的工作。

  ```bash
   curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
  ```

- 登入您的 Azure 帳戶。

  ```bash
  az login
  ```

- Az login 命令： Azure CLI 可能會在您的預設網頁瀏覽器內啟動實例或索引標籤，並提示您使用您的 Microsoft 帳戶登入 Azure。 如果瀏覽器啟動不發生，請在中開啟新的頁面， [https://aka.ms/devicelogin](https://aka.ms/devicelogin) 然後輸入終端機中顯示的授權碼。

- 若要使用命令列，請輸入下列命令：

  ```bash
  az login -u <username> -p <password>
  ```

## <a name="create-a-subscription"></a>建立訂用帳戶

如果您有訂用帳戶，請略過此步驟。 如果您沒有訂用帳戶，您可以選擇在 [Azure 入口網站內建立一個](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) 訂用帳戶，或選擇 [隨用隨付](https://azure.microsoft.com/offers/ms-azr-0003p/) 訂用帳戶。

- 若要使用 Azure 入口網站建立訂用帳戶，請 **從 [** **首頁** ] 區段流覽至 [訂用帳戶]。

  ![Azure 訂用帳戶。](./images/subscriptions.png)

- 此命令會設定訂用帳戶：

  ```bash
  az account set --subscription "Subscription Name"

  Example: az account set --subscription "ComputePM LibrarySub"
  ```

## <a name="create-a-resource-group"></a>建立資源群組

訂用帳戶設定好之後，您必須建立資源群組。 其中一個選項是使用 Azure 入口網站來建立它。 流覽至 [ **首頁** ] 區段、搜尋 **資源群組**、選取它、填寫必要欄位，然後選取 [ **建立**]。

![資源群組：建立資源群組。](./images/resource-group.png)

或者，您可以使用 Azure CLI 來建立資源群組。

- 提供先前步驟中的相同預設位置。

  ```bash
  az group create -l location -n name -s Subscription_NAME_OR_ID
  ```

- 使用範例測試帳戶更新螢幕擷取畫面和訂用帳戶名稱。

  例如：`az group create -l eastus -n manual_migration -s ComputePM LibrarySub`

- 在上一個步驟中，會建立一個資源群組做為「manual_migration」。 在後續步驟中使用相同的資源群組。

探索 [Azure 中的位置](https://azure.microsoft.com/global-infrastructure/data-residency/) ，以取得詳細資訊。

## <a name="create-a-storage-account"></a>建立儲存體帳戶

下一步是在已建立的資源群組中 [建立儲存體帳戶](https://ms.portal.azure.com/#create/Microsoft.StorageAccount) 。 您可以使用 Azure 入口網站或 Azure CLI 來建立儲存體帳戶。

- 若要使用入口網站建立，請流覽至該入口網站、搜尋儲存體帳戶，然後選取 [ **新增** ] 按鈕。 填寫必要欄位之後，請選取 [ **建立**]。

  ![建立儲存體帳戶。](./images/create-storage-account.png)

- 或者，您可以使用 Azure CLI：

  ```bash
  az storage account create -n storageAccountName -g resourceGroupName --sku Standard_LRS --kind BlobStorage -l location
  ```

  範例：`az storage account create -n onpremisesstorage -g manual_migration --sku Standard_LRS --kind BlobStorage -l eastus`

- 在先前的命令中， `--kind` 表示儲存體帳戶的類型。 一旦 `onpremisesstorage` 建立帳戶，它就會成為內部部署備份的目的地。

## <a name="back-up-on-premises-data"></a>備份內部部署資料

- 在備份內部部署資料之前，請先啟用 Moodle 網站的 **維護模式** 。 從內部部署虛擬機器執行下列命令：

  ```bash
  sudo /usr/bin/php admin/cli/maintenance.php --enable
  ```

- 執行下列命令以檢查 Moodle 網站的狀態：

  ```bash
  sudo /usr/bin/php admin/cli/maintenance.php
  ```

- 當您備份內部部署 Moodle 和 moodledata 檔案、設定和資料庫時，備份至單一目錄。 下圖摘要說明：

  ![Moodle 備份目錄結構。](./images/directory-structure.png)

- 若要複製所有資料，請在任何想要的位置中建立空的儲存體目錄：

  ```bash
  sudo -s
  ```

  例如，位置是 `/home/azureadmin` 。

  ```bash
  cd /home/azureadmin
  mkdir storage
  ```

### <a name="back-up-moodle-and-moodledata"></a>備份 Moodle 和 moodledata

- Moodle 目錄是由網站 HTML 內容所組成。 Moodledata 包含 Moodle 網站資料。

- 複製 Moodle 和 moodledata 的命令如下：

  ```bash
  cp -R /var/www/html/moodle /home/azureadmin/storage/
  cp -R /var/moodledata /home/azureadmin/storage/
  ```

### <a name="backup-php-and-web-server-configurations"></a>備份 PHP 和 web 伺服器設定

- 將、、和目錄等 PHP 配置檔案複製 `php-fpm.conf` `php.ini` `pool.d` `conf.d` 到 `phpconfig` 目錄下的目錄 `configuration` 。

- 將和等 ngnix 設定 `nginx.conf` 複製 `sites-enabled/dns.conf` 到 `nginxconfig` 目錄下的目錄 `configuration` 。

  ```bash
  cd /home/azureadmin/storage mkdir configuration
  ```

- 複製 nginx 和 PHP 設定的命令如下：

  ```bash
  cp -R /etc/nginx /home/azureadmin/storage/configuration/nginx
  cp -R /etc/php /home/azureadmin/storage/configuration/php
  ```

### <a name="create-a-backup-of-the-database"></a>建立資料庫的備份

- 如果您已經安裝 mysql-用戶端，請略過安裝的步驟。 如果您沒有 mysql 用戶端，請立即安裝：

  ```bash
  sudo -s
  ```

- 執行下列命令來檢查是否已安裝 mysql-用戶端：

  ```bash
  mysql -V
  ```

- 如果未安裝 mysql-用戶端，請執行下列命令：

  ```bash
  sudo apt-get install mysql-client
  ```

- 此命令可讓您備份資料庫：

  ```bash
  mysqldump -h dbServerName -u dbUserId -pdbPassword dbName > /home/azureadmin/storage/database.sql
  ```

- 將 dbServerName、dbUserId、dbPassword 和 dbName 取代為內部部署資料庫詳細資料。

- 建立 `storage.tar.gz` 備份目錄的封存檔案：

  ```bash
  cd /home/azureadmin/ tar -zcvf storage.tar.gz storage
  ```

## <a name="download-and-install-azcopy"></a>下載並安裝 AzCopy

執行下列命令以安裝 AzCopy：

  ```bash
  sudo -s
  wget https://aka.ms/downloadazcopy-v10-linux
  tar -xvf downloadazcopy-v10-linux
  sudo rm /usr/bin/azcopy
  sudo cp ./azcopy_linux_amd64_*/azcopy /usr/bin/
  ```

## <a name="copy-archived-files-to-azure-blob"></a>將封存的檔案複製到 Azure Blob

使用 AzCopy 將封存的內部部署檔案複製到 Azure Blob。

- 若要使用 AzCopy，請先產生 SAS 權杖。 移至所建立的 **儲存體帳戶資源**，然後流覽至左面板中的 [ **共用存取** 簽章]。

  ![範例儲存體帳戶。](./images/storage-account-created.png)

- 選取 [ **容器** ] 和 [核取方塊]，並設定 SAS 權杖的開始和到期日。 選取 [產生 SAS 與連接字串]。

  ![正在產生 SAS 權杖。](images/SAS-token-generation.png)

- 複製並儲存 SAS 權杖以供日後使用。

- 用來在儲存體帳戶中建立容器的命令：

  ```bash
  az storage container create --account-name <storageAccountName> --name <containerName> --auth-mode login
  ```

  例如：`az storage container create --account-name onpremisesstorage --name migration --auth-mode login`

  `--auth-mode login` 表示登入時的驗證模式。 登入之後，將會建立容器。

- 也可以使用 Azure 入口網站來建立容器。 流覽至所建立的相同儲存體帳戶，選取容器，然後選取 [ **新增** ] 按鈕。

- 輸入強制容器名稱之後，請選取 [ **建立** ] 按鈕。

  ![新的容器。](images/new-container.png)

- 將封存檔案複製到 Azure Blob 帳戶的命令：

  ```bash
  sudo azcopy copy /home/azureadmin/storage.tar.gz 'https://<storageAccountName>.blob.core.windows.net/<containerName>/<SAStoken>'
  ```

  例如：`azcopy copy /home/azureadmin/storage.tar.gz 'https://onpremisesstorage.blob.core.windows.net/migration/?sv=2019-12-12&ss='`

  ![Azure Blob 中的封存。](images/archive-in-blob.png)

- Azure Blob 帳戶內現在應該會有一個封存複本。

## <a name="next-steps"></a>後續步驟

如需 Moodle 遷移程式的詳細資訊 [，請繼續 Moodle 遷移工作、架構和範本](./migration-arch.md) 。

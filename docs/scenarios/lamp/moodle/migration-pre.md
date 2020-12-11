---
title: 如何準備進行 Moodle 遷移
description: 瞭解如何準備 Moodle 遷移。 瞭解如何備份 Moodle 檔，並建立遷移所需的資源。
author: UmakanthOS
ms.author: brblanch
ms.date: 11/30/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: internal
ms.openlocfilehash: eaed30fb982f10e51de85951a48b89a790549c3a
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97025617"
---
# <a name="how-to-prepare-for-a-moodle-migration"></a>如何準備進行 Moodle 遷移

將 Moodle 應用程式從內部部署環境遷移至 Azure 之前，您應該匯出資料。 本指南說明匯出流程的步驟。

## <a name="install-the-azure-cli"></a>安裝 Azure CLI

請遵循下列步驟，在您的內部部署環境中設定 Azure CLI：

1. 在可用於 Azure 工作的主機上，輸入下列命令以安裝 Azure CLI：

   ```bash
   curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
   ```

1. 在 Azure CLI 中，輸入此命令以登入您的 Azure 帳戶：

   ```bash
   az login -u <username> -p <password>
   ```

1. 如果 Azure CLI 開啟瀏覽器視窗或索引標籤，請使用您的 Microsoft 帳戶登入 Azure。 如果瀏覽器視窗未開啟，請移至 [https://aka.ms/devicelogin](https://aka.ms/devicelogin) ，然後輸入終端機中顯示的授權碼。

## <a name="create-a-subscription"></a>建立訂用帳戶

如果您已經有 Azure 訂用帳戶，請略過此步驟。

如果您沒有 Azure 訂用帳戶，您可以 [免費建立一個](https://azure.microsoft.com/free/)訂用帳戶。 您也可以設定 [隨用隨付訂](https://azure.microsoft.com/offers/ms-azr-0003p/)用帳戶，也可以在 Azure 中建立訂用帳戶。

- 若要使用 Azure 入口網站建立訂用帳戶，請 [開啟 [](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)訂用帳戶]，選取 [ **新增**]，然後輸入必要的資訊。

  ![Azure 入口網站中 [訂閱] 頁面的螢幕擷取畫面。](./images/azure-subscriptions-page.png)

- 若要使用 Azure CLI 建立訂用帳戶，請輸入下列命令：

  ```azurecli
  az account set --subscription '<subscription name>'
  ```

  例如，輸入：

  `az account set --subscription 'ComputePM LibrarySub'`

## <a name="create-a-resource-group"></a>建立資源群組

設定好訂用帳戶之後，請在 Azure 中建立資源群組。 您可以使用 Azure 入口網站或 CLI 來建立群組。

- 若要使用 Azure 入口網站，請遵循下列步驟：

  1. 開啟 [ [資源群組](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups)]，然後選取 [ **新增**]。

  1. 輸入您的訂用帳戶名稱、資源組名和區域。 如需可用區域的清單，請參閱 [Azure 中的資料](https://azure.microsoft.com/global-infrastructure/data-residency/) 存放區。 記下您輸入的資源組名，讓您可以在稍後的步驟中使用該名稱。

  1. 選取 [檢閱 + 建立]。

  ![[建立資源群組] 頁面的螢幕擷取畫面，其中包含 Azure 入口網站 [訂用帳戶]、[資源群組] 和 [區域] 方塊，以及 [審核 + 建立] 按鈕。](./images/resource-group.png)

- 若要使用 Azure CLI 建立資源群組，請輸入下列命令：

  ```azurecli
  az group create -l <region> -n <resource group name> -s '<subscription name>'
  ```

  例如，輸入：

  `az group create -l eastus -n manual_migration -s 'ComputePM LibrarySub'`

  您以參數提供的值會 `-l` 指定預設位置。 使用您在先前步驟中使用的相同位置。 記下您所建立的資源組名，並在稍後的步驟中使用該名稱。

## <a name="create-a-storage-account"></a>建立儲存體帳戶

接下來，在您剛才建立的資源群組中建立儲存體帳戶。 您將使用此儲存體帳戶來備份您的內部部署 Moodle 資料。

您可以使用 Azure 入口網站或 Azure CLI 來建立儲存體帳戶。

- 若要使用 Azure 入口網站，請遵循下列步驟：

  1. 開啟 [ [建立儲存體帳戶](https://ms.portal.azure.com/#create/Microsoft.StorageAccount)]。

  1. 輸入下列資訊：

     - 您的訂用帳戶名稱
     - 您剛才建立的資源組名
     - 儲存體帳戶名稱
     - 您的區域

  1. 針對 [ **帳戶類型**]，輸入 **BlobStorage**。

  1. 針對 **複寫**，請輸入 **讀取權限異地冗余儲存體 (GRS)**。

  1. 選取 [檢閱 + 建立]。

  ![Azure 入口網站中的 [建立儲存體帳戶] 頁面的螢幕擷取畫面，其中包含多個輸入方塊和 [審核 + 建立] 按鈕。](./images/create-storage-account.png)

- 若要使用 Azure CLI 建立儲存體帳戶，請輸入下列命令：

  ```azurecli
  az storage account create -n <storage account name> -g <resource group name> --sku <storage account SKU> --kind <storage account type> -l <region>
  ```

  例如，輸入：

  `az storage account create -n onpremisesstorage -g manual_migration --sku Standard_LRS --kind BlobStorage -l eastus`

  `--kind`參數會指定儲存體帳戶的類型。

## <a name="back-up-on-premises-data"></a>備份內部部署資料

備份您的內部部署 Moodle 資料之前，請遵循下列步驟，在您的 Moodle 網站上開啟 **維護模式** ：

1. 在內部部署虛擬機器上，輸入此命令：

   ```bash
   sudo /usr/bin/php admin/cli/maintenance.php --enable
   ```

2. 輸入下列命令以檢查 Moodle 網站的狀態：

   ```bash
   sudo /usr/bin/php admin/cli/maintenance.php
   ```

當您備份內部部署 Moodle 和 moodledata 檔案、設定和資料庫時，備份至單一目錄。 下圖摘要說明此概念：

![顯示 Moodle 備份儲存體目錄結構的圖表。](./images/directory-structure.png)

### <a name="create-a-storage-directory"></a>建立儲存體目錄

複製您的資料之前，請在任何想要的位置建立空的儲存體目錄。 例如，如果位置為 `/home/azureadmin` ，請輸入下列命令：

  ```bash
  sudo -s
  cd /home/azureadmin
  mkdir storage
  ```

### <a name="back-up-moodle-directories"></a>備份 Moodle 目錄

在您的內部部署環境中， `moodle` 目錄包含網站 HTML 內容。 `moodledata`目錄包含 Moodle 網站資料。

輸入下列命令，將檔案從 `moodle` 和 `moodledata` 目錄複寫到儲存體目錄中：

  ```bash
  cp -R /var/www/html/moodle /home/azureadmin/storage/
  cp -R /var/moodledata /home/azureadmin/storage/
  ```

### <a name="back-up-php-and-web-server-configurations"></a>備份 PHP 和 web 伺服器設定

若要備份設定檔，請遵循下列步驟：

1. 輸入下列命令，在您的儲存體目錄下建立新的目錄：

   ```bash
   cd /home/azureadmin/storage
   mkdir configuration
   ```

2. 輸入下列命令來複製 PHP 和 nginx 設定檔：

   ```bash
   cp -R /etc/php /home/azureadmin/storage/configuration/
   cp -R /etc/nginx /home/azureadmin/storage/configuration/
   ```

   `php`目錄會儲存 PHP 設定檔，例如 `php-fpm.conf` 、、 `php.ini` `pool.d` 和 `conf.d` 。 `nginx`目錄會儲存 ngnix 設定，例如 `nginx.conf` 和 `sites-enabled/dns.conf` 。

### <a name="back-up-the-database"></a>備份資料庫

遵循下列步驟來備份您的資料庫：

1. 輸入下列命令來檢查是否已安裝 mysql-用戶端：

   ```bash
   sudo -s
   mysql -V
   ```

2. 如果已安裝 mysql-用戶端，請略過此步驟。 否則，請輸入此命令以安裝 mysql-client：

   ```bash
   sudo apt-get install mysql-client
   ```

3. 輸入此命令來備份資料庫：

   ```bash
   mysqldump -h <database server name> -u <database user ID> -p<database password> <database name> > /home/azureadmin/storage/database.sql
   ```

   針對 `<database server name>` 、 `<database user ID>` 、 `<database password>` 和 `<database name>` ，請使用您的內部部署資料庫所使用的值。

### <a name="create-an-archive"></a>建立封存

輸入此命令可 `storage.tar.gz` 針對您的備份目錄建立保存檔案：

```bash
cd /home/azureadmin/ tar -zcvf storage.tar.gz storage
```

## <a name="download-and-install-azcopy"></a>下載並安裝 AzCopy

輸入下列命令以安裝 AzCopy：

```bash
sudo -s
wget https://aka.ms/downloadazcopy-v10-linux
tar -xvf downloadazcopy-v10-linux
sudo rm /usr/bin/azcopy
sudo cp ./azcopy_linux_amd64_*/azcopy /usr/bin/
```

## <a name="copy-archived-files-to-azure-blob-storage"></a>將封存的檔案複製到 Azure Blob 儲存體

遵循下列步驟以使用 AzCopy 將封存的內部部署檔案複製到 Azure Blob 儲存體。

### <a name="generate-a-security-token"></a>產生安全性權杖

若要產生 AzCopy 的共用存取簽章 (SAS) 權杖，請遵循下列步驟：

1. 在 Azure 入口網站中，移至您稍早建立的儲存體帳戶頁面。

1. 在左面板中，選取 [ **共用存取** 簽章]。

   ![儲存體帳戶 Azure 入口網站中頁面的螢幕擷取畫面，其中已醒目提示 [共用存取簽章] （在左側面板中）。](./images/new-storage-account-page.png)

1. 在 [ **允許的資源類型**] 下，選取 [ **容器**]。

1. 在 [ **開始與到期日期/時間**] 下，輸入 SAS 權杖的開始和結束時間。

1. 選取 [產生 SAS 與連接字串]。

   ![Azure 入口網站的螢幕擷取畫面，其中顯示儲存體帳戶的共用存取簽章頁面。](./images/shared-access-signature-page.png)

1. 製作 SAS 權杖的複本，以便在稍後的步驟中使用。

### <a name="create-a-container"></a>建立容器

在儲存體帳戶中建立容器。 您可以使用此步驟的 Azure CLI 或 Azure 入口網站。

- 若要使用 Azure CLI，請輸入下列命令：

  ```bash
  az storage container create --account-name <storage account name> --name <container name> --auth-mode login
  ```

  例如，輸入：

  `az storage container create --account-name onpremisesstorage --name migration --auth-mode login`

  當您搭配的 `--auth-mode` 值使用參數時 `login` ，Azure 會使用您的認證進行驗證，然後建立容器。

- 若要使用 Azure 入口網站建立容器，請遵循下列步驟：

  1. 在入口網站中，移至您稍早建立的儲存體帳戶頁面。

  1. 選取 [ **容器**]，然後選取 [ **新增**]。

  1. 輸入容器的名稱，然後選取 [ **建立**]。

     ![對話方塊的螢幕擷取畫面，其中包含 [名稱] 方塊和 [建立] 按鈕建立新容器的 Azure 入口網站。](./images/new-container.png)

### <a name="copy-the-archive-file-to-azure-blob-storage"></a>將封存檔案複製到 Azure Blob 儲存體

輸入此命令以將封存檔案複製到您在 Blob 儲存體中建立的容器：

```bash
sudo azcopy copy /home/azureadmin/storage.tar.gz 'https://<storage account name>.blob.core.windows.net/<container name>/<SAS token>'
```

例如，輸入：

`azcopy copy /home/azureadmin/storage.tar.gz 'https://onpremisesstorage.blob.core.windows.net/migration/?sv=2019-12-12&ss='`

您的 Blob 儲存體帳戶現在應該包含封存的複本。

![顯示 Blob 儲存體帳戶 Azure 入口網站中頁面的螢幕擷取畫面。 您可以看見儲存體目錄的壓縮 tar 檔案。](./images/archive-in-blob-storage.png)

## <a name="next-steps"></a>後續步驟

繼續 [Moodle 遷移架構和範本](./migration-arch.md)。

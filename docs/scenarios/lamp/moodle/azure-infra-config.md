---
title: 如何設定 Moodle 背景工作節點
description: 瞭解如何設定 Moodle 的虛擬機器擴展集。 瞭解如何使用私人 IP 位址從控制器存取擴展集。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/30/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 902c7b93e88725ec7c37ddaac09f92db8287af49
ms.sourcegitcommit: 32e8e7a835a688eea602f2af1074aa926ab150c3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687630"
---
# <a name="how-to-set-up-moodle-worker-nodes"></a>如何設定 Moodle 背景工作節點

遵循下列步驟來設定 Moodle 的虛擬機器擴展集或背景工作節點。

## <a name="virtual-machine-scale-set-instances"></a>虛擬機器擴展集實例

將私人 IP 位址指派給虛擬機器擴展集實例。 您只能使用與 IP 位址位於相同虛擬網路中的控制器虛擬機器來存取此 IP 位址。 本文說明如何設定該 IP 位址，然後設定您的 Moodle 遷移所建立的 Azure 虛擬機器擴展集。

### <a name="access-the-virtual-machine-scale-set"></a>存取虛擬機器擴展集

請遵循下列步驟來存取虛擬機器擴展集：

1. 判斷 Azure 針對您的虛擬機器擴展集實例所使用的私人 IP 位址：

   1. 登入 [Azure 入口網站](https://ms.portal.azure.com/#home)，並找出部署所建立的資源群組。

   1. 開啟虛擬機器擴展集資源的頁面。

   1. 在左面板中，選取 [ **實例**]。

   1. 開啟正在執行的實例。 在 [ **總覽** ] 區段中，複製與該實例相關聯的私人 IP 位址。

1. 輸入下列命令以從控制器虛擬機器登入虛擬機器擴展集：

   ```bash
   sudo -s
   sudo ssh azureadmin@<private IP address>
   ```

   在命令中， `<private IP address>` 是虛擬機器擴展集的私人 IP 位址。 例如，輸入：

   ```bash
   sudo -s
   sudo ssh azureadmin@172.31.X.X
   ```

### <a name="create-a-backup-directory"></a>建立備份目錄

遷移程式的 [先前步驟會將備份檔案解壓縮](./migration-start.md#back-up-the-current-configuration) 至中的命名 `storage` 目錄 `/home/azureadmin` 。 此 `storage` 目錄包含 `moodle` 和 `moodledata` 目錄、設定目錄和資料庫備份檔案。 登入您的擴展集虛擬機器實例之後，請輸入下列命令來建立這些檔案的備份目錄：

```bash
cd /home/azureadmin/
mkdir -p backup
mkdir -p backup/moodle
```

### <a name="configure-the-php-and-web-server"></a>設定 PHP 和網頁伺服器
若要設定 PHP 和 web 伺服器，請執行下列步驟：

1. 將 PHP 版本設定為變數：

   ```bash
   _PHPVER=`/usr/bin/php -r "echo PHP_VERSION;" | /usr/bin/cut -c 1,2,3`
   echo $_PHPVER
   ```

1. 建立 PHP 和 web 伺服器設定的備份：

   ```bash
   sudo mv /etc/nginx/sites-enabled/*.conf /home/azureadmin/backup/
   sudo mv /etc/php/$_PHPVER/fpm/pool.d/www.conf /home/azureadmin/backup/www.conf  
   ```

1. 複製 PHP 和 web 伺服器設定檔：

   ```bash
   sudo cp /moodle/config/nginx/*.conf  /etc/nginx/sites-enabled/
   sudo cp /moodle/config/php/www.conf /etc/php/$_PHPVER/fpm/pool.d/
   ```

### <a name="install-missing-extensions"></a>安裝遺失的延伸模組

請採取下列步驟來安裝遺失的延伸模組：

1. 若要取得內部部署所安裝 PHP 延伸模組的清單，請在內部部署虛擬機器上輸入下列命令：

   ```bash
   php -m
   ```

1. 使用 Azure Resource Manager 範本來安裝下列 PHP 擴充功能： php-fpm、cli、捲曲、zip、梨、g.、dev、mcrypt、soap、json、redis、bcmath、gd、mysql、xmlrpc、國際、xml 和 bz2。

1. 如果內部部署 Moodle 應用程式具有不在控制器虛擬機器中的任何其他 PHP 延伸模組，請使用下列命令手動安裝它們：

   ```bash
   sudo apt-get install -y php-<extension name>
   ```

## <a name="next-steps"></a>後續步驟

在 [Moodle 遷移之後](./migration-post.md)，繼續進行後續操作。

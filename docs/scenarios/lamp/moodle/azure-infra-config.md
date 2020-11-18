---
title: 如何設定 Moodle 控制器實例和背景工作節點
description: 瞭解如何在 Azure 基礎結構設定)  (設定 Moodle 控制器實例和背景工作節點。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 9a40bc5b34d85a94641e2381c7773621dfe09e9f
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714538"
---
# <a name="how-to-set-up-moodle-controller-instance-and-worker-nodes"></a>如何設定 Moodle 控制器實例和背景工作節點

## <a name="virtual-machine-scale-set-instances"></a>虛擬機器擴展集實例

虛擬機器擴展集實例是指派給私人 Ip，而且只有在相同虛擬網路中的控制器虛擬機器才能存取。 啟用閘道以將虛擬機器擴展集實例連線至私人 IP，並遵循 [如何建立虛擬網路閘道並透過私人 ip](./vpn-gateway.md) 連線，以取得虛擬機器擴展集實例的閘道存取權。 存取虛擬機器擴展集之前，請將它設定為啟用密碼。

若要建立虛擬機器擴展集實例私人 IP：

- 登入 [Azure 入口網站](https://ms.portal.azure.com/#home)，並找出已建立的資源群組。
- 尋找並流覽至虛擬機器擴展集資源。
- 在左面板中，選取 [ **實例**]。
- 流覽至正在執行的實例，並在 [ **總覽** ] 區段中尋找與其相關聯的私人 IP。

登入虛擬機器擴展集和控制器虛擬機器;執行下列命令：

```bash
 sudo -s
 sudo ssh azureadmin@172.31.X.X
```

>172.31. 是虛擬機器擴展集實例私人 IP。

登入擴展集虛擬機器實例。 執行下列步驟：

- 備份目錄會解壓縮為儲存體/at/home/azureadmin。 此儲存體目錄包含 Moodle、moodledata 和設定目錄，以及資料庫備份檔案。 這些將會複製到所需的位置。

- 建立備份目錄。

  ```bash
  cd /home/azureadmin/
  mkdir -p backup
  mkdir -p backup/moodle
  ```

- 若要設定 PHP 和 web 伺服器，請建立 PHP 和 web 伺服器設定的備份，並將 PHP 版本設定為變數。

   ```bash
  _PHPVER=`/usr/bin/php -r "echo PHP_VERSION;" | /usr/bin/cut -c 1,2,3`
  echo $_PHPVER
   ```

  ```bash
  sudo mv /etc/nginx/sites-enabled/*.conf  /home/azureadmin/backup/
  sudo mv /etc/php/$_PHPVER/fpm/pool.d/www.conf /home/azureadmin/backup/www.conf  
  ```

- 複製 PHP 和 web 伺服器設定檔。

  ```bash
  sudo cp /moodle/config/nginx/*.conf  /etc/nginx/sites-enabled/
  sudo  cp /moodle/config/php/www.conf /etc/php/$_PHPVER/fpm/pool.d/
  ```

- 安裝遺漏的 PHP 擴充功能，並使用 Azure Resource Manager 範本安裝下列 PHP 擴充功能： php-fpm、cli、捲曲、zip、梨、g.、dev、mcrypt、soap、json、redis、bcmath、gd、mysql、xmlrpc、國際、xml 和 bz2。

- 若要取得內部部署環境中所安裝 PHP 延伸模組的清單，請在內部部署虛擬機器上執行下列命令：

  ```bash
  php -m
  ```

- 如果內部部署的任何其他 PHP 延伸模組不在控制器虛擬機器中，則可以手動安裝。

  ```bash
  sudo apt-get install -y php-<extensionName>
  ```

## <a name="next-steps"></a>後續步驟

如需 Moodle 遷移程式中下一個步驟的詳細資訊，請繼續進行 [Moodle 遷移之後的後續操作方式](./migration-post.md) 。

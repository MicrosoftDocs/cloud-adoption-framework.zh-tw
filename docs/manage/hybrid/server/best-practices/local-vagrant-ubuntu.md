---
title: 部署以 Vagrant 裝載的本機 Ubuntu 伺服器，並將它連線到 Azure Arc
description: 部署以 Vagrant 裝載的本機 Ubuntu 伺服器，並將它連線到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 367b08aa62b68120def9e9788129cbe21b483d6c
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102111968"
---
# <a name="deploy-a-local-ubuntu-server-hosted-with-vagrant-and-connect-it-to-azure-arc"></a>部署以 Vagrant 裝載的本機 Ubuntu 伺服器，並將它連線到 Azure Arc

本文提供使用 [Vagrant](https://www.vagrantup.com/)部署本機 **Ubuntu** 虛擬機器，並將它連接為已啟用 Azure Arc 之伺服器資源的指引。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. [安裝或更新 AZURE CLI 至2.7 版或更新版本](/cli/azure/install-azure-cli)。 使用下列命令來檢查您目前安裝的版本。

    ```console
    az --version
    ```

3. Vagrant 依賴基礎程式管理程式。 在本指南中，我們將使用「Oracle VM VirtualBox」。

    1. 安裝 [VirtualBox](https://www.virtualbox.org/wiki/Downloads)。

        - 如果您是 macOS 使用者，請執行 `brew cask install virtualbox`
        - 如果您是 Windows 使用者，您可以使用 [Chocolatey 套件](https://chocolatey.org/packages/virtualbox)
        - 如果您是 Linux 使用者，您可以在[這裡](https://www.virtualbox.org/wiki/Linux_Downloads)找到所有套件安裝方法

    2. 安裝 [Vagrant](https://www.vagrantup.com/docs/installation)

        - 如果您是 macOS 使用者，請執行 `brew cask install vagrant`
        - 如果您是 Windows 使用者，您可以使用 [Chocolatey 套件](https://chocolatey.org/packages/vagrant)
        - 如果您是 Linux 使用者，請參閱 [這裡](https://www.vagrantup.com/downloads)

4. 建立 Azure 服務主體。

    若要將 Vagrant 的虛擬機器連線到 Azure Arc，需要有指派參與者角色的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

    ```console
    az login
    az ad sp create-for-rbac -n "<Unique SP Name>" --role contributor
    ```

    例如：

    ```console
    az ad sp create-for-rbac -n "http://AzureArcServers" --role contributor
    ```

    輸出應該看起來像這樣︰

    ```json
    {
      "appId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "displayName": "AzureArcServers",
      "name": "http://AzureArcServers",
      "password": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "tenant": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }
    ```

    > [!NOTE]
    > 強烈建議您將服務主體的範圍設為特定的 [Azure 訂用帳戶和資源群組](/cli/azure/ad/sp)。

5. Vagrant 檔案會在 VM OS 上執行腳本，以安裝所需的所有成品，並插入環境變數。 編輯 [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/local/vagrant/ubuntu/scripts/vars.sh) shell 腳本，以符合您所建立的 Azure 服務主體。

    - `subscriptionId` = 您的 Azure 訂用帳戶識別碼
    - `appId` = 您的 Azure 服務主體名稱
    - `password` = 您的 Azure 服務主體密碼
    - `tenantId` = 您的 Azure 租使用者識別碼
    - `resourceGroup` = Azure 資源組名
    - `location` = Azure 區域

## <a name="deployment"></a>部署

與任何 Vagrant 部署一樣，都需要 [vagrantfile](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/local/vagrant/ubuntu/Vagrantfile) 和 [Vagrant](https://www.vagrantup.com/docs/boxes) 方塊。 高層級的部署將會：

- 下載 Ubuntu 16.04 影像檔案 [Vagrant box](https://app.vagrantup.com/ubuntu/boxes/xenial64)
- 執行安裝腳本

在編輯 `scripts/vars.sh` 腳本以符合您的環境之後，請從 `Vagrantfile` 資料夾執行 `vagrant up` 。 由於這是第一次建立 VM，因此第一次執行的速度會比要遵循的 VM **慢很多** ，因為部署是首次下載 Ubuntu box。

![[Vagrant up] 命令的螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-vagrant-up.png)

下載完成之後，就會開始布建。 如下列螢幕擷取畫面所示，此程式不會超過三分鐘。

![已完成的 ' vagrant up ' 命令的螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-vagrant-up-complete.png)

完成時，您將會在新的資源群組內部署本機 Ubuntu VM，並以新的啟用 Azure Arc 的伺服器連接。

![Azure 入口網站中啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-server.png)

![Azure 入口網站中已啟用 Azure Arc 之伺服器的詳細資料螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-server-details.png)

## <a name="semi-automated-deployment-optional"></a>半自動化部署 (選擇性) 

最後一個步驟是將 VM 註冊為新啟用 Azure Arc 的伺服器資源。

![[Vagrant up] 命令的另一個螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-vagrant-up-2.png)

如果您想要示範/控制實際的註冊程式，請完成下列步驟：

1. 在 [`install_arc_agent`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/local/vagrant/ubuntu/scripts/install_arc_agent.sh) shell 腳本中，將區段標記為批註 `run connect command` 並儲存檔案。 您也可以對資源群組的建立進行批註或變更。

    ![[Azcmagent connect] 命令的螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-azcmagent.png)

    ![[Az group create] 命令的螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-azgroup-create.png)

2. 使用命令透過 SSH 連線至 VM `vagrant ssh` 。

    ![連接至 Vagrant 電腦的 SSH 金鑰螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-ssh.png)

3. 執行 `azcmagent connect` 您使用環境變數標記為批註的相同命令。

    ![[Azcmagent connect] 命令的另一個螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-azcmagent-2.png)

## <a name="delete-the-deployment"></a>刪除部署

若要刪除整個部署，請執行 `vagrant destroy -f` 命令。 Vagrantfile 包含 `before: destroy` Vagrant 觸發程式，它會執行腳本來刪除 Azure 資源群組，然後再終結實際的 VM。

![[Vagrant 終結] 命令的螢幕擷取畫面。](./media/local-vagrant/vagrant-ubuntu-vagrant-destroy.png)

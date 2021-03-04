---
title: 使用虛擬機器擴充功能和 Azure Resource Manager 範本，將自訂腳本部署至 Azure Arc Linux 和 Windows server
description: 瞭解如何使用可提供部署後設定和自動化工作的虛擬機器擴充功能，執行自訂腳本至已啟用 Azure Arc 的伺服器。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: b8ac9e7f9041fe94d0644e06e342a1a1f6978d8a
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112002"
---
# <a name="use-virtual-machine-extensions-and-an-azure-resource-manager-template-to-deploy-custom-scripts-to-azure-arc-linux-and-windows-servers"></a>使用虛擬機器擴充功能和 Azure Resource Manager 範本，將自訂腳本部署至 Azure Arc Linux 和 Windows server

本文提供有關如何使用虛擬機器擴充功能來執行自訂腳本至啟用 Azure Arc 之伺服器的指引。 虛擬機器擴充功能是小型的應用程式，可提供部署後設定和自動化工作，例如軟體安裝、防毒軟體防護，或執行自訂腳本的機制。

您可以使用 Azure 入口網站、Azure CLI、Azure Resource Manager 範本 (ARM 範本) 、PowerShell 或 Linux shell 腳本，或使用 Azure 原則來管理將擴充功能部署到已啟用 Azure Arc 的伺服器。 在下列程式中，您將使用 ARM 範本來部署自訂腳本擴充功能。 此延伸模組會在虛擬機器上下載並執行腳本。 它適用于部署後設定、軟體安裝，或任何其他設定或管理工作。

> [!IMPORTANT]
> 本文中的程式假設您已經部署 Vm，或是在內部部署或其他雲端上執行的伺服器，而且您已將這些 Vm 連線到 Azure Arc。如果您尚未這麼做，下列資訊可協助您將這項工作自動化。

- [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md)
- [GCP Windows 實例](./gcp-terraform-windows.md)
- [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
- [AWS Amazon Linux 2 EC2 實例](./aws-terraform-al2.md)
- [VMware vSphere Ubuntu VM](./vmware-terraform-ubuntu.md)
- [VMware vSphere Windows Server VM](./vmware-terraform-windows.md)
- [Vagrant Ubuntu box](./local-vagrant-ubuntu.md)
- [Vagrant Windows box](./local-vagrant-windows.md)

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. 如先前所述，本指南會從您已部署 Vm 或伺服器至 Azure Arc 的位置開始。下列螢幕擷取畫面顯示已連線到 Azure Arc 的 GCP 伺服器，並在 Azure 中顯示為資源。

    ![從已啟用 Azure Arc 之伺服器的資源群組螢幕擷取畫面。](./media/arc-vm-extension-custom-script/resource-group.png)

    ![從已啟用 Azure Arc 之伺服器線上狀態的螢幕擷取畫面。](./media/arc-vm-extension-custom-script/connected-status.png)

3. [安裝或更新 AZURE CLI](/cli/azure/install-azure-cli)。 Azure CLI 應執行2.7 版或更新版本。 用 `az --version` 來檢查您目前安裝的版本。

4. 建立 Azure 服務主體。

    若要將 VM 或裸機伺服器連線至 Azure Arc，則需要使用「參與者」角色指派的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

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

若要示範自訂腳本擴充功能，請使用下列 Linux 和 Windows 腳本。

- [Linux](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/scripts/custom_script_linux.sh)：腳本會在作業系統上修改當天的訊息。
- [Windows](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/scripts/custom_script_windows.ps1)：腳本會在 VM 上安裝 Windows 終端機、Microsoft Edge、7-Zip 和 Visual Studio Code [Chocolatey](https://chocolatey.org/) 套件。

## <a name="azure-arc-enabled-servers-custom-script-extension-deployment"></a>啟用 Azure Arc 的伺服器自訂腳本延伸模組部署

1. 編輯[Windows](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/extensions/arm/customscript-templatewindows.parameters.json)或[Linux](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/extensions/arm/customscript-templatelinux.parameters.json)的延伸模組參數檔案

   ![ARM 範本參數檔案的螢幕擷取畫面。](./media/arc-vm-extension-custom-script/parameters-file.png)

2. 提供下列資訊以符合您的環境設定：

    - 在 Azure Arc 中註冊的 VM 名稱。

    ![從已啟用 Azure Arc 之伺服器的電腦名稱稱螢幕擷取畫面。](./media/arc-vm-extension-custom-script/machine-name.png)

    - 您註冊啟用 Azure Arc 之伺服器的資源群組位置。

    ![Azure 區域的螢幕擷取畫面。](./media/arc-vm-extension-custom-script/azure-region.png)

    - 您想要在伺服器上執行之腳本的公用 URI，在此案例中，請使用原始格式的腳本 URL。
      - 若為 Windows： [公用 URI](https://raw.githubusercontent.com/microsoft/azure_arc/main/azure_arc_servers_jumpstart/scripts/custom_script_windows.ps1)
      - 針對 Linux： [公用 URI](https://raw.githubusercontent.com/microsoft/azure_arc/main/azure_arc_servers_jumpstart/scripts/custom_script_linux.sh)

3. 若要執行任一個腳本，請使用下列命令：

    - Windows：

         ```powershell
         powershell -ExecutionPolicy Unrestricted -File custom_script_windows.ps1
         ```

    - Linux：

         ```bash
         ./custom_script_linux.sh
         ```

4. 若要部署適用于 Linux 或 Windows 的 ARM 範本，請流覽至 [ [部署] 資料夾](https://github.com/microsoft/azure_arc/tree/main/azure_arc_servers_jumpstart/extensions/arm) ，並使用符合您作業系統的範本來執行下列命令：

    ```bash
    az deployment group create --resource-group <Name of the Azure resource group> \
    --template-file <The `customscript-template.json` template file location for Linux or Windows> \
    --parameters <The `customscript-template.parameters.json` template file location>
    ```

5. 範本部署完成後，您應該會看到如下所示的輸出：

    ![ARM 範本輸出的螢幕擷取畫面。](./media/arc-vm-extension-custom-script/output.png)

6. 選取 [ **擴充** 功能設定]，以在 azure 入口網站中確認已啟用 azure Arc 之伺服器的成功部署。 您應該會看到已安裝自訂腳本擴充功能。

    ![自訂腳本擴充功能的螢幕擷取畫面。](./media/arc-vm-extension-custom-script/custom-script-extension.png)

確認自訂腳本執行成功的另一種方式是連接到 Vm，並確認已設定作業系統。

- 針對 Linux VM，請使用 SSH 來連接 VM，並查看腳本所自訂之日期的訊息：

  ![每日更新訊息的螢幕擷取畫面。](./media/arc-vm-extension-custom-script/daily-message.png)

- 透過 RDP 連接到 Windows VM，並確認已安裝其他軟體： Microsoft Edge、7-zip 和 Visual Studio Code。

  ![已安裝其他軟體的螢幕擷取畫面。](./media/arc-vm-extension-custom-script/additional-software.png)

## <a name="clean-up-your-environment"></a>清除您的環境

請完成下列步驟來清除您的環境。

遵循每個指南的終止指示，從每個環境移除虛擬機器。

- [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md) 和 [GCP Windows 實例](./gcp-terraform-windows.md)
- [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
- [Vmware VSphere UBUNTU VM](./vmware-terraform-ubuntu.md) 和 [Vmware VSPHERE Windows Server vm](./vmware-terraform-windows.md)
- [Vagrant Ubuntu box](./local-vagrant-ubuntu.md) 和 [Vagrant Windows box](./local-vagrant-windows.md)

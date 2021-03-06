---
title: 使用 VMware PowerCLI 將 VMware vSphere Linux 虛擬機器上架到 Azure Arc
description: 使用 VMware PowerCLI 將 VMware vSphere Linux 虛擬機器上架到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: b547c84549ddeb1549047178c3e792009708aaf4
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102114348"
---
# <a name="use-vmware-powercli-to-scale-onboarding-vmware-vsphere-linux-virtual-machines-to-azure-arc"></a>使用 VMware PowerCLI 將 VMware vSphere Linux 虛擬機器上架到 Azure Arc

本文提供使用所提供之 [Vmware PowerCLI](https://code.vmware.com/web/dp/tool/vmware-powercli/) 腳本的指引，讓您可以在多個 VMware vSphere 虛擬機器中執行已自動調整規模的 azure arc 電腦代理程式部署，進而將這些 vm 作為已啟用 azure arc 的伺服器上架。

本指南假設您已有 VMware 虛擬機器的現有清查，並且將使用 PowerCLI PowerShell 模組，將 Vm 的上架程式自動化至 Azure Arc。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. [安裝或更新 AZURE CLI 至2.7 版或更新版本](/cli/azure/install-azure-cli)。 使用下列命令來檢查您目前安裝的版本。

    ```console
    az --version
    ```

3. 安裝 VMware PowerCLI。

    > [!NOTE]
    > 本指南已在發行 (12.0.0) 的最新版本中進行測試，但較早的版本也應可運作。

    - **支援的 PowerShell 版本：** VMware PowerCLI 12.0.0 與下列 PowerShell 版本相容：
        - Windows PowerShell 5.1
        - PowerShell 7
        - 您可以在 [這裡](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.install.doc/GUID-F02D0C2D-B226-4908-9E5C-2E783D41FE2D.html) 找到詳細的安裝指示，但最簡單的方式是使用 `VMware.PowerCLI` 下列命令，從 PowerShell 資源庫使用模組。

          ```powershell
          Install-Module -Name VMware.PowerCLI
          ```

4. 若要能夠從 vCenter 讀取 VM 清查，以及叫用 VM 作業系統層級上的腳本，需要下列許可權：

    - [`VirtualMachine.GuestOperations`](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-6A952214-0E5E-4CCF-9D2A-90948FF643EC.html) 使用者帳戶

    - 使用[唯讀角色](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.security.doc/GUID-93B962A7-93FA-4E96-B68F-AE66D3D6C663.html)指派的 VMware vCenter Server 使用者

5. 建立 Azure 服務主體。

    若要將 VMware vSphere 虛擬機器連線到 Azure Arc，需要有指派「參與者」角色的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

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

## <a name="automation-flow"></a>自動化流程

此案例的自動化流程包含下列步驟：

1. 編輯 [`vars.ps1`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/scaled_deployment/powercli/linux/vars.ps1) PowerCLI 腳本。

2. 執行 [`scale-deploy.ps1`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/scaled_deployment/powercli/linux/scale_deploy.ps1) PowerShell 腳本時：

    - 腳本會 `vars.sh` 使用使用者的 Azure 環境變數自動產生 shell 腳本。

    - 腳本執行將會起始對 vCenter 的驗證，並會掃描 Azure Arc 候選 Vm 所在的目標 VM 資料夾，並將自動產生的 `vars.sh` 和 `install_azure_arc_agent.sh` shell 腳本複製到 `/vmware/scaled-deploy/powercli/linux` 該 vm 資料夾中每個 Vm 的 vm Linux OS。

3. `install_azure_arc_agent.sh`Shell 腳本會在 vm 的來賓 OS 上執行，並會安裝已連線到 Azure arc 的電腦代理程式，以便將 vm 上架到 Azure arc。

## <a name="predeployment"></a>前置部署

為了示範此案例的之前和之後，下列螢幕擷取畫面顯示專用的空白 Azure 資源群組、具有候選 Vm 的 vCenter VM 資料夾，以及未 `/var/opt/` 安裝任何代理程式的目錄。

![空白 Azure 資源群組的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-empty.png)

![香草 VMware vSphere 虛擬機器的螢幕擷取畫面，不含 Azure Arc 代理程式。](./media/vmware-scale-powercli/cli-linux-vanilla-1.png)

![未使用 Azure Arc 代理程式的香草 VMware vSphere 虛擬機器的另一個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-vanilla-2.png)

## <a name="deployment"></a>部署

執行 PowerCLI 腳本之前，您必須先設定腳本將使用的 [環境變數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/scaled_deployment/powercli/linux/vars.ps1) `install_azure_arc_agent.sh` 。 這些變數是以您剛才建立的 Azure 服務主體、您的 Azure 訂用帳戶和租使用者，以及 VMware vSphere 認證和資料為基礎。

- 使用命令抓取您的 Azure 訂用帳戶識別碼和租使用者識別碼 `az account list`

- 使用「必要條件」一節中建立的 Azure 服務主體識別碼和密碼：

    ![匯出環境變數的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-export-variables.png)

- 從 `azure_arc_servers_jumpstart\vmware\scaled-deploy\powercli\linux` 資料夾中，以系統管理員身分開啟 PowerShell 會話，然後執行 `scale-deploy.ps1` 腳本。

    ![[scale_deploy.ps1] 的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-scale-deploy-1.png)

    !['scale_deploy.ps1' 的第二個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-scale-deploy-2.png)

    !['scale_deploy.ps1' 的第三個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-scale-deploy-3.png)

- 完成時，VM 將會安裝 Azure Arc 連線的機器代理程式，以及已填入新啟用 Azure Arc 之伺服器的 Azure 資源群組。

    ![已安裝 Azure Arc 代理程式的電腦螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-agent.png)

    ![Azure 資源群組中已啟用 Azure Arc 之新伺服器的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-servers-1.png)

    ![Azure 資源群組中新啟用 Azure Arc 之伺服器的另一個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-linux-servers-2.png)

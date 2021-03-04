---
title: 使用 VMware PowerCLI 將 VMware vSphere Windows Server 虛擬機器上架到 Azure Arc
description: 使用 VMware PowerCLI 將 VMware vSphere Windows Server 虛擬機器上架到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: a1ea6c6d66c6b47377228a1b35f237ccb71fcbdf
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794801"
---
# <a name="use-vmware-powercli-to-scale-onboarding-vmware-vsphere-windows-server-virtual-machines-to-azure-arc"></a>使用 VMware PowerCLI 將 VMware vSphere Windows Server 虛擬機器上架到 Azure Arc

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
    > 本指南已使用最新版本的 PowerCLI 進行測試， (12.0.0) 但較早的版本也應正常運作

    - **支援的 PowerShell 版本：** VMware PowerCLI 12.0.0 與下列 PowerShell 版本相容：
      - Windows PowerShell 5.1
      - PowerShell 7
      - 您可以在 [這裡](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.install.doc/GUID-F02D0C2D-B226-4908-9E5C-2E783D41FE2D.html) 找到詳細的安裝指示，但最簡單的方式是使用下列命令，從 PowerShell 資源庫使用 PowerCLI 模組。

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

您可以在下面找到此案例的自動化流程：

1. 使用者編輯 `vars.ps1` PowerCLI 腳本。

2. `scale_deploy.ps1`腳本執行將會起始對 vCenter 的驗證，並會掃描 Azure Arc 候選 vm 所在的目標 VM 資料夾，並將 `vars.ps1` 和 `install-azure-arc-agent.ps1` PowerCLI 腳本複製到該 vm 資料夾中的每個 vm 上，位於[此資料夾](https://github.com/microsoft/azure_arc/tree/main/azure_arc_servers_jumpstart/vmware/scaled_deployment/powercli/windows)中的 vm Windows OS。

3. `install-azure-arc-agent.ps1`PowerCLI 腳本將會在 vm 來賓 OS 上執行，並會安裝已連線到 Azure arc 的電腦代理程式，以便將 VM 上架到 Azure arc

## <a name="predeployment"></a>前置部署

為了示範此案例的之前和之後，下列螢幕擷取畫面顯示專用的空白 Azure 資源群組、具有候選 Vm 的 vCenter VM 資料夾，以及 Windows 中的 [ **應用程式] & 功能** ] 視圖，顯示未安裝任何代理程式。

![空白 Azure 資源群組的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-empty.png)

![香草 VMware vSphere 虛擬機器的螢幕擷取畫面，不含 Azure Arc 代理程式。](./media/vmware-scale-powercli/cli-windows-vanilla-1.png)

![未使用 Azure Arc 代理程式的香草 VMware vSphere 虛擬機器的另一個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-vanilla-2.png)

## <a name="deployment"></a>部署

執行 PowerCLI 腳本之前，您必須先設定 *install-azure-arc-agent.ps1* 腳本將使用的 [環境變數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/scaled_deployment/powercli/windows/vars.ps1)。 這些變數是以您剛才建立的 Azure 服務主體、您的 Azure 訂用帳戶和租使用者，以及 VMware vSphere 認證和資料為基礎。

1. 使用命令抓取您的 Azure 訂用帳戶識別碼和租使用者識別碼 `az account list`

2. 使用「必要條件」一節中建立的 Azure 服務主體識別碼和密碼：

    ![匯出環境變數的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-export-variables.png)

3. 從 `azure_arc_servers_jumpstart\vmware\scaled-deploy\powercli\windows` 資料夾中，以系統管理員身分開啟 PowerShell 會話，然後執行 `scale-deploy.ps1` 腳本。

    ![如何使用 PowerShell 腳本進行擴充部署的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-scale-deploy-1.png)

    ![如何使用 PowerShell 腳本進行擴充部署的第二個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-scale-deploy-2.png)

    ![如何使用 PowerShell 腳本進行擴充部署的第三個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-scale-deploy-3.png)

4. 完成時，VM 將會安裝 Azure Arc 連線的機器代理程式，以及已填入新啟用 Azure Arc 之伺服器的 Azure 資源群組。

    ![已安裝 Azure Arc 代理程式的電腦螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-agent.png)

    ![Azure 資源群組中已啟用 Azure Arc 之新伺服器的螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-servers-1.png)

    ![Azure 資源群組中新啟用 Azure Arc 之伺服器的另一個螢幕擷取畫面。](./media/vmware-scale-powercli/cli-windows-servers-2.png)

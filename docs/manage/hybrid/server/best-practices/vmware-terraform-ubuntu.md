---
title: 使用 Terraform 方案部署 VMware Ubuntu 虛擬機器，並將其連線到 Azure Arc
description: 使用 Terraform 方案部署 VMware Ubuntu 虛擬機器，並將它連線到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 99e146fe7d1a7218efc5de212c163ea87739d557
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794989"
---
# <a name="use-a-terraform-plan-to-deploy-a-vmware-ubuntu-virtual-machine-and-connect-it-to-azure-arc"></a>使用 Terraform 方案部署 VMware Ubuntu 虛擬機器，並將其連線到 Azure Arc

本文提供指導方針，說明如何使用提供的 [Terraform](https://www.terraform.io/) 方案來部署 Ubuntu Server、VMware vSphere 虛擬機器，並將其連線為啟用 Azure Arc 的伺服器資源。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. [安裝或更新 AZURE CLI 至2.7 版或更新版本](/cli/azure/install-azure-cli)。 使用下列命令來檢查您目前安裝的版本。

    ```console
    az --version
    ```

3. [安裝 Terraform >= 0.12](https://learn.hashicorp.com/tutorials/terraform/install-cli)

4. VMware vCenter Server 使用者，具有從 vSphere web 用戶端的範本部署虛擬機器的 [許可權](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vm_admin.doc/GUID-4D0F8E63-2961-4B71-B365-BBFA24673FDB.html) 。

5. 建立 Azure 服務主體。

    若要將 VMware vSphere 虛擬機器連線到 Azure Arc，需要有指派「參與者」角色的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

    ```bash
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

### <a name="preparing-an-ubuntu-server-vmware-vsphere-vm-template"></a>準備 Ubuntu server VMware vSphere VM 範本

使用下列指南部署 Ubuntu server VM 並將它連線到 Azure Arc 之前，需要 VMware vSphere 範本。 [本文說明如何](./vmware-ubuntu-template.md) 使用 VMware vSphere 6.5 和更新版本輕鬆地建立這類範本。

> [!NOTE]
> 如果您已經有 Ubuntu server VM 範本，仍建議使用本指南作為參考。

## <a name="deployment"></a>部署

執行 Terraform 計畫之前，您必須先設定計劃將使用的環境變數。 這些變數是根據您剛才建立的 Azure 服務主體、您的 Azure 訂用帳戶和租使用者，以及 VMware vSphere 認證。

1. 使用命令取出您的 Azure 訂用帳戶識別碼和租使用者識別碼 `az account list` 。

2. Terraform 方案會在 Microsoft Azure 和 VMware vSphere 中建立資源。 然後，它會在虛擬機器上執行腳本，以安裝 Azure Arc 代理程式和所有必要的構件。 此腳本需要有關 VMware vSphere 和 Azure 環境的特定資訊。 [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/ubuntu/terraform/scripts/vars.sh)使用適當的值編輯和更新每個變數。

    - `TF-VAR-subscription-id` = 您的 Azure 訂用帳戶識別碼
    - `TF-VAR-client-id` = 您的 Azure 服務主體名稱
    - `TF-VAR-client-secret`= 您的 Azure 服務主體密碼
    - `TF-VAR-tenant-id` = 您的 Azure 租使用者識別碼
    - `TF-VAR-resourceGroup` = Azure 資源組名
    - `TF-VAR-location` = Azure 區域
    - `TF-VAR-vsphere-user` = vCenter 系統管理員使用者名稱
    - `TF-VAR-vsphere-password` = vCenter 管理員密碼
    - `TF-VAR-vsphere-server` = vCenter server FQDN/IP
    - `TF-VAR-admin-user` = 操作系統管理員使用者名稱
    - `TF-VAR-admin-password` = 操作系統管理員密碼

3. 從 CLI 流覽至複製的存放庫 `azure_arc_servers_jumpstart/vmware/ubuntu/terraform` 目錄。

4. 使用 source 命令來匯出您所編輯的環境變數， [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/ubuntu/terraform/scripts/vars.sh) 如下所示。 Terraform 需要設定這些設定，才能讓計畫正常執行。 請注意，此腳本也會在 Terraform 部署時，在虛擬機器上遠端執行。

    `source ./scripts/vars.sh`

5. 除了 `TF-VAR` 您剛才匯出的環境變數之外，您還可以在中編輯 Terraform 變數， [`terraform.tfvars`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/ubuntu/terraform/terraform.tfvars) 以符合您的 VMware vSphere 環境。

    ![TF-VAR 環境變數的螢幕擷取畫面](./media/vmware-terraform-ubuntu/variables.png)

6. 執行 `terraform init` 命令，此命令會下載 Terraform AzureRM、local 和 vSphere 提供者。

    ![' Terraform init ' 命令的螢幕擷取畫面。](./media/vmware-terraform-ubuntu/terraform-init.png)

7. 執行 `terraform apply --auto-approve` 命令，並等候計畫完成。

8. Terraform 部署完成後，新的 Ubuntu server VM 將會啟動並執行，而且會在新建立的 Azure 資源群組中投影為已啟用 Azure Arc 的伺服器資源。

    ![[Terraform apply] 已完成的螢幕擷取畫面。](./media/vmware-terraform-ubuntu/terraform-apply.png)

    ![新 VMware vSphere Ubuntu server 虛擬機器的螢幕擷取畫面。](./media/vmware-terraform-ubuntu/new-vm.png)

    ![Azure 資源群組中已啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/vmware-terraform-ubuntu/server-1.png)

    ![Azure 資源群組中已啟用 Azure Arc 之伺服器的另一個螢幕擷取畫面。](./media/vmware-terraform-ubuntu/server-2.png)

## <a name="delete-the-deployment"></a>刪除部署

- 最直接的方法是透過 Azure 入口網站刪除 Azure Arc 資源，直接選取資源並將其刪除。 此外，也請刪除 VMware vSphere VM。

  ![已刪除且已啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/vmware-terraform-ubuntu/delete-server.png)

  如果您以手動方式刪除實例，則也應該刪除 Terraform 方案所建立的 *install-azure-arc-agent.sh* 。

- 如果您想要卸載整個環境，請使用如下 `terraform destroy --auto-approve` 所示的命令。

  ![[Terraform 終結] 命令的螢幕擷取畫面。](./media/vmware-terraform-ubuntu/terraform-destroy.png)

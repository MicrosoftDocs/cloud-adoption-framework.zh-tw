---
title: 使用 Terraform 方案部署 Google Cloud Platform Windows 實例，並將其連線到 Azure Arc
description: 使用 Terraform 方案部署 Google Cloud Platform Windows 實例，並將其連線到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 33bfa7ef30dabe154c6547f83185160ddd0d3340
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794377"
---
# <a name="use-a-terraform-plan-to-deploy-a-google-cloud-platform-windows-instance-and-connect-it-to-azure-arc"></a>使用 Terraform 方案部署 Google Cloud Platform Windows 實例，並將其連線到 Azure Arc

本文提供指導方針，說明如何使用提供的 [Terraform](https://www.terraform.io/) 方案來部署 WINDOWS Server GCP 實例，並將它連接為已啟用 Azure Arc 的伺服器資源。

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

4. **啟用帳單的 Google Cloud 帳戶：** [建立免費試用帳戶](https://cloud.google.com/free)。 若要建立 Windows Server 虛擬機器，您必須升級您的帳戶以啟用帳單。 從功能表選取 [ **帳單** ]，然後選取右下方的 [ **升級** ]。

    ![顯示如何在 GCP 帳戶上啟用帳單的第一個螢幕擷取畫面。](./media/gcp-windows/billing-1.png)

    ![第二個螢幕擷取畫面顯示如何啟用 GCP 帳戶的帳單。](./media/gcp-windows/billing-2.png)

    ![顯示如何在 GCP 帳戶上啟用帳單的第三個螢幕擷取畫面。](./media/gcp-windows/billing-3.png)

    **免責聲明：** 若要避免非預期的費用，請遵循本文結尾的「刪除部署」一節。

5. 建立 Azure 服務主體。

    若要將 GCP 的虛擬機器連線到 Azure Arc，需要有指派參與者角色的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

    ```console
    az login
    az ad sp create-for-rbac -n "<Unique SP Name>" --role contributor
    ```

    例如：

    ```console
    az ad sp create-for-rbac -n "http://AzureArcGCP" --role contributor
    ```

    輸出應該看起來像這樣︰

    ```json
    {
      "appId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "displayName": "AzureArcGCP",
      "name": "http://AzureArcGCP",
      "password": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "tenant": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }
    ```

    > [!NOTE]
    > 強烈建議您將服務主體的範圍設為特定的 [Azure 訂用帳戶和資源群組](/cli/azure/ad/sp)。

## <a name="create-a-new-gcp-project"></a>建立新的 GCP 專案

1. 流覽至 [GOOGLE API 主控台](https://console.cloud.google.com) ，並使用您的 Google 帳戶登入。 登入後，請建立名為 [的新專案](https://cloud.google.com/resource-manager/docs/creating-managing-projects) `Azure Arc demo` 。 建立之後，請務必複製專案識別碼，因為它與專案名稱通常不同。

    ![GCP 主控台中 [新增專案] 頁面的第一個螢幕擷取畫面。](./media/gcp-windows/new-project-1.png)

    ![GCP 主控台中 [新增專案] 頁面的第二個螢幕擷取畫面。](./media/gcp-windows/new-project-2.png)

2. 建立新專案並在頁面頂端的下拉式清單中選取之後，您必須啟用專案的「計算引擎 API」存取。 按一下 [ **+ 啟用 api 和服務** ]，並搜尋 *計算引擎*。 然後選取 [ **啟用** ] 以啟用 API 存取。

    ![GCP 主控台中 [計算引擎 API] * * 的第一個螢幕擷取畫面。](./media/gcp-windows/comp-eng-api-1.png)

    ![GCP 主控台中的第二個螢幕擷取畫面： [計算引擎 API]。](./media/gcp-windows/comp-eng-api-2.png)

3. 接下來，設定服務帳戶金鑰，Terraform 會使用此金鑰來建立和管理 GCP 專案中的資源。 移至 [ [建立服務帳戶金鑰] 頁面](https://console.cloud.google.com/apis/credentials/serviceaccountkey)。 從下拉式清單中選取 [ **新增服務帳戶** ]，並為其指定名稱，選取 [專案]，然後選取 [擁有者] 作為角色，然後選取 [ **建立**]。 這會下載 JSON 檔案，其中包含 Terraform 管理資源所需的所有認證。 將下載的 JSON 檔案複製到 `azure_arc_servers_jumpstart/gcp/windows/terraform` 目錄。

    ![如何在 GCP 主控台中建立服務帳戶的螢幕擷取畫面。](./media/gcp-windows/svc-account.png)

## <a name="deployment"></a>部署

執行 Terraform 計畫之前，您必須先設定並匯出計畫將使用的環境變數。 這些變數是以您剛才建立的 Azure 服務主體、您的 Azure 訂用帳戶和租使用者，以及 GCP 專案名稱為基礎。

1. 使用命令取出您的 Azure 訂用帳戶識別碼和租使用者識別碼 `az account list` 。

2. Terraform 方案會在 Microsoft Azure 和 Google Cloud Platform 中建立資源。 然後，它會在 GCP 的虛擬機器上執行腳本，以安裝 Azure Arc 代理程式和所有必要的構件。 此腳本需要 GCP 和 Azure 環境的特定資訊。 [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/windows/terraform/scripts/vars.sh)使用適當的值編輯和更新每個變數。

    - `TF-VAR-subscription-id` = 您的 Azure 訂用帳戶識別碼
    - `TF-VAR-client-id` = 您的 Azure 服務主體應用程式識別碼
    - `TF-VAR-client-secret` = 您的 Azure 服務主體密碼
    - `TF-VAR-tenant-id` = 您的 Azure 租使用者識別碼
    - `TF-VAR-gcp-project-id` = GCP 專案識別碼
    - `TF-VAR-gcp-credentials-filename` = GCP 認證 JSON 檔案名

3. 從 CLI 流覽至複製的存放庫 `azure_arc_servers_jumpstart/gcp/windows/terraform` 目錄。

4. 使用 source 命令來匯出您所編輯的環境變數， [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/windows/terraform/scripts/vars.sh) 如下所示。 Terraform 需要設定這些設定，才能讓計畫正常執行。

    ```console
    source ./scripts/vars.sh
    ```

5. 執行 `terraform init` 會下載 Terraform AzureRM 提供者的命令。

    ![' Terraform init ' 命令的螢幕擷取畫面。](./media/gcp-windows/terraform-init.png)

6. 接下來，執行 `terraform apply --auto-approve` 命令並等候計畫完成。 Terraform 腳本完成時，您將部署 GCP Windows Server 2019 VM 並起始腳本，以將 Azure Arc 代理程式下載至 VM，並將 VM 連線為新 Azure 資源群組內新的已啟用 Azure Arc 的伺服器。 代理程式需要幾分鐘的時間才會完成布建，因此請拿咖啡。

    ![[Terraform apply] 命令的螢幕擷取畫面。](./media/gcp-windows/terraform-apply.png)

7. 幾分鐘後，您應該可以開啟 Azure 入口網站並流覽至 `arc-gcp-demo` 資源群組。 在 GCP 中建立的 Windows Server 虛擬機器將會顯示為資源。

    ![Azure 入口網站中啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/gcp-windows/server.png)

## <a name="semi-automated-deployment-optional"></a>半自動化部署 (選擇性) 

Terraform 計畫會自動安裝 Azure Arc 代理程式，並在第一次啟動 VM 時執行 PowerShell 腳本，以將 VM 連線到 Azure 作為受控資源。

![[Azcmagent connect] 命令的螢幕擷取畫面。](./media/gcp-windows/azcmagent-connect.png)

如果您想要示範/控制實際的註冊程式，請執行下列動作：

1. `terraform apply`執行命令之前，請先 [`main.tf`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/windows/terraform/main.tf) 開啟並批註 `windows-startup-script-ps1 = local-file.install_arc_agent-ps1.content` 該行，然後儲存檔案。

    ![顯示 ' ' 已標記為停用 Azure Arc 代理程式自動上線的螢幕擷取畫面。](./media/gcp-windows/main-tf.png)

2. 依照 `terraform apply --auto-approve` 上面的指示執行。

3. 開啟 GCP 主控台並流覽至 [ [計算實例] 頁面](https://console.cloud.google.com/compute/instances)，然後選取已建立的 VM。

    ![GCP 主控台中的伺服器螢幕擷取畫面。](./media/gcp-windows/gcp-server.png)

    ![顯示如何在 GCP 中重設 Windows server 密碼的螢幕擷取畫面。](./media/gcp-windows/reset-password.png)

4. 選取 [ **設定密碼** ] 並指定使用者名稱，以建立 VM 的使用者和密碼。

    ![螢幕擷取畫面，顯示如何在 GCP 中設定 Windows 伺服器的使用者名稱和密碼。](./media/gcp-windows/name-pword.png)

5. 從 GCP 主控台中的 [VM] 頁面選取 RDP 按鈕，然後以您剛才建立的使用者名稱和密碼登入，以 RDP 連線至 VM。

    ![顯示如何使用 RDP 連線至 GCP 實例的螢幕擷取畫面。](./media/gcp-windows/gcp-rdp.png)

6. 登入後， **以系統管理員** 身分開啟 PowerShell ISE。 確定您執行的是 x64 版本的 PowerShell ISE，而不是 x86 版本。 開啟後，選取 [檔案] **> [新增** ] 以建立空白檔案 `.ps1` 。 然後貼上的整個內容 `./scripts/install_arc_agent.ps1` 。 按一下 [播放] 按鈕以執行腳本。 完成時，您應該會看到顯示電腦上架成功的輸出。

    ![顯示 Windows Powershell 整合式腳本環境與 Azure Arc 代理程式連接腳本的螢幕擷取畫面。](./media/gcp-windows/ise-script.png)

## <a name="delete-the-deployment"></a>刪除部署

若要刪除您在此示範中建立的所有資源，請使用如下 `terraform destroy --auto-approve` 所示的命令。

  ![[Terraform 終結] 命令的螢幕擷取畫面。](./media/gcp-windows/terraform-destroy.png)

或者，您可以直接從 [GCP 主控台](https://console.cloud.google.com/compute/instances)刪除 GCP VM。

  ![顯示如何從 GCP 主控台刪除虛擬機器的螢幕擷取畫面。](./media/gcp-windows/delete-vm.png)

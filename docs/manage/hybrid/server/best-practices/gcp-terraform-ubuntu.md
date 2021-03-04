---
title: 使用 Terraform 方案部署 Google Cloud Platform Ubuntu 實例，並將其連線到 Azure Arc
description: 使用 Terraform 方案來部署 Google Cloud Platform Ubuntu 實例，並將其連線到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: e1f004ca4572e6faa337730cb18004397e446ae3
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102114399"
---
# <a name="use-a-terraform-plan-to-deploy-a-google-cloud-platform-ubuntu-instance-and-connect-it-to-azure-arc"></a>使用 Terraform 方案部署 Google Cloud Platform Ubuntu 實例，並將其連線到 Azure Arc

本文提供的指引可讓您使用所提供的 [Terraform](https://www.terraform.io/) 方案來部署 Google Cloud PLATFORM (GCP) 實例，並將其連線為啟用 Azure Arc 的伺服器資源。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. [安裝或更新 AZURE CLI 至2.7 版或更新版本](/cli/azure/install-azure-cli)。 使用下列命令來檢查您目前安裝的版本。

    ```console
    az --version
    ```

3. [產生 ssh 金鑰](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (或使用現有的 ssh 金鑰) 

4. [建立免費的 Google Cloud Platform 帳戶](https://cloud.google.com/free)

5. [安裝 Terraform >= 0.12](https://learn.hashicorp.com/tutorials/terraform/install-cli)

6. 建立 Azure 服務主體。

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

<!-- docutune:casing "Compute Engine API" -->

## <a name="create-a-new-gcp-project"></a>建立新的 GCP 專案

1. 流覽至 [GOOGLE API 主控台](https://console.developers.google.com) ，並使用您的 Google 帳戶登入。 登入後，請建立名為 [的新專案](https://cloud.google.com/resource-manager/docs/creating-managing-projects) `Azure Arc demo` 。 建立之後，請務必複製專案識別碼，因為它通常會與專案名稱相同。

    ![GCP 主控台中 [新增專案] 頁面的第一個螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-new-project-1.png)

    ![GCP 主控台中 [新增專案] 頁面的第二個螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-new-project-2.png)

2. 建立新專案並在頁面頂端的下拉式清單中選取之後，您必須啟用專案的「計算引擎 API」存取。 按一下 [ **+ 啟用 api 和服務** ]，並搜尋 *計算引擎*。 然後選取 [ **啟用** ] 以啟用 API 存取。

    ![GCP 主控台中 [計算引擎 API] * * 的第一個螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-comp-eng-api-1.png)

    ![GCP 主控台中的第二個螢幕擷取畫面： [計算引擎 API]。](./media/gcp-ubuntu/ubuntu-comp-eng-api-2.png)

3. 接下來，設定服務帳戶金鑰，Terraform 會使用此金鑰來建立和管理 GCP 專案中的資源。 移至 [ [建立服務帳戶金鑰] 頁面](https://console.cloud.google.com/apis/credentials/serviceaccountkey)。 從下拉式清單中選取 [ **新增服務帳戶** ]，並為其指定名稱，選取 [專案]，然後選取 [擁有者] 作為角色，然後選取 [ **建立**]。 這會下載 JSON 檔案，其中包含 Terraform 管理資源所需的所有認證。 將下載的 JSON 檔案複製到 `azure_arc_servers_jumpstart/gcp/ubuntu/terraform` 目錄。

    ![如何在 GCP 主控台中建立服務帳戶的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-svc-account.png)

4. 最後，請確定您的 SSH 金鑰可在中使用 `~/.ssh` ，並命名為 `id_rsa.pub` 和 `id_rsa` 。 如果您遵循上述的 ssh keygen 指南來建立金鑰，則應該已經正確設定。 如果沒有，您可能需要修改 [`main.tf`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/ubuntu/terraform/main.tf) 才能使用具有不同路徑的金鑰。

## <a name="deployment"></a>部署

執行 Terraform 計畫之前，您必須先匯出計畫將使用的環境變數。 這些變數是以您剛才建立的 Azure 服務主體、您的 Azure 訂用帳戶和租使用者，以及 GCP 專案名稱為基礎。

1. 使用命令取出您的 Azure 訂用帳戶識別碼和租使用者識別碼 `az account list` 。

2. Terraform 方案會在 Microsoft Azure 和 Google Cloud Platform 中建立資源。 然後，它會在 GCP 的虛擬機器上執行腳本，以安裝 Azure Arc 代理程式和所有必要的構件。 此腳本需要 GCP 和 Azure 環境的特定資訊。 [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/ubuntu/terraform/scripts/vars.sh)使用適當的值編輯和更新每個變數。

    - `TF_VAR_subscription_id`= 您的 Azure 訂用帳戶識別碼
    - `TF_VAR_client_id` = 您的 Azure 服務主體應用程式識別碼
    - `TF_VAR_client_secret` = 您的 Azure 服務主體密碼
    - `TF_VAR_tenant_id` = 您的 Azure 租使用者識別碼
    - `TF_VAR_gcp_project_id` = GCP 專案識別碼
    - `TF_VAR_gcp_credentials_filename` = GCP 認證 JSON 檔案名

3. 從 CLI 流覽至複製的存放庫 `azure_arc_servers_jumpstart/gcp/ubuntu/terraform` 目錄。

4. 使用 source 命令來匯出您所編輯的環境變數， [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/ubuntu/terraform/scripts/vars.sh) 如下所示。 Terraform 需要設定這些設定，才能讓計畫正常執行。 請注意，此腳本也會在 Terraform 部署過程中，自動在 GCP 虛擬機器上遠端執行。

    ```console
    source ./scripts/vars.sh
    ```

5. 執行 `terraform init` 會下載 Terraform AzureRM 提供者的命令。

    ![' Terraform init ' 命令的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-terraform-init.png)

6. 接下來，執行 `terraform apply --auto-approve` 命令並等候計畫完成。 完成時，您會在新的資源群組內部署 GCP Ubuntu VM，並將其連線為新的已啟用 Azure Arc 的伺服器。

7. 開啟 Azure 入口網站並流覽至 `arc-gcp-demo` 資源群組。 在 GCP 中建立的虛擬機器將會顯示為資源。

    ![Azure 入口網站中啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-server.png)

## <a name="semi-automated-deployment-optional"></a>半自動化部署 (選擇性) 

您可能已經注意到，執行的最後一個步驟是將 VM 註冊為新的已啟用 Azure Arc 的伺服器資源。

![執行 ' azcmagent connect ' 命令的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-azcmagent-connect.png)

如果您想要示範/控制實際的註冊程式，請執行下列動作：

1. 在 [`install_arc_agent.sh.tmpl`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/ubuntu/terraform/scripts/install_arc_agent.sh.tmpl) 腳本範本中，將區段標記為批註 `run connect command` 並儲存檔案。

    ![顯示 ' main.tf ' 已批註的螢幕擷取畫面，以停用 Azure Arc 代理程式的自動上架。](./media/gcp-ubuntu/ubuntu-main-tf.png)

2. 藉由執行來取得 GCP VM 的公用 IP `terraform output` 。

    ![Terraform 輸出的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-terraform.png)

3. 使用 SSH 將 VM 作為 `ssh arcadmin@xx.xx.xx.xx` `xx.xx.xx.xx` 主機 IP。

    ![連接到 GCP 伺服器的 SSH 金鑰螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-ssh.png)

4. 匯出中的所有環境變數 [`vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/gcp/ubuntu/terraform/scripts/vars.sh)

    ![使用 ' vars.sh ' 匯出環境變數的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-export-variables.png)

5. 執行以下命令：

    ```console
    azcmagent connect --service-principal-id $TF_VAR_client_id --service-principal-secret $TF_VAR_client_secret --resource-group "Azure Arc gcp-demo" --tenant-id $TF_VAR_tenant_id --location "westus2" --subscription-id $TF_VAR_subscription_id
    ```

    ![已成功完成 [azcmagent connect] 命令的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-azcmagent.png)

6. 完成時，您的 VM 將會向 Azure Arc 註冊，並可透過 Azure 入口網站在資源群組中看到。

## <a name="delete-the-deployment"></a>刪除部署

若要刪除您在此示範中建立的所有資源，請使用如下 `terraform destroy --auto-approve` 所示的命令。

![[Terraform 終結] 命令的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-terraform-destroy.png)

或者，您可以直接從 [GCP 主控台](https://console.cloud.google.com/compute/instances)刪除 GCP VM。

![顯示如何從 GCP 主控台刪除虛擬機器的螢幕擷取畫面。](./media/gcp-ubuntu/ubuntu-delete-vm.png)

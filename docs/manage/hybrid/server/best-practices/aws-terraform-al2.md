---
title: 使用 Terraform 計畫在 Amazon 彈性計算雲端上部署 Amazon Linux 2 實例，並將其連線到 Azure Arc
description: 使用 Terraform 方案，在 Amazon 彈性計算雲端上部署 Amazon Linux 2 實例，並將其連線到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: ea1ea4913b3adb406806696254bdec0d514341a9
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112223"
---
# <a name="use-a-terraform-plan-to-deploy-an-amazon-linux-2-instance-on-amazon-elastic-compute-cloud-and-connect-it-to-azure-arc"></a>使用 Terraform 計畫在 Amazon 彈性計算雲端上部署 Amazon Linux 2 實例，並將其連線到 Azure Arc

本文提供指導方針，說明如何使用提供的 [Terraform](https://www.terraform.io/) 方案來部署 Amazon Web SERVICES (AWS) Amazon 彈性計算雲端 (EC2) Linux 2 實例，並將它連接為已啟用 Azure Arc 的伺服器資源。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. [安裝或更新 AZURE CLI](/cli/azure/install-azure-cli)。 Azure CLI 應執行2.7.0 版或更新版本。 用 `az --version` 來檢查您目前安裝的版本。

3. [產生 ssh 金鑰](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (或使用現有的 ssh 金鑰) 

4. [建立免費的 AWS 帳戶](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

5. [安裝 Terraform >= 0.12](https://learn.hashicorp.com/tutorials/terraform/install-cli)

6. 建立 Azure 服務主體。

    若要將 AWS 的虛擬機器連線到 Azure Arc，需要有指派參與者角色的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令：

    ```console
    az login
    az ad sp create-for-rbac -n "http://AzureArcAWS" --role contributor
    ```

    輸出應如下所示：

    ```json
    {
      "appId": "XXXXXXXXXXXXXXXXXXXXXXXX",
      "displayName": "AzureArcAWS",
      "name": "http://AzureArcAWS",
      "password": "XXXXXXXXXXXXXXXXXXXXXXXX",
      "tenant": "XXXXXXXXXXXXXXXXXXXXXXXX"
    }
    ```

    > [!NOTE]
    > 強烈建議您將服務主體的範圍設為特定的 [Azure 訂用帳戶和資源群組](/cli/azure/ad/sp)。

## <a name="create-an-aws-identity"></a>建立 AWS 身分識別

為了讓 Terraform 在 AWS 中建立資源，我們必須使用適當的許可權建立新的 AWS IAM 角色，並設定 Terraform 來使用它。

1. 登入 [AWS 管理主控台](https://console.aws.amazon.com/console/home)

2. 登入之後，請選取左上方的 [ **服務** ] 下拉式清單。 在 [ **安全性、身分識別和合規性**] 下方，選取 [ **IAM** ] 以存取 [身分 [識別與存取管理] 頁面](https://console.aws.amazon.com/iam/home)

    ![AWS cloud 主控台的螢幕擷取畫面。](./media/aws-terraform-al2/al2-aws-console.png)

    ![身分識別和存取管理 AWS 雲端主控台的螢幕擷取畫面。](./media/aws-terraform-al2/al2-aws-iam.png)

3. 從左側功能表中按一下 [ **使用者** ]，然後選取 [ **新增使用者** ] 以建立新的 IAM 使用者。

    ![如何在 AWS cloud 主控台中建立新使用者的螢幕擷取畫面。](./media/aws-terraform-al2/al2-new-user-1.png)

4. 在 [**新增使用者**] 頁面上，將使用者命名 `Terraform` 並選取 [程式 **設計存取**] 核取方塊，然後選取 **[下一步]** 。

    ![第二個螢幕擷取畫面：如何在 AWS cloud 主控台中建立新的使用者。](./media/aws-terraform-al2/al2-new-user-2.png)

5. 在 [**設定許可權**] 頁面上，選取 [**直接附加現有的原則**]，然後核取 [ **AmazonEC2FullAccess** ] 旁的方塊，如螢幕擷取畫面所示，然後選取 [**下一步]** 。

    ![如何在 AWS cloud 主控台中建立新使用者的第三個螢幕擷取畫面。](./media/aws-terraform-al2/al2-new-user-3.png)

6. 在 [ **標記** ] 頁面上，指派具有索引鍵的標記 `azure-arc-demo` ，然後選取 **[下一步]** 繼續前往 [審核] 頁面。

    ![AWS cloud 主控台中標籤的螢幕擷取畫面。](./media/aws-terraform-al2/al2-tags.png)

7. 確認所有專案都正確無誤，然後選取 [ **建立使用者**]。

    ![第四個螢幕擷取畫面：如何在 AWS cloud 主控台中建立新的使用者。](./media/aws-terraform-al2/al2-new-user-4.png)

8. 建立使用者之後，您會看到使用者的存取金鑰識別碼和密碼存取金鑰。 選取 [ **關閉**] 之前，請先將這些值複製下來。 在下一個頁面上，您可以看到這看起來應該像這樣的範例。 擁有這些金鑰之後，您就可以搭配 Terraform 使用它們來建立 AWS 資源。

    ![在 AWS cloud 主控台中成功建立使用者的螢幕擷取畫面。](./media/aws-terraform-al2/al2-new-user-5.png)

## <a name="configure-terraform"></a>設定 Terraform

執行 Terraform 計畫之前，您必須先匯出計畫將使用的環境變數。 這些變數是根據您的 Azure 訂用帳戶和租使用者、Azure 服務主體，以及您剛才建立的 AWS IAM 使用者和金鑰。

1. 使用命令取出您的 Azure 訂用帳戶識別碼和租使用者識別碼 `az account list` 。

2. Terraform 方案會在 Microsoft Azure 和 AWS 中建立資源。 然後，它會在 AWS EC2 虛擬機器上執行腳本，以安裝 Azure Arc 代理程式和所有必要的構件。 此腳本需要 AWS 和 Azure 環境的特定資訊。 [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/AL2/terraform/scripts/vars.sh)使用適當的值編輯和更新每個變數。

    - `TF_VAR_subscription_id`= 您的 Azure 訂用帳戶識別碼
    - `TF_VAR_client_id`= 您的 Azure 服務主體應用程式識別碼
    - `TF_VAR_client_secret` = 您的 Azure 服務主體密碼
    - `TF_VAR_tenant_id`= 您的 Azure 租使用者識別碼
    - `AWS_ACCESS_KEY_ID` = AWS 存取金鑰
    - `AWS_SECRET_ACCESS_KEY` = AWS 秘密金鑰

3. 從 Azure CLI 流覽至複製的存放庫 `azure_arc_servers_jumpstart/aws/al2/terraform` 目錄。

4. 使用 source 命令來匯出您所編輯的環境變數， [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/AL2/terraform/scripts/vars.sh) 如下所示。 Terraform 需要設定這些設定，才能讓計畫正常執行。 請注意，此腳本也會在 Terraform 部署過程中，自動在 AWS 虛擬機器上遠端執行。

    ```console
    source ./scripts/vars.sh
    ```

5. 確定您的 SSH 金鑰可在中使用 `~/.ssh` ，並將其命名為 `id_rsa.pub` 和 `id_rsa` 。 如果您遵循上述的 ssh keygen 指南來建立金鑰，則應該已經正確設定。 如果沒有，您可能需要修改 [`main.tf`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/AL2/terraform/main.tf) 才能使用具有不同路徑的金鑰。

6. 執行 `terraform init` 會下載 Terraform AzureRM 提供者的命令。

    ![' Terraform init ' 命令的螢幕擷取畫面。](./media/aws-terraform-al2/al2-terraform-init.png)

## <a name="deployment"></a>部署

1. 執行 `terraform apply --auto-approve` 命令，並等候計畫完成。 完成時，您會在新的資源群組內部署 AWS Amazon Linux 2 EC2 實例，並將其連接為新的已啟用 Azure Arc 的伺服器。

2. 開啟 Azure 入口網站並流覽至 `arc-servers-demo` 資源群組。 在 AWS 中建立的虛擬機器將會顯示為資源。

    ![顯示 azure 入口網站中已啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/aws-terraform-al2/al2-server.png)

## <a name="semi-automated-deployment-optional"></a>半自動化部署 (選擇性) 

您可能已經注意到，執行的最後一個步驟是將 VM 註冊為新的已啟用 Azure Arc 的伺服器資源。

  ![[Azcmagent connect] 命令的螢幕擷取畫面。](./media/aws-terraform-al2/al2-azcmagent.png)

如果您想要示範/控制實際的註冊程式，請執行下列動作：

1. 在 [`install_arc_agent.sh.tmpl`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/AL2/terraform/scripts/install_arc_agent.sh.tmpl) 腳本範本中，將區段標記為批註 `run connect command` 並儲存檔案。

    ![已批註化 ' azcmagent connect ' 命令的螢幕擷取畫面。](./media/aws-terraform-al2/al2-azcmagent-commented.png)

2. 藉由執行來取得 AWS VM 的公用 IP `terraform output` 。

    ![Terraform 輸出的螢幕擷取畫面。](./media/aws-terraform-al2/al2-terraform-output.png)

3. 使用 SSH 連線至 VM `ssh ec2-user@xx.xx.xx.xx` ，其中 `xx.xx.xx.xx` 是主機 IP。

    ![連接到 EC2 伺服器的 SSH 金鑰螢幕擷取畫面。](./media/aws-terraform-al2/al2-ssh.png)

4. 匯出中的所有環境變數 [`vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/AL2/terraform/scripts/vars.sh)

    ![在 ' var.sh ' 中匯出的環境變數螢幕擷取畫面。](./media/aws-terraform-al2/al2-export-variables.png)

5. 執行以下命令：

    ```console
    azcmagent connect --service-principal-id $TF_VAR_client_id --service-principal-secret $TF_VAR_client_secret --resource-group "Arc-Servers-Demo" --tenant-id $TF_VAR_tenant_id --location "westus2" --subscription-id $TF_VAR_subscription_id
    ```

    ![[Azcmagent connect] 命令的另一個螢幕擷取畫面。](./media/aws-terraform-al2/al2-azcmagent-2.png)

6. 完成時，您的 VM 將會向 Azure Arc 註冊，並可透過 Azure 入口網站在資源群組中看到。

## <a name="delete-the-deployment"></a>刪除部署

若要刪除您在此示範中建立的所有資源，請使用如下 `terraform destroy --auto-approve` 所示的命令。
    ![[Terraform 終結] 命令的螢幕擷取畫面。](./media/aws-terraform-al2/al2-terraform-destroy.png)

或者，您也可以從 [AWS 主控台](https://console.aws.amazon.com/ec2/v2/home)終止 AWS EC2 實例，以直接將它刪除。
    ![如何在 AWS 主控台中終止實例的螢幕擷取畫面。](./media/aws-terraform-al2/al2-terminate.png)

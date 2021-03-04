---
title: 使用 Ansible 將 Amazon Web Services Amazon 彈性計算雲端實例的上架調整至 Azure Arc
description: 使用 Ansible 將 Amazon Web Services Amazon 彈性計算雲端實例的上架調整至 Azure Arc
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 58b8355659779ef4258dd9d2935e8b09d3d9559b
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794420"
---
# <a name="use-ansible-to-scale-onboarding-amazon-web-services-amazon-elastic-compute-cloud-instances-to-azure-arc"></a>使用 Ansible 將 Amazon Web Services Amazon 彈性計算雲端實例的上架調整至 Azure Arc

本文提供指導方針，說明如何使用 [Ansible](https://www.ansible.com/) 來擴大 Amazon Web Services 的 (AWS) Amazon 彈性計算雲端 (amazon EC2) 實例到 Azure Arc。

本指南假設您對 Ansible 有基本的瞭解。 提供基本的 Ansible 腳本和設定，以使用 [`amazon.aws.aws-ec2`](https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html) 外掛程式動態載入 EC2 伺服器清查。

即使您還沒有現有的 Ansible 測試環境，也可以使用本指南，其中包含的 Terraform 計畫將會建立包含四個 Windows Server 2019 伺服器和四部 Ubuntu server 的範例 AWS EC2 伺服器清查，以及具有簡單設定的基本 CentOS 7 Ansible 控制伺服器。

> [!WARNING]
> 提供的 Ansible 範例活頁簿使用 WinRM 搭配密碼驗證和 HTTP 來設定 Windows 伺服器。 這不建議用於生產環境。 如果您打算在生產環境中使用 Ansible 搭配 Windows 主機，則應該使用 [WinRM OVER HTTPS](/troubleshoot/windows-client/system-management-components/configure-winrm-for-https) 搭配憑證。

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

4. [建立免費的 AWS 帳戶](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

5. [安裝 Terraform >= V 0.13](https://learn.hashicorp.com/tutorials/terraform/install-cli)

   - 建立 Azure 服務主體。

      若要將 AWS 的虛擬機器連線到 Azure Arc，需要有指派參與者角色的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

      ```console
      az login
      az ad sp create-for-rbac -n "<Unique SP Name>" --role contributor
      ```
  
      例如：
  
      ```console
      az ad sp create-for-rbac -n "http://AzureArcAWS" --role contributor
      ```
  
      輸出應該看起來像這樣︰
  
      ```json
      {
      "appId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "displayName": "AzureArcAWS",
      "name": "http://AzureArcAWS",
      "password": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "tenant": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
      }
      ```
  
      > [!NOTE]
      > 強烈建議您將服務主體的範圍設為特定的 [Azure 訂用帳戶和資源群組](/cli/azure/ad/sp)。
  
## <a name="create-an-aws-identity"></a>建立 AWS 身分識別

為了讓 Terraform 在 AWS 中建立資源，我們必須使用適當的許可權建立新的 AWS IAM 角色，並設定 Terraform 來使用它。

1. 登入 [AWS 管理主控台](https://console.aws.amazon.com/console/home)

2. 登入之後，請選取左上方的 [ **服務** ] 下拉式清單。 在 [ **安全性、身分識別和合規性**] 下方，選取 [ **IAM** ] 以存取 [身分 [識別與存取管理] 頁面](https://console.aws.amazon.com/iam/home)

    ![AWS cloud 主控台的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-aws-console.png)

    ![AWS cloud 主控台中身分識別和存取管理的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-iam.png)

3. 從左側功能表中選取 [ **使用者** ]，然後選取 [ **新增使用者** ] 以建立新的 IAM 使用者。

    ![AWS cloud 主控台中新使用者的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-new-user-1.png)

4. 在 [**新增使用者**] 頁面上，將使用者命名為 **Terraform** ，並選取 [程式 **設計存取**] 核取方塊，然後選取 **[下一步]** 。

    ![在 AWS cloud 主控台中建立新使用者的第二個螢幕擷取畫面。](./media/aws-scale-ansible/ansible-new-user-2.png)

5. 在下一個頁面上， **設定許可權**，選取 [ **直接附加現有的原則** ]，然後核取 [ **AmazonEC2FullAccess** ] 旁的方塊，如螢幕擷取畫面所示，然後選取 [ **下一步]**。

    ![在 AWS cloud 主控台中建立新使用者的第三個螢幕擷取畫面。](./media/aws-scale-ansible/ansible-new-user-3.png)

6. 在 [ **標記** ] 頁面上，指派具有索引鍵的標記， `azure-arc-demo` 然後選取 **[下一步]** 以繼續前往 [ **審核** ] 頁面。

    ![AWS cloud 主控台中標籤的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-tags.png)

7. 確認所有專案都正確無誤，然後選取 [ **建立使用者**]。

    ![AWS cloud 主控台中新使用者的第四個螢幕擷取畫面。](./media/aws-scale-ansible/ansible-new-user-4.png)

8. 建立使用者之後，您會看到使用者的存取金鑰識別碼和密碼存取金鑰。 選取 [ **關閉**] 之前，請先將這些值複製下來。 在下一個頁面上，您可以看到這看起來應該像這樣的範例。 擁有這些金鑰之後，您就可以搭配 Terraform 使用它們來建立 AWS 資源。

    ![在 AWS cloud 主控台中成功建立使用者的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-new-user-5.png)

## <a name="option-1-create-a-sample-aws-server-inventory-and-ansible-control-server-using-terraform-and-onboard-the-servers-to-azure-arc"></a>選項1：使用 Terraform 建立範例 AWS 伺服器清查和 Ansible 控制伺服器，並將伺服器上架到 Azure Arc

> [!NOTE]
> 如果您已經有現有的 AWS 伺服器清查和 Ansible 伺服器，請跳至選項2。

### <a name="configure-terraform"></a>設定 Terraform

執行 Terraform 計畫之前，您必須先匯出計畫將使用的環境變數。 這些變數是根據您的 Azure 訂用帳戶和租使用者、Azure 服務主體，以及您剛才建立的 AWS IAM 使用者和金鑰。

1. 使用命令取出您的 Azure 訂用帳戶識別碼和租使用者識別碼 `az account list` 。

2. Terraform 方案會在 Microsoft Azure 和 AWS 中建立資源。 然後，它會在 AWS EC2 虛擬機器上執行腳本，以安裝 Ansible 和所有必要的構件。 此 Terraform 方案需要使用環境變數來存取 AWS 和 Azure 環境的特定資訊。 [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/scaled_deployment/ansible/terraform/scripts/vars.sh)使用適當的值編輯和更新每個變數。

   - `TF-VAR-subscription-id` = 您的 Azure 訂用帳戶識別碼
   - `TF-VAR-client-id` = 您的 Azure 服務主體應用程式識別碼
   - `TF-VAR-client-secret` = 您的 Azure 服務主體密碼
   - `TF-VAR-tenant-id` = 您的 Azure 租使用者識別碼
   - `AWS-ACCESS-KEY-ID` = AWS 存取金鑰
   - `AWS-SECRET-ACCESS-KEY` = AWS 秘密金鑰

3. 從您的 shell 流覽至 `azure_arc_servers_jumpstart/aws/scaled_deployment/ansible/terraform` 複製之存放庫的) 目錄。

4. 使用 source 命令來匯出您所編輯的環境變數， [`scripts/vars.sh`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/scaled_deployment/ansible/terraform/scripts/vars.sh) 如下所示。 Terraform 需要設定這些設定，才能讓計畫正常執行。

    ```console
    source ./scripts/vars.sh
    ```

5. 確定您的 SSH 金鑰可在中使用 `~/.ssh` ，並將其命名為 `id-rsa.pub` 和 `id-rsa` 。 如果您遵循上述的 SSH keygen 指南來建立金鑰，則應該已經正確設定。 如果沒有，您可能需要修改 [`aws-infrastructure.tf`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/scaled_deployment/ansible/terraform/aws_infra.tf) 才能使用具有不同路徑的金鑰。

6. 執行命令，此 `terraform init` 命令會下載所需的 Terraform 提供者。

    ![' Terraform init ' 命令的螢幕擷取畫面。](./media/aws-scale-ansible/terraform-init.png)

### <a name="deploy-server-infrastructure"></a>部署伺服器基礎結構

1. 從 `azure_arc_servers_jumpstart/aws/scaled_deployment/ansible/terraform` 目錄中執行， `terraform apply --auto-approve` 並等候計畫完成。 成功完成時，您將會有四個 Windows Server 2019 伺服器、四部 Ubuntu 伺服器，以及一部 CentOS 7 Ansible control Server。

2. 開啟 AWS 主控台，並確認您可以看到已建立的伺服器。

    ![AWS 主控台的螢幕擷取畫面，其中顯示 EC2 實例。](./media/aws-scale-ansible/ec2-instances.png)

### <a name="run-the-ansible-playbook-to-onboard-the-aws-ec2-instances-as-azure-arc-enabled-servers"></a>執行 Ansible 腳本，將 AWS EC2 實例上架為啟用 Azure Arc 的伺服器

1. 當 Terraform 方案完成時，它會在名為的輸出變數中顯示 Ansible 控制伺服器的公用 IP `ansible-ip` 。 執行以 SSH 連線到 Ansible 伺服器 `ssh centos@xx.xx.xx.xx` ，其中 `xx.xx.xx.xx` 會將取代為您 Ansible 伺服器的 IP 位址。

    ![使用 Ansible 連接到遠端伺服器之 SSH 金鑰的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-ssh.png)

2. 執行，將目錄變更為 `ansible` 目錄 `cd ansible` 。 此資料夾包含範例 Ansible 設定，以及我們將用來將伺服器上架到 Azure Arc 的腳本。

    ![列出 ' ' 檔案之 shell 腳本的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-cfg.png)

3. `aw-ec2`Ansible 外掛程式需要 AWS 認證，以動態方式讀取您的 AWS 伺服器清查。 我們會將它們匯出為環境變數。 執行下列命令，並將和的值取代為 `AWS-ACCESS-KEY-ID` `AWS-SECRET-ACCESS-KEY` 您稍早建立的 AWS 認證。

    ```console
    export AWS-ACCESS-KEY-ID="XXXXXXXXXXXXXXXXX"
    export AWS-SECRET-ACCESS-KEY="XXXXXXXXXXXXXXX"
    ```

4. 將檔案中 Azure 租使用者識別碼和訂用帳戶識別碼的預留位置值取代為 [`group-vars/all.yml`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/scaled_deployment/ansible/terraform/ansible_config/group_vars/all.yml) 您環境的適當值。

    ![YAML 檔中的變數螢幕擷取畫面。](./media/aws-scale-ansible/yml-variables.png)

5. 執行下列命令來執行 Ansible 腳本，並以您的 Azure 服務主體識別碼和服務主體密碼取代。

    ```console
    ansible-playbook arc-agent.yml -i ansible-plugins/inventory-uswest2-aws-ec2.yml --extra-vars '{"service-principal-id": "XXXXXXX-XXXXX-XXXXXXX", "service-principal-secret": "XXXXXXXXXXXXXXXXXXXXXXXX"}'
    ```

    如果腳本執行成功，您應該會看到類似下列螢幕擷取畫面的輸出。

    ![正在執行之 Ansible 腳本的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-playbook.png)

6. 開啟 Azure 入口網站並流覽至 aws 示範的資源群組。 您應該會看到列出啟用 Azure Arc 的伺服器。

    ![Azure 入口網站的螢幕擷取畫面，其中已啟用 Azure Arc 的伺服器。](./media/aws-scale-ansible/onboarding-servers.png)

### <a name="clean-up-environment-by-deleting-resources"></a>藉由刪除資源來清理環境

若要刪除您在此示範中建立的所有資源，請使用如下 `terraform destroy --auto-approve` 所示的命令。

![[Terraform 終結] 命令的螢幕擷取畫面。](./media/aws-scale-ansible/terraform-destroy.png)

## <a name="option-2-onboarding-an-existing-aws-server-inventory-to-azure-arc-using-your-own-ansible-control-server"></a>選項2：使用您自己的 Ansible 控制伺服器將現有的 AWS 伺服器清查上線至 Azure Arc

> [!NOTE]
> 如果您沒有現有的 AWS 伺服器清查和 Ansible 伺服器，請流覽回選項1。

### <a name="review-provided-ansible-configuration-and-playbook"></a>複習提供的 Ansible 設定和腳本

1. 流覽至 `ansible-config` 目錄，並檢查提供的設定。 提供的設定包含基本檔案 `ansible.cfg` 。 此檔案會啟用 [`amazon.aws.aws-ec2`](https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws-ec2-inventory.html) Ansible 外掛程式，該外掛程式會使用 AWS IAM 角色動態載入您的伺服器清查。 確定您使用的 IAM 角色具有足夠的許可權，可存取您想要上架的清查。

    ![顯示 ' ansible ' 檔案詳細資料的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-cfg-details.png)

2. 此 [`inventory-uswest2-aws-ec2.yml`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/aws/scaled_deployment/ansible/terraform/ansible_config/ansible_plugins/inventory_uswest2_aws_ec2.yml) 檔案會設定 `aws-ec2` 外掛程式，以依套用 `uswest2` 的標記從區域和群組資產提取清查。 視需要調整此檔案，以支援將您的伺服器清查上架，例如變更區域或調整群組或篩選。

   `./ansible-config/group-vars`您應調整中的檔案，以提供您想要用來將各種 Ansible 主機群組上線的認證。

3. 當您調整所提供的設定以支援您的環境時，請執行下列命令來執行 Ansible 腳本，並以您的 Azure 服務主體識別碼和服務主體密碼取代。

    ```console
    ansible-playbook arc-agent.yml -i ansible-plugins/inventory-uswest2-aws-ec2.yml --extra-vars '{"service-principal-id": "XXXXXXX-XXXXX-XXXXXXX", "service-principal-secret": "XXXXXXXXXXXXXXXXXXXXXXXX"}'
    ```

    如前所述，如果腳本執行成功，您應該會看到類似下列螢幕擷取畫面的輸出：

      ![正在執行之 Ansible 腳本的螢幕擷取畫面。](./media/aws-scale-ansible/ansible-playbook.png)

    如先前所述，開啟 Azure 入口網站並流覽至 aws 示範的資源群組。 您應該會看到列出啟用 Azure Arc 的伺服器。

    ![Azure 入口網站的螢幕擷取畫面，其中顯示已啟用 Azure Arc 的伺服器。](./media/aws-scale-ansible/onboarding-servers.png)

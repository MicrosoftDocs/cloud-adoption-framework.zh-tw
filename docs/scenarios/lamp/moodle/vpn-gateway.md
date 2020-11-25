---
title: 建立虛擬網路閘道並連接至 Vm
description: 使用私人 IP 位址和密碼，建立虛擬網路閘道、憑證和 VPN，並使用 SSH 連線至虛擬機器擴展集實例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/23/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 0f30f40a1204c52657868d56e22726b2ab23b4d2
ms.sourcegitcommit: bd6104aaa0e0145dcb0f577107d2792bc5b48790
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96038337"
---
# <a name="create-a-virtual-network-gateway-and-connect-to-vms"></a>建立虛擬網路閘道並連接至 Vm

部署 Azure Moodle 資源之後，請建立 Azure 虛擬網路閘道，讓您可以透過私人 IP 位址連線到 Moodle 虛擬機器擴展集實例。

## <a name="create-a-virtual-network-gateway"></a>建立虛擬網路閘道

您可以使用 Azure 入口網站或 Azure 命令列介面 (Azure CLI) 來建立虛擬網路閘道。

在 [Azure 入口網站](https://portal.azure.com)：

1. 搜尋並選取 [ **虛擬網路閘道**]。
   
1. 選取 [ **建立虛擬網路閘道**]。
   
1. 在 [ **建立虛擬網路閘道** ] 頁面上，完成下欄欄位：
   - 選取您的 **訂** 用帳戶。
   - 填入閘道的 **名稱** 。
   - 選取 Moodle Azure Resource Manager (ARM) 範本部署的 **虛擬網路** 。
   - 填入 **公用 IP 位址名稱**。
   
1. 保留其餘欄位的預設填滿值。
   
1. 選取 [ **審核 + 建立**]，並在驗證通過時選取 [ **建立**]。

![顯示 Azure 入口網站建立虛擬網路閘道] 畫面的螢幕擷取畫面。](images/vpn-gateway.png)

或者，執行下列 Azure CLI 命令以建立閘道：

```azurecli
az network vnet-gateway create -g <moodle resource group> -n <new virtual network gateway name> --public-ip-address <new gateway public ip address name> --vnet <moodle virtual network> --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
```

## <a name="generate-certificates"></a>產生憑證

使用 Windows PowerShell ISE 來產生根和子系憑證。

- 執行下列命令以產生根憑證：

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

- 執行下列命令以產生子憑證：

  ```powershell
  New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="export-the-certificates"></a>匯出憑證

匯出憑證以將其安裝在您的系統上。

1. 從 Windows [開始] 功能表中，選取 [ **執行**]，然後輸入 **mmc**。
   
1. 在 Microsoft Management Console 左方流覽窗格的 [ **個人** ] 資料夾底下，選取 [ **憑證**]。
   
尋找 **P2SRootCert** 和 **P2SChildCert** 憑證。

若要匯出根憑證：

1. 以滑鼠右鍵按一下或按住 **P2SRootCert**，指向 [ **所有** 工作]，然後選取 [ **匯出**]。
1. 在 [憑證匯出精靈] 中選取 [下一步]。
1. 選取 [ **否，不要匯出私密金鑰**]，然後選取 **[下一步]**。
1. 選取 [ **Base-64 編碼的 x.509 ( .cer)**，然後選取 **[下一步]**。
1. 輸入檔案名，然後選取 **[下一步]**。
1. 選取 [完成]。
1. [ **匯出成功** ] 訊息隨即出現。 選取 [確定]。

若要匯出子憑證：

1. 以滑鼠右鍵按一下或按住 **P2SChildCert**，指向 [ **所有** 工作]，然後選取 [ **匯出**]。
1. 在 [憑證匯出精靈] 中選取 [下一步]。
1. 選取 **[是，匯出私密金鑰**]，然後選取 **[下一步]**。
1. 選取 [ **個人資訊交換-PKCS #12 (]。PFX)**，然後選取 **[下一步]**。
1. 選取 [ **密碼** ] 核取方塊，並提供並確認密碼。
1. 在 [ **加密**] 底下，選取 [ **TripleDES-SHA1**]，然後選取 **[下一步]**。
1. 輸入檔案名，然後選取 **[下一步]**。
1. 選取 [完成]。
1. [ **匯出成功** ] 訊息隨即出現。 選取 [確定]。

## <a name="configure-the-virtual-network-gateway"></a>設定虛擬網路閘道

1. 在文字編輯器中開啟匯出的根憑證檔案，然後複製內容。
1. 在 Azure 入口網站中，移至您的虛擬網路閘道。
1. 在左側導覽中，選取 [ **點對站-** 設定]。
1. 選取 [ **立即設定**]。
1. 例如，在 [ **位址集區**] 下，輸入 GatewaySubnet 位址集區 `192.168.xx.0/24` 。
1. 在 [通道 **類型**] 下，選取 [ **IKEv2**]。
1. 在 [ **驗證類型**] 下，選取 [ **Azure 認證**]。
1. 在 [ **根憑證**：
   - 輸入 **Name** 的 **Root** 。
   - 將複製的根憑證程式碼貼到 [ **公開憑證資料**] 底下。
1. 選取 [儲存]。

## <a name="download-and-connect-through-the-vpn-client"></a>下載並透過 VPN 用戶端連接

若要建立 VPN 閘道連線：

1. 儲存虛擬私人網路設定之後，請選取功能表列中的 [ **下載 VPN 用戶端** ]。
1. 將下載的 VPN 用戶端 zip 檔案的內容解壓縮，開啟 `WindowsAMD64` 資料夾，然後執行檔案 `VpnClientSetupAmd64.exe` 以安裝 VPN 用戶端。
1. 在 Windows 中，移至 **主控台**  >  **網路和網際網路**  >  **網路** 連線，以查看已安裝的 VPN。
1. 在 VPN 上按一下滑鼠右鍵，然後選取 **[連線]**。
1. 在 [ **VPN** ] 視窗中，選取 **[連線]**。

## <a name="configure-ssh-password-authentication"></a>設定 SSH 密碼驗證

若要設定密碼驗證，請從控制器虛擬機器 (VM) ：

1. 開啟 `sshd` 設定檔以進行編輯：
   
   ```bash
   sudo vi /etc/ssh/sshd_config
   ```
   
1. 更新下列參數：
   
   - `PasswordAuthentication`從變更 `no` 為 `yes` 。
   - 尋找批註 `UseLogin` 、移除 `#` ，然後將值變更為 `yes` 。
   
1. 按下 ESC 鍵並輸入 `:wq!` 以儲存變更。
   
1. `sshd`執行下列命令來重新開機：
   
   ```bash
   sudo systemctl restart sshd
   ```
   
1. 執行下列命令來設定密碼：
   
   ```bash
   sudo passwd <username>
   ```
   
   例如，此命令會 `sudo passwd azureadmin` 設定使用者的密碼 `azureadmin` 。
   
1. 出現提示時，輸入並重新輸入密碼。

## <a name="sign-in-to-vms-from-the-controller-vm"></a>從控制器 VM 登入 Vm

透過 SSH 登入具有私人 IP 位址的擴展集 Vm。

1. 登入控制器 VM。
   
1. 執行此命令以連接到私人 VM：
   
   ```bash
   sudo ssh <username>@<private IP address>
   ```
   
   例如， `sudo ssh azureadmin@102.xx.xx.xx`
   
1. 在提示字元中，輸入密碼。

## <a name="next-steps"></a>後續步驟

繼續 [Moodle 手動遷移步驟](migration-start.md) ，以進行 Moodle 遷移程式中的後續步驟。

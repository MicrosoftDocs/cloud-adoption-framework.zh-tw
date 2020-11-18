---
title: 如何建立虛擬網路閘道，並透過私人 IP 連接
description: 瞭解如何建立虛擬網路閘道，並透過私人 IP 連接。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: f4b3c36fcbebda0501a05f4b0751832202be9bd2
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714491"
---
# <a name="how-to-create-a-virtual-network-gateway-and-connect-through-a-private-ip"></a>如何建立虛擬網路閘道，並透過私人 IP 連接

本檔說明如何在 Azure 中設定虛擬網路閘道。

## <a name="getting-started"></a>開始使用

在 Azure 入口網站中建立虛擬網路閘道，請執行下列步驟：

- 搜尋並選取 [ **虛擬網路閘道**]。
- 選取 [ **建立** ] 以開啟視窗。
- 填寫所有欄位，例如 **名稱、區域、閘道類型、SKU** 和 **VNet**。 保留其餘的預設值。
- 選取與在相同資源群組下建立的虛擬機器相關聯的虛擬網路。
- 選取 [ **建立** ] 以開始部署。

![建立虛擬網路閘道。](images/vpn-gateway.png)

- 使用此 Azure CLI 命令建立虛擬網路閘道：

    ```bash
    az network vnet-gateway create -g MyResourceGroup -n MyVnetGateway --public-ip-address MyGatewayIp --vnet MyVnet --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
    ```

## <a name="generate-certificates"></a>產生憑證

- 開啟 Windows PowerShell ISE 至根和子系憑證。

- 用來產生根憑證的命令：

  ```bash
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
   -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

- 用來產生子憑證的命令：

  ```bash
  New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="export-certificates"></a>匯出憑證

遵循這些步驟可讓您在本機系統上成功安裝憑證。

- 開啟 Microsoft Management Console 以匯出憑證。
- 移至 [ **執行**]。 輸入 **MMC** 以開啟 [憑證]。
- 選取 [**個人**] 資料夾底下的 **憑證** 以開啟頁面。
- 重新整理頁面以尋找 **根和子系憑證**。

憑證類型：

**若要匯出根憑證：**

- 選取根憑證、選取並保存 (或以滑鼠右鍵按一下憑證上的) ，然後移至 [ **所有** 工作]。
- 選取 [ **匯出** 新視窗]，然後選取 **[下一步**]。
- 選取 [ **否，不要匯出私密金鑰** ]，然後按一下 **[下一步]**。
- 選取 [ **Base-64 編碼的 x.509 ( .cer)** 然後按一下 **[下一步]**。
- 選取 [ **流覽] 並選取路徑**、輸入名稱，然後選取 **[下一步]**。
- 將會出現此訊息： **已成功匯出**。

**若要匯出子憑證：**

- 選取用戶端憑證、選取並保存 (或以滑鼠右鍵按一下憑證上的) ，然後移至 [ **所有** 工作]。
- 選取 [ **匯出** 新視窗]，然後選取 **[下一步**]。
- 選取 **[是**， **匯出私密金鑰**]，然後按一下 **[下一步]**。
- 選取 [ **個人資訊交換**]、[ **PKCS**]，然後選取 [ **下一步]**。
- 選取 [密碼] 核取方塊，並提供密碼。
- 將加密設定為 TripleDES-SHA1，然後選取 **[下一步]**。
- 選取 [ **流覽] 並選取路徑**、輸入名稱，然後選取 **[下一步]**。
- 將會出現此訊息： **已成功匯出**。
- 在您選擇的編輯器中開啟根憑證檔案，然後複製程式碼。

## <a name="configure-the-virtual-network-gateway"></a>設定虛擬網路閘道

- 移至建立虛擬網路閘道的資源群組。
- 移至左側面板上的 [點對站-設定]。
- 按一下中央面板中的 [立即設定]。
- 新增位址集區 (例如： 192.168.. 0/24) 。
- 針對 [通道 **類型**]，選取 [ **IKEv2**]。
- 將 [驗證類型] 設定為 [ **Azure 認證**]。
- 在入口網站中貼上複製的根憑證程式碼，將它命名為 **root**，然後選取 [ **儲存**]。

## <a name="download-and-connect-to-the-vpn-client"></a>下載並連接到 VPN 用戶端

- 從入口網站儲存設定之後，請下載 VPN 用戶端。
- 開啟下載的 VPN 用戶端 zip 檔案，開啟 `WindowsAMD64` 資料夾，然後安裝檔案 `VPNClinetsetupAMD64` 。
- 移至 **Control Panel\Network 和網際網路 \ 網路 Connections** 以查看已安裝的 VPN。
- 在 VPN 上按一下滑鼠右鍵，然後選取 **[連線]**。
- 會出現新視窗。 選取 [連線 **]** 按鈕以進行連線。

已建立 VPN 閘道連線。

## <a name="log-in-to-the-virtual-machine"></a>登入虛擬機器

- 透過 SSH 金鑰登入具有私人 IP 的虛擬機器。

- 執行此命令來設定密碼驗證：

  ```bash
  sudo vi /etc/ssh/sshd_config
  ```

- 更新這些參數：將密碼驗證類型從 [ **否** ] 變更為 **[是]**、尋找批註的 UserLogin、移除 **#** 批註，然後變更為 **[是]**。

- 按下 `ESC` 按鍵，然後輸入 `:wq!` 以儲存變更。

- 重新開機 SSHD：

  ```bash
  sudo systemctl restart sshd
  ```

- 密碼長度必須為14個字元。 使用下列命令來設定密碼：

  ```bash
  sudo passwd <username>
  ```

  例如：`sudo passwd azureadmin`

密碼驗證已完成。

## <a name="log-in-to-virtual-machine-instance-from-a-controller-virtual-machine"></a>從控制器虛擬機器登入虛擬機器實例

- 登入您的用戶端虛擬機器。

- 執行下列命令以連線至私用虛擬機器：

  ```bash
  sudo ssh <username>@<private_IP>
  ```

例如：`sudo ssh azureadmin@102.xx.xx.xx`

- 依照提示輸入密碼。

## <a name="next-steps"></a>後續步驟

繼續瞭解 [如何開始手動 Moodle 遷移](./migration-start.md) ，以取得有關 Moodle 遷移程式中下一個步驟的資訊。

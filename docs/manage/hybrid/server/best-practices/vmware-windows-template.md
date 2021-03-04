---
title: 建立適用于 Windows Server 2019 的 VMware vSphere 範本
description: 建立適用于 Windows Server 2019 的 VMware vSphere 範本。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 16a110dd380149aaa72cbb316cbb549eec12ef42
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102114263"
---
# <a name="create-a-vmware-vsphere-template-for-windows-server-2019"></a>建立適用于 Windows Server 2019 的 VMware vSphere 範本

本文提供建立 Windows Server 2019 VMware vSphere 虛擬機器範本的指引。

## <a name="prerequisites"></a>必要條件

> [!NOTE]
> 本指南假設您已熟悉一些 VMware vSphere，且您已瞭解如何安裝 Windows Server。 此外，它也不會檢查 VMware 或 Windows 的最佳作法。

- [下載最新的 Windows Server ISO 檔案](https://www.microsoft.com/windows-server/trial)

- VMware vSphere 6.5 和更新版本

- 雖然可以在本機使用，但建議您將檔案上傳至 vSphere 資料存放區或 vCenter 內容庫，以加快部署速度。

## <a name="creating-windows-server-2019-vm-template"></a>正在建立 Windows Server 2019 VM 範本

### <a name="deploying-and-installing-windows-server"></a>部署和安裝 Windows Server

1. 部署新的虛擬機器。

    ![如何建立新的 VMware vSphere 虛擬機器的螢幕擷取畫面。](./media/vmware-template/windows-template-new-vm-1.png)

    ![如何建立新 VMware vSphere 虛擬機器的第二個螢幕擷取畫面。](./media/vmware-template/windows-template-new-vm-2.png)

    ![如何建立新 VMware vSphere 虛擬機器的第三個螢幕擷取畫面。](./media/vmware-template/windows-template-new-vm-3.png)

    ![如何建立新 VMware vSphere 虛擬機器的第四個螢幕擷取畫面。](./media/vmware-template/windows-template-new-vm-4.png)

    ![如何建立新 VMware vSphere 虛擬機器的第五個螢幕擷取畫面。](./media/vmware-template/windows-template-new-vm-5.png)

    ![第六張螢幕擷取畫面，說明如何建立新的 VMware vSphere 虛擬機器。](./media/vmware-template/windows-template-new-vm-6.png)

2. 請務必選取 **Microsoft Windows Server 2016 或更新版本 (64 位)** 作為「客體作業系統」。

    ![Windows Server 客體作業系統的螢幕擷取畫面。](./media/vmware-template/windows-template-guest-os.png)

3. 指向 Windows Server ISO 檔案位置。

    ![如何建立新的 VMware vSphere 虛擬機器的第七個螢幕擷取畫面。](./media/vmware-template/windows-template-new-vm-7.png)

    ![如何建立新的 VMware vSphere 虛擬機器的第八個螢幕擷取畫面。](./media/vmware-template/windows-template-new-vm-8.png)

- 開啟 VM 電源，然後啟動 Windows Server 安裝。

    ![Windows Server 安裝的第一個螢幕擷取畫面。](./media/vmware-template/windows-template-installation-1.png)

    ![Windows Server 安裝的第二個螢幕擷取畫面。](./media/vmware-template/windows-template-installation-2.png)

    ![Windows Server 安裝的第三個螢幕擷取畫面。](./media/vmware-template/windows-template-installation-3.png)

    ![Windows Server 安裝的第四個螢幕擷取畫面。](./media/vmware-template/windows-template-installation-4.png)

    ![Windows Server 安裝的第五個螢幕擷取畫面。](./media/vmware-template/windows-template-installation-5.png)

    ![Windows Server 安裝的第六個螢幕擷取畫面。](./media/vmware-template/windows-template-installation-6.png)

    ![Windows Server 安裝的第七個螢幕擷取畫面。](./media/vmware-template/windows-template-installation-7.png)

### <a name="post-installation"></a>安裝後

將 VM 轉換成範本之前，需要執行幾個動作。

1. 安裝 VMware 工具並重新啟動。

    ![VMware 工具安裝的第一個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-1.png)

    ![VMware 工具安裝的第二個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-2.png)

    ![VMware 工具安裝的第三個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-3.png)

    ![VMware 工具安裝的第四個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-4.png)

    ![VMware 工具安裝的第五個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-5.png)

    ![VMware 工具安裝的第六個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-6.png)

    ![VMware 工具安裝的第七個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-7.png)

    ![VMware 工具安裝的第八張螢幕擷取畫面。](./media/vmware-template/windows-template-tools-8.png)

    ![VMware 工具安裝的第九個螢幕擷取畫面。](./media/vmware-template/windows-template-tools-9.png)

2. 執行 Windows 更新。

3. 在 PowerShell 中執行命令以將 PowerShell 執行原則變更為， `Bypass` `Set-ExecutionPolicy -ExecutionPolicy Bypass` 稍後可以透過群組原則或 PowerShell 腳本) 來微調 (。

4. 藉由執行 PowerShell 腳本，允許對 OS 進行 WinRM 通訊 [`allow_winrm`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/vmware/winsrv/terraform/scripts/allow_winrm.ps1) 。

5. 下列各項都不是強制性的，但應該針對 Windows 範本考慮：

    - 停用使用者帳戶控制 (稍後可以透過群組原則或 PowerShell 腳本進行調整) 
    - 關閉 Windows Defender 防火牆 (稍後可以透過群組原則或 PowerShell 腳本進行調整) 
    - 停用 Internet Explorer 增強式安全性設定 (ESC)  (稍後可以透過群組原則或 PowerShell 腳本進行微調) 
    - 啟用遠端桌面
    - 在 PowerShell 中，安裝 [Chocolatey](https://chocolatey.org/install)

      ```powershell
      Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
      ```

    - 安裝您可能想要包含在範本中的所有基準應用程式。

### <a name="convert-to-template"></a>轉換成範本

將 VM CPU 計數和記憶體資源降至最低，並將 VM 轉換成範本、將 CD/DVD 光碟機切換至用戶端裝置，並將其中斷連線，並將 VM 轉換成範本。

![如何減少虛擬機器的 CPU 計數和記憶體的螢幕擷取畫面。](./media/vmware-template/windows-template-reduce.png)

![如何將虛擬機器轉換成範本的螢幕擷取畫面。](./media/vmware-template/windows-template-convert.png)

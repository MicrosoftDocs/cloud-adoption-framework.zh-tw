---
title: 建立適用于 Ubuntu server 18.04 的 VMware vSphere 範本
description: 建立適用于 Ubuntu server 18.04 的 VMware vSphere 範本。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 1fac9dcacad7ff5f6230cdfdc9e18910a9792109
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794781"
---
# <a name="create-a-vmware-vsphere-template-for-ubuntu-server-1804"></a>建立適用于 Ubuntu server 18.04 的 VMware vSphere 範本

本文提供建立 Ubuntu Server 18.04 VMware vSphere 虛擬機器範本的指引。

## <a name="prerequisites"></a>必要條件

> [!NOTE]
> 本指南假設您有一些 VMware vSphere 的熟悉度。 它也不是用來複習 VMware 或 Ubuntu 的最佳作法。

- [下載最新的 Ubuntu server 18.04 ISO 檔案](https://releases.ubuntu.com/18.04/)

- VMware vSphere 6.5 和更新版本

- 雖然可以在本機使用，但為了加快部署速度，請將檔案上傳至 vSphere 資料存放區或 vCenter 內容庫。

## <a name="creating-ubuntu-1804-vm-template"></a>正在建立 Ubuntu 18.04 VM 範本

### <a name="deploying-and-installing-ubuntu"></a>部署和安裝 Ubuntu

- 部署新的虛擬機器

    ![如何建立新的 VMware vSphere 虛擬機器的螢幕擷取畫面。](./media/vmware-template/ubuntu-template-new-vm-1.png)

    ![如何建立新 VMware vSphere 虛擬機器的第二個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-new-vm-2.png)

    ![如何建立新 VMware vSphere 虛擬機器的第三個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-new-vm-3.png)

    ![如何建立新 VMware vSphere 虛擬機器的第四個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-new-vm-4.png)

    ![如何建立新 VMware vSphere 虛擬機器的第五個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-new-vm-5.png)

    ![第六張螢幕擷取畫面，說明如何建立新的 VMware vSphere 虛擬機器。](./media/vmware-template/ubuntu-template-new-vm-6.png)

- 請務必選取 **Ubuntu Linux (64-bit)** 作為虛擬作業系統。

    ![Ubuntu Linux (64 位) 來賓 OS 的螢幕擷取畫面。](./media/vmware-template/ubuntu-template-guest-os.png)

- 指向 [Ubuntu server ISO 檔案位置]。

    ![如何建立新的 VMware vSphere 虛擬機器的第七個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-new-vm-7.png)

    ![如何建立新的 VMware vSphere 虛擬機器的第八個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-new-vm-8.png)

- 開啟 VM 電源，並啟動 Ubuntu 安裝。 這裡沒有特定的指示，但：

  -  (選擇性： ) 考慮使用靜態 IP
  - 安裝 OpenSSH 伺服器

    ![Ubuntu 安裝的第一個螢幕擷取畫面](./media/vmware-template/ubuntu-template-installation-1.png)

    ![Ubuntu 安裝的第二個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-2.png)

    ![Ubuntu 安裝的第三個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-3.png)

    ![Ubuntu 安裝的第四個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-4.png)

    ![Ubuntu 安裝的第五個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-5.png)

    ![Ubuntu 安裝的第六個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-6.png)

    ![Ubuntu 安裝的第七個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-7.png)

    ![Ubuntu 安裝的第八張螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-8.png)

    ![Ubuntu 安裝的第九個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-9.png)

    ![Ubuntu 安裝的第十個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-10.png)

    ![Ubuntu 安裝的第十個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-11.png)

    ![Ubuntu 安裝的第十二個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-12.png)

    ![Ubuntu 安裝的第十三個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-13.png)

    ![Ubuntu 安裝的第十四個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-14.png)

    ![Ubuntu 安裝的第十五個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-15.png)

    ![Ubuntu 安裝的第十六個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-16.png)

    ![第十七個 Ubuntu 安裝的螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-17.png)

    ![Ubuntu 安裝的第十八張螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-18.png)

    ![第十九個 Ubuntu 安裝的螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-19.png)

    ![Ubuntu 安裝的第二十個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-20.png)

    ![Ubuntu 安裝的第二十個螢幕擷取畫面。](./media/vmware-template/ubuntu-template-installation-21.png)

### <a name="post-installation"></a>安裝後

將 VM 轉換成範本之前，需要執行幾個動作。

- 最好是讓您的作業系統套件保持在最新狀態

    ```console
    sudo apt-get update
    sudo apt-get upgrade -y
    ```

- 防止 cloudconfig 保留原始的主機名稱，並重設主機名稱

    ```console
    sudo sed -i 's/preserve_hostname: false/preserve_hostname: true/g' /etc/cloud/cloud.cfg
    sudo truncate -s0 /etc/hostname
    sudo hostnamectl set-hostname localhost
    ```

- 移除目前的網路設定

    ```console
    sudo rm /etc/netplan/50-cloud-init.yaml
    ```

- 清除 shell 歷程記錄並關閉 VM

    ```console
    cat /dev/null > ~/.bash_history && history -c
    sudo shutdown now
    ```

### <a name="convert-to-template"></a>轉換成範本

將 VM CPU 計數和記憶體資源降至最低，並將 VM 轉換成範本、將 CD/DVD 光碟機切換至用戶端裝置，並將其中斷連線，並將 VM 轉換成範本。

![如何減少虛擬機器的 CPU 計數和記憶體的螢幕擷取畫面。](./media/vmware-template/ubuntu-template-reduce.png)

![如何將虛擬機器轉換成範本的螢幕擷取畫面。](./media/vmware-template/ubuntu-template-convert.png)

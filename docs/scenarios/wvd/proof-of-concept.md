---
title: Windows 虛擬桌面概念證明
description: 使用適用于 Azure 的雲端採用架構來瞭解 Windows 虛擬桌面的遷移最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: e305e04723fc982419e1cf5625f0ad3c43610a21
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196386"
---
# <a name="windows-virtual-desktop-proof-of-concept"></a>Windows 虛擬桌面概念證明

在 Contoso 雲端採用小組部署其使用者桌面之前，它會藉由完成並測試概念證明，來驗證 Azure 登陸區域和終端使用者網路容量的設定。 

下列遷移程式的方法已簡化，以概述概念證明的執行。

1. **評估**：小組會使用預設的虛擬機器 (VM) 大小來部署主機集區。 評量資料可協助團隊識別預期的並行使用者會話數目，以及支援這些並行會話所需的 Vm 數目。
2. **部署**：小組會使用來自 Azure Marketplace 的 Windows 10 圖庫映射和評估步驟1的大小，建立集區桌面的 [主機集](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace) 區。
3. **部署**：小組會針對已遷移的工作負載 [建立 RemoteApp 應用程式群組](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups#create-a-remoteapp-group) 。
4. **部署**：小組會 [建立 FSLogix 設定檔容器](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile) 來儲存使用者設定檔。
5. **版本**：小組會測試應用程式群組的效能和延遲以及已部署的桌面，以進行使用者的取樣。
6. **發行**：小組將上線其使用者，告訴他們如何透過 [Windows 桌面用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-windows-7-and-10)、 [web 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-web)、 [Android 客戶](https://docs.microsoft.com/azure/virtual-desktop/connect-android)端、 [macOS 客戶](https://docs.microsoft.com/azure/virtual-desktop/connect-macos)端或 [iOS 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-ios)連線。

## <a name="assumptions"></a>假設

概念證明方法可以符合某些生產需求，但它是以數個假設為基礎。

對於 Windows 虛擬桌面的任何企業遷移而言，下列假設都不太可能證明成立。 採用小組應該假設生產部署需要個別的部署，以更密切地配合在 Windows 虛擬桌面評估期間所識別的生產需求。 假設如下：

* 終端使用者與 Azure 中指派的登陸區域具有低延遲的連線。
* 所有使用者都可以從共用的桌面集區工作。
* 所有使用者都可以使用 Azure Marketplace 的 Windows &nbsp; 10 企業版多會話映射。
* 所有使用者設定檔都會遷移至 FSLogix 設定檔容器的 [Azure 檔案儲存體]、[Azure NetApp Files] 或 [以 VM 為基礎的儲存體服務]。
* 所有使用者都可以由一個通用角色來描述，每個虛擬中央處理 (單元的 vCPU) 和 4 &nbsp; gb (gb) 的 RAM，每個[as per the VM sizing recommendations](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/virtual-machine-recs#multi-session-recommendations)使用者都有一高密度的每一種。
* 所有工作負載都與 Windows &nbsp; 10 多會話相容。
* 可以接受虛擬桌面和應用程式群組之間的延遲，以供生產環境使用。

為了根據概念證明設定參考來計算 Windows 虛擬桌面案例的成本，小組會使用 [美國東部](https://azure.com/e/448606254c9a44f88798892bb8e0ef3c)、 [西歐](https://azure.com/e/61a376d5f5a641e8ac31d1884ade9e55)或 [東南亞](https://azure.com/e/7cf555068922461587d0aa99a476f926)的定價計算機。 
> [!NOTE]
> 這些範例全都會使用 Azure 檔案儲存體做為使用者設定檔的儲存體服務。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [評估 Windows 虛擬桌面移轉或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

---
title: Windows 虛擬桌面概念證明
description: 使用「雲端採用架構」來瞭解完成和測試 Windows 虛擬桌面概念證明的最佳作法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/17/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: d43eafcc0ce5a0d85488c38e31b79c48d74de613
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94996990"
---
<!-- cSpell:ignore FSLogix onboards remoteapp macos -->

# <a name="windows-virtual-desktop-proof-of-concept"></a>Windows 虛擬桌面概念證明

在 Contoso 雲端採用小組部署其使用者桌上型電腦之前，它會藉由完成並測試概念證明，來驗證 Azure 登陸區域和終端使用者網路容量的設定。

下列遷移程式的方法已簡化，以概述概念證明的執行。

1. **評定**：小組會使用預設虛擬機器 (VM) 大小來部署主機集區。 評量資料可協助小組識別預期的並行使用者會話數目，以及支援這些並行會話所需的 Vm 數目。
2. **部署**：小組會使用來自 Azure Marketplace 的 Windows 10 資源庫影像以及評量步驟1的大小調整，為集區桌面 [建立主機集](/azure/virtual-desktop/create-host-pools-azure-marketplace) 區。
3. **部署**：小組會針對已遷移的工作負載 [建立 RemoteApp 應用程式群組](/azure/virtual-desktop/manage-app-groups#create-a-remoteapp-group) 。
4. **部署**：小組會 [建立 FSLogix 設定檔容器](/azure/virtual-desktop/create-host-pools-user-profile) 來儲存使用者設定檔。
5. **版本**：小組會測試應用程式群組的效能和延遲，以及為使用者取樣所部署的桌面。
6. **版本**：小組將上線其使用者，讓他們瞭解如何透過 [Windows 桌面用戶端](/azure/virtual-desktop/connect-windows-7-and-10)、 [web 客戶](/azure/virtual-desktop/connect-web)端、 [Android 客戶](/azure/virtual-desktop/connect-android)端、 [macOS 客戶](/azure/virtual-desktop/connect-macos)端或 [iOS 用戶端](/azure/virtual-desktop/connect-ios)進行連線。

## <a name="assumptions"></a>假設

概念證明方法可符合某些生產需求，但它是以許多假設為基礎。

在 Windows 虛擬桌面的任何企業遷移中，下列所有假設都不太可能證明成立。 採用小組應該假設生產部署需要個別的部署，以更密切地配合在 Windows 虛擬桌面評估期間所識別的生產環境需求。 假設如下：

- 終端使用者對 Azure 中指派的登陸區域具有低延遲的連線。
- 所有使用者都可以從桌面的共用集區工作。
- 所有使用者都可以使用 Azure Marketplace 的 Windows &nbsp; 10 企業版多會話映射。
- 所有的使用者設定檔都會遷移至 FSLogix 設定檔容器的 Azure 檔案儲存體、Azure NetApp Files 或以 VM 為基礎的儲存體服務。
- 所有使用者都可以透過每個虛擬中央處理器的每個虛擬中央處理單位（每個虛擬中央處理器 (vCPU) 和 4 &nbsp; (gb) RAM）來描述，並 [根據 VM 大小調整建議](/windows-server/remote/remote-desktop-services/virtual-machine-recs#multi-session-recommendations)。
- 所有工作負載都與 Windows &nbsp; 10 企業版多重會話相容。
- 虛擬桌面和應用程式群組之間的延遲可供生產環境使用。

為了根據概念證明設定參考來計算 Windows 虛擬桌面案例的成本，小組會使用適用于 [美國東部](https://azure.com/e/448606254c9a44f88798892bb8e0ef3c)、 [西歐](https://azure.com/e/61a376d5f5a641e8ac31d1884ade9e55)或 [東南亞](https://azure.com/e/7cf555068922461587d0aa99a476f926)的定價計算機。
> [!NOTE]
> 這些範例全都使用 Azure 檔案儲存體作為使用者設定檔的儲存體服務。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [評估 Windows 虛擬桌面移轉或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

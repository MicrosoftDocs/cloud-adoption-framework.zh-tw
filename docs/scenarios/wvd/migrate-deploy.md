---
title: 將 Windows 虛擬桌面部署至 Azure
description: 使用適用于 Azure 的雲端採用架構，利用最佳作法來部署 Windows 虛擬桌面，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/17/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 8d0b754ed37665b01fb3b9852b1af027c9c37beb
ms.sourcegitcommit: a0ddde4afcc7d8c21559e79d406dc439ee4f38d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97713788"
---
<!-- cSpell:ignore NTFS Logix -->

# <a name="windows-virtual-desktop-deployment-or-migration"></a>Windows 虛擬桌面部署或遷移

本文中的指導方針假設您已 [建立 Windows 虛擬桌面) 的規劃 ](./plan.md)、 [評估桌上型電腦部署需求](./migrate-assess.md)、 [完成概念證明](./proof-of-concept.md)，現在已準備好遷移或部署您的 windows 虛擬桌面實例。

## <a name="initial-scope"></a>初始範圍

Windows 虛擬桌面實例的部署遵循類似于 [概念證明](./proof-of-concept.md) 程式的流程。 使用此初始範圍作為基準，以說明評定輸出所需的各種範圍變更。

- 使用 Azure Marketplace 的 Windows 10 資源庫映射和該程式的步驟1調整大小，建立集區桌面的[主機集](/azure/virtual-desktop/create-host-pools-azure-marketplace)區 &nbsp; 。
- 針對已遷移的工作負載[建立 RemoteApp 應用程式群組](/azure/virtual-desktop/manage-app-groups#create-a-remoteapp-group)。
- [建立 FSLogix 設定檔容器](/azure/virtual-desktop/create-host-pools-user-profile) 以儲存使用者設定檔。

部署和遷移包含了角色遷移、應用程式遷移和使用者設定檔的遷移。 視工作負載評量的結果而定，每個遷移工作可能會有變更。 本文可協助您根據評量意見反應來識別範圍變更的方式。

## <a name="iterative-methodology"></a>反復的方法

每個角色可能都需要先前所述初始範圍的反復專案，而導致多個主機集區。 根據 Windows 虛擬桌面評量，採用小組應該根據每個角色的角色或使用者數目來定義反覆運算。 將程式細分為角色導向的反復專案有助於降低對企業的變更速度，並讓小組專注于每個角色集區的適當測試或上線。

## <a name="scope-considerations"></a>範圍考慮

每個要遷移或部署的角色群組的設計檔中都應包含下列每一組考慮。 在先前討論過的 [初始範圍](#initial-scope)中，考慮範圍考慮之後，就可以開始部署或遷移。

### <a name="azure-landing-zone-considerations"></a>Azure 登陸區域考量

部署角色群組之前，應該在 Azure 區域中建立一個登陸區域，以支援要部署的每個角色。 每個指派的登陸區域都應根據 [登陸區域審核需求](./ready.md)進行評估。

如果指派的 Azure 登陸區域不符合您的需求，則應該新增範圍來進行對環境的任何修改。

### <a name="application-and-desktop-considerations"></a>應用程式與桌面的考慮

某些角色可能相依于舊版解決方案，與 Windows &nbsp; 10 企業版多會話不相容。 在這些情況下，某些角色可能需要專用的桌面。 在部署和測試之前，可能不會探索到此相依性。

如果在程式中延遲發現，未來的反復專案應配置給繼承應用程式的現代化或遷移。 這會降低桌面體驗的長期成本。 這些未來的反復專案應該根據現代化的整體定價影響以及與專用桌面相關聯的額外成本，來優先處理和完成。 為了避免管線中斷和企業成果的實現，這種優先順序應該不會影響目前的反覆運算。

有些應用程式可能需要補救、現代化或遷移至 Azure，以支援所需的終端使用者體驗。 這些變更可能會在發行後推出。 或者，當桌面延遲可能會影響商務功能時，應用程式的變更可能會為某些角色的遷移建立封鎖的相依性。

### <a name="user-profile-considerations"></a>使用者設定檔考慮

[初始範圍](#initial-scope)假設您使用的是以[VM 為基礎的 FSLogix 使用者設定檔容器](/azure/virtual-desktop/create-host-pools-user-profile)。

您可以使用 [Azure NetApp Files 來裝載使用者配置](/azure/virtual-desktop/create-fslogix-profile-container)檔。 這麼做將需要範圍中的幾個額外步驟，包括：

- **每一 NetApp 實例：** 設定 NetApp files、磁片區和 Active Directory 連接。
- **每一主機/角色：** 設定工作階段主機虛擬機器上的 FSLogix。
- **每位使用者：** 將使用者指派給主機會話。

您也可以使用 [Azure 檔案儲存體來裝載使用者設定檔](/azure/virtual-desktop/create-file-share)。 這麼做將需要範圍中的幾個額外步驟，包括：

- **每 Azure 檔案儲存體實例：** 另外也支援設定儲存體帳戶、磁片類型，以及 Active Directory 連線 ([Active Directory Domain Services (AD DS](/azure/virtual-desktop/create-profile-container-adds)) 、指派 Active Directory 使用者群組的 Azure 角色型存取控制存取、套用新的技術檔案系統許可權，以及取得儲存體帳戶存取金鑰。
- **每一主機/角色：** 設定工作階段主機虛擬機器上的 FSLogix。
- **每位使用者：** 將使用者指派給主機會話。

某些角色或使用者的使用者設定檔可能也需要資料移轉工作，這可能會延遲遷移特定的角色，直到可以在本機 Active Directory 或個別使用者桌上型電腦內補救使用者設定檔為止。 這項延遲可能會大幅影響 Windows 虛擬桌面案例以外的範圍。 補救之後，就可以繼續初始範圍和先前的方法。

## <a name="deploy-or-migrate-windows-virtual-desktop"></a>部署或遷移 Windows 虛擬桌面

在 Windows 虛擬桌面遷移或部署的生產環境範圍中，將所有考慮納入考慮之後，就可以開始處理。 採用反復的步調，採用小組現在會部署主機集區、應用程式和使用者設定檔。 完成此階段之後，就可以開始 [測試和上線使用者](./migrate-release.md) 的部署後工作。

## <a name="next-steps"></a>後續步驟

[將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

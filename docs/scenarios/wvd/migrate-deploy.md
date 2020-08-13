---
title: 將 Windows 虛擬桌面部署至 Azure
description: 使用適用于 Azure 的雲端採用架構來瞭解 Windows 虛擬桌面的遷移最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: b79adeedd43d53a9c950fbb8707e34ba6bc2973a
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196449"
---
<!-- cSpell:ignore NTFS Logix -->

# <a name="windows-virtual-desktop-deployment-or-migration"></a>Windows 虛擬桌面部署或遷移

本文中的指導方針假設您已 [建立 Windows 虛擬桌面) 的計畫 ](./plan.md)、 [評估桌面部署需求](./migrate-assess.md)、 [完成概念證明](./proof-of-concept.md)，而且現在已準備好遷移或部署您的 Windows 虛擬桌面實例。

## <a name="initial-scope"></a>初始範圍

Windows 虛擬桌面實例的部署會遵循與 [概念證明](./proof-of-concept.md) 程式類似的進程。 使用此初始範圍做為基準，以說明評量輸出所需的各種範圍變更。

- 使用 Azure Marketplace 的 Windows 10 元件庫映射，以及該程式的步驟1大小，為集區桌面[建立主機集](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace)區 &nbsp; 。
- 為已遷移的工作負載[建立 RemoteApp 應用程式群組](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups#create-a-remoteapp-group)。
- [建立 FSLogix 設定檔容器](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile) 來儲存使用者設定檔。

部署和遷移包含了角色遷移、應用程式遷移，以及使用者設定檔遷移。 視工作負載評估的結果而定，每個這些遷移工作可能會有所變更。 本文可協助您根據評量的意見反應，來識別範圍會變更的方式。

## <a name="iterative-methodology"></a>反復方法

每個角色都可能需要先前所述初始範圍的反復專案，而導致多個主機集區。 根據 Windows 虛擬桌面評估，採用小組應該定義以每個角色的角色或使用者數目為依據的反復專案。 將程式分解成以角色為導向的反復專案有助於降低對企業的變更速度影響，並可讓小組專注于每個角色集區的適當測試或上架。

## <a name="scope-considerations"></a>範圍考慮

下列每一組考慮都應該包含在每個要遷移或部署之角色群組的設計檔中。 將範圍考慮納入先前討論的 [初始範圍](#initial-scope)之後，就可以開始部署或遷移。

### <a name="azure-landing-zone-considerations"></a>Azure 登陸區域考量

在您部署角色群組之前，應該在 Azure 區域中建立登陸區域，以支援要部署的每個角色。 應根據 [登陸區域審查需求](./ready.md)來評估每個指派的登陸區域。

如果指派的 Azure 登陸區域不符合您的需求，則應新增範圍來進行對環境所做的任何修改。

### <a name="application-and-desktop-considerations"></a>應用程式和桌上型電腦的考慮

某些角色可能相依于舊版解決方案，而這與 Windows &nbsp; 10 多會話不相容。 在這些情況下，某些角色可能需要專用的桌上型電腦。 必須等到部署和測試之後，才會探索此相依性。 

如果在程式中的延遲後發現，未來的反復專案應該配置給繼承應用程式的現代化或遷移。 這會降低桌面體驗的長期成本。 這些未來的反復專案應該根據現代化的整體定價影響，以及與專用桌面相關聯的額外成本，來設定優先順序和完成。 為了避免管線中斷並實現商業結果，此優先順序不會影響目前的反復專案。

某些應用程式可能需要補救、現代化或遷移至 Azure，以支援所需的使用者體驗。 這些變更可能會在發行後出現。 或者，當桌面延遲可能會影響商務功能時，應用程式變更可能會針對某些角色的遷移建立封鎖相依性。

### <a name="user-profile-considerations"></a>使用者設定檔考慮

[初始範圍](#initial-scope)會假設您使用的是以[VM 為基礎的 FSLogix 使用者設定檔容器](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile)。

您可以使用 [Azure NetApp Files 來裝載使用者配置](https://docs.microsoft.com/azure/virtual-desktop/create-fslogix-profile-container)檔。 這麼做將需要範圍中的幾個額外步驟，包括：

- **每個 netapp 實例**：設定 netapp files、磁片區和 Active Directory 連線。
- **每個主機/角色**：在工作階段主機虛擬機器上設定 FSLogix。
- **每位使用者**：將使用者指派給主機會話。

您也可以使用 [Azure 檔案儲存體來裝載使用者設定檔](https://docs.microsoft.com/azure/virtual-desktop/create-file-share)。 這麼做將需要範圍中的幾個額外步驟，包括：

- **根據 Azure 檔案儲存體實例**：設定儲存體帳戶、磁片類型和 Active Directory 連線 ([Active Directory Domain Services (AD DS) 也支援](https://docs.microsoft.com/azure/virtual-desktop/create-profile-container-adds)、指派 Active Directory 使用者群組的角色型存取控制存取、套用新的技術檔案系統許可權，以及取得儲存體帳戶存取金鑰。
- **每個主機/角色**：在工作階段主機虛擬機器上設定 FSLogix。
- **每位使用者**：將使用者指派給主機會話。

某些角色或使用者的使用者設定檔可能也需要資料移轉工作，這可能會延遲特定角色的遷移，直到使用者設定檔可以在本機 Active Directory 或個別使用者桌上型電腦內進行補救為止。 這項延遲可能會大幅影響 Windows 虛擬桌面案例外的範圍。 在補救之後，可以繼續初始範圍和上述方法。

## <a name="deploy-or-migrate-windows-virtual-desktop"></a>部署或遷移 Windows 虛擬桌面

將所有考慮納入 Windows 虛擬桌面遷移或部署的生產範圍之後，就可以開始處理常式。 在反復的步調中，採用小組現在會部署主機集區、應用程式和使用者設定檔。 完成此階段之後， [測試和上線使用者](./migrate-release.md) 的部署後工作就可以開始。

## <a name="next-steps"></a>後續步驟

[將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

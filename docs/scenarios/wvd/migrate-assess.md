---
title: 評估 Azure 的 Windows 虛擬桌面
description: 使用適用于 Azure 的雲端採用架構來瞭解 Windows 虛擬桌面的遷移最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 47bcc7420cdcea5ace4397737e420434da9fd27f
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451811"
---
# <a name="windows-virtual-desktop-assessment"></a>Windows 虛擬桌面評量

[Windows 虛擬桌面概念證明](./proof-of-concept.md)會提供初始範圍做為基準實作為基礎。 該概念證明的輸出不太可能符合生產需求。 評估程式可透過資料驅動的程式，做為測試假設的主要方法。 評量資料可協助回答一系列的重要問題、驗證/使假設失效，並在必要時調整範圍，以支援您的 Windows 虛擬桌面案例。 使用這個假設驗證方法，可以加速將使用者桌面遷移或部署至 WVD。

## <a name="assess-windows-virtual-desktop-deployments"></a>評估 Windows 虛擬桌面部署

每個 Windows 虛擬桌面評估都會評估使用者角色、一致的主機集區（Vm）、使用者應用程式和資料，以及使用者設定檔（資料）的組合。 在評估期間，目標是使用資料來回答本節中的問題。 這些答案會塑造部署和發行 WVD 遷移的實際範圍。

這些問題的答案是從資料開始。 在計畫方法中，特別是[最佳作法](../../plan/index.md)和[數位資產評](../../digital-estate/index.md)量，應已收集並分析資料，以建立您的遷移計畫。 不過，此特定工作負載評估中的問題可能會需要額外的資料。 具體而言，需要每位使用者所使用之桌面、使用者和工作負載的相關資料，才能開發 WVD 部署計畫。

如果您使用[Movere](https://docs.microsoft.com/azure/migrate/migrate-services-overview#movere)做為資料收集工具，您可能會有開發角色所需的資料，並使用[Azure Migrate](https://docs.microsoft.com/azure/migrate)中的資料來回答這些問題，就像其他任何遷移案例一樣。

如果您沒有必要的資料來回答本節中的所有問題，則會有額外的協力廠商軟體廠商，可以提供個別的探索程式來增加您擁有的資料。 [Lakeside](https://docs.microsoft.com/azure/migrate/migrate-services-overview#isv-integration)也會與「VDI」遷移目標一節中的 Azure Migrate 整合，並對應 WVD 部署的計畫，包括角色、主機集區、應用程式和使用者設定檔。

### <a name="user-personas"></a>使用者角色

有多少不同的角色需要支援此遷移案例中包含的所有使用者？ 根據下列各項，定義角色將會成為值區使用者的結果：

- **個人**集區：特定的使用者群組需要專用的桌面，而不是集區嗎？ 例如，安全性、合規性、高效能或雜訊方面的需求，可能會導致某些在不屬於共用策略的專用桌上型電腦上執行的使用者。 這會藉由[在 WVD 主機集區部署期間指定 [個人] 主機](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace#begin-the-host-pool-setup-process)集區類型來設定。
- **密度：** 特定的使用者群組需要較低密度的桌面體驗嗎？ 例如 繁重的使用者需要每個 vCPU 2 個使用者，而不是每個 vCPU 有6位使用者的淺使用者假設。 這會在[WVD 主機集區部署的集區設定](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace#begin-the-host-pool-setup-process)中輸入。
- **效能：** 特定的使用者群組需要更高的效能桌面體驗嗎？ 例如 有些使用者所需的記憶體比每個 vCPU 所假設的 4 GB RAM 多出 vCPU。 VM 大小會在[WVD 主機集區部署的虛擬機器詳細資料](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace#virtual-machine-details)中輸入。
- **圖形化處理（GPU）：** 特定使用者群組有較高的圖形化需求嗎？ 例如，某些使用者需要在 Azure 中以 GPU 為基礎的 Vm，在此指南中會示範如何設定[Gpu vm](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu)。
- **Azure 區域：** 特定群組 OS 使用者是從不同的地理區域操作嗎？ 例如，在設定主機集區之前，來自每個區域的使用者應該使用[估計工具](https://azure.microsoft.com/services/virtual-desktop/assessment/#estimation-tool)來測試 Azure 的延遲。 測試使用者應共用最小的延遲 Azure 區域，以及前三個 Azure 區域的延遲（以毫秒為單位）。
- **商務功能：** 特定的使用者群組可由商業單位、程式碼或其商務功能貯存嗎？ 這種類型的群組將有助於在後續的作業階段中調整公司成本。
- **使用者計數：** 每個不同的角色會有多少個使用者？
- **會話計數上限：** 根據作業的地理位置和時數，每個角色的最大負載需要多少並行使用者？

上述每個問題的區別將從**商業功能、成本中心**、地理區域和技術需求開始說明使用者的角色。 下表可協助記錄回應以填入已完成的評估或設計檔。

|  | 角色群組1  | 角色群組2  | 角色群組3  |
|---------|---------|---------|---------|
| 集區  | 集區 | 集區 | 專用（安全性考慮） |
| 密度 | 淺（6個使用者/vCPU） | 繁重（2個使用者/vCPU） | 專用（1個使用者/vCPU） |
| 效能 | 低 | 高記憶體 | 低 |
| GPU | N/A | 必要 | 不適用 |
| Azure 區域 | 北美洲 | 西歐 | 北美洲 |
| 使用者計數 | 1000 | 50 | 20 |
| 會話計數 | 200 | 50 | 10 |

每個角色（或每個具有不同商務功能的使用者群組）也有不同的技術需求，都需要特定的主機集區設定。

終端使用者評估會提供所需的資料：集區類型、密度、大小、CPU/GPU、登陸區域等。

主機集區設定評估現在會將該資料對應至部署計畫。 調整技術需求、業務需求和成本有助於判斷主機集區的適當數目和設定。

如需價格的範例，請參閱[美國東部](https://azure.com/e/448606254c9a44f88798892bb8e0ef3c)、[西歐](https://azure.com/e/61a376d5f5a641e8ac31d1884ade9e55)或[東南亞](https://azure.com/e/7cf555068922461587d0aa99a476f926)區域。

### <a name="application-groups"></a>應用程式群組

目前內部部署環境的 Movere 和 lakeside 掃描都會提供有關在使用者桌上型電腦上執行之應用程式的資料。 使用該資料，建立每個角色所需的所有應用程式清單。 針對每個必要的應用程式，下列問題的答案將會塑造部署反復專案。

- 是否需要為角色安裝任何應用程式才能使用此桌面？ 除非此角色使用100% 的 web 型軟體即服務應用程式，否則您可能需要為每個角色[設定自訂主要 VHD 映射](https://docs.microsoft.com/azure/virtual-desktop/set-up-customize-master-image)，並將必要的應用程式安裝在主要映射上。
- 此角色需要 Office 365 應用程式嗎？ 若是如此，您必須將[Office 365 新增至自訂的主要 VHD 映射](https://docs.microsoft.com/azure/virtual-desktop/install-office-on-wvd-master-image)。
- 此應用程式是否與 Windows 10 多會話相容？ 如果應用程式不相容，可能需要[個人集](https://docs.microsoft.com/azure/virtual-desktop/configure-host-pool-personal-desktop-assignment-type)區來執行自訂 VHD 映射。 對於 Windows 虛擬桌面的應用程式相容性問題，[桌面應用程式會確保](https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered)服務也會提供協助。
- 任務關鍵性應用程式是否可能會受到虛擬桌面電腦與任何後端系統之間的延遲影響？ 若是如此，您可能會想要考慮將支援應用程式的後端系統移轉至 Azure。

這些問題的答案可能會要求計畫在桌上型電腦或部署之前，先包含對桌面映射的補救或支援應用程式元件。

## <a name="next-step-deploy-or-migrate-windows-virtual-desktop-instances"></a>下一步：部署或遷移 Windows 虛擬桌面實例

下列文章清單會帶您前往在雲端採用旅程圖的特定點上找到的指引。

- [部署或遷移 Windows 虛擬桌面實例](./migrate-deploy.md)
- [將您的 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

---
title: 評估 Azure 的 Windows 虛擬桌面
description: 使用適用于 Azure 的雲端採用架構來瞭解 Windows 虛擬桌面的遷移最佳作法，以協助降低複雜性並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 095e7fb461b397e0ba43961ee8ea7853d3fb60d3
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196424"
---
# <a name="windows-virtual-desktop-assessment"></a>Windows 虛擬桌面評量

Windows 虛擬桌面 [概念證明](./proof-of-concept.md) 會提供初始範圍，做為 Contoso 雲端採用小組的基準實行。 但是該概念證明的輸出不太可能符合其生產需求。 

Windows 虛擬桌面評量練習可透過資料驅動的程式，做為測試假設的焦點方法。 評量資料可協助小組回答一系列的重要問題、驗證或使其假設失效，並視需要調整範圍，以支援小組的 Windows 虛擬桌面案例。 藉由使用這個假設驗證方法，小組可以加速遷移或部署其使用者桌面，以 Windows 虛擬桌面。

## <a name="assess-windows-virtual-desktop-deployments"></a>評估 Windows 虛擬桌面部署

每個 Windows 虛擬桌面評估都會評估使用者角色的組合、虛擬機器的一致主機集區 (Vm) 、使用者應用程式和資料，以及 (資料) 的使用者設定檔。 在評估期間，小組的目標是要使用資料來回答本節中的問題。 答案會塑造部署和發行 Windows 虛擬桌面遷移的實際範圍。

這些問題的答案是從資料開始。 在計畫方法中，特別是 [最佳作法](../../plan/index.md) 和 [數位資產評](../../digital-estate/index.md)量，應已收集並分析資料，以建立遷移計畫。 不過，此特定工作負載評估中的問題可能會需要額外的資料。 若要開發 Windows 虛擬桌面部署計畫，必須提供每位使用者所要使用之桌面、使用者和工作負載的相關資料。

如果您使用 [Movere](https://docs.microsoft.com/azure/migrate/migrate-services-overview#movere) 做為資料收集工具，您可能會有開發人員所需的資料，並使用 [Azure Migrate](https://docs.microsoft.com/azure/migrate)中的資料來回答這些問題，就像其他任何遷移案例一樣。

如果您沒有必要的資料來回答本節中的所有問題，則其他協力廠商軟體廠商可以提供個別的探索程式來增加您擁有的資料。 廠商 [Lakeside](https://docs.microsoft.com/azure/migrate/migrate-services-overview#isv-integration)也會與虛擬桌面基礎結構遷移目標一節中的 Azure Migrate 整合。 廠商可以協助您對應 Windows 虛擬桌面部署的計畫，包括角色、主機集區、應用程式和使用者設定檔。

### <a name="user-personas"></a>使用者角色

有多少不同的角色需要支援此遷移案例中包含的所有使用者？ 根據下列準則，定義角色將會成為值區使用者的結果：

- **個人**集區：特定的使用者群組需要專用的桌面，而不是集區嗎？ 例如，安全性、合規性、高效能或雜訊方面的需求，可能會導致某些在不屬於共用策略的專用桌上型電腦上執行的使用者。 您將在 [Windows 虛擬桌面主機集區部署期間指定 [個人] 的主機集區類型](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace#begin-the-host-pool-setup-process)，以輸入這項資訊。
- **密度**：特定使用者群組需要較低密度的桌面體驗嗎？ 例如，較粗的密度可能會要求每個虛擬中央處理單位有兩個使用者 (vCPU) ，而不是每個 vCPU 的六位使用者的輕量使用者假設。 您會在 [Windows 虛擬桌面主機集區部署的集區設定](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace#begin-the-host-pool-setup-process)中輸入密度資訊。
- **效能**：特定的使用者群組需要更高效能的桌面體驗嗎？ 例如，某些使用者在每個 vCPU 中所需的記憶體比每個 vCPU 所假設的 4 gb &nbsp; (GB) 。 您會在 [Windows 虛擬桌面主機集區部署的虛擬機器詳細資料](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace#virtual-machine-details)中，輸入 VM 大小。
- **圖形處理 (GPU) **：特定使用者群組是否有更大的圖形化需求？ 例如，某些使用者需要在 Azure 中以 GPU 為基礎的 Vm，如設定 [Gpu vm 的本指南](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu)所示。
- **Azure 區域**：特定群組的 OS 使用者會從不同的地理區域操作嗎？ 例如，在設定主機集區之前，每個區域的使用者應該使用 [估計工具](https://azure.microsoft.com/services/virtual-desktop/assessment/#estimation-tool)來測試 Azure 的延遲。 測試使用者應該在前三個 Azure 區域中共用最低延遲的 Azure 區域和延遲（以毫秒為單位）。
- **商務功能**：特定群組的使用者可依業務單位、收費碼或其商務功能貯存嗎？ 這種類型的群組將有助於在後續的作業階段中調整公司成本。
- **使用者計數**：每個不同的角色會有多少個使用者？
- **最大會話計數**：根據作業的地理位置和時數，每個角色的最大負載需要多少並行使用者？

上述每個問題的區別將從商業功能、成本中心、地理區域和技術需求開始說明使用者的角色。 下表可協助記錄回應以填入已完成的評估或設計檔：

| 條件  | 角色群組 &nbsp; 1  | 角色群組 &nbsp; 2  | 角色群組 &nbsp; 3  |
|---------|---------|---------|---------|
| 集區  | 集區 | 集區 | 專用 (安全性考慮)  |
| 密度 | Light (6 &nbsp; 使用者/vCPU)  | 繁重的 (2 &nbsp; 使用者/vCPU)  | 專用 (1 &nbsp; 使用者/vCPU)  |
| 效能 | 低 | 高記憶體 | 低 |
| GPU | N/A | 必要 | 不適用 |
| Azure 區域 | 北美洲 | 西歐 | 北美洲 |
| 使用者計數 | 1000 | 50 | 20 |
| 會話計數 | 200 | 50 | 10 |

每個角色或每個具有不同商務功能與技術需求的使用者群組，都需要特定的主機集區設定。

使用者評估會提供所需的資料：集區類型、密度、大小、CPU/GPU、登陸區域區域等等。

主機集區設定評估現在會將該資料對應至部署計畫。 調整技術需求、業務需求和成本有助於判斷主機集區的適當數目和設定。

如需價格的範例，請參閱 [美國東部](https://azure.com/e/448606254c9a44f88798892bb8e0ef3c)、 [西歐](https://azure.com/e/61a376d5f5a641e8ac31d1884ade9e55)或 [東南亞](https://azure.com/e/7cf555068922461587d0aa99a476f926) 區域。

### <a name="application-groups"></a>應用程式群組

目前內部部署環境的 Movere 和 Lakeside 掃描都可以提供在使用者桌上型電腦上執行之應用程式的相關資料。 藉由使用該資料，您可以建立每個角色所需的所有應用程式清單。 針對每個必要的應用程式，下列問題的答案將會塑造部署反復專案：

- 是否需要為角色安裝任何應用程式才能使用此桌面？ 除非此角色使用100% 的 web 型軟體即服務應用程式，否則您可能需要為每個角色 [設定自訂主要 VHD 映射](https://docs.microsoft.com/azure/virtual-desktop/set-up-customize-master-image) ，並將必要的應用程式安裝在主要映射上。
- 此角色需要 Office 365 應用程式嗎？ 若是如此，您必須 [將 Office 365 新增至自訂的主要 VHD 映射](https://docs.microsoft.com/azure/virtual-desktop/install-office-on-wvd-master-image)。
- 此應用程式是否與 Windows &nbsp; 10 多會話相容？ 如果應用程式不相容，可能需要 [個人集](https://docs.microsoft.com/azure/virtual-desktop/configure-host-pool-personal-desktop-assignment-type) 區來執行自訂 VHD 映射。 如需應用程式和 Windows 虛擬桌面相容性問題的協助，請參閱 [桌面應用程式確保](https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered) 服務。
- 任務關鍵性應用程式是否可能會受到 Windows 虛擬桌面實例與任何後端系統之間的延遲影響？ 若是如此，您可能會想要考慮將支援應用程式的後端系統移轉至 Azure。

這些問題的答案可能會要求計畫在桌上型電腦或部署之前，先包含對桌面映射的補救或支援應用程式元件。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

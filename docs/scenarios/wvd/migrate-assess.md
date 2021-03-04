---
title: 評定適用于 Azure 的 Windows 虛擬桌面
description: 使用適用于 Azure 的雲端採用架構，利用可加速遷移或部署程式的最佳作法，來評估您的 Windows 虛擬桌面遷移案例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/17/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: cb9e7c1b58643a94416c4026a1256d21becb56e7
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101787921"
---
# <a name="windows-virtual-desktop-assessment"></a>Windows 虛擬桌面評估

Windows 虛擬桌面 [概念證明](./proof-of-concept.md) 可提供初始範圍作為 Contoso 雲端採用小組的基準實行。 但是該概念證明的輸出不太可能符合其生產需求。

Windows 虛擬桌面評估練習是透過資料驅動程式，作為測試假設的焦點方法。 評量資料可協助小組回答一系列的重要問題、驗證或使其假設無效，並視需要調整範圍，以支援小組的 Windows 虛擬桌面案例。 藉由使用這個假設驗證方法，小組可以加速將其使用者桌面遷移或部署至 Windows 虛擬桌面。

## <a name="assess-windows-virtual-desktop-deployments"></a>評估 Windows 虛擬桌面部署

每個 Windows 虛擬桌面評估都會評估使用者角色的組合、虛擬機器的一致主機集區 (Vm) 、終端使用者應用程式和資料，以及 (資料) 的使用者設定檔。 在評估期間，小組的目標是使用資料來回答本節中的問題。 答案將塑造 Windows 虛擬桌面遷移的實際部署和發行的範圍。

這些問題的答案是從資料開始。 在計畫方法中（特別是 [最佳作法](../../plan/index.md) 和 [數位資產評](../../digital-estate/index.md)量），應已收集並分析資料，以建立遷移計畫。 不過，此特定工作負載評量中的問題可能需要額外的資料。 開發 Windows 虛擬桌面部署計畫時，需要每位使用者所要使用之桌上型電腦、使用者和工作負載的相關資料。

如果您使用 [Movere](/azure/migrate/migrate-services-overview#movere) 做為資料收集工具，您可能會擁有開發角色所需的資料，並使用 [Azure 遷移](/azure/migrate)中的資料來回答這些問題，就像任何其他遷移案例一樣。

如果您沒有回答本節中所有問題所需的資料，另一個協力廠商軟體廠商可以提供個別的探索程式，以增強您所擁有的資料。 廠商（ [lakeside](/azure/migrate/migrate-services-overview#isv-integration)）也會與「虛擬桌面基礎結構遷移目標」一節中的「Azure 遷移」進行整合。 廠商可協助您對應 Windows 虛擬桌面部署的計畫，包括角色、主機集區、應用程式和使用者設定檔。

### <a name="user-personas"></a>使用者角色

需要多少相異的角色才能支援此遷移案例中包含的所有使用者？ 根據下列準則，定義角色會成為值區使用者的結果：

- **個人** 集區：特定的使用者群組是否需要專用的桌面，而不是集區？ 例如，安全性、合規性、高效能或雜訊非鄰近性需求可能會導致某些在不屬於共用策略的專用桌上型電腦上執行的使用者。 您將在 [Windows 虛擬桌面主機集區部署期間指定個人的主機集區類型](/azure/virtual-desktop/create-host-pools-azure-marketplace#begin-the-host-pool-setup-process)來輸入這項資訊。
- **密度：** 特定的使用者群組是否需要較低密度的桌面體驗？ 例如，每個虛擬中央處理單位可能需要兩位使用者 (vCPU) ，而不是每個 vCPU 的使用者假設有六位使用者。 您將在 [Windows 虛擬桌面主機集區部署的集區設定](/azure/virtual-desktop/create-host-pools-azure-marketplace#begin-the-host-pool-setup-process)中輸入密度資訊。
- **效能：** 特定的使用者群組需要較高的效能桌面體驗嗎？ 例如，有些使用者的每個 vCPU 需要的記憶體比每個 vCPU 所假設的 4 gb &nbsp; (GB) 。 您將在 [Windows 虛擬桌面主機集區部署的虛擬機器詳細資料](/azure/virtual-desktop/create-host-pools-azure-marketplace#virtual-machine-details)中，輸入 VM 大小調整。
- **圖形處理 (GPU) ：** 特定的使用者群組是否有更大的圖形需求？ 例如，有些使用者需要在 Azure 中以 GPU 為基礎的 Vm，如本 [指南的設定 Gpu vm](/azure/virtual-desktop/configure-vm-gpu)所示。
- **Azure 區域：** 特定群組的 OS 使用者是否可以從不同的地理區域運作？ 例如，在設定主機集區之前，每個區域的使用者都應該使用「 [估計」工具](https://azure.microsoft.com/services/virtual-desktop/assessment/#estimation-tool)來測試 Azure 的延遲。 測試使用者應該共用最低延遲的 Azure 區域和前三個 Azure 區域的延遲（以毫秒為單位）。
- **商務功能：** 特定群組的使用者是否可由商務單位、收費程式碼或其商務功能貯存？ 這種類型的群組有助於在後續的作業階段中調整公司成本。
- **使用者計數：** 每個不同的角色會有多少使用者？
- **最大會話計數：** 根據作業的地理位置和小時數，每個角色在最大負載期間預期會有多少並行使用者？

上述每個問題的差異將開始以商務功能、成本中心、地理區域和技術需求來說明使用者角色。 下表可協助記錄回應，以填入已完成的評估或設計檔：

| 條件 | 角色群組 &nbsp; 1 | 角色群組 &nbsp; 2 | 角色群組 &nbsp; 3 |
|---------|---------|---------|---------|
| 集區 | 集區 | 集區 | 專用 (安全性考慮)  |
| 密度 | Light (6 &nbsp; 使用者/vCPU)  | 大量 (2 &nbsp; 使用者/vCPU)  | 專用 (1 &nbsp; 使用者/vCPU)  |
| 效能 | 低 | 高記憶體 | 低 |
| GPU | N/A | 必要 | 不適用 |
| Azure 區域 | 北美洲 | 西歐 | 北美洲 |
| 使用者計數 | 1,000 | 50 | 20 |
| 會話計數 | 200 | 50 | 10 |

每個角色（或每個具有不同商務功能和技術需求的使用者群組）都需要特定的主機集區設定。

使用者評量會提供所需的資料：集區類型、密度、大小、CPU/GPU、登陸區域區域等等。

主機集區設定評量現在會將該資料對應至部署計畫。 調整技術需求、商務需求和成本將有助於判斷主機集區的適當數目和設定。

請參閱 [美國東部](https://azure.com/e/448606254c9a44f88798892bb8e0ef3c) [、西歐](https://azure.com/e/61a376d5f5a641e8ac31d1884ade9e55)或 [東南亞](https://azure.com/e/7cf555068922461587d0aa99a476f926) 區域的定價範例。

### <a name="application-groups"></a>應用程式群組

目前內部部署環境的 Movere 和 lakeside 掃描都可以提供在使用者桌上型電腦上執行之應用程式的相關資料。 藉由使用該資料，您可以建立每個角色所需的所有應用程式清單。 針對每個必要的應用程式，下列問題的答案將會塑造部署反覆運算：

- 是否需要安裝任何應用程式，角色才能使用此桌面？ 除非角色使用100% 的 web 型軟體即服務應用程式，否則您可能需要為每個角色 [設定自訂的主要 VHD 映射](/azure/virtual-desktop/set-up-customize-master-image) ，並在主要映射上安裝必要的應用程式。
- 此角色是否需要 Microsoft 365 應用程式？ 如果是，您必須 [將 Microsoft 365 新增至自訂的主要 VHD 映射](/azure/virtual-desktop/install-office-on-wvd-master-image)。
- 此應用程式是否與 Windows &nbsp; 10 企業版多重會話相容？ 如果應用程式不相容，可能需要 [個人集](/azure/virtual-desktop/configure-host-pool-personal-desktop-assignment-type) 區才能執行自訂 VHD 映射。 如需有關應用程式和 Windows 虛擬桌面相容性問題的協助，請參閱 [桌面應用程式確保](/fasttrack/win-10-app-assure-assistance-offered) 服務。
- 要徑任務應用程式是否可能會受到 Windows 虛擬桌面實例與任何後端系統之間的延遲？ 若是如此，您可能會想要考慮將支援應用程式的後端系統移轉至 Azure。

這些問題的答案可能會要求計畫在進行桌上型電腦遷移或部署之前，先在桌面映射或支援應用程式元件中納入補救。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

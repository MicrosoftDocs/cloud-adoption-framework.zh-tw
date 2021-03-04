---
title: 混合式和多重雲端遷移
description: 瞭解遷移至混合式和多重雲端環境
author: brianblanchard
ms.author: brblanch
ms.date: 02/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: e2e-hybrid
ms.openlocfilehash: eb8b0805fc05621be7efddbf7afd3aba767333f9
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794905"
---
# <a name="hybrid-and-multicloud-migration"></a>混合式和多重雲端遷移

在 [遷移方法](../../migrate/index.md)中，遷移至雲端的作業已被視為混合式或多重雲端的程式。 在遷移至混合式和多重雲端環境時，該方法的大部分指導方針都會保持相關。 從該方法的最大轉變，與長期的遷移目標有關。

![從單向雲端遷移轉移至下一段所述的雙向混合式和多重雲端遷移。](../../_images/hybrid/primary-cloud-provider.png)

一般情況下，遷移工作已視為單向街道;資產會移至雲端 (或新的雲端) 並保持在該處。 在混合式和多重雲端環境中，遷移工作比較像是 multilane 高速公路;資產會根據不斷變化的商務或技術需求，在多個公用和私人雲端之間移動。 這項遷移策略的轉換對遷移程式沒有任何影響，但可能會直接影響遷移之前和之後的所有工作。

## <a name="impact-on-migration-specific-processes"></a>對遷移特定進程的影響

雖然它們對遷移程式的直接影響很小，但是要注意 divergences 可能會提高組織的成功可能性。 遷移工作負載的動作是由三個高階進程所組成， (或短期) 衝刺中重複執行，直到完成遷移為止。 以下是這些程式如何變更的簡短介紹：

- **評定工作負載：** 在遷移之前，有幾個考慮會塑造工作負載的評估方式。
- **部署工作負載：** 部署工作負載的程式大多都不會變更。 但是，您可能會想要使用更多 [Azure 遷移](/azure/migrate/) 生態系統來加速特定類型的遷移。
- **發行工作負載：** 一旦部署工作負載之後，在發行至生產環境流量之前，測試週期中會出現最大的轉變。

本指南稍後會討論如何在您的遷移程式中評估、部署或發行工作負載的其他指引。 但首先，請參閱下一節，以瞭解將會影響您的遷移的上游和下游程式較大的變更。

## <a name="impact-on-upstream-and-downstream-processes"></a>對上游和下游進程的影響

在混合式和多重雲端環境中遷移工作負載時，真正的影響是在遷移之前和之後的工作。 在以混合式和多重雲端方法遷移工作負載之前，請參閱 [混合式和多重雲端簡介](./index.md) 和 [整合作業簡介](./unified-operations.md) ，以瞭解您的遷移工作以外的其他變更。

> [!WARNING]
> 上述連結提供高階見解，讓您能夠為您完成成功的設定。 在這些文章中，有重要的技術變更和影響的連結。 請勿在混合式和多重雲端策略下繼續進行遷移，而不需要對您的 [計畫](./plan.md)、 [環境就緒](./ready.md)程度和 [營運管理](./manage.md)的影響有基本的瞭解。 若無法準備這些活動，將會產生較高的營運成本，而且可能會產生非預期的廠商鎖定。

## <a name="assess-workloads-for-hybrid-and-multicloud-migration"></a>評估混合式和多重雲端遷移的工作負載

標準遷移中使用的 Azure 產品仍然適用于混合式和多重雲端遷移。 具體而言，Azure 遷移和服務對應可用來瞭解您的數位資產，並概述您的相依性。 如需這兩種工具的詳細資訊，請參閱 [評估工作負載的使用者入門指南](../../migrate/azure-migration-guide/assess.md)。 當您建立計畫或評估混合式和多重雲端工作負載的方式時， [Azure 中的數位資產評量的最佳作法](../../plan/contoso-migration-assessment.md) 仍適用。

當混合式和多重雲端遷移遇到評量挑戰時，會在其遷移團隊的評定流程中指出缺乏成熟度。 下列考慮應該分解為您的評估計畫：

- 評估您的工作負載時，務必考慮與 Azure 和 Azure 登陸區域的相容性。 在工作負載評估期間，您也必須考慮與任何混合式網路、混合式身分識別、混合式安全性，或在其他混合式或多重雲端環境中建立的混合式管理/治理條件約束的相容性。
- 更詳盡的強調也必須放在相依性上，因為其他雲端可能會裝載較大百分比的資產。
- 請務必瞭解混合式和多重雲端決策背後的原因，以利用支援的工具來評估各種工作負載的相容性：

  - 如果您要現代化您的資料中心，以允許內部部署的雲端原生解決方案， **Azure STACK HCI 相容性** 很重要。
  - 如果您要透過以容器為基礎的基礎結構來維護可移植 **性，Kubernetes 相容性** 很重要。
  - **Azure Edge 相容性** 可能很重要，可延伸工作負載，並減少互動點的延遲。
  - **法規、合規性或商務需求** 可能會要求某些資產或資料保留在內部部署環境。 若要監視已遷移的資產，您可能需要新增其他監視工具。

這些文章將協助您成熟此類型遷移所需的最具影響力流程：

- **[職責](../..//migrate/migration-considerations/assess/index.md#accountability-during-assessment)**
- **[工作負載分類](../../migrate/migration-considerations/assess/classify.md)**
- **[雲端相容性](../../migrate/migration-considerations/assess/evaluate.md)**
- **[敏捷變更管理](../../migrate/migration-considerations/assess/release-iteration-backlog.md)**
- **[建立定義完善的遷移待處理專案](../../plan/plan-intro.md)**

## <a name="deploy-migrated-workloads-for-hybrid-and-multicloud"></a>部署混合式和多重雲端的已遷移工作負載

遷移至雲端時，一定要清楚清查所有的相依資產和網路路徑，以確保這些資產都部署在正確的雲端中。 在遷移工作負載之前，明確的清查 (或數位資產評量) 在混合式環境中更為重要。 嘗試將工作負載遷移至混合式和多重雲端環境之前，請參閱上述的 *評估工作負載* 一節。

[Azure 遷移](/azure/migrate/migrate-services-overview) 是將工作負載從私人雲端遷移至 Azure 的主要解決方案。 從其他公用雲端遷移至 Azure 的最佳做法將有所差異。 而且，您可能需要在遷移至 Azure Stack HCI 時新增一些額外的工具。 請參閱下列教學課程：

- **[從 AWS 遷移至 Azure](/azure/migrate/tutorial-migrate-aws-virtual-machines)**
- **[從 GCP 遷移至 Azure](/azure/migrate/tutorial-migrate-gcp-virtual-machines)**
- **[遷移至 Azure Stack HCI](../../scenarios/azure-stack/migrate-deploy.md#deploy-workloads)**

## <a name="release-migrated-workloads-for-hybrid-and-multicloud"></a>為混合式和多重雲端發行已遷移的工作負載

在遷移至雲端時，無法過測試、基準測試/調整大小和促銷計畫的重要性。 混合式和多重雲端工作負載對於非集中式資產和連接它們的網路有更大的相依性。 它們更容易發生延遲、連線能力和路由問題，而這些問題可能看似雲端平臺效能問題。 混合式和多重雲端工作負載的測試和偵錯工具需要比部署到單一雲端提供者的工作負載需要更多的時間配置，以供身分識別和網路中的額外層級使用。

以下是在遷移至混合式和多重雲端環境時，要包含在測試計劃中的幾個考慮：

- **[調整商務和 IT 測試](../../migrate/migration-considerations/optimize/business-test.md)**
- **[基準測試和重新調整資產大小](../../migrate/migration-considerations/optimize/optimize.md)**
- **[成本和調整大小的最佳作法](../../migrate/azure-best-practices/migrate-best-practices-costs.md)**
- **[升級到生產環境](../../migrate/migration-considerations/optimize/promote.md)**
- **[針對整合作業做好準備](./unified-operations.md)**

## <a name="next-step-govern-hybrid-and-multicloud-environments"></a>下一步：管理混合式和多重雲端環境

下列文章清單會帶您前往雲端採用旅程中特定點的指導方針，以協助您成功進行雲端採用案例。

- [管理混合式和多重雲端環境](./govern.md)
- [管理混合式和多重雲端環境](./manage.md)

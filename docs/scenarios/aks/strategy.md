---
title: 新式容器採用策略
description: 描述案例對策略的影響
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: ed78eeae296d2a2bb2c80ea6006ef8e92622709e
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793661"
---
# <a name="strategic-impact-of-modern-containers"></a>新式容器的策略性影響

最佳做法是鼓勵客戶使用 [雲端採用架構的策略方法](../../strategy/index.md)來建立單一的集中式雲端採用策略。 如果您還沒有這麼做，請使用 [策略和計畫範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄您的雲端採用策略。

本文將協助您針對將會影響您策略的容器，提供許多相關考慮。

## <a name="modern-container-motivations"></a>新式容器動機

當創新的優先順序很高時，組織最常選擇容器。 但是，管理高度元件化的雲端原生應用程式也是很常見的驅動程式。 在遷移非容器化的工作負載時，如果工作負載可攜性和環境一致性可透過工作負載中最基本的功能變更，則容器化工作負載也可能是個不錯的選項。

接著，Kubernetes 會提供一層協調流程，讓組織更輕鬆地利用容器的優點，並改善容器化環境和工作負載的整體作業。 在 Azure 中，Azure Kubernetes Service (AKS) 提供端對端容器協調流程即服務。

## <a name="modern-container-outcomes"></a>新式容器結果

若要測量容器和 Kubernetes 採用工作的影響，下列是可追蹤和評估的一些關鍵結果：

- 布建 **和調整時間：** 彈性基礎結構，以減少布建和調整支援您工作負載的資源的時間。
- **加速開發時間：** 結合開發工具、自動化部署，以及橫跨一致和標準化環境的整合式監視，可讓開發人員更專注于建立絕佳的產品，而不是在開發和生產環境中支援基礎結構設定。
- **工作負載可攜性：** 使用常見的容器化環境將應用程式移至任何位置。 因為 Kubernetes 叢集和基礎結構之間有一個抽象層，所以您可以在開發和生產環境之間，或在雲端提供者之間移動工作負載，而不需要投入雲端提供者原生解決方案。
- **安全的部署實務：** Kubernetes 工作負載和結構支援許多部署方法，可讓工作負載安心地推出。
- **整合式存取管理：** 您現有的身分識別與存取管理 (IAM) 提供者和叢集之間的整合，可確保部署的所有階段都有安全的環境。
- **基礎結構隔離：** Kubernetes 叢集的範圍可透過完全私人供應專案的公用雲端供應專案，讓您可以明確地控制您的工作負載和其執行時間環境的暴露程度。
- **網路可檢視性：** 進入和離開 Kubernetes 叢集的流量，可能會受到內部流量監視和控制，以達成所需的安全性結果。
- **人才保有：** 新式基礎結構可改善雇用和保留人才的原因，是因為使用了最適合使用的新式、業界標準、雲端中立的工具。

## <a name="next-step-plan-for-modern-containers"></a>下一步：規劃新式容器

下列文章清單會帶您前往雲端採用旅程中特定點的指導方針，以協助您成功進行雲端採用案例。

- [規劃新式容器](./plan.md)
- [檢查您的環境或 Azure 登陸區域](./ready.md)
- [將工作負載遷移至新式容器](./migrate.md)
- [使用新式容器解決方案進行創新](/azure/architecture/reference-architectures/containers/aks-start-here?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
- [管理新式容器解決方案](./govern.md)
- [管理新式容器解決方案](./manage.md)

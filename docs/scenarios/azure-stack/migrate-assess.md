---
title: 評估 Azure Stack Hub 遷移的工作負載
description: 評估 Azure Stack 中樞遷移的工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 0278ef7006deec80580a6f457863049452c5069d
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88236993"
---
# <a name="assess-workloads-for-azure-stack-hub-migration"></a>評估 Azure Stack Hub 遷移的工作負載

本文假設您已決定將 [Azure Stack 整合到您的雲端策略](./index.md)、開發 [Azure Stack 中樞遷移的計畫](./plan.md)，以及 [您的環境已準備好進行遷移](./ready.md)。

在計畫方法的組織數位資產合理化期間，系統會探索並清查每個工作負載，並根據量化資料進行初始決策。 在部署每個工作負載之前，請務必以定性資料驗證資料和決策。

## <a name="placement"></a>放置

第一個要考慮的資料點是位置。 也就是說，此工作負載會遷移至您的公用雲端、私人雲端或其他雲端平臺，例如主權雲端或服務提供者的 Azure 環境？

下列各節中的資訊可協助您驗證有關放置的決策。 這項資訊也會協助在部署您的工作負載期間，將會有説明的介面資料。

## <a name="stakeholder-value"></a>專案關係人值

評估與企業和 IT 專案關係人遷移此工作負載的價值：

- 較少的摩擦：短期焦點、有限的長期可用性。
- 更多的摩擦：長期投資，更容易反復執行並繼續現代化。
- 兩者的餘額。

## <a name="governance-risk-and-compliance"></a>治理、風險和合規性

評估法規、合規性和隱私權需求的影響：

- 哪些資料可以放在 Azure 上，而哪些資料需要保存在內部部署？
- 誰可以管理基礎平臺？
- 資料是否相依于位置？
- 儲存資料是否有過期日期？

## <a name="success-metrics"></a>成功的計量

判斷成功的計量和可用性容錯：

- 效能
- 可用性
- 災害復原
- 部署或遷移方法

## <a name="licensing"></a>授權

評估授權和支援的影響：

- 是否有會限制轉換的產品授許可權制？
- 新環境中的應用程式或資料集是否可支援？
- 是否有協力廠商軟體廠商需要提供支援聲明？

## <a name="operations-requirements"></a>作業需求

- 藉由檢查 IT 管理的雲端服務和應用程式特定服務之間的相互關聯，避免重複工作並將服務等級協定 (Sla) 。
- 請考慮在部署和遷移應用程式期間協調服務布建所需的自動化。
- 為協助滿足您的作業需求，請考慮擴充 [性和可用性](https://azure.microsoft.com/blog/azure-stack-iaas-part-six/) 服務，例如依使用量付費、虛擬機器 (VM) 可用性設定組、vm 擴展集、網路介面卡，以及新增和調整 vm 和磁片大小的功能。

## <a name="monitoring"></a>監視

- 使用妥善定義的計量，以構成您提供給使用者的 Sla 基礎，來監視系統健康情況和操作狀態和效能。
- 檢查安全性與合規性，評估雲端環境符合應用程式所強加之法規和合規性需求的程度。
- 備份/還原和複寫/容錯移轉的處理常式為何？
- 尋找適用于基礎結構即服務、平臺即服務和軟體即服務資源的資料保護服務。
- 結合多項廠商、技術和功能，以達成全方位的保護原則。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

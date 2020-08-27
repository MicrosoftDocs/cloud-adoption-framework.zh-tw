---
title: 評估 Azure Stack Hub 遷移的工作負載
description: 評定 Azure Stack Hub 遷移的工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 195b630b994a83a39026374f27de297b010ae024
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88885468"
---
# <a name="assess-workloads-for-azure-stack-hub-migration"></a>評估 Azure Stack Hub 遷移的工作負載

本文假設您已決定將 [Azure Stack 整合到您的雲端策略](./index.md)，您已開發 [Azure Stack Hub 遷移的計畫](./plan.md)，而且 [您的環境已準備好進行遷移](./ready.md)。

在計畫方法的組織數位資產合理化期間，每個工作負載都經過探索和清查，並根據量化資料進行初始決策。 部署每個工作負載之前，請務必使用定性資料來驗證資料和決策。

## <a name="placement"></a>放置

要考慮的第一個資料點是放置。 亦即，此工作負載是否會遷移到您的公用雲端、私人雲端或其他雲端平臺，例如主權雲端或服務提供者的 Azure 環境？

下列各節中的資訊可協助驗證您的放置相關決策。 這項資訊也會協助您在部署工作負載時，將會有説明的介面資料。

## <a name="stakeholder-value"></a>專案關係人值

評估將此工作負載與商務和 IT 專案關係人遷移的價值：

- 減少摩擦：短期焦點、受限的長期可用性。
- 更多衝突：長期投資、更輕鬆地逐一查看並繼續現代化。
- 這兩者的餘額。

## <a name="governance-risk-and-compliance"></a>治理、風險和合規性

評估法規、合規性和隱私權需求的影響：

- 哪些資料可以存放在 Azure 上，哪些資料需要保持在內部部署？
- 誰可以管理基礎平臺？
- 資料是否相依于該位置？
- 是否有儲存資料的到期日期？

## <a name="success-metrics"></a>成功計量

判斷成功的計量和可用性容限：

- 效能
- 可用性
- 復原
- 部署或遷移方法

## <a name="licensing"></a>授權

評估授權和支援的影響：

- 是否有會限制轉換的產品授許可權制？
- 新環境中的應用程式或資料集是否可支援？
- 是否有需要提供支援聲明的協力廠商軟體廠商？

## <a name="operations-requirements"></a>作業需求

- 藉由檢查 IT 管理的雲端服務和應用程式專屬服務之間的相互關聯，以避免重複工作並將服務等級協定優化 (Sla) 。
- 考慮在部署和遷移應用程式期間協調服務布建所需的自動化。
- 為了協助符合您的作業需求，請考慮擴充 [性和可用性](https://azure.microsoft.com/blog/azure-stack-iaas-part-six/) 服務，例如依使用量付費、虛擬機器 (VM) 可用性設定組、VM 擴展集、網路介面卡，以及新增和調整 vm 和磁片大小的能力。

## <a name="monitoring"></a>監視

- 使用定義完善的計量來監視系統健全狀況和操作狀態和效能，以構成您提供給使用者的 Sla。
- 檢查安全性與合規性，評估雲端環境符合應用程式所強加之法規和合規性需求的程度。
- 備份/還原和複寫/容錯移轉有哪些處理常式？
- 尋找適用于基礎結構即服務、平臺即服務和軟體即服務資源的資料保護服務。
- 納入多種廠商、技術和功能，以達成全方位的保護原則。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

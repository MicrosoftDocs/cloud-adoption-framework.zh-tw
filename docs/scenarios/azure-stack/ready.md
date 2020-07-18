---
title: 準備好您的雲端環境來進行 Azure Stack Hub 遷移
description: 準備好您的雲端環境來進行 Azure Stack Hub 遷移
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: a83c14475cc4d0c46e6c69e4061ea115e5b9b62b
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451823"
---
# <a name="ready-your-cloud-environment-for-azure-stack-hub-migration"></a>準備好您的雲端環境來進行 Azure Stack Hub 遷移

本文假設您已決定將[Azure Stack 整合到您的雲端策略](./index.md)，並已開發[Azure Stack Hub 遷移的規劃](./plan.md)。

評估必須先解決的基礎結構相依性：

- 身分識別
- 連線能力
- 安全性
- 加密

## <a name="hybrid-environment-configuration"></a>混合式環境設定

如果您正在開發混合式環境，並在 Azure 的公用雲端中有部分的 IT 組合，而您在 Azure Stack Hub 私用雲端中組合的其他部分，則您會先在公用雲端中設定一些基本事項。 [準備好方法](../../ready/index.md)中的指引可協助您在公用雲端中建立登陸區域。

**登陸區域和雲端平臺連線：** 在該過程中，請確定您在目前的資料中心與 Azure 之間具有穩定的網路連線。 建立網路連線之後，請測試與 Azure 連線的延遲、頻寬和可靠性。

**治理和作業：** 同時遷移至這兩個雲端時，您必須進行一些早期決策，這會影響環境。 最佳做法是以雲端原生作業和在公用雲端中執行的治理工具為建立。 這種方法可降低在資料中心執行昂貴系統的成本，或在 Azure Stack 中樞上耗用容量的費用。 遷移至任一形式的 Azure 雲端時，您必須決定遵循最佳做法，或繼續使用現有系統進行操作、管理和變更管理。

## <a name="private-cloud-environment"></a>私用雲端環境

如果您選擇只使用 Azure 的私用雲端版本，Azure Stack 中樞，則必須考慮相同的決策點。

**內部部署治理和作業：** 最佳作法仍然建議使用 Azure 公用雲端版本中的雲端原生作業和治理工具。 請務必及早評估這些最佳作法，並判斷最佳做法是否適用于您的案例。

**登陸區域和雲端平臺連線：** 如果您的工作負載遷移將會部署到 Azure Stack 中樞，則請務必記錄並測試使用者與 Azure Stack 設備之間網路路由的延遲、頻寬和可靠性。

## <a name="next-step-assess-workloads-before-migration"></a>下一個步驟：在遷移前評估工作負載

下列文章提供在雲端採用旅程圖的特定時間點中找到的指引。 評估工作負載的第一篇文章，會比在規劃過程中的評估更深入，以確保您已準備好遷移每個工作負載。

- [評估 Azure Stack 中樞的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack 中樞](./migrate-deploy.md)
- [管理 Azure Stack 中樞](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

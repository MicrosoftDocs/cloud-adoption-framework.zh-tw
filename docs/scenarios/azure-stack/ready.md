---
title: 準備好您的雲端環境以進行 Azure Stack Hub 遷移
description: 準備好您的雲端環境以進行 Azure Stack Hub 的遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 5bd33e4e61dbef039b8d993606596fb707d66085
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88885383"
---
# <a name="ready-your-cloud-environment-for-azure-stack-hub-migration"></a>準備好您的雲端環境以進行 Azure Stack Hub 遷移

本文假設您已決定將 [Azure Stack 整合到您的雲端策略](./index.md) ，並已開發 Azure Stack Hub 的 [遷移計畫](./plan.md)。

評估必須先解決的基礎結構相依性：

- 身分識別
- 連線能力
- 安全性
- 加密

## <a name="hybrid-environment-configuration"></a>混合式環境設定

在混合式環境中，您的 IT 組合的某些部分會在 Azure 公用雲端中，而其他部分則位於您的 Azure Stack Hub 私用雲端中。 若要開發這樣的環境，您必須先設定公用雲端中的一些基本元素。 若要開始在公用雲端中建立登陸區域，請確定 [您的環境已準備好進行雲端採用](../../ready/index.md)。

**登陸區域和雲端平臺**連線：在此過程中，請確定您目前的資料中心與 Azure 之間有穩定的網路連線。 在您建立網路連線之後，請測試 Azure 連線的延遲、頻寬和可靠性。

**治理和作業**：當您遷移至這兩個雲端時，您必須進行幾個早期的決策，將會影響環境。 藉由套用最佳作法，您可以建立在公用雲端中執行的雲端原生作業和治理工具。 這種方法可減少在您的資料中心內執行昂貴系統的成本，或在 Azure Stack Hub 上耗用容量。 當您遷移至任一種形式的雲端時，您需要遵循最佳作法，或繼續使用現有的系統進行操作、治理及變更管理。

## <a name="private-cloud-environment"></a>私用雲端環境

如果您選擇只使用私人雲端版本的 Azure （Azure Stack Hub），則必須考慮相同的決策點：

**內部部署治理和作業**：最佳作法仍建議使用 Azure 公用雲端版本中的雲端原生作業和治理工具。 請務必提早評估這項最佳作法，並判斷它是否適用于您的案例。

**登陸區域和雲端平臺**連線：如果您的工作負載遷移將會部署到您的 Azure Stack Hub，請務必記錄並測試終端使用者與 Azure Stack 設備之間網路路由的延遲、頻寬和可靠性。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

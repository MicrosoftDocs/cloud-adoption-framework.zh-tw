---
title: 準備好您的雲端環境來進行 Azure Stack Hub 遷移
description: 準備好您的雲端環境來進行 Azure Stack Hub 遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 38e66e353b0757ca34d9b0b61639ce36739d5f05
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237197"
---
# <a name="ready-your-cloud-environment-for-azure-stack-hub-migration"></a>準備好您的雲端環境來進行 Azure Stack Hub 遷移

本文假設您已決定將 [Azure Stack 整合到您的雲端策略](./index.md) ，並已開發 [Azure Stack Hub 遷移的規劃](./plan.md)。

評估必須先解決的基礎結構相依性：

- 身分識別
- 連線能力
- 安全性
- 加密

## <a name="hybrid-environment-configuration"></a>混合式環境設定

在混合式環境中，您的 IT 組合中有些部分位於 Azure 公用雲端中，而其他元件則位於您的 Azure Stack Hub 私人雲端中。 若要開發這類環境，您必須先在公用雲端中設定幾個基本元素。 若要開始在公用雲端中建立登陸區域，請參閱 [確定您的環境已準備好進行雲端採用](../../ready/index.md)。

**登陸區域和雲端平臺**連線：在此過程中，請確定您在目前的資料中心與 Azure 之間具有穩定的網路連線。 建立網路連線之後，請測試與 Azure 連線的延遲、頻寬和可靠性。

**治理和操作**：當您同時遷移至這兩個雲端時，您需要進行一些早期決策，以影響環境。 藉由套用最佳作法，您就可以建立雲端原生作業和在公用雲端中執行的治理工具。 這種方法可降低在您的資料中心執行昂貴系統，或在 Azure Stack hub 上耗用容量的成本。 當您遷移至任一種雲端時，您需要遵循最佳做法，或繼續使用現有系統進行作業、治理和變更管理。

## <a name="private-cloud-environment"></a>私用雲端環境

如果您選擇僅使用私人雲端版本的 Azure （Azure Stack 中樞），您將需要考慮相同的決策點：

內部**部署治理和作業**：最佳作法仍然建議使用 Azure 公用雲端版本中的雲端原生作業和治理工具。 請務必及早評估這項最佳作法，並判斷它是否適用于您的案例。

**登陸區域和雲端平臺**連線：如果您的工作負載遷移將會部署到您的 Azure Stack 中樞，則請務必記錄並測試使用者與 Azure Stack 設備之間網路路由的延遲、頻寬和可靠性。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

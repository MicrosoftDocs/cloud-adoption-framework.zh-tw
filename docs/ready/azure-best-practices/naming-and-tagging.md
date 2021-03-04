---
title: 開發 Azure 資源的命名和標記策略
description: 閱讀企業雲端採用工作的資源命名和標記策略總覽。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal, readiness, fasttrack-edit
ms.openlocfilehash: 027504b257fc05e58c42329b78c1e4c8fe91f50b
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115776"
---
# <a name="develop-your-naming-and-tagging-strategy-for-azure-resources"></a>開發 Azure 資源的命名和標記策略

組織您的雲端資產，以支援治理、營運管理和會計需求。 定義完善的命名和元資料標記慣例有助於快速找出及管理資源。 這些慣例也有助於透過計費和回報會計機制，將雲端使用量成本與商務小組產生關聯。

儘早定義您的命名和標記策略。 使用下列連結可協助您定義和實行策略：

- [定義您的命名慣例](./resource-naming.md)
- [Azure 資源類型的建議縮寫](./resource-abbreviations.md)
- [定義您的標記策略](./resource-tagging.md)
- [資源命名與標記決策指南](../../decision-guides/resource-tagging/index.md)

> [!NOTE]
> 每個企業都有自己的組織和管理需求。 這些建議可協助您開始討論您的雲端採用小組。 當討論繼續進行時，請使用下列範本記錄您在將這些建議與特定商務需求一致時所進行的命名和標記決定。
>
> 下載 [命名和標記慣例追蹤範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/naming-and-tagging-conventions-tracking-template.xlsx)。

## <a name="purpose-of-naming-and-tagging"></a>命名和標記的用途

為了安全起見，正確地表示和命名您的資源是不可或缺的。 發生安全性事件時，請務必迅速找出受影響的系統、這些系統支援的功能，以及潛在的業務衝擊。 安全性服務（例如 [azure 資訊安全中心](/azure/security-center/security-center-introduction) 和 [azure Sentinel](/azure/sentinel/) ）參考資源與其相關聯的記錄，以及依資源名稱的警示資訊。

Azure 會定義 [azure 資源的命名規則和限制](/azure/azure-resource-manager/management/resource-name-rules)。 本指引提供詳細的建議，以支援企業雲端採用工作。

變更資源名稱可能很困難。 在開始進行任何大型雲端部署之前，請先建立完整的命名慣例。

## <a name="naming-and-tagging-strategy"></a>命名和標記策略

命名和標記策略包含業務和營運詳細資料，這些是資源名稱和中繼資料標籤的元件：

- 此策略的商務端可確保資源名稱和標記包含識別小組所需的組織資訊。 您可以與負責資源成本的業務擁有者一起使用資源。

- 在營運方面，可確保名稱和標記包含 IT 小組用來識別工作負載、應用程式、環境和重要性的資訊，以及用來管理資源的其他實用資訊。

## <a name="next-steps"></a>下一步

瞭解如何定義 Azure 資源和資產的命名慣例，以及查看 Azure 中資源和資產的範例名稱。

> [!div class="nextstepaction"]
> [命名您的 Azure 資源和資產](./resource-naming.md)

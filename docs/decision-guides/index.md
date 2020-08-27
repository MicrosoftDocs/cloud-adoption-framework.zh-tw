---
title: 架構決策指南
description: 使用這些核心雲端部署基礎結構元件模式和模型，來支援您的特定雲端部署案例。
author: rotycenh
ms.author: abuck
ms.date: 02/11/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: 7045ae53927ba95282b2ef610349617e6d3d7b1d
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88881762"
---
# <a name="architectural-decision-guides"></a>架構決策指南

雲端採用架構中的架構決策指南說明在建立雲端治理設計指引時有幫助的模式和模型。 每個決策指南著重於雲端部署的一個核心基礎結構元件，並列出可以支援特定雲端部署案例的模式和模型。

當您開始為組織建立雲端治理時，可據以採取動作的治理旅程會提供基準藍圖。 這些旅程假設不一定能反映貴組織的需求和優先順序。

這些決策指南藉由提供替代模式和模型來補充範例治理旅程，這些模式和模型可以協助您讓在範例設計指引中所做的架構設計選擇與您自己的需求保持一致。

## <a name="decision-guidance-categories"></a>決策指南類別

下列類別分別代表所有雲端部署的基礎技術。 治理旅程範例可依據範例業務需求做出與這些技術有關的設計決策，而且這些決策中有些部分可能不符合您組織的需求。 以下各節會討論這些類別的替代選項，讓您能夠選擇更適合自己需求的模式或模型。

[訂用帳戶](./subscriptions/index.md)：規劃您的雲端部署訂用帳戶設計和帳戶結構，以符合您的組織擁有權、計費及管理功能。

[身分識別](./identity/index.md)：將雲端式身分識別服務與您現有的身分識別資源整合，以支援您 IT 環境內的授權和存取控制。

[原則強制執行](./policy-enforcement/index.md)：請針對雲端部署的資源和工作負載，定義並強制執行符合組織需求的組織性原則規則。

[資源一致性](./resource-consistency/index.md)：請確定您的雲端型資源部署和組織保持一致，以強制執行資源管理和原則需求。

[資源標記](./resource-tagging/index.md)：組織雲端式資源以支援計費模型、雲端計量方法和管理，以及將資源使用量和成本最佳化。 資源標記需要一致且井然有序的命名和中繼資料配置。

[軟體定義網路](./software-defined-network/index.md)：使用快速部署和修改虛擬網路功能，將安全工作負載部署到雲端。 軟體定義網路可以支援敏捷工作流程、隔離資源，並且整合雲端型系統與您現有的 IT 基礎結構。

[加密](./encryption/index.md)：採用加密措施保護敏感性資料，以滿足組織的合規性和安全性原則需求。

[記錄和報告](./logging-and-reporting/index.md)：監視雲端型資源產生的記錄資料。 分析資料可提供工作負載的操作、維護與合規性狀態的健康情況見解。

## <a name="next-steps"></a>後續步驟

深入了解訂用帳戶和帳戶如何作為雲端部署的基礎。

> [!div class="nextstepaction"]
> [訂用帳戶設計](./subscriptions/index.md)

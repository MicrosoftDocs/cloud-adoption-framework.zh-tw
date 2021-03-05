---
title: 管理混合式和多重雲端工作負載的組合
description: 將治理功能擴充至混合式、多重雲端和邊緣部署。
author: brianblanchard
ms.author: brblanch
ms.date: 02/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: e2e-hybrid
ms.openlocfilehash: 2affa2383f958ec432f163b68b216a8ce394283c
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208501"
---
# <a name="govern-your-portfolio-of-hybrid-and-multicloud-workloads"></a>管理混合式和多重雲端工作負載的組合

雲端基本上改變了 IT 治理。 您現在可以使用自動化護欄和合規性工具來取代大量的手動審核和變更控制流程。 雲端採用和工作負載小組可以放心地創新，瞭解是否偵測到合規性和治理需求，而且通常會自動化。 這種新發現分享自由的關鍵是雲端的基礎結構即程式碼基礎。 所有資產都等於已定義的程式碼區塊，可進行測試及控管，如同任何其他程式碼基底。

在混合式、多重雲端和邊緣策略中，雲端治理的優點現在可以擴充到雲端以外的範圍。 您可以結合 [Azure Arc](/azure/azure-arc/overview) 與 [azure 原則](/azure/governance/policy/overview)、 [azure 藍圖](/azure/governance/blueprints/overview)和其他治理工具。 此組合可將許多治理護欄延伸至幾乎任何雲端資源、私用或公用雲端。 [整合作業](./unified-operations.md) 是使用原生 Azure 工具擴充治理控制項的最佳概念。

## <a name="deploy-a-unified-operations-mvp-for-governance"></a>部署治理的整合操作 MVP

妥善定義的治理從音效資源一致性實務著手。 組織資源、資源群組、訂用帳戶和 [管理群組可簡化治理](/azure/governance/management-groups/overview)。 透過幾個步驟來擴展您的雲端治理實務：

- 將的標記新增 `hosting platform` 至所有混合式、多重雲端和邊緣資產。
- 從 AWS、GCP 等標記資源。
- 查詢您的資源，以查看每個資源的裝載位置。

若要開始使用，請 [清查並標記您的混合式和多重雲端資源](../../manage/hybrid/server/best-practices/arc-inventory-tagging.md)。

在您建立標記標準並帶入某些資產之後，您可以使用熟悉的治理工具（例如 Azure 原則）來開始管理這些資源。 若要將原則指派給您的混合式和多重雲端資源，請參閱 [使用 Azure 原則來管理啟用 Azure Arc 的伺服器](../../manage/hybrid/server/best-practices/arc-policies-mma.md)的建議作法。

## <a name="governance-disciplines"></a>治理專業領域

有了對整合作業和 Azure Arc 的基本瞭解，您可以將雲端治理的專業領域延伸至裝載于 Azure 環境外部的部署。

安全性基準是最常見的方法，可讓您在統一的作業案例中擴充治理專業領域。 下列最佳做法將有助於在所有環境中保留您的安全性基準：

- [使用 Azure 安全性中心收集和偵測跨雲端的安全性資料](/azure/security-center/quickstart-onboard-machines)
- [使用 Azure Sentinel 調查及回應安全性威脅](/azure/sentinel/tutorial-investigate-cases)
- [將 AWS 帳戶連線至 Azure Defender](/azure/security-center/quickstart-onboard-aws)
- [將 GCP 帳戶連線至 Azure Defender](/azure/security-center/quickstart-onboard-gcp)

## <a name="next-steps"></a>下一步

如需雲端採用旅程的詳細指引，請參閱下列文章：

- [管理混合式和多重雲端環境](./manage.md)

---
title: 原則強制執行決策指南
description: 使用適用於 Azure 的雲端採用架構來了解在 Azure 移轉中作為核心設計優先事項的原則強制執行訂用帳戶。
author: rotycenh
ms.author: abuck
ms.date: 02/11/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: 75d89b4b626a8e31bdaf6c0022f4bc98dd4161d6
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196110"
---
# <a name="policy-enforcement-decision-guide"></a>原則強制執行決策指南

定義組織原則沒有效率，除非可以跨組織強制執行它。 規劃任何雲端移轉的關鍵層面，是判斷如何最佳地合併雲端平台提供的工具與您的現有 IT 流程，讓您的整個雲端資產原則合規性發揮到極致。

![繪製符合下列快速連結的原則強制執行選項 (從最簡單到最複雜)](../../_images/decision-guides/decision-guide-policy-enforcement.png)

跳至：[基準最佳做法](#baseline-best-practices) | [原則合規性監視](#policy-compliance-monitoring) | [原則強制執行](#policy-enforcement) | [跨組織原則](#cross-organization-policy) | [自動化強制執行](#automated-enforcement)

隨著您的雲端資產成長，您會面臨到跨更大的資源和訂用帳戶陣列維護及強制執行原則的對應需求。 當您的資產變得更大，組織原則需求也增加時，就必須擴充原則強制執行流程範圍，以確保遵循的原則皆一致並迅速偵測違規。

對於較小的雲端組織，平台所提供資源或訂用帳戶層級的原則強制執行機制通常就已足夠。 較大的部署所需的強制執行範圍也較大，可能需要利用更複雜的強制執行機制，牽涉到部署標準、資源群組和組織，並且讓原則強制執行與您的記錄和報告系統整合。

決定原則強制執行流程範圍的主要因素，是組織的[雲端治理需求](../../govern/index.md)、雲端資產的大小與性質，以及[訂用帳戶設計](../subscriptions/index.md)如何反映組織。 當資產大小增加或集中管理原則強制執行的需求變大，兩者都有必要增加強制執行範圍。

## <a name="baseline-best-practices"></a>基準最佳做法

對於單一訂用帳戶和簡單的雲端部署，使用 Azure 中資源和訂用帳戶的原生功能，即可強制執行許多公司原則。 一致使用雲端採用架構[決策指南](../index.md)中所討論的模式，可協助建立基準層級的原則合規性，而不需要另行投資原則強制執行。 這些功能包括：

- [部署範本](../resource-consistency/index.md)可以佈建具有標準化結構和設定的資源。
- [標記和命名標準](../resource-tagging/index.md)有助於組織作業，並支援會計和業務需求。
- 流量管理和網路限制可以透過[軟體定義網路](../software-defined-network/index.md)實作。
- [角色型存取控制](../identity/index.md)可以保護和隔離您的雲端資源。

透過檢查這些指南通篇討論的標準模式應用程式如何能夠協助符合您的組織需求，開始您的雲端原則強制執行規劃。

## <a name="policy-compliance-monitoring"></a>原則合規性監視

除了仰賴 Azure 平台提供的原則強制執行機制，還要確保能夠確認雲端式應用程式和服務皆符合組織原則。 這包括實作在資源變得不相容時，向負責的合作對象發出警示的通知功能。 有效地[記錄和報告](../logging-and-reporting/index.md)您雲端工作負載的合規性狀態，是公司原則強制執行策略中的關鍵部分。

隨著您的雲端資產成長，額外工具 (例如 [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center)) 可以提供整合的安全性和威脅偵測，協助套用集中式原則管理和警示您的內部部署和雲端資產。

## <a name="policy-enforcement"></a>強制執行原則

在 Azure 中，您可以在管理群組、訂用帳戶或資源群組層級套用組態設定和資源建立規則，協助確保原則對齊。

[Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview)是 Azure 服務，用於建立、指派和管理原則。 這些原則會對您的資源強制執行不同的規則和效果，讓這些資源能符合公司標準和服務等級協定的規範。 Azure 原則會評估您的資源是否符合指派的原則。 例如，您可能要限制環境中虛擬機器的 SKU 大小。 實作對應原則之後，新資源和現有資源都會針對合規性進行評估。 使用正確的原則，現有的資源就可以合規。

## <a name="cross-organization-policy"></a>跨組織原則

隨著您的雲端資產成長跨越需要強制執行的許多訂用帳戶，您必須將焦點放在整個雲端資產的強制執行策略，以確保原則一致性。

您的[訂用帳戶設計](../subscriptions/index.md)必須考量原則，因為這與您的組織結構相關。 除了協助支援您的訂用帳戶設計內的複雜組織[Azure 管理群組](../../ready/azure-best-practices/organize-subscriptions.md)可用來跨多個訂用帳戶指派 Azure 原則規則。

## <a name="automated-enforcement"></a>自動化強制執行

雖然標準化部署範本在較小規模中有效率，而 [Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/overview)可進行 Azure 解決方案的大規模標準化佈建和部署協調流程。 跨多個訂用帳戶的工作負載可以針對任何已建立的資源，以一致的原則設定進行部署。

針對整合雲端與內部部署資源的 IT 環境，您需要使用記錄和報告系統來提供混合式監視功能。 您的第三方或自訂作業監視系統可能會提供額外的原則強制執行功能。 針對更大或更成熟的雲端資產，請考量如何最佳地整合這些系統與雲端資產。

## <a name="next-steps"></a>後續步驟

原則強制執行只是雲端採用程序期間，其中一個需針對架構做出決策的核心基礎結構元件。 請瀏覽架構決策指南概觀，以了解在為其他類型的基礎結構制定設計決策時，使用的替代模式或模型。

> [!div class="nextstepaction"]
> [架構相關決策指南](../index.md)

---
title: Azure 中的安全性基準工具
description: 瞭解 Azure 原生工具如何協助成熟的原則和流程，以支援安全性基準專業領域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: ea3fdbc23847b1e0c20c84bade64e5d0e96b439e
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101787632"
---
# <a name="security-baseline-tools-in-azure"></a>Azure 中的安全性基準工具

[安全性基準專業領域](./index.md)是[雲端治理的五個專業領域](../governance-disciplines.md)之一。 此專業領域著重在建立原則的方式，以保護網路、資產，以及最重要的資料，也就是位於雲端提供者解決方案上的資料。 在雲端治理的五個專業領域中，安全性基準專業領域牽涉到數位資產和資料的分類。 它也牽涉到與資料、資產和網路安全性相關聯的風險、業務容錯和風險降低策略檔。 從技術觀點來看，此專業領域也包含有關[加密](../../decision-guides/encryption/index.md)、[網路需求](../../decision-guides/software-defined-network/index.md)、混合式身分[識別策略](../../decision-guides/identity/index.md)和工具的決策，以自動化跨[資源群組](../../decision-guides/resource-consistency/index.md)[強制執行](../../decision-guides/policy-enforcement/index.md)安全性原則。

下列 Azure 工具清單可協助您成熟支援此專業領域的原則和流程。

| 工具 | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal/) 和 [azure Resource Manager](/azure/azure-resource-manager/management/overview) | [Azure Key Vault](/azure/key-vault/)  | [Azure AD](/azure/active-directory/fundamentals/active-directory-whatis) | [Azure 原則](/azure/governance/policy/overview) | [Azure 資訊安全中心](/azure/security-center/security-center-introduction) | [Azure 監視器](/azure/azure-monitor/overview) |
|------------------------------------------------------------|---------------------------------|-----------------|----------|--------------|-----------------------|---------------|
| 對資源和建立資源套用存取控制   | 是                             | 否              | 是      | 否           | 否                    | 否            |
| 保護虛擬網路                                    | 是                             | 否              | 否       | 是          | 否                    | 否            |
| 為虛擬磁碟機加密                                     | 否                              | 是             | 否       | 否           | 否                    | 否            |
| 為 PaaS 儲存體和資料庫加密                         | 否                              | 是             | 否       | 否           | 否                    | 否            |
| 管理混合式識別服務                            | 否                              | 否              | 是      | 否           | 否                    | 否            |
| 限制允許的資源類型                         | 否                              | 否              | 否       | 是          | 否                    | 否            |
| 強制執行地理區域限制                          | 否                              | 否              | 否       | 是          | 否                    | 否            |
| 監視網路和資源的安全性健康情況          | 否                              | 否              | 否       | 否           | 是                   | 是           |
| 偵測惡意活動                                  | 否                              | 否              | 否       | 否           | 是                   | 是           |
| 事先偵測弱點                        | 否                              | 否              | 否       | 否           | 是                   | 否            |
| 設定備份和災害復原                     | 是                             | 否              | 否       | 否           | 否                    | 否            |

如需 Azure 安全性工具和服務的完整清單，請參閱 [Azure 可用的安全性服務與技術](/azure/security/fundamentals/services-technologies)。

客戶通常會使用協力廠商工具來啟用安全性基準專業領域活動。 如需詳細資訊，請參閱在 [Azure 資訊安全中心整合安全性解決方案](/azure/security-center/security-center-partner-integration)文章。

除了安全性工具以外， [Microsoft 365 中的合規性管理](https://www.microsoft.com/microsoft-365/enterprise/compliance-management) 還提供廣泛的指引、報告和相關檔，可協助您在遷移規劃過程中執行風險評估。

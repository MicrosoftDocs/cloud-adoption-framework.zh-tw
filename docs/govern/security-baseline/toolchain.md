---
title: Azure 中的安全性基準工具
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 這項工具的說明可協助改善 Azure 中的安全性基準。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 3bf3eea5486fbd349094663dc5f37527f5042bb5
ms.sourcegitcommit: d19e026d119fbe221a78b10225230da8b9666fe1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71221697"
---
# <a name="security-baseline-tools-in-azure"></a>Azure 中的安全性基準工具

[安全性基準](./index.md)是[五個雲端治理專業領域](../governance-disciplines.md)的其中之一。 這個專業領域著重于建立原則來保護網路、資產，以及最重要的是雲端提供者解決方案上的資料的方式。 在雲端治理的五個專業領域中，安全性基準專業領域牽涉到數位資產和資料的分類。 它也包含與資料、資產和網路的安全性相關聯的風險、商務容錯和風險降低策略的檔。 從技術觀點來看，此專業領域也包括有關[加密](../../decision-guides/encryption/index.md)、[網路需求](../../decision-guides/software-defined-network/index.md)、混合式身分[識別策略](../../decision-guides/identity/index.md)，以及[自動化強制執行](../../decision-guides/policy-enforcement/index.md)安全性原則之工具的參與決策跨[資源群組](../../decision-guides/resource-consistency/index.md)。

下列 Azure 工具清單可協助您成熟支援安全性基準的原則和流程。

| Tool | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal) / [資源管理員](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)  | [Azure 金鑰保存庫](https://docs.microsoft.com/azure/key-vault)  | [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) | [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview) | [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-intro) | [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview) |
|------------------------------------------------------------|---------------------------------|-----------------|----------|--------------|-----------------------|---------------|
| 對資源和建立資源套用存取控制   | 是                             | 否              | 是      | 否           | 否                    | 否            |
| 保護虛擬網路                                    | 是                             | 否              | 否       | 是          | 否                    | 否            |
| 為虛擬磁碟機加密                                     | 否                              | 是             | 否       | 否           | 否                    | 否            |
| 為 PaaS 儲存體和資料庫加密                         | 否                              | 是             | 否       | 否           | 否                    | 否            |
| 管理混合式識別服務                            | 否                              | 否              | 是      | 否           | 否                    | 否            |
| 限制允許的資源類型                         | 否                              | 否              | 否       | 是          | 否                    | 否            |
| 強制執行地理區域限制                          | 否                              | 否              | 否       | 是          | 否                    | 否            |
| 監視網路和資源的安全性健康情況          | 否                              | 否              | 否       | 否           | yes                   | 是           |
| 偵測惡意活動                                  | 否                              | 否              | 否       | 否           | yes                   | 是           |
| 事先偵測弱點                        | 否                              | 否              | 否       | 否           | 是                   | 否            |
| 設定備份和災害復原                     | 是                             | 否              | 否       | 否           | 否                    | 否            |

如需 Azure 安全性工具和服務的完整清單，請參閱 [Azure 可用的安全性服務與技術](https://docs.microsoft.com/azure/security/azure-security-services-technologies)。

客戶也通常會使用協力廠商工具來促進安全性基準活動。 如需詳細資訊，請參閱[在 Azure 資訊安全中心整合安全性解決方案](https://docs.microsoft.com/azure/security-center/security-center-partner-integration)一文。

除了安全性工具以外，[Microsoft 信任中心](https://www.microsoft.com/trustcenter/guidance/risk-assessment)也包含廣泛的指引、報告和相關文件，可協助您在移轉規劃程序中執行風險評估。

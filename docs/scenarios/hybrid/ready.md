---
title: 適用于混合式和多重雲端策略的現成方法
description: 使用 Azure 登陸區域準備您的環境以進行混合式和多重雲端
author: brianblanchard
ms.author: brblanch
ms.date: 02/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: e2e-hybrid
ms.openlocfilehash: b127e1b507e68d38d516f1f9cf5f438e989698ac
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794289"
---
# <a name="ready-prepare-your-environment-for-hybrid-and-multicloud"></a>就緒：準備您的環境以進行混合式和多重雲端

適用于 Microsoft Azure 雲端採用架構的 [準備方法](../../ready/index.md) ，會引導客戶完成環境準備，以準備進行雲端採用。 「就緒」區段包含 (Azure 登陸區域) 的技術加速器，這是任何雲端採用環境中的基本組建區塊。 登陸區域會將 Azure 環境的設定自動化，並配合雲端採用架構中的最佳作法指引。 準備進行混合式和多重雲端部署時，有許多環境設定可能稍有不同。

本文概述準備環境以進行下列動作所需的重要考慮和變更：

- 擴充連線界限以在混合式和多重雲端平臺之間進行連線
- 改善雲端原生服務（例如容器、Kubernetes 和超交集基礎結構解決方案）的支援，以降低混合和多重雲端的採用分歧
- 建立適當的雲端原生工具，以支援將 Azure 作為雲端平臺的 **其中一個**
- 在所有雲端環境中執行整合作業工具，以允許整合作業

本文將引導您完成達成每個環境目標所需的考慮。

## <a name="evaluate-your-cloud-mix"></a>評估您的雲端混合

選擇混合式和多重雲端環境不是二元決策，但是更接近一系列的決策， (如下圖所示) 。 在設定您的 Azure 環境 (或任何其他雲端環境) 之前，請務必找出您的雲端環境將如何支援您特定的雲端裝載決策組合。 以下是常見雲端混合的一些範例：

![3個圖顯示不同的客戶如何跨雲端提供者散發工作負載。](../../_images/hybrid/cloud-mix.png)

上圖說明與客戶最常看到的三種雲端混合。 每個藍點都代表一個工作負載。 每個橙色圓形都代表不同環境所支援的商務程式。 每個雲端混合都需要非常不同的 Azure 環境設定。

- **混合式第一個：** 大部分的工作負載都維持在內部部署環境，通常是混合傳統、混合式和可移植的資產裝載模型。 少量的特定工作負載會部署至 edge、Azure 或其他雲端提供者。
- **Azure first：** 大部分的工作負載都已移至 Azure。 少量的工作負載會維持在內部部署環境。 策略性決策導致在邊緣或多重雲端環境中的工作負載數量很少。
- **多重雲端 first：** 大部分的工作負載目前裝載在不同的公用雲端上，例如 GCP 或 AWS。 策略性決策促使了少量的工作負載存留在 Azure 或邊緣。 我們每個月都會看到穩定的客戶，從混合式 *第一次* 混合到 *Azure first* mix 的流程，與其雲端策略逐漸成熟。 但是，我們也支援許多已制定策略性決策來設定混合式或多重雲端混合優先順序的客戶。 Azure 在每個混合中扮演著角色。

為混合式和多重雲端準備任何雲端環境時，應考慮的最重要事項如下：

- 您今天支援混合式、邊緣和多重雲端混合的混合式、邊緣和？
- 何種混合最適合您的策略發展？
- 您想要獨立操作每個平臺，還是透過整合作業方法？

這些問題的答案將取決於您的應用程式和資料的混合式和多重雲端策略。 只要清楚瞭解您想要的雲端混合，就可以開始考慮適合您環境的最佳設定。

## <a name="analyze-your-cloud-mix"></a>分析您的雲端混合

如果您不確定目前的雲端混合，Azure 控制平面中的工具可以提供協助。 Azure Arc 是 Azure Resource Manager 的內建權益，可讓您瞭解雲端混合。 此工具組通常是控制雲端混合的第一步。 您可以使用 Azure Arc (作為在所有雲端平臺上進行探索的免費工具) 。

若要開始評估跨多個雲端提供者的雲端混合，請在幾個簡單的步驟中完成簡單的清查和標記練習：

- 將的標記新增 `hosting platform` 至所有混合式、多重雲端和邊緣資產。
- 從 AWS、GCP 或其他雲端提供者登入和標記資源。
- 查詢資源以查看其託管位置。

若要開始使用，請 [清查並標記您的混合式和多重雲端資源](../../manage/hybrid/server/best-practices/arc-inventory-tagging.md)

下列連結可協助您在每個雲端提供者中上架及標記資產：

- 將 Azure 資產上架： [Linux vm](../../manage/hybrid/server/best-practices/arm-template-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/arm-template-windows.md)
- 在您的本機資料中心內架資產： [Linux vm](../../manage/hybrid/server/best-practices/onboard-server-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/onboard-server-windows.md)
- 將 VMware 資產上架： [Linux vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-windows.md)
- 將 AWS 資產上線： [Linux vm 搭配 Terraform](../../manage/hybrid/server/best-practices/aws-terraform-al2.md) 和 [AWS Ubuntu with Terraform](../../manage/hybrid/server/best-practices/aws-terraform-ubuntu.md)
- 將 GCP 資產上架： [Ubuntu vm](../../manage/hybrid/server/best-practices/gcp-terraform-ubuntu.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/gcp-terraform-windows.md)

## <a name="configure-your-initial-azure-environment"></a>設定您的初始 Azure 環境

針對上述每個雲端混合，您將需要一個 Azure 環境來支援、管理及管理您的雲端資源。 雲端採用架構的就緒方法可協助您使用幾個簡單的步驟來準備您的環境：

- 請考慮每個 [Azure 登陸區域設計區域](../../ready/landing-zone/design-areas.md) ，以正確地評估您的技術需求。
- 將您的需求與 [Azure 登陸區域的執行選項](../../ready/landing-zone/implementation-options.md)進行比較，以找出並執行最適合的範本以開始設定。

## <a name="modify-your-environment-to-reflect-your-cloud-mix"></a>修改您的環境以反映您的雲端混合

一旦建立 Azure 環境之後，您就可以開始修改 Azure 環境，以支援最適當的雲端混合。 應考慮下列修改：

- 身分 **識別：** 哪個雲端將裝載您的主要身分識別提供者？ 如果該提供者在 Azure 之外，您可能需要將您的身分識別提供者與 Azure Active Directory 整合。 如需身分識別提供者的其他指引，請參閱 [這篇文章](/azure/active-directory/external-identities/identity-providers) 。
- **公用網路連線能力：** 最佳做法建議，所有輸入和輸出流量都應該盡可能在一個雲端平臺之間進行路由。 但是，您的需求或雲端混合可能需要更多的對等模型。 如果您的雲端混合用來滿足冗余和可靠性需求，這是特別常見的情況。 您將如何設定每個雲端平臺與公用網際網路之間的連線？
- **備份和復原：** 客戶通常會在其雲端混合的最可靠提供者集中，集中備份和復原策略。 這通常會導致其中一個雲端提供者作為共用的復原中心。 在每個案例中，azure 備份和 Azure Site Recovery 都可提供協助。
- **雲端平臺連線能力：** 如果您的雲端平臺將共用常見的復原、作業或治理資源，您可能需要在每個雲端平臺之間進行連線。 您要如何設定每個雲端平臺之間的連線能力？

### <a name="the-most-important-consideration"></a>最重要的考慮

**您是否會獨立操作每個雲端，或透過整合中央操作方法？**

獨立作業可以加倍或三倍的擁有權總成本。 對於某些客戶而言，擁有權總成本 (TCO) 成本增加可乘以10次以上。 為了將您的員工成本和需求降至最低，最佳做法是針對混合式和多重雲端策略的所有雲端混合，指向統一的作業方法。

若要深入瞭解您的雲端作業，請參閱混合式和多重雲端解決方案的 [整合作業](./unified-operations.md)、 [治理](./govern.md) 和 [作業管理](./manage.md) 文章。

## <a name="next-step-prepare-for-hybrid-and-multicloud-migration"></a>下一步：準備混合式和多重雲端遷移

下列文章清單會帶您前往雲端採用旅程中特定點的指引，以協助您在雲端採用案例中成功。

- [混合式和多重雲端遷移](./migrate.md)
- [管理混合式和多重雲端環境](./govern.md)
- [管理混合式和多重雲端環境](./manage.md)

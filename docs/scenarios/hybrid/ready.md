---
title: 適用于混合式和多重雲端策略的現成方法
description: 使用 Azure 登陸區域準備您的環境，以進行混合式和多重雲端案例。
author: brianblanchard
ms.author: brblanch
ms.date: 02/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: e2e-hybrid
ms.openlocfilehash: 7a4c37d9db441ba019bdddc2b82b263b4f20dd4e
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102209045"
---
# <a name="ready-prepare-your-environment-for-a-hybrid-and-multicloud-scenario"></a>就緒：準備您的環境以進行混合式和多重雲端案例

適用于 Azure 的雲端採用架構的 [準備方法](../../ready/index.md) ，會引導客戶完成環境準備，以準備進行雲端採用。 「就緒」區段包含 (Azure 登陸區域) 的技術加速器，這是任何雲端採用環境中的基本組建區塊。 登陸區域會將 Azure 環境的設定自動化，並配合雲端採用架構中的最佳作法指引。 當您準備進行混合式和多重雲端部署時，環境設定可能稍有不同。

本文概述準備您的環境所需的重要考慮和變更：

- 展開連線界限，以在混合式和多重雲端平臺之間進行連接。
- 改善容器、Kubernetes 和超融合式基礎結構解決方案等雲端原生服務的支援，以降低混合和多重雲端採用的分歧。
- 建立適當的雲端原生工具，以支援使用 Azure 做為您的 *其中一個* 雲端平臺。
- 在所有雲端環境中執行整合作業工具，以允許整合作業。

本文將引導您完成達成每個環境目標所需的考慮。

## <a name="evaluate-your-cloud-mix"></a>評估您的雲端混合

選擇混合式和多重雲端環境不是二元決策。 它更接近一系列的決策，如下圖所示。 設定 Azure 環境或任何其他雲端環境之前，請務必先找出您的雲端環境將如何支援您特定的雲端裝載決策組合。 一些常見的雲端混合範例如下所示。

![三個圖顯示不同的客戶如何跨雲端提供者散發工作負載。](../../_images/hybrid/cloud-mix.png)

上圖說明與客戶一起看到的三種最常見的雲端混合。 每個藍點都代表一個工作負載。 每個橙色圓形都代表不同環境所支援的商務程式。 這兩個雲端混合都需要不同的 Azure 環境設定。

- **混合式優先**：大部分的工作負載都維持在內部部署環境，通常是混合傳統、混合式和可移植的資產裝載模型。 有幾個特定的工作負載會部署至 edge、Azure 或其他雲端提供者。
- **Azure first**：大部分的工作負載都已移至 azure。 有些工作負載會維持在內部部署環境。 策略性決策促使一些工作負載存留在邊緣或多重雲端環境中。
- **多重雲端 first**：大部分的工作負載目前裝載于不同的公用雲端，例如 GCP 或 AWS。 策略性決策促使一些工作負載存留在 Azure 或邊緣。 我們每個月都會看到穩定的客戶，從混合式 *第一次* 混合到 *Azure first* mix 的流程，與其雲端策略逐漸成熟。 但是，我們也支援已做出策略性決策來排列混合式或多重雲端混合優先順序的客戶。 Azure 在每個混合中扮演著角色。

當您準備任何適用于混合式和多重雲端的雲端環境時，以下是要考慮的最重要專案：

- 您今天支援混合式、邊緣和多重雲端環境的混合？
- 哪些混合最適合您未來的策略？
- 您想要獨立操作每個平臺，或透過整合的作業方法？

這些問題的答案將取決於您的應用程式和資料的混合式和多重雲端策略。 只要清楚瞭解您想要的雲端混合，就可以開始考慮適合您環境的最佳設定。

## <a name="analyze-your-cloud-mix"></a>分析您的雲端混合

如果您不確定目前的雲端混合，Azure 控制平面中的工具可以提供協助。 Azure Arc 是 Azure Resource Manager 的內建權益，可讓您瞭解雲端混合。 此工具組通常是控制雲端混合的第一步。 您可以使用 Azure Arc (作為在所有雲端平臺上進行探索的免費工具) 。

若要開始評估跨多個雲端提供者的雲端混合，請在幾個步驟中完成清查和標記練習：

- 將的標記新增 `hosting platform` 至所有混合式、多重雲端和邊緣資產。
- 從 AWS、GCP 或其他雲端提供者中，帶入並標記資源。
- 查詢資源以查看其託管位置。

若要開始使用，請 [清查並標記您的混合式和多重雲端資源](../../manage/hybrid/server/best-practices/arc-inventory-tagging.md)。

下列連結可協助您在每個雲端提供者之間，開啟並標記資產：

- **Azure 資產**： [Linux Vm](../../manage/hybrid/server/best-practices/arm-template-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/arm-template-windows.md)
- **您本機資料中心的資產**： [Linux Vm](../../manage/hybrid/server/best-practices/onboard-server-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/onboard-server-windows.md)
- **VMware 資產**： [Linux Vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-windows.md)
- **AWS 資產**：搭配使用 Terraform 和 [AWS Ubuntu 和 Terraform 的](../../manage/hybrid/server/best-practices/aws-terraform-ubuntu.md) [Linux vm](../../manage/hybrid/server/best-practices/aws-terraform-al2.md)
- **GCP 資產**： [Ubuntu Vm](../../manage/hybrid/server/best-practices/gcp-terraform-ubuntu.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/gcp-terraform-windows.md)

## <a name="configure-your-initial-azure-environment"></a>設定您的初始 Azure 環境

針對上述每個雲端混合，您將需要一個 Azure 環境來支援、管理及管理您的雲端資源。 雲端採用架構的準備方法可協助您使用幾個步驟來準備您的環境：

- 請考慮每個 [Azure 登陸區域設計區域](../../ready/landing-zone/design-areas.md) ，以正確地評估您的技術需求。
- 將您的需求與 [Azure 登陸區域的執行選項](../../ready/landing-zone/implementation-options.md) 進行比較，以找出並執行最適合的範本以開始設定。

## <a name="modify-your-environment-to-reflect-your-cloud-mix"></a>修改您的環境以反映您的雲端混合

建立您的 Azure 環境之後，您就可以開始修改它，以支援最適當的雲端混合。 請考慮下列修改：

- 身分 **識別**：哪個雲端將裝載您的主要身分識別提供者？ 如果該提供者在 Azure 之外，您可能需要將您的身分識別提供者與 Azure Active Directory 整合。 如需有關識別提供者的詳細資訊，請參閱 [這篇文章](/azure/active-directory/external-identities/identity-providers)。
- **公用網路連線能力**：最佳作法建議所有輸入和輸出流量都應盡可能在一個雲端平臺之間路由傳送。 但是您的需求或雲端混合可能需要更多的對等模型。 如果您的雲端混合用來滿足冗余和可靠性需求，則這項安排特別常見。 您將如何設定每個雲端平臺與公用網際網路之間的連線？
- **備份和** 復原：客戶通常會在其雲端混合中，將其備份和復原策略集中在最可靠的提供者周圍。 通常會產生一個雲端提供者作為共用的復原中心。 在每個案例中，azure 備份和 Azure Site Recovery 都可提供協助。
- **雲端平臺連線能力**：如果您的雲端平臺將共用常見的復原、作業或治理資源，您可能需要在每個雲端平臺之間進行連線。 您要如何設定每個雲端平臺之間的連線能力？

### <a name="the-most-important-consideration"></a>最重要的考慮

**您是否會獨立操作每個雲端，或透過整合中央操作方法？**

獨立作業可 (TCO) 的擁有權總成本加倍或三倍。 對某些客戶而言，TCO 成本增加可乘以10次。 為了將您的員工成本和需求降至最低，統一的作業方法最適合用於混合式和多重雲端策略的所有雲端混合。

若要深入瞭解如何統一您的雲端作業，請參閱混合式和多重雲端解決方案的 [整合操作](./unified-operations.md)、 [治理](./govern.md)和 [作業管理](./manage.md) 文章。

## <a name="next-steps"></a>下一步

如需雲端採用旅程的詳細指引，請參閱下列文章：

- [混合式和多重雲端遷移](./migrate.md)
- [管理混合式和多重雲端環境](./govern.md)
- [管理混合式和多重雲端環境](./manage.md)

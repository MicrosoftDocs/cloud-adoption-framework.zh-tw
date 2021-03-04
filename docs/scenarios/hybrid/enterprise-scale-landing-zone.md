---
title: Enterprise-Scale 混合式和多重雲端的支援
description: 描述企業規模如何能加速採用 AKS
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 5d3845c27b176d9ca474077e1b7ffa5cc82038e2
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794537"
---
# <a name="enterprise-scale-support-for-hybrid-and-multicloud"></a>混合式和多重雲端的企業級支援
  
企業規模的登陸區域提供特定的架構方法、參考架構和參考，以針對任務關鍵性技術平臺和任何支援的工作負載準備登陸區域。 

企業規模的設計是以混合式和多重雲端為考慮。 支援混合式和多重雲端需要三個簡單的參考架構新增：

- 混合式：新增混合式網路連線能力
- 多重雲端：新增多重雲端網路連線能力
- 整合作業：新增 Azure Arc 以擴充治理和作業支援 

## <a name="prerequisite"></a>必要條件

本文假設企業規模登陸區域已成功執行。 如需有關此必要條件的詳細資訊，請在部署 AKS 建築集之前，先複習企業規模的 [總覽](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/) 和 [實施指引](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/implementation) 。

## <a name="design-guidelines"></a>設計指導方針

推動 Azure 企業規模登陸區域的雲端採用架構設計的關鍵決策指南。 有6個重要的設計區域，可用來檢查及修改您的企業規模登陸區域或任何其他 Azure 登陸區域的執行：

- [身分識別和存取管理](../../ready/enterprise-scale/identity-and-access-management.md)
- [網路拓樸和連線能力](../../ready/enterprise-scale/network-topology-and-connectivity.md)
- [管理與監視](../../ready/enterprise-scale/management-and-monitoring.md)
- [商務持續性和災害復原](../../ready/enterprise-scale/business-continuity-and-disaster-recovery.md)
- [安全性、治理和合規性](../../ready/enterprise-scale/security-governance-and-compliance.md)
- [平台自動化和 DevOps](../../ready/enterprise-scale/platform-automation-and-devops.md)

## <a name="implementation-additions"></a>執行新增

若要擴充企業規模以因應混合式和多重雲端需求，請考慮下列簡單的步驟：

- 混合式：從 [網路拓撲] 和 [連線能力] 設計區域套用相關的設計指引。
    - 或從 VWan 或中樞/輪輻參考實作為開始，開始進行連接考慮。 TODO：加入連結
- 多重雲端：將多重雲端連線新增到任何企業級的執行 TODO：新增連結
- 整合作業：新增 Azure Arc & 將資產從其他雲端平臺上架，以將標準治理和作業工具套用至這些非 Azure 資產。 TODO：新增要管理的連結

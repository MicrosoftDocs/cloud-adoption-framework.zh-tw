---
title: 新式容器採用案例簡介
description: 描述案例
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 1a9cb60e3579589c0d6c6399da0dd7c3f656ebbe
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101801566"
---
# <a name="introduction-to-the-modern-containers-adoption-scenario"></a>新式容器採用案例簡介

當客戶因應更大、更複雜的雲端採用形式時，對雲端的旅程也會變得更複雜。 本文系列結合了準備 Kubernetes 和容器整合到雲端策略所需的技術和非技術性考慮。

此案例著重于兩個目標的結果：

- **容器化解決方案：** 容器會在技術資產與基礎結構之間建立一個抽象層。 組織會在整體策略中包含容器，以減少廠商的鎖定，並讓工作負載更具可攜性。
- **使用 Kubernetes 管理容器：** Kubernetes 提供控制平面來管理和部署容器化應用程式、管理計算密度，以及描述工作負載的高可用性需求。

此文章系列概述如何將容器和容器管理整合到您的策略、規劃、採用和操作雲端。

## <a name="components-of-the-scenario"></a>案例的元件

此案例的設計目的是要引導整個雲端採用生命週期的端對端客戶旅程。 完成旅程需要一些重要的指導方針：

- **雲端採用架構：** 這些文章會逐步解說每個 CAF 方法的最小考慮與實施。 使用這些文章來準備決策者、集中 IT 和雲端中心，以將容器和容器管理作為您的技術策略的一部分。
- **Azure Well-Architected Framework：** 這些文章概述當工作負載需要使用容器或容器管理解決方案（例如 Kubernetes）進行部署時，每個工作負載擁有者應該進行的考慮。
- 參考架構：這些參考解決方案有助於加速使用 Azure Kubernetes Service (AKS) 來部署容器解決方案。
- **精選 Azure 產品：** 深入瞭解在 Azure 中支援您的容器及容器管理原則的產品。
- **Microsoft 學習課程模組：** 享有實行、維護及支援容器與 AKS 解決方案所需的實際操作技能。

## <a name="common-customer-journeys"></a>常見的客戶旅程

**AKS 參考架構：** 左側窗格中所列的參考架構示範如何透過 Azure Kubernetes Service (AKS) ，部署各種經過證實的架構，以管理您的容器和 Kubernetes 平臺。 這些架構是在 Azure 中 Kubernetes 的建議起點。

**將現有的工作負載遷移至 AKS：** Azure 中 AKS 的常見使用案例是將現有的 web 型工作負載直接現代化到容器型或雲端原生解決方案，而不是傳統的遷移工作。 有關 [遷移至容器](./migrate.md) 的文章將示範 Azure 遷移如何加快標準遷移程式中的容器遷移速度。

**集中部署和管理容器：** 左窗格中的第一組文章會提供容器策略集中化的豐富指引。 本文系列旨在協助中央 IT 或優秀團隊的雲端中心瞭解容器如何影響您的雲端策略，以及如何提供一致的集中式支援。

**準備大規模的容器治理和操作：** 企業規模的結構集會示範如何使用企業規模的登陸區域，以確保跨多個登陸區域的一致治理、安全性和作業，大規模地集中管理容器。

**執行特定的 Azure 產品：** 使用精選產品一節中所述的各種 Azure 產品，加速並改進容器和 Kubernetes 功能。

## <a name="next-step-integrate-modern-containers-into-your-cloud-adoption-journey"></a>下一步：將新式容器整合到您的雲端採用旅程

下列文章清單會帶您前往雲端採用旅程中特定點的指導方針，以協助您成功進行雲端採用案例。

- [新式容器的策略](./strategy.md)
- [規劃新式容器](./plan.md)
- [檢查您的環境或 Azure 登陸區域](./ready.md)
- [將工作負載遷移至新式容器](./migrate.md)
- [使用新式容器解決方案進行創新](/azure/architecture/reference-architectures/containers/aks-start-here?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
- [管理新式容器解決方案](./govern.md)
- [管理新式容器解決方案](./manage.md)

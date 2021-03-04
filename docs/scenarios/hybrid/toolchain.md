---
title: Azure 混合式和多重雲端產品簡介
description: 介紹可協助啟用混合式和多重雲端解決方案的 Azure 產品
author: BrianBlanchard
ms.author: brblanch
ms.date: 01/11/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.custom: e2e-hybrid
ms.openlocfilehash: 026170a7d3a70abed6d396eae395bc4c3cd525e4
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794286"
---
# <a name="introduction-to-hybrid-and-multicloud-products-on-azure"></a>Azure 上的混合式和多重雲端產品簡介

依賴有效的多重雲端，multiedge 的混合式方法比以往更重要。 Azure 已經過設計，因為一開始會將焦點放在支援客戶的混合式需求，而在過去幾年來，在 Azure 產品之間進行混合式整合的中心。

由於客戶在採用多個雲端時，越來越複雜，因此有許多 Azure 產品都放寬了該觀點，以支援客戶的內部部署、多重雲端、邊緣和整合作業需求。 混合式整合表示客戶可以一致地建立和部署應用程式和資料庫、順暢地運作，並透過整合治理和管理，在不同的環境之間提供整合的雲端安全性。

本文不會介紹具有混合式和多重雲端功能的所有 Azure 產品，但引進了幾項核心產品，可在您的雲端組合中將這項功能解除鎖定。

請參閱 [azure 混合式和多重雲端中樞](/hybrid/) ，深入瞭解您可以使用 azure 的混合式和多重雲端產品。

![以下列出 Azure 混合式和多重雲端產品的總覽](../../_images/hybrid/hybrid-hero-slide.png)

本文系列有助於將這些工具整合到相關的程式中，從初始商務策略到工作負載優化，一直到您的作業管理週期。

## <a name="manage-hybrid-and-multicloud-environments-with-unified-operations-tools"></a>使用整合的操作工具管理混合式和多重雲端環境

- [Azure Arc](/azure/azure-arc/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 這個以雲端為基礎的服務會將以 Azure Resource Manager 為基礎的管理模型延伸至非 Azure 資源，包括虛擬機器 (vm) 、Kubernetes 叢集和容器化資料庫。
- [啟用 Azure Arc 的伺服器](/azure/azure-arc/servers/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) ：此混合式服務可讓您管理 Windows 和 Linux 機器（裝載于 Azure 外部）、公司網路或其他雲端提供者。 這類似于您管理原生 Azure Vm 的方式。
- [啟用 Azure Arc Kubernetes](/azure/azure-arc/kubernetes/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 此混合式服務可讓您在 Azure 內部或外部簡化 Kubernetes 叢集的部署與管理。
- [啟用 Azure arc 的 Sql server](/sql/sql-server/azure-arc/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 這部分已啟用 azure arc 的伺服器會將 azure 服務延伸至 SQL server 實例，該實例是在客戶的資料中心、邊緣或多重雲端環境中裝載于 azure 外部。
- [具備 Azure Arc 功能的資料服務](/azure/azure-arc/data/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) ，此混合式服務可讓您在內部部署、邊緣和使用 Kubernetes 的基礎結構以及您選擇的基礎結構的公用雲端中執行 azure 資料服務。
- [啟用 Azure arc 的 SQL 受控實例](/azure/azure-arc/data/managed-instance-overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 此 Azure SQL Database 資料服務可在裝載啟用 Azure Arc 之資料服務的基礎結構上建立。

## <a name="deploy-hybrid-and-multicloud-solutions"></a>部署混合式和多重雲端解決方案

- [Azure STACK HCI (20h2) ](/azure-stack/hci/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 這是超融合式基礎結構 (HCI) 叢集解決方案，可在混合式內部部署環境中裝載虛擬化的 Windows 和 Linux 作業系統 (作業系統) 工作負載及其儲存體。 叢集是由二到16個實體節點所組成。
- Azure [STACK hci 上的 Azure Kubernetes Service](/azure-stack/aks-hci/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) ，這是 AKS 的執行，可在 AZURE stack HCI 上自動執行大規模的容器化應用程式。
- [Azure Kubernetes 服務](/azure/aks/intro-kubernetes?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 這項服務可讓您輕鬆地在 Azure 中部署受控 Kubernetes 叢集。
- [Azure IoT Edge](/azure/iot-edge/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 會將雲端式解決方案部署到您本機環境的邊緣，並透過 azure 的完整支援來管理這些裝置及其所產生的 IoT 資料。

## <a name="connect-your-hybrid-and-multicloud-environments"></a>連接您的混合式和多重雲端環境

- [虛擬 WAN](/azure/virtual-wan/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 會連接到安全的全域分支解決方案。
- [ExpressRoute](/azure/expressroute/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 建立與 Microsoft 雲端服務的快速私人連線
- [VPN 閘道](/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 將加密的流量傳送至 Azure
- [Azure 防火牆](/azure/firewall/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 完全具狀態的防火牆即服務，具有內建的高可用性和不受限制的雲端擴充性。

---
title: Azure 混合式和多重雲端產品簡介
description: 介紹可協助啟用混合式和多重雲端解決方案的 Azure 產品。
author: BrianBlanchard
ms.author: brblanch
ms.date: 01/11/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.custom: e2e-hybrid
ms.openlocfilehash: 8d552d0e9e109cbb2d9e98da27d29fc7fe6bdf74
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208892"
---
# <a name="introduction-to-hybrid-and-multicloud-products-on-azure"></a>Azure 上的混合式和多重雲端產品簡介

依賴有效的多重雲端，multiedge 的混合式方法比以往更加重要。 Azure 著重于支援混合式客戶的混合式需求與 Azure 產品之間的混合式整合。

客戶在採用多個雲端時，會變得更精密。 許多 Azure 產品都有放寬了的觀點，可支援客戶的內部部署、多重雲端、邊緣和整合作業需求。 混合式整合表示客戶可以一致地建立和部署應用程式和資料庫、順暢地運作，並透過整合治理和管理，在不同的環境之間提供整合的雲端安全性。

本文介紹一些具有混合和多重雲端功能的 Azure 產品，可在您的雲端組合中提供這項功能。

若要深入瞭解如何使用 Azure 的混合式和多重雲端產品，請參閱 [azure 混合式和多重雲端中樞](/hybrid/)。

![此圖顯示本文所列的 Azure 混合式和多重雲端產品的總覽。](../../_images/hybrid/hybrid-hero-slide.png)

本文系列有助於將這些工具整合至相關的程式，範圍從初始商務策略到工作負載優化，以及長期進入您的作業管理週期。

## <a name="manage-hybrid-and-multicloud-environments-with-unified-operations-tools"></a>使用整合的操作工具管理混合式和多重雲端環境

- [Azure Arc](/azure/azure-arc/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是以雲端為基礎的服務，可將以 Azure Resource Manager 為基礎的管理模型延伸至非 Azure 資源，例如虛擬機器、Kubernetes 叢集和容器化資料庫。
- [啟用 Azure Arc 的伺服器](/azure/azure-arc/servers/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是一種混合式服務，可讓您在公司網路或其他雲端提供者上，用來管理裝載于 Azure 外部的 Windows 和 Linux 機器。 這項功能類似于您管理原生 Azure Vm 的方式。
- [啟用 Azure Arc 的 Kubernetes](/azure/azure-arc/kubernetes/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是一項混合式服務，可讓您用來簡化 azure 內部或外部的 Kubernetes 叢集部署和管理。
- [啟用 Azure arc 的 Sql server](/sql/sql-server/azure-arc/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是已啟用 azure arc 之伺服器的一部分，可將 azure 服務延伸至 SQL Server 實例，該實例會在客戶的資料中心、邊緣或多重雲端環境中託管于 azure 外部。
- [啟用 Azure Arc 的資料服務](/azure/azure-arc/data/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是一種混合式服務，可讓您使用 Kubernetes 和您選擇的基礎結構，在內部部署、邊緣和公用雲端中執行 azure 資料服務。
- [啟用 Azure arc 的 Sql 受控實例](/azure/azure-arc/data/managed-instance-overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是 Azure SQL Database 資料服務，可在您選擇裝載啟用 Azure Arc 之資料服務的基礎結構時建立。

## <a name="deploy-hybrid-and-multicloud-solutions"></a>部署混合式和多重雲端解決方案

- [Azure STACK HCI (20h2) ](/azure-stack/hci/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是一種超融合式基礎結構 (HCI) 叢集解決方案，可在混合式內部部署環境中裝載虛擬化的 Windows 和 Linux 作業系統工作負載及其儲存體。 叢集是由2到16個實體節點所組成。
- Azure [Kubernetes Service (AKS) On Azure STACK hci](/azure-stack/aks-hci/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)是 AKS 的，可在 AZURE stack HCI 上大規模地自動執行容器化應用程式。
- [Azure Kubernetes Service](/azure/aks/intro-kubernetes?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 可讓您輕鬆地在 azure 中部署受控 Kubernetes 叢集。
- [Azure IoT Edge](/azure/iot-edge/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 會將雲端式解決方案部署到您本機環境的邊緣，並完整支援 azure 來管理這些裝置及其所產生的 IoT 資料。

## <a name="connect-your-hybrid-and-multicloud-environments"></a>連接您的混合式和多重雲端環境

- [Azure 虛擬 WAN](/azure/virtual-wan/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 會連接到安全的全域分支解決方案。
- [Azure ExpressRoute](/azure/expressroute/?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 建立與 Microsoft 雲端服務的快速私人連線。
- [AZURE VPN 閘道](/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 會將加密的流量傳送至 azure。
- [Azure 防火牆](/azure/firewall/overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 是完全具狀態的防火牆即服務，具有內建的高可用性和不受限制的雲端擴充性。

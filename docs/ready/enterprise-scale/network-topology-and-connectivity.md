---
title: 網路拓撲和連線能力總覽
description: 檢查與 Microsoft Azure 之間的網路和連線能力相關的主要設計考慮和最佳作法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 7fe77aaade7da6a4eabcc935c0d26678ac07deb4
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112785"
---
<!-- docutune:casing "Azure VPN Gateway" L7 -->
<!-- cSpell:ignore autoregistration BGPs MACsec MPLS MSEE onprem privatelink VPNs -->

# <a name="network-topology-and-connectivity-overview"></a>網路拓撲和連線能力總覽

這一系列的文章會檢查與 Microsoft Azure 之間的網路和連線能力相關的主要設計考慮和最佳作法。

## <a name="plan-for-ip-addressing"></a>規劃 IP 定址

您的組織務必規劃 Azure 中的 IP 位址，以確保在內部部署位置和 Azure 區域之間的 IP 位址空間不會重迭。
[本節提供規劃混合式執行之 IP 位址的指引](../azure-best-practices/plan-for-ip-addressing.md)

## <a name="configure-dns-and-name-resolution-for-on-premises-and-azure-resources"></a>設定內部部署和 Azure 資源的 DNS 和名稱解析

網域名稱系統 (DNS) 是整個企業規模架構中的重要設計主題。 某些組織可能會想要在 DNS 中使用現有的投資。 其他人可能會看到雲端採用將其內部 DNS 基礎結構現代化，並使用原生 Azure 功能的機會。
[本節將探討針對混合式執行規劃 DNS 和名稱解析的指導方針。](../azure-best-practices/dns-for-on-premises-and-azure-resources.md)

## <a name="define-an-azure-network-topology"></a>定義 Azure 網路拓撲

網路拓撲是企業規模架構的重要元素，因為它會定義應用程式彼此通訊的方式。 [本節探討適用于 Azure 部署的技術與拓撲方法。](../azure-best-practices/define-an-azure-network-topology.md) 它著重于兩個核心方法：以 Azure 虛擬 WAN 和傳統拓撲為基礎的拓撲。

## <a name="virtual-wan-network-topology"></a>虛擬 WAN 網路拓撲

[本節將探討用來執行 Azure 虛擬 WAN 網路拓撲的選項。](../azure-best-practices/virtual-wan-network-topology.md)

## <a name="traditional-azure-networking-topology"></a>傳統的 Azure 網路拓撲

[本節將探討用來執行傳統 Azure 網路拓撲的選項。](../azure-best-practices/traditional-azure-networking-topology.md)

## <a name="connectivity-to-azure"></a>Azure 的連線能力

[本節將擴充網路拓撲，以考慮將內部部署位置連線到 Azure 的建議模型。](../azure-best-practices/connectivity-to-azure.md)

## <a name="private-link-and-dns-integration-at-scale"></a>大規模的私人連結和 DNS 整合

[本節說明如何整合適用于 PaaS 服務的 Azure Private Link 與中樞和輪輻網路架構中的 Azure 私人 DNS 區域。](../azure-best-practices/private-link-and-dns-integration-at-scale.md)

## <a name="connectivity-to-azure-paas-services"></a>Azure PaaS 服務的連線能力

本節將探討先前的連線章節， [探討使用 Azure PaaS 服務的建議連線方法。](../azure-best-practices/connectivity-to-azure-paas-services.md)

## <a name="plan-for-inbound-and-outbound-internet-connectivity"></a>規劃輸入和輸出網際網路連線能力

[本節說明與公用網際網路之間的輸入和輸出連線所建議的連線性模型。](../azure-best-practices/plan-for-inbound-and-outbound-internet-connectivity.md)

## <a name="plan-for-application-delivery"></a>規劃應用程式傳遞

[本節將探索主要的建議，以安全、可高度擴充且高可用性的方式，提供面向內部和外部應用程式。](../azure-best-practices/plan-for-app-delivery.md)

## <a name="plan-for-landing-zone-network-segmentation"></a>規劃登陸區域網路分割

[本節將探討在登陸區域內提供高度安全內部網路分割的主要建議，以推動網路的零信任實行。](../azure-best-practices/plan-for-landing-zone-network-segmentation.md)

## <a name="define-network-encryption-requirements"></a>定義網路加密需求

[本節將探討可在內部部署與 Azure 之間，以及在 Azure 區域之間達成網路加密的重要建議。](../azure-best-practices/define-network-encryption-requirements.md)

## <a name="plan-for-traffic-inspection"></a>規劃流量檢查

在許多產業中，組織要求將 Azure 中的流量鏡像至網路封包收集器，以進行深度檢查和分析。 這項需求通常著重于輸入和輸出網際網路流量。 本節[探討在 Azure 虛擬網路內鏡像或點擊流量的重要考慮和建議方法](../azure-best-practices/plan-for-traffic-inspection.md)。

## <a name="connectivity-to-other-cloud-providers"></a>與其他雲端提供者的連線能力

本節[提供將 Azure 企業規模登陸區域架構整合至其他雲端提供者的不同連線方法](../azure-best-practices/connectivity-to-other-providers.md)。

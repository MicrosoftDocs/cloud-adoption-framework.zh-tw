---
title: 安全性基準範例原則聲明
description: 請參閱這些範例安全性基準原則聲明，以協助草擬原則聲明以滿足您組織的需求。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 02a1c18a73b784cb9245ed7c83d86c21c0690148
ms.sourcegitcommit: af45c1c027d7246d1a6e4ec248406fb9a8752fb5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "77707202"
---
# <a name="security-baseline-sample-policy-statements"></a>安全性基準範例原則聲明

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求的清楚摘要說明。
- **技術選項：** IT 小組與開發人員可以在執行原則時使用的可操作建議、規格或其他指引。

下列範例原則聲明會解決與安全性相關的常見業務風險。 這些語句是您在草擬原則聲明以滿足組織需求時可以參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項來處理每個已識別的風險。 與商務、安全性和 IT 小組密切合作，為您唯一的風險集合找出最佳的原則。

## <a name="asset-classification"></a>資產分類

**技術風險：** 未正確識別為要徑任務或涉及敏感性資料的資產可能無法獲得足夠的保護，因而導致潛在的資料流程失或業務中斷。

**原則聲明：** 所有已部署的資產都必須依據重要性和資料分類來分類。 部署至雲端之前，必須先由雲端治理小組和應用程式擁有者審查分類。

**潛在的設計選項：** 建立[資源標記標準](../../decision-guides/resource-tagging/index.md)，並確保 IT 人員可以使用[Azure 資源標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)，一致地將其套用到任何已部署的資源。

## <a name="data-encryption"></a>資料加密

**技術風險：** 儲存期間會公開受保護資料的風險。

**原則聲明：** 所有受保護的資料都必須在待用時加密。

**潛在的設計選項：** 如需有關如何在 Azure 平臺上執行靜態資料加密的討論，請參閱[azure 加密總覽](https://docs.microsoft.com/azure/security/security-azure-encryption-overview)一文。 也應該考慮其他控制項，例如在帳戶資料加密中，以及如何變更儲存體帳戶設定的控制。

## <a name="network-isolation"></a>網路隔離

**技術風險：** 網路內的網路與子網之間的連線會引進潛在的弱點，而造成任務關鍵性服務的資料流程失或中斷。

**原則聲明：** 包含受保護資料的網路子網必須與其他任何子網隔離。 受保護資料子網路之間的網路流量必須定期稽核。

**潛在的設計選項：** 在 Azure 中，網路和子網的隔離是透過[Azure 虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)來管理。

## <a name="secure-external-access"></a>保護外部存取

**技術風險：** 允許從公用網際網路存取工作負載，會造成入侵的風險，因而導致未經授權的資料暴露或業務中斷。

**原則聲明：** 沒有包含受保護資料的子網可以透過公用網際網路或跨資料中心直接存取。 這些子網路的存取必須透過中繼子網路進行路由。 這些子網路的所有存取都必須經過防火牆解決方案，該解決方案可以執行封包掃描和封鎖功能。

**潛在的設計選項：** 在 Azure 中，藉由在[公用網際網路和雲端式網路之間部署 DMZ](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)來保護公用端點。 請考慮[Azure 防火牆](https://docs.microsoft.com/azure/firewall)的部署、設定和自動化。

## <a name="ddos-protection"></a>DDoS 保護

**技術風險：** 分散式阻斷服務（DDoS）攻擊可能會導致業務中斷。

**原則聲明：** 將自動化 DDoS 風險降低機制部署到所有可公開存取的網路端點。 沒有任何 DDoS，IaaS 支援的公眾面向網站不應公開至網際網路。

**潛在的設計選項：** 使用[Azure DDoS 保護](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)標準，將 DDoS 攻擊所造成的中斷降至最低。

## <a name="secure-on-premises-connectivity"></a>保護內部部署連線

**技術風險：** 您的雲端網路與內部部署之間透過公用網際網路的未加密流量很容易遭到攔截，引進了資料暴露的風險。

**原則聲明：** 內部部署與雲端網路之間的所有連線，都必須透過安全的加密 VPN 連線或專用的私人 WAN 連結進行。

**潛在的設計選項：** 在 Azure 中，使用 ExpressRoute 或 Azure VPN 來建立內部部署與雲端網路之間的私人連線。

## <a name="network-monitoring-and-enforcement"></a>網路監視和強制執行

**技術風險：** 網路設定的變更可能會導致新的弱點和資料暴露風險。

**原則聲明：** 治理工具必須審查並強制執行安全性基準小組所定義的網路設定需求。

**潛在的設計選項：** 在 Azure 中，您可以使用[azure 網路監看員](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)來監視網路活動， [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)有助於識別安全性弱點。 Azure 原則可讓您根據安全性小組定義的限制，限制網路資源和資源設定原則。

## <a name="security-review"></a>安全性檢閱

**技術風險：** 經過一段時間，新的安全性威脅和攻擊類型會出現，因而增加了您的雲端資源暴露或中斷的風險。

**原則聲明：** 可能影響雲端部署的趨勢和潛在入侵，應定期由安全性小組審查，以提供雲端中使用的安全性基準工具的更新。

**潛在的設計選項：** 建立週期性安全性審查會議，其中包括相關 IT 與治理小組成員。 請參閱現有的安全性資料和計量，以在目前的原則和安全性基準工具中建立間距，並更新原則以補救任何新的風險。 利用[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview)和[Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-intro)，針對您的部署特定的新興威脅，取得可操作的深入解析。

## <a name="next-steps"></a>後續步驟

使用本文提及的範例作為起點，以開發與您雲端採用方案保持一致的原則來解決特定的安全性風險。

若要開始自行開發與安全性基準相關的自訂原則聲明，請下載[安全性基準範本](./template.md)。

若要加速採用這個專業領域，請選擇最符合您環境的可採取動作的[治理指南](../guides/index.md)。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度，建立治理和溝通安全性基準原則遵循情況的流程。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)

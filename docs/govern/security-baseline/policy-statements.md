---
title: 安全性基準範例原則聲明
description: 請參閱這些範例安全性基準原則聲明，以協助您解決組織需求的草稿原則聲明。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 06ba3164410c5828da7dd97a999a0c152580a910
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101792375"
---
# <a name="security-baseline-sample-policy-statements"></a>安全性基準範例原則聲明

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求的清楚摘要說明。
- **技術選項：** 可操作的建議、規格或其他指引，可供 IT 小組和開發人員在執行原則時使用。

下列範例原則聲明會解決常見的安全性相關商務風險。 這些語句是在草擬原則聲明以解決組織需求時可參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項可處理每個已識別的風險。 與商務、安全性和 IT 小組密切合作，為您的唯一風險組合找出最佳原則。

## <a name="asset-classification"></a>資產分類

**技術風險：** 未正確識別為任務關鍵性或涉及機密資料的資產，可能無法獲得足夠的保護，導致潛在的資料流程失或業務中斷。

**原則聲明：** 所有已部署的資產都必須依照重要性和資料分類進行分類。 部署至雲端之前，雲端治理小組和應用程式擁有者必須先審核分類。

**可能的設計選項：** 建立 [資源標記標準](../../decision-guides/resource-tagging/index.md) ，並確保 IT 人員能以一致的方式，使用 [Azure 資源標記](/azure/azure-resource-manager/management/tag-resources)將它們套用至任何已部署的資源。

## <a name="data-encryption"></a>資料加密

**技術風險：** 在儲存體期間公開受保護資料的風險。

**原則聲明：** 所有受保護的資料在待用時都必須加密。

**可能的設計選項：** 如需有關如何在 Azure 平臺上執行待用資料加密的討論，請參閱 [azure 加密簡介](/azure/security/fundamentals/encryption-overview) 文章。 您也應該考慮其他控制項（例如帳戶資料加密），以及如何變更儲存體帳戶設定的控制權。

## <a name="network-isolation"></a>網路隔離

**技術風險：** 網路內的網路與子網之間的連線，會產生可能導致資料遺失或任務關鍵性服務中斷的潛在弱點。

**原則聲明：** 包含受保護資料的網路子網必須與任何其他子網隔離。 受保護資料子網路之間的網路流量必須定期稽核。

**可能的設計選項：** 在 Azure 中，網路和子網隔離是透過 [Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview)來管理。

## <a name="secure-external-access"></a>保護外部存取

**技術風險：** 允許從公用網際網路存取工作負載會造成入侵的風險，進而導致未經授權的資料暴露或業務中斷。

**原則聲明：** 您可以透過公用網際網路或跨資料中心直接存取包含受保護資料的子網。 這些子網路的存取必須透過中繼子網路進行路由。 這些子網路的所有存取都必須經過防火牆解決方案，該解決方案可以執行封包掃描和封鎖功能。

**可能的設計選項：** 在 Azure 中，藉由在 [公用網際網路與您的雲端式網路之間部署周邊網路](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)來保護公用端點。 請考慮部署、設定和自動化 [Azure 防火牆](/azure/firewall/overview)。

## <a name="ddos-protection"></a>DDoS 保護

**技術風險：** 分散式阻斷服務 (DDoS) 攻擊可能會導致業務中斷。

**原則聲明：** 將自動化的 DDoS 風險降低機制部署到所有可公開存取的網路端點。 不需要 DDoS 就能向網際網路公開不受 IaaS 支援的公眾對應網站。

**可能的設計選項：** 使用 [Azure DDoS 保護標準](/azure/ddos-protection/ddos-protection-overview) ，將 DDoS 攻擊所造成的中斷情況降到最低。

## <a name="secure-on-premises-connectivity"></a>保護內部部署連線

**技術風險：** 您的雲端網路與內部部署之間透過公用網際網路的未加密流量很容易被攔截，因而引進了資料暴露的風險。

**原則聲明：** 內部部署與雲端網路之間的所有連線都必須透過安全的加密 VPN 連線或專用的私人 WAN 連結進行。

**可能的設計選項：** 在 Azure 中，使用 ExpressRoute 或 Azure VPN 來建立您的內部部署與雲端網路之間的私人連線。

## <a name="network-monitoring-and-enforcement"></a>網路監視和強制執行

**技術風險：** 網路設定的變更可能會導致新的弱點和資料暴露風險。

**原則聲明：** 治理工具必須先審核並強制執行安全性基準小組所定義的網路設定需求。

**可能的設計選項：** 在 Azure 中，您可以使用 [Azure 網路](/azure/network-watcher/network-watcher-monitoring-overview)監看員來監視網路活動， [Azure 資訊安全中心](/azure/security-center/security-center-network-recommendations) 可協助找出安全性弱點。 Azure 原則可讓您根據安全性小組定義的限制，限制網路資源和資源設定原則。

## <a name="security-review"></a>安全性檢閱

**技術風險：** 經過一段時間，新的安全性威脅和攻擊類型便會出現，從而提高雲端資源暴露或中斷的風險。

**原則聲明：** 安全性小組應定期檢查可能影響雲端部署的趨勢和潛在攻擊，以提供雲端中所使用之安全性基準工具的更新。

**可能的設計選項：** 建立包含相關 IT 和治理小組成員的一般安全性審核會議。 請檢查現有的安全性資料和計量，以建立目前原則和安全性基準工具的差距，並更新原則來補救任何新風險。 使用 [Azure Advisor](/azure/advisor/advisor-overview) 和 [Azure 資訊安全中心](/azure/security-center/security-center-introduction) 來取得可操作的深入解析，以瞭解您的部署特定的新興威脅。

## <a name="next-steps"></a>下一步

使用本文提及的範例作為起點，以開發與您雲端採用方案保持一致的原則來解決特定的安全性風險。

若要開始開發您自己的自訂安全性基準原則聲明，請下載 [安全性基準專業版範本](./template.md)。

若要加速採用此專業領域，請選擇最符合您環境的可 [操作治理指南](../guides/index.md) 。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度，建立治理和溝通安全性基準原則遵循情況的流程。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)

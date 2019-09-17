---
title: 安全性基準範例原則聲明
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 安全性基準範例原則聲明
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 253646b16d98a35c8cae8eb7f5c57aa60d55d580
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71030265"
---
# <a name="security-baseline-sample-policy-statements"></a>安全性基準範例原則聲明

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 明確地摘要說明原則需求。
- **技術選項：** IT 小組與開發人員可以在實作原則時使用之可操作的建議、規格或其他指引。

下列範例原則聲明會解決與安全性相關的常見業務風險。 這些語句是您在草擬原則聲明以滿足組織需求時可以參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項來處理每個已識別的風險。 與商務、安全性和 IT 小組密切合作，為您唯一的風險集合找出最佳的原則。

## <a name="asset-classification"></a>資產分類

**技術風險：** 不正確地識別為任務關鍵性或是涉及敏感性資料的資產，可能不會收到足夠的保護，導致潛在的資料外洩或業務中斷。

**原則聲明：** 所有已部署的資產都必須依據嚴重性和資料分類來分類。 部署至雲端之前，必須先由雲端治理小組和應用程式擁有者審查分類。

**潛在的設計選項：** 建立[資源標記標準](../../decision-guides/resource-tagging/index.md)，並確保 IT 人員使用 [Azure 資源標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)，一致地將其套用至任何部署的資源。

## <a name="data-encryption"></a>資料加密

**技術風險：** 儲存期間公開的受保護資料有風險。

**原則聲明：** 所有受保護的資料在待用時都必須經過加密。

**潛在的設計選項：** 請參閱 [Azure 加密概觀](https://docs.microsoft.com/azure/security/security-azure-encryption-overview)一文，以取得如何在 Azure 平台上執行待用資料加密的討論。

## <a name="network-isolation"></a>網路隔離

**技術風險：** 網路內網路與子網路之間的連線會引入潛在的弱點，導致資料外洩或任務關鍵性服務中斷。

**原則聲明：** 包含受保護資料的網路子網路必須與其他任何子網路隔離。 受保護資料子網路之間的網路流量必須定期稽核。

**潛在的設計選項：** 在 Azure 中，網路和子網路隔離是透過 [Azure 虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)進行管理。

## <a name="secure-external-access"></a>保護外部存取

**技術風險：** 允許從公用網際網路存取工作負載，會引入入侵風險，導致未獲授權資料曝光或業務中斷。

**原則聲明：** 沒有包含受保護資料的任何子網路可以透過公用網際網路或跨資料中心直接存取。 這些子網路的存取必須透過中繼子網路進行路由。 這些子網路的所有存取都必須經過防火牆解決方案，該解決方案可以執行封包掃描和封鎖功能。

**潛在的設計選項：** 在 Azure 中，請藉由部署[公用網際網路與雲端型網路之間的 DMZ](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)，來保護公用端點。

## <a name="ddos-protection"></a>DDoS 保護

**技術風險：** 分散式阻斷服務 (DDoS) 攻擊會導致業務中斷。

**原則聲明：** 將自動化 DDoS 風險降低機制部署至所有可公開存取的網路端點。

**潛在的設計選項：** 使用 [Azure DDoS 保護](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)將 DDoS 攻擊所造成的中斷情況降到最低。

## <a name="secure-on-premises-connectivity"></a>保護內部部署連線

**技術風險：** 您的雲端網路與內部部署之間的未加密流量 (透過網際網路) 容易遭到攔截，引入資料曝光風險。

**原則聲明：** 內部部署與雲端網路之間的所有連線必須透過安全加密 VPN 連線或專用私人 WAN 連結進行。

**潛在的設計選項：** 在 Azure 中，使用 ExpressRoute 或 Azure VPN 來建立您的內部部署與雲端網路之間的私人連線。

## <a name="network-monitoring-and-enforcement"></a>網路監視和強制執行

**技術風險：** 變更網路設定會導致新的弱點和資料曝光風險。

**原則聲明：** 治理工具必須稽核並強制執行安全性基準小組所定義的網路設定需求。

**潛在的設計選項：** 在 Azure 中，網路活動可以使用 [Azure 網路監看員](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)來監視，而 [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)可協助識別安全性弱點。 Azure 原則可讓您根據安全性小組定義的限制，限制網路資源和資源設定原則。

## <a name="security-review"></a>安全性檢閱

**技術風險：** 經過一段時間，會出現新的安全性威脅和攻擊類型，增加您的雲端資源曝光或中斷的風險。

**原則聲明：** 安全性小組應定期檢閱可能影響雲端部署的趨勢與潛在攻擊，以更新雲端中使用的安全性基準工具。

**潛在的設計選項：** 建立定期安全性檢閱會議，包括相關 IT 和治理小組成員。 請參閱現有的安全性資料和計量，以在目前的原則和安全性基準工具中建立間距，並更新原則以補救任何新的風險。

## <a name="next-steps"></a>後續步驟

使用本文提及的範例作為起點，以開發與您雲端採用方案保持一致的原則來解決特定的安全性風險。

若要開始自行開發與安全性基準相關的自訂原則聲明，請下載[安全性基準範本](./template.md)。

若要加速採用這個專業領域，請選擇最符合您環境的可採取動作的[治理指南](../guides/index.md)。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度，建立治理和溝通安全性基準原則遵循情況的流程。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)

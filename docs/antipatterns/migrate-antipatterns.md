---
title: 雲端遷移反模式
description: 在選擇架構之前，請避免反模式、建立安全性與合規性護欄、瞭解相依性，以及執行完整評量。
author: lpassig
ms.author: brblanch
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: aef255bf7aba6155942b58e6fa791151e433d532
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117276"
---
# <a name="cloud-migration-antipatterns"></a>雲端遷移反模式

客戶通常會在雲端採用的遷移階段遇到反模式。 下列量值有助於避免遷移反模式：

- 確定安全性與合規性護欄都已就緒。
- 瞭解潛在的應用程式和伺服器相依性。
- 根據徹底的評量選擇架構。

## <a name="antipattern-migrate-modernize-or-innovate-without-guardrails"></a>反模式：不護欄來遷移、現代化或創新

當客戶將其第一個工作負載部署到雲端時，會將其視為測試創新解決方案的平臺。 他們享有雲端中可用的彈性。 但是，只要這些工作負載變得生產力、需要保存公司資料，或需要存取公司系統，進度會變慢，因為他們需要遵守合規性、法規和安全性標準。

### <a name="example-omit-security-guardrails"></a>範例：省略安全性護欄

公司想要將其線上商店現代化，以改善其使用者體驗。 您應將線上商店網站和基礎清查資料庫移至 Azure，以進行現代化。 因為清查資料庫與公司的 SAP 系統之間存在相依性，所以這些系統必須進行通訊。 因此，公司需要建立混合式雲端。

線上店麵團隊是創新的，因此它會開始現代化應用程式，但由於混合式連線，因此不會考慮安全性需求。 當它測試應用程式時，它發現 IT 安全性小組不允許在 Azure 和內部部署系統內進行通訊，因為不符合安全性和合規性需求。

### <a name="preferred-outcome-establish-security-and-compliance-guardrails"></a>慣用結果：建立安全性與合規性護欄

將工作負載移至雲端之前，請先將安全性和合規性護欄放在原處。 這些護欄可確保工作負載符合安全性和合規性需求。 讓雲端治理和雲端安全性小組在 [Azure 登陸區域](/azure/cloud-adoption-framework/ready/landing-zone/)內提供護欄。 請檢查護欄，特別是針對混合式工作負載。 請參閱 [雲端採用架構的企業規模登陸區域架構](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture) ，以取得可支援工作負載小組的護欄，使其能以快速、一致、符合規範且安全的方式運作。

## <a name="antipattern-migrate-modernize-or-innovate-without-an-assessment"></a>反模式：不需評量即可遷移、現代化或創新

當公司考慮遷移或現代化專案時，它需要瞭解潛在的應用程式和伺服器相依性，以便更精確地進行規劃。 在應用程式創新案例中，公司使用架構設計會話和參考架構，而不是 aimless 工程工作，來體驗更多的成就。

### <a name="example-cause-downtime-by-migrating-without-planning-thoroughly"></a>範例：在未進行完整規劃的情況下遷移，導致停機時間

小組成員計畫將應用程式遷移至雲端，以降低公司的碳足跡。 遷移計畫（識別要遷移的第一個資產）是根據設定管理資料庫 (CMDB) 專案和單一應用程式擁有者訪談。 當小組成員遷移其中一個應用程式的資料庫伺服器之後，其他幾個應用程式擁有者就會呼叫它，以抱怨他們的應用程式無法正常運作。 CMDB 中所描述的相依性不再是正確的，導致其他應用程式發生非預期的停機。

### <a name="preferred-outcome-assess-infrastructure-before-migrating-or-modernizing"></a>慣用結果：在遷移或現代化前評估基礎結構

針對大規模的遷移或現代化專案，請在開始遷移之前執行基礎結構評定。 這項評估可協助您找出相依性和相容性問題。 如需適用于[azure 的 Microsoft 雲端採用架構](/azure/cloud-adoption-framework/overview)提供的[最佳做法](/azure/cloud-adoption-framework/migrate/azure-best-practices/)詳細資訊，請參閱[azure 遷移指南](/azure/cloud-adoption-framework/migrate/azure-migration-guide/)。

在現代化專案中，請使用其他應用程式評量來識別編碼反模式、相容性問題和技術債務。 如需現代化層面的詳細資訊，請參閱 [Azure 應用程式遷移範例的總覽](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-overview)。

針對創新專案，請參閱 [Azure 創新解決方案指南](/azure/cloud-adoption-framework/innovate/innovation-guide/) ，以取得協助來識別規劃和開發創新雲端解決方案的正確方式。

使用 [ (ADS) 的架構設計會話 ](/azure/architecture/serverless-quest/ads) ，協助您設計、建立及部署高品質、穩固的架構，以在企業內調整規模。 使用 ADS 白板來探索、構想和規劃解決方案。

## <a name="antipattern-dictate-an-architecture"></a>反模式：聽寫架構

在雲端中進行開發時，公司可能會採用微服務優先的策略，假設微服務架構永遠優於傳統的整合型架構。 如果公司沒有針對其應用程式執行適當的應用程式評定和到期的工作，此策略可能會失敗。 其他架構方法可能更適合應用程式。 選擇或聽寫微服務架構或適用于所有情況的架構，通常會導致失敗的專案。

### <a name="example-use-a-microservice-architecture-for-all-apps"></a>範例：將微服務架構用於所有應用程式

公司的首席資訊長 (CIO) 在雲端中建立新的應用程式時，會建立使用微服務架構的原則。 公司開發人員從未使用過微服務架構。 他們需要開發簡單的 web 應用程式。 在處理應用程式幾個月之後，開發人員發現他們可能已完成開發，如果它們已開始使用整合型架構。 公司尚未達到更快的上市時間，還有其他優點。

### <a name="preferred-outcome-base-architectural-decisions-on-assessments"></a>慣用結果：評定的基礎架構決策

與其根據特定架構樣式進行 fixating，請根據使用案例或架構的評量和到期時間來制定架構決策。 請勿限制可使用的架構，因為自由選擇是雲端的其中一項主要優點。 因為它的反模式方式是要避免的，因此只挑選架構。 如需詳細資訊，請參閱 [Azure 應用程式架構指南](/azure/architecture/guide) 和 [雲端設計模式](/azure/architecture/patterns)。

## <a name="antipattern-use-a-single-subscription"></a>反模式：使用單一訂用帳戶

公司通常決定只使用一個訂用帳戶來裝載其所有工作負載。 它們通常會在執行需要速度更快的快速遷移時進行這項選擇。 這種決策會導致設計不佳的環境。 這些公司可以快速地遇到訂用帳戶限制，這表示他們需要重新設計架構。

### <a name="example-migrate-under-one-subscription"></a>範例：在一個訂用帳戶下遷移

集團決定將旅館部門旋轉為不同的公司。 旅館部門必須將其 IT 資產移動或遷移至新的位置。 新旅館公司選擇雲端優先的方法，並將所有 IT 資產遷移至雲端。 由於有時間限制，新的公司會將所有專案遷移至一個訂用帳戶，並使用龐大的虛擬網路，在此可以正確地分隔職責和安全性模型。 在完成微調之後的三個月後，旅館公司會判斷其資產的安全性與管理方式不受保護，而且會在訂用帳戶限制內運作。

### <a name="preferred-outcome-use-a-segmentation-strategy"></a>慣用結果：使用分割策略

在遷移至 Azure 之前，請先分隔不同的職責，並規劃不同的環境。 當您將不同的階段合併成一個訂用帳戶時，可以快速觸及訂用帳戶限制。 建立 [分割策略](/azure/architecture/framework/security/design-segmentation) ，讓您更輕鬆地 [實行治理和合規性](../ready/enterprise-scale/management-group-and-subscription-organization.md)。

## <a name="next-steps"></a>下一步

- [Azure 移轉指南概觀](/azure/cloud-adoption-framework/migrate/azure-migration-guide/)
- [Azure 雲端移轉最佳做法檢查清單](/azure/cloud-adoption-framework/migrate/azure-best-practices/)
- [管理群組和訂用帳戶組織](../ready/enterprise-scale/management-group-and-subscription-organization.md)

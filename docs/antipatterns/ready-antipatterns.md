---
title: 雲端就緒反模式
description: 使用預覽服務（假設內建的復原和可用性），並假設該服務已準備好供雲端使用，以避免雲端採用的就緒反模式，例如使用預覽服務。
author: lpassig
ms.author: brblanch
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 70f6676af37a7e2fcacc42a415dbacdc3ec1818c
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117267"
---
# <a name="cloud-readiness-antipatterns"></a>雲端就緒反模式

客戶通常會在雲端採用的準備階段期間遇到反模式。 這些反模式可能會導致非預期的停機時間、嚴重損壞修復問題和可用性問題。

## <a name="antipattern-assume-released-services-are-ready-for-production"></a>反模式：假設發行的服務已準備好用於生產環境

由於雲端運算的發展迅速，公司通常會發行新服務的預覽版本。 客戶通常會假設他們可以在生產環境中使用任何可用的雲端服務。 但是，問題可能是因為下列原因所造成：

- 預覽服務通常不會提供執行時間服務等級協定 (Sla) 。
- 新服務通常不會像已經可用的雲端服務一樣成熟。

### <a name="example-use-a-preview-service-in-production"></a>範例：在生產環境中使用預覽服務

研究機構在生產環境中使用預覽版雲端服務。 服務似乎非常適合其使用案例。 但是，此局不會在服務上執行預期的審慎。 此局也不會遵循其參考架構的需求與指導方針。

問題出在導致非預期停機的預覽服務。 此局開始認為雲端服務在一般情況中不會像承諾一樣成熟或復原。

### <a name="preferred-outcome-use-pre-approved-cloud-services-in-production"></a>慣用結果：在生產環境中使用預先核准的雲端服務

當您評估目前處於預覽狀態的新服務時，請只在概念證明 (PoC) 案例中使用這些服務。 請勿在生產環境中使用這些服務，因為它們沒有 Sla。 在核准雲端服務時，尋找功能與成熟度之間的適當平衡。 請參閱 [雲端服務的審查檢查清單](https://www.microsoft.com/trust-center/compliance/due-diligence-checklist) ，以取得已建立的架構，讓您可以用來快速評估雲端服務。

## <a name="antipattern-assume-increased-resiliency-and-availability"></a>反模式：假設提高的復原能力和可用性

雲端運算通常可提供優於內部部署運算的優點。 範例包括：

- 提高復原能力：在失敗之後復原。
- 可用性：在狀況良好的狀態下執行，而不會有顯著的停機時間。

因為大部分的雲端服務都提供這些優點，所以許多公司都假設所有雲端服務預設都提供復原和高可用性。 在現實情況下，這些功能通常只會以額外的成本提供，並具有額外的技術工作。

### <a name="example-assume-high-availability"></a>範例：假設高可用性

啟動程式可在基礎結構即服務 (IaaS) 服務上執行任務關鍵性應用程式。 啟動時，開發人員已查看虛擬機器 (VM) ，其執行時間 SLA 為99.9%。 因為他們想要降低成本，所以會使用單一 VM 和 premium 儲存體。

當 VM 失敗時，其應用程式無法復原。 非預期的停機結果。 他們會假設雲端預設會提供高可用性。 他們並不知道效能保證在下列兩者之間可能有所差異：

- 服務模型（例如平臺即服務） (PaaS) 和軟體即服務 (SaaS) 。
- 技術架構，例如負載平衡的可用性設定組和可用性區域。

### <a name="preferred-outcome-reduce-failures-while-balancing-resiliency-and-costs"></a>慣用結果：在平衡復原和成本時減少失敗

請參閱受信任、成熟的資源，以取得可減少失敗範圍之架構最佳作法的相關資訊：

- [參考架構](/azure/architecture/reference-architectures)
- [Microsoft Azure Well-Architected Framework](/azure/architecture/framework/)

識別成本與功能（例如 [高復原和可用性](/azure/architecture/framework/resiliency/overview)）之間的適當平衡。 提高復原能力和可用性通常會導致成本增加。 例如：

- 單一 VM 可能會有保證正常運作時間99.9% 的 SLA。
- 兩部執行相同工作負載的 Vm 會提供99.95 –99.99% 執行時間的 SLA。

在設計雲端式解決方案時，參與 *需求設計* 的基本流程。 使用 [SLA 估算器](https://github.com/mspnp/samples/tree/master/Reliability/SLAEstimator) 來協助計算應用程式的端對端 SLA。

## <a name="antipattern-become-a-cloud-provider"></a>反模式：成為雲端提供者

有些公司嘗試讓其內部 IT 部門成為雲端提供者。 然後會負責參考架構。 它也需要提供 IaaS 和 PaaS 給業務單位。 因為這種類型的工作不是 IT 核心業務的一部分，所以產生的服務供應專案可能缺乏可用性、復原能力、效率和安全性。

### <a name="example-provide-monolithic-managed-cloud-services"></a>範例：提供整合式受控雲端服務

公司的 IT 部門建立了卓越的雲端 (CCoE) ，作為 IT 與業務單位之間的仲介。 為了確保 corporation 符合雲端規範，管理面板會將提供整合式端對端服務的工作指派給 CCoE。 CCoE 會設定內部雲端採購入口網站，讓業務單位可用來訂購完全受控的雲端 VM 即服務。 但是，它會控制誰可以存取和使用整個平臺。 如此一來，它會主動防止營業單位利用 Azure 所提供的完整服務範圍。 業務單位無法存取雲端入口網站。 他們只會透過安全的 shell (SSH) 和遠端桌面通訊協定來取得存取權， (RDP) 給他們所訂購的伺服器。

基於許多原因，CCoE 會在提供整合式服務以包裝雲端中可用的每項服務時遇到問題：

- 雲端跨多個解決方案區域提供大量的服務。 相較于開發 IaaS 解決方案，設計和工程物聯網 (IoT) 和人工智慧 (AI) 解決方案需要不同的專業知識和技能集。
- 雲端服務經常變更。
- 嘗試提供整合型服務，可讓 IT 管理程式，而不是業務單位，以大幅增加市場的上市時間。

### <a name="preferred-outcome-provide-guardrails"></a>慣用結果：提供護欄

採用雲端技術時，讓 IT 部門從 IT 工作負載開始獲得雲端的第一手體驗。 使用 [適用于 Azure 的 Microsoft 雲端採用架構](/azure/cloud-adoption-framework) 來識別您的 [第一個採用專案](../strategy/first-adoption-project.md)。

使用成熟的 [雲端作業模型](../operating-model/compare.md) ，例如 [集中式作業](../operating-model/compare.md#centralized-operations) ，讓 IT 負責定義平臺護欄，例如治理。 然後，業務單位可以透過安全且一致的方式，在其定義的護欄內採用雲端專案。

請考慮在一開始就只採用一個主要公用雲端提供者，因為在安裝、管理和使用上，所有主要的平臺都有很大的差異。

盡可能針對 IT 工具使用 SaaS 解決方案，例如：

- 程式碼存放庫。
- 持續整合和持續傳遞 (CI/CD) 。
- 協同作業系統。

針對雲端工作負載，建議使用可大規模安全且安全地運作的熟悉程式。

## <a name="next-steps"></a>下一步

- [可靠性要件簡介](/azure/architecture/framework/resiliency/overview)
- [第一個採用專案](../strategy/first-adoption-project.md)
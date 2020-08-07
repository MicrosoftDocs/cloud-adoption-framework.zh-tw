---
title: Azure 區域決策指南
description: 瞭解雲端平臺區域，以及可能影響您的 Azure 區域選取專案的因素和特性。
author: doodlemania2
ms.author: dermar
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: 5a7f7b3422b57ab2786f03b5c0b50d9e171a7107
ms.sourcegitcommit: 452e09b543e7b84f943db5b02480ba2d18afd939
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87866174"
---
# <a name="azure-regions-decision-guide"></a>Azure 區域決策指南

Azure 包含世界各地的許多區域。 每個 [Azure 區域](https://azure.microsoft.com/global-infrastructure/regions)都有一組特定的特性，因此要選擇哪個區域來使用就變得格外重要。 其中包括可用的服務、容量、條件約束和主權：

- **可用的服務：** 根據各種因素而定，部署到每個區域的服務會有所不同。 選取含有您所需服務的區域，以將工作負載部署至其中。 如需詳細資訊，請參閱[不同區域提供的產品](https://azure.microsoft.com/global-infrastructure/services)。
- **容量：** 每個區域都有最大容量。 這可能會影響哪些類型的訂用帳戶可以部署哪些類型的服務，以及在何種情況下。 這與訂用帳戶配額不同。 如果您打算將大規模的資料中心遷移至 Azure，您可能會想要洽詢您當地的 Azure 欄位小組或帳戶管理員，以確認您可以視需要進行部署。
- **條件約束：** 某些條件約束會放在特定區域的服務部署上。 例如，某些區域只能作為備份或容錯移轉目標。 其他值得注意的條件約束是[資料主權需求](https://azure.microsoft.com/global-infrastructure/geographies)。
- **主權：** 某些地區專屬於特定的主權實體。 雖然所有區域都是 Azure 區域，但這些主權區域會與 Azure 的其餘部分完全隔離。 這些不一定是由 Microsoft 管理，而且可能僅限於特定類型的客戶。 這些主權區域包括：
    - [Azure China](https://azure.microsoft.com/global-infrastructure/china)
    - [Azure 德國](https://azure.microsoft.com/global-infrastructure/germany)： azure 德國已淘汰，並會改用德國的標準 nonsovereign azure 區域。
    - [Azure 美國政府](https://azure.microsoft.com/global-infrastructure/government)
    - [澳大利亞](https://azure.microsoft.com/global-infrastructure/australia)的兩個區域是由 Microsoft 所管理，但提供給澳大利亞政府及其客戶和承包商使用。 因此，這些區域會攜帶類似于其他主權雲端的用戶端條件約束。

## <a name="operate-in-multiple-geographic-regions"></a>在多個地理區域營運

當企業在多個地理區域中運作（這可能是復原的必要項）時，這會導致下列形式的潛在複雜度：

- 資產散發
- 使用者存取設定檔
- 合規性需求
- 區域復原

區域選擇對於整體雲端採用策略而言非常重要。 讓我們從網路考量來開始。

## <a name="network-considerations"></a>網路考量

想要有健全的雲端部署，就需要有經過深思熟慮、已將 Azure 區域納入考量的網路。 您應該考慮下列事項：

- Azure 區域會成對部署。 發生嚴重的區域失敗時，會將相同地緣政治界限內的另一個區域指定為其配對的區域。 請考慮將部署到配對的區域，做為主要和次要復原策略。 這項策略的一個例外狀況是 `Brazil South` ，其與配對 `South Central US` 。 如需詳細資訊，請參閱 [Azure 配對區域](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)。

  - Azure 儲存體支援[異地冗余儲存體 (GRS) ](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)。 這表示您的資料有三個複本會儲存在您的主要區域中，而三個額外的複本則會儲存在配對的區域中。 您無法變更 GRS 的存放裝置配對。
  - 依賴 Azure 儲存體 GRS 的服務可以利用此配對區域功能。 若要這樣做，您必須將應用程式和網路導向支援該功能。
  - 如果您不打算使用 GRS 來支援您的區域復原需求，則不應使用配對的區域作為次要。 如果發生區域失敗，配對區域中的資源將會因為資源遷移而承受極大壓力。 您可以藉由復原至替代網站來避免這項壓力，並在復原期間取得更快速的速度。
  > [!WARNING]
  > 請勿嘗試使用 Azure GRS 來備份或復原 VM。 相反地，請使用[Azure 備份](https://azure.microsoft.com/services/backup)和[Azure Site Recovery](https://azure.microsoft.com/services/site-recovery)搭配[Azure 受控磁片](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)，以支援您的基礎結構即服務 (IaaS) 工作負載復原。

- Azure 備份和 Azure Site Recovery 會與您的網路設計一同合作，以利您獲得 IaaS 和資料備份所需的區域復原能力。 請確定網路已優化，因此資料傳輸會保留在 Microsoft 骨幹上，並盡可能使用[虛擬網路對等互連](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)。 某些具有全域部署的較大型組織可能會改為使用[ExpressRoute premium](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)來路由區域之間的流量，並可能儲存區域輸出費用。

- Azure 資源群組是特定區域。 不過，針對資源群組內的資源，跨越多個區域是正常的。 考慮到發生區域性失敗時，對資源群組的控制平面作業將會在受影響的區域中失敗，即使其他區域中的資源 (在該資源群組內) 仍會繼續運作。 這可能會同時影響到網路設計和資源群組設計。

- 許多平臺即服務 (PaaS) 服務，Azure 支援[服務端點](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)或[Azure 私人連結](https://docs.microsoft.com/azure/private-link/private-link-overview)中。 這兩種解決方案都對區域復原、遷移及治理方面，都有顯著的影響您的網路考慮。

- 許多 PaaS 服務依賴自己的區域復原解決方案。 例如，Azure SQL Database 和 Azure Cosmos DB 都可讓您輕鬆地複寫到其他區域。 Azure DNS 等服務沒有區域相依性。 當您考慮將在採用程式中使用的服務時，請務必清楚瞭解每個 Azure 服務可能需要的容錯移轉功能和復原步驟。

- 除了部署至多個區域以支援嚴重損壞修復，許多組織會選擇以主動-主動模式部署，而不依賴容錯移轉。 此方法可提供全域負載平衡、額外容錯和網路效能提升的額外優點。 若要利用此模式，您的應用程式必須支援在多個區域中執行主動-主動。

> [!WARNING]
> Azure 區域是高可用性的結構，其 Sla 會套用至其中執行的服務。 但您絕對不應該對任務關鍵性應用程式進行單一區域相依性。 請一律規劃區域失敗，以及練習修復和緩和步驟。

在考慮網路拓撲之後，您必須先查看可能需要的其他檔和進程對齊。 下列方法可協助您評估潛在挑戰，並建立整體行動方針：

- 考慮實作更穩固的整備和治理機制。
- 清查受影響的地理位置。 編譯受影響的區域和國家/地區清單。
- 記載資料主權需求。 所識別的國家/地區是否有治理資料主權的合規性需求？
- 記載使用者群。 位於所識別國家/地區的員工、合作夥伴或客戶是否會受到雲端移轉工作影響？
- 記載資料中心和資產。 在所識別的國家/地區中，是否有任何資產可能會包含在移轉工作中？
- 記錄區域 SKU 可用性和容錯移轉需求。

請讓移轉程序的變更彼此配合，以應付初始清查。

## <a name="document-complexity"></a>記錄複雜度

下表可協助記錄先前步驟中的結果：

| 區域        | Country     | 當地員工 | 本機外部使用者   | 本機資料中心或資產 | 資料主權需求 |
|---------------|-------------|-----------------|------------------------|-----------------------------|-------------------------------|
| 北美洲 | 美國         | 是             | 合作夥伴和客戶 | 是                         | 否                            |
| 北美洲 | 加拿大      | 否              | 客戶              | 是                         | 是                           |
| 歐洲        | 德國     | 是             | 合作夥伴和客戶 | 無-僅限網路           | 是                           |
| 亞太地區  | 南韓 | 是             | 合作夥伴               | 是                         | 否                            |

## <a name="relevance-of-data-sovereignty"></a>資料主權的相關性

世界各地的政府組織都已開始建立資料主權需求，例如一般資料保護規定 (GDPR)。 此性質的合規性需求通常會要求在特定區域內或甚至在特定國家/地區內進行資料在地化，以保護其公民。 在某些情況下，與客戶、員工或合作夥伴相關的資料必須儲存在與終端使用者位於相同區域內的雲端平台上。

對於全球性公司而言，解決這項挑戰已成為進行雲端移轉的重要動機。 為了保持合規性需求，某些公司選擇將重複的 IT 資產部署到該區域內的雲端提供者。 在上表中，德國是此案例的良好範例。 這個範例包括客戶、合作夥伴和員工，但目前不是德國的 IT assents。 此公司可能會選擇將一些資產部署至 GDPR 區域內的資料中心，可能會使用德文的 Azure 資料中心。 若能了解受 GDPR 影響的資料，將有助於雲端採用小組了解此情況下的最佳移轉方法。

### <a name="why-is-the-location-of-end-users-relevant"></a>為何與終端使用者的位置有所關聯？

為多個國家/地區的終端使用者提供支援的公司已開發出技術解決方案來因應終端使用者的流量。 在某些情況下，這會涉及資產在地化。 在其他案例中，公司可能會選擇採用全球廣域網路 (WAN) 解決方案，透過網路導向的解決方案來解決不同的使用者群。 不論是哪一種情況，遷移策略都可能受到那些不同使用者的使用設定檔所影響。

因為該公司支援德國的員工、合作夥伴和客戶，但目前沒有資料中心，所以此公司可能已實行租用線解決方案。 這種類型的解決方案會將流量路由傳送到其他國家/地區的資料中心。 這個既存的路由機制會對遷移後的應用程式所感受到的效能帶來重大風險。 在已建立並經過調整的全球性 WAN 中另外再插入躍點會讓使用者感覺移轉之後的應用程式效能變差。 尋找和修正這些問題可能會讓專案的進度大幅延遲。

在下列每個程式中，解決此複雜性的指引會包含在必要條件、評估、遷移和優化程式中。 了解每個國家/地區的使用者設定檔對於妥善管理此複雜性至關重要。

### <a name="why-is-the-location-of-datacenters-relevant"></a>為何與資料中心的位置有所關聯？

現有資料中心的位置會影響移轉策略。 例如：

**架構決策：** 目的地區域是遷移策略設計中的首要步驟之一。 這通常會受到現有資產的位置所影響。 此外，雲端服務的可用性和這些服務的單位成本可能會因一個區域而異。 瞭解目前和未來的資產位於何處會影響架構決策，而且可能會影響預算估計。

**資料中心**相依性：上表中的資料顯示各種全域資料中心之間的相依性很可能。 這些相依性可能不會在許多組織中明確地記載或瞭解，這種類型的規模運作。 貴公司評估使用者設定檔的方法有助於識別您組織中的部分相依性。 此外，您的小組也應該探索其他評估步驟，以減輕相依性所引發的風險和複雜度。

## <a name="implement-the-general-approach"></a>實行一般方法

下列方法會使用資料驅動模型來解決全域遷移的複雜性。 當遷移範圍包含多個區域時，雲端採用小組應該評估下列準備就緒考慮：

- 資料主權可能需要當地語系化某些資產，但許多資產可能不受這些合規性限制。 記錄、報告、網路路由、身分識別和其他中央 IT 服務等專案，可能會有資格裝載為多個訂用帳戶的共用服務，甚至是多個區域。 雲端採用小組應該使用這些服務的共用服務模型進行評估，如[使用共用服務的中樞和輪輻拓撲的參考架構](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/shared-services)中所述。
- 當您部署類似環境的多個實例時，環境 factory 可以建立一致性、改善治理，並加速部署。 [複雜企業治理指南](../../govern/guides/complex/index.md)建立了一套方法，會產生規模橫跨多個區域的環境。

當小組熟悉基準方法且準備就緒時，您應該考慮幾個資料驅動必要條件：

- **一般探索：** 完成[記錄複雜度](#document-complexity)資料表。
- **針對每個受影響的國家/地區執行使用者設定檔分析：** 請務必及早瞭解遷移程式中的一般使用者路由。 變更全球租用線路並將 ExpressRoute 等連線新增至雲端資料中心會讓網路功能延遲好幾個月。 請盡早在程序中解決這一點。
- **初始數位資產合理化：** 每當有複雜度引進遷移策略時，您就應該完成初始的數位資產合理化。 請參閱[數位資產合理化](../../digital-estate/index.md)的指導方針。
  - **其他數位資產需求：** 建立標記原則，以識別受資料主權需求影響的任何工作負載。 所需的標記應該從數位資產合理化開始，並帶到遷移遷移後的資產。
- **評估中樞和輪輻模型：** 分散式系統通常會共用共同的相依性。 這些相依性通常可以透過中樞和輪輻模型的執行來解決。 雖然這類模型不屬於遷移程式的範圍，但在[準備好](../../ready/index.md)的程式的未來反覆運算期間，應該將它標示為要考慮。
- **遷移待處理專案的優先順序：** 當需要進行網路變更以支援支援多個區域的工作負載生產環境部署時，雲端策略小組應該追蹤並管理有關這些網路變更的升級。 較高層級的執行支援有助於加速變更，方法是釋放策略小組來重新調整待處理專案的優先順序，並確保全域工作負載不會遭到網路變更封鎖。 只有在完成網路變更之後，才應該設定這類工作負載的優先順序。

這些必要條件有助於建立處理常式，以在執行遷移策略時解決這種複雜性。

## <a name="assess-process-changes"></a>評定程序變更

在遷移案例中，面對全球資產和使用者群的複雜性時，您應該加入幾個重要的活動，以評估您的遷移候選項目。 這些活動會產生資料，以清楚瞭解全域使用者和資產的障礙和結果。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

**評估跨資料中心的**相依性：Azure Migrate 中的相依性[視覺效果工具](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)可以協助找出相依性。 在移轉之前使用這些工具是最佳做法。 在處理全球複雜性時，這會成為評定程序的必要步驟。 透過[相依性群組](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies)，視覺效果可協助您識別為了支援工作負載所需的資產會有什麼 IP 位址和連接埠。

> [!IMPORTANT]
>
> - 需要瞭解資產位置和 IP 位址架構的主題專家，才能識別位於次要資料中心的資產。
> - 評估視覺效果中的下游相依性和用戶端，以瞭解雙向相依性。

**識別全域使用者的影響：** 必要條件使用者設定檔分析的輸出應該識別受全域使用者設定檔影響的任何工作負載。 當遷移候選項位於受影響的工作負載清單中時，準備進行遷移的架構設計人員應參閱網路和操作主題專家。 它們有助於驗證網路路由和效能預期。 架構至少應該包含最接近的網路作業中心與 Azure 之間的 ExpressRoute 連線。 ExpressRoute 連線的[參考架構](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/expressroute)可協助設定必要的連接。

**合規性設計：** 必要條件使用者設定檔分析的輸出應該識別受資料主權需求影響的任何工作負載。 在評估程式的架構活動期間，指派的架構設計人員應該諮詢合規性主題專家。 它們有助於瞭解跨多個區域進行遷移和部署的任何需求。 這些需求會大幅影響設計策略。 [多區域 web 應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)和[多區域多層式應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)的參考架構可以協助設計。

> [!WARNING]
> 當您使用上述其中一個參考架構時，可能需要從複寫程式中排除特定的資料元素，以符合資料主權需求。 這會在升階程序中添加額外的步驟。

## <a name="migration-process-changes"></a>遷移程式變更

當您要遷移的應用程式必須部署到多個區域時，雲端採用小組必須考慮幾個事項。 其中包括 Azure Site Recovery 的保存庫設計、設定和進程伺服器設計、網路頻寬設計，以及資料同步處理。

### <a name="suggested-action-during-the-migration-process"></a>在遷移過程中的建議動作

**Azure Site Recovery 保存庫設計：** Azure Site Recovery 是雲端原生複寫和數位資產同步處理至 Azure 的建議工具。 Site Recovery 會將資產相關資料複寫到 Site Recovery 保存庫，而該保存庫繫結至特定區域和 Azure 資料中心內的特定訂用帳戶。 當您將資產複寫到第二個區域時，您可能也需要第二個 Site Recovery 保存庫。

設定**和進程伺服器設計：** Site Recovery 適用于設定和進程伺服器的本機實例，其系結至單一 Site Recovery 保存庫。 這表示您可能需要在源資料中心內安裝這些伺服器的第二個實例，以加速複寫。

**網路頻寬設計：** 在複寫和進行中的同步處理期間，您會透過網路將二進位資料從來源資料中心移至目標 Azure 資料中心內的 Site Recovery 保存庫。 此程序會耗用頻寬。 將工作負載重複到第二個區域會使耗用的頻寬量加倍。 當頻寬有限或工作負載涉及大量設定或資料漂移時，便可能會干擾到完成移轉所需的時間。 更重要的是，它可能會影響仍取決於源資料中心頻寬的使用者或應用程式的體驗。

**資料同步處理：** 最大的頻寬耗盡通常來自于資料平臺的同步處理。 如[多區域 Web 應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)和[多區域多層式架構應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)的參考架構所定義，系統往往必須進行資料同步處理才能讓應用程式保持一致。 如果這是應用程式所需的操作狀態，則在來來源資料平臺與每個雲端平臺之間完成同步處理可能是明智的做法。 在遷移應用程式和仲介層資產之前，您應該先執行此動作。

**Azure 到 azure**的嚴重損壞修復：替代選項可進一步降低複雜性。 如果時間軸和資料同步處理方法為雙步驟部署， [azure 到 azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture)的嚴重損壞修復可能是可接受的解決方案。 在此案例中，您會使用單一 Site Recovery 保存庫和設定或進程伺服器設計，將工作負載遷移至第一個 Azure 資料中心。 測試工作負載之後，您可以從已遷移的資產將它復原到第二個 Azure 資料中心。 這種方法可減少對來源資料中心資源的影響，並利用 Azure 資料中心之間所能提供的更快傳送速率和高頻寬上限。

> [!NOTE]
> 這種方法可以透過額外的輸出頻寬費用來增加短期遷移成本。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

當您解決優化和升級期間的全域複雜性時，您可能需要在每個額外區域中重複工作。 當單一部署可接受時，您可能仍然需要複製商務測試和商務變更計畫。

### <a name="suggested-action-during-the-optimize-and-promote-process"></a>最佳化和升階程序期間的建議動作

**Pretest 優化：** 初始自動化測試可以識別可能的優化機會，如同任何遷移作業。 在全域工作負載的情況下，請在每個區域中獨立測試工作負載。 網路或目標 Azure 資料中心的次要設定變更可能會影響效能。

**商務變更計畫：** 針對任何複雜的遷移案例，建立商務變更計畫。 這可確保與商務程式、使用者體驗及整合變更所需之工作的變更相關的清楚溝通。 如果是全球性的移轉工作，此方案應該考量到每個受影響地理區域中的使用者。

**商務測試：** 結合商務變更計畫時，每個區域可能都需要進行商務測試。 這可確保適當的效能，並遵循已修改的網路路由模式。

**促銷航班：** 通常升級會以單一活動的方式發生，將生產流量重新路由傳送至已遷移的工作負載。 在進行全域發行工作的情況下，您應該在航班 (或預先定義的使用者集合) 中提供促銷。 這可讓雲端策略小組和雲端採用小組更清楚地觀察效能，並改善對每個區域的使用者所提供的支援。 升階發行小眾測試版通常是在網路層級進行控制，方法是將特定 IP 範圍的路由從來源工作負載資產變更為遷移後的新資產。 在遷移指定的終端使用者集合後，即可重新路由下一個群組。

**航班優化：** 促銷航班的其中一個優點是，它可讓您更深入觀察及進一步優化已部署的資產。 第一個發行小眾測試版在生產環境中使用了一小段時間後，如果 IT 作業程序允許，則建議您再改善遷移後的資產。

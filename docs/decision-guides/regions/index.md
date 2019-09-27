---
title: 區域決策指南
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解雲端平台區域的選取。
author: doodlemania2
ms.author: dermar
ms.date: 09/19/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: 041e1ccaf6ec0e928b6868f4e8c90849c8d4dea8
ms.sourcegitcommit: d19e026d119fbe221a78b10225230da8b9666fe1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71224587"
---
# <a name="azure-regions"></a>Azure 區域

Azure 是由世界各地的許多區域所組成的。 每個 [Azure 區域](https://azure.microsoft.com/global-infrastructure/regions)都有一組特定的特性，因此要選取哪個區域來使用就變得格外重要。

1. **可用服務：** 部署到每個區域的服務會根據各種因素而有所不同。 您必須選取含有您所需服務的區域，以將工作負載部署至其中。 如需哪些服務可在哪些區域中取得的詳細資訊，請參閱[依區域提供的產品](https://azure.microsoft.com/global-infrastructure/services)。
1. **容量：** 每個區域都有容量上限。 一般來說，使用者雖不會對此有所概念，但這一點卻會影響哪些類型的訂用帳戶能夠在何種情況下部署哪些類型的服務。 這與訂用帳戶配額不同。 如果您打算大規模地將資料中心移轉至 Azure，您可以洽詢當地的 Azure 現場小組或客戶經理，來確認您是否能夠以所需規模進行部署。
1. **條件約束：** 某些區域會對服務的部署設有某些條件約束。 例如，某些區域只能作為備份或容錯移轉目標。 其他值得注意的條件約束是[資料主權需求](https://azure.microsoft.com/global-infrastructure/geographies)。
1. **主權：** 有專門用於特定主權實體的特定區域。 雖然所有區域都是 Azure 區域，但這些主權區域會與其餘 Azure 區域徹底分開、不一定會由 Microsoft 管理，而且可能會有客戶類型的條件約束。 這些主權區域包括：
    1. [Azure China](https://azure.microsoft.com/global-infrastructure/china)
    1. [Azure 德國](https://azure.microsoft.com/global-infrastructure/germany) (即將淘汰，並改用標準的 Azure 德國區域 (非主權))
    1. [Azure 美國政府](https://azure.microsoft.com/global-infrastructure/government)
    1. 注意：[澳大利亞](https://azure.microsoft.com/global-infrastructure/australia)有兩個區域雖由 Microsoft 管理，卻提供給澳大利亞政府及其客戶和承包商使用，因此會有和其他主權雲端類似的用戶端條件約束。

## <a name="operating-in-multiple-geographic-regions"></a>在多個地理區域營運

對於在多個地理區域營運的企業，復原能力雖然重要，卻也會帶來更多複雜性。 這些複雜性會以四種主要形式表現：

- 資產散發
- 使用者存取設定檔
- 合規性需求
- 區域復原

當我們進一步考慮上述複雜性時，您將會開始了解區域的選擇對於整體雲端採用策略有多重要。 讓我們從網路考量來開始。

## <a name="networking-considerations"></a>網路考量

想要有健全的雲端部署，就需要有經過深思熟慮、已將 Azure 區域納入考量的網路。 在考慮過上述要作為部署目的地區域的特性之後，就必須部署網路。 雖然關於網路的詳盡討論不在本文涵蓋範圍內，但您必須考慮一些事項：

1. Azure 區域會成對部署。 如果某個區域發生嚴重失敗，系統便會將同一地緣政治界限*內的另一個區域指定為其配對區域。 請考慮將部署到配對區域作為主要和次要復原策略。 *Azure 巴西明顯例外，其配對區域正好是美國中南部。 如需詳細資訊，請參閱[這裡](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions)。
    1. Azure 儲存體支援[異地備援儲存體 (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)，這表示您的資料會有三個複本儲存在主要區域內，且會有另外三個複本儲存在配對區域內。 您無法變更 GRS 的儲存體配對。
    1. 依賴 Azure 儲存體 GRS 的服務可以利用此配對區域功能。 若要這樣做，您必須將應用程式和網路導向支援該功能。
    1. 如果您不打算利用 GRS 來支援區域復原需求，則建議您「不要」  利用配對區域作為次要區域。 如果發生區域失敗，配對區域中的資源將會因為資源遷移而承受極大壓力。 避免這種壓力，您便可在復原期間復原至替代網站，而提升復原速度。
    > [!WARNING]
    > 請勿嘗試利用 Azure GRS 來備份或復原 VM。 相反地，請利用 [Azure 備份](https://azure.microsoft.com/services/backup)和 [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery) 以及[受控磁碟](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)來支援 IaaS 工作負載的復原。
2. Azure 備份和 Azure Site Recovery 會與您的網路設計一同合作，以利您獲得 IaaS 和資料備份所需的區域復原能力。 請確定網路已進行最佳化，讓資料傳輸留在 Microsoft 骨幹上，並盡可能利用 [VNet 對等互連](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)。 某些具有全球性部署的較大型組織可改為使用 [ExpressRoute Premium](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)，以在各區域之間路由傳送流量，從而節省區域性輸出費用。
3. Azure 資源群組是區域專屬的建構。 不過，讓資源群組內的資源跨越多個區域是很常見的事。 當您這麼做時，請務必考慮到在發生區域性失敗時，針對資源群組的控制平面作業將會在受影響的區域中失敗，但其他區域中的資源 (屬於該資源群組) 則會繼續運作。 這可能會同時影響到網路和資源群組的設計。
4. Azure 內的許多 PaaS 服務支援[服務端點](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)和/或[私人連結](https://docs.microsoft.com/azure/private-link/private-link-overview)。 在考慮區域復原、移轉和治理時，這兩種解決方案都會大幅影響您的網路考量。
5. 許多 PaaS 服務依賴自己的區域復原解決方案。 例如，Azure SQL Database 可讓您輕鬆地複寫到 N 個額外區域，就和 CosmosDB 一樣。 某些服務則不會有任何區域相依性，例如 Azure DNS。 在考慮要在採用程序中利用哪些服務時，請務必清楚了解每個 Azure 服務可能需要的容錯移轉功能和復原步驟。
6. 除了部署到多個區域以支援災害復原外，許多組織還會選擇以主動-主動模式進行部署，因此不需要容錯移轉功能。 這有額外的好處，那就是提供全球性負載平衡，並額外提升容錯和網路效能。 若要利用此模式，應用程式必須支援在多個區域中執行主動-主動模式。

> [!WARNING]
> Azure 區域是具有高可用性的建構，並且會對其中執行的服務套用 SLA。 不過，請絕對不要對任務關鍵性應用程式採用單一區域相依性。 請一律要對區域性失敗做好規劃，並實行復原和風險降低步驟。

在考慮過要能保持運作所需的網路拓撲後，還必須讓其他文件與程序保持配合。 下列方法可協助您評估潛在挑戰，並建立整體行動方針：

- 考慮實作更穩固的整備和治理機制。
- 清查受影響的地理位置。 編譯受影響的區域和國家/地區清單。
- 記載資料主權需求：所識別的國家/地區是否有治理資料主權的合規性需求？
- 記載使用者群：位於所識別國家/地區的員工、合作夥伴或客戶是否會受到雲端移轉工作影響？
- 記載資料中心和資產：在所識別的國家/地區中，是否有任何資產可能會包含在移轉工作中？
- 記錄區域 SKU 可用性和容錯移轉需求。

請讓移轉程序的變更彼此配合，以應付初始清查。

## <a name="documenting-complexity"></a>記載複雜性

下表可協助記載得自上述步驟的結果：

|區域  |國家 (地區)  |當地員工  |當地外部使用者  |當地資料中心或資產 |資料主權需求  |
|---------|---------|---------|---------|---------|---------|
|北美洲     |USA         |yes         |合作夥伴和客戶         |yes         |否         |
|北美洲     |加拿大         |否         |客戶         |yes         |yes         |
|歐洲     |德國         |yes         |合作夥伴和客戶         |否 - 只有網路         |yes         |
|亞太地區     |南韓         |yes         |合作夥伴         |yes         |否         |

<!-- markdownlint-disable MD026 -->

## <a name="data-sovereignty-relevancy"></a>資料主權相關性

世界各地的政府組織都已開始建立資料主權需求，例如一般資料保護規定 (GDPR)。 此性質的合規性需求通常會要求在特定區域內或甚至在特定國家/地區內進行資料在地化，以保護其公民。 在某些情況下，與客戶、員工或合作夥伴相關的資料必須儲存在與終端使用者位於相同區域內的雲端平台上。

對於全球性公司而言，解決這項挑戰已成為進行雲端移轉的重要動機。 為了保持合規性需求，某些公司選擇將重複的 IT 資產部署到該區域內的雲端提供者。 在上述範例資料表中，德國正是此案例的最佳範例。 在此範例中，德國境內有客戶、合作夥伴和員工，但沒有既存的 IT 資產。 此公司可能會選擇將一些資產部署至 GDPR 區域內的資料中心，甚至可能會使用 German Azure 資料中心。 若能了解受 GDPR 影響的資料，將有助於雲端採用小組了解此情況下的最佳移轉方法。

### <a name="why-is-the-location-of-end-users-relevant"></a>為何與終端使用者的位置有所關聯？

為多個國家/地區的終端使用者提供支援的公司已開發出技術解決方案來因應終端使用者的流量。 在某些情況下，這會涉及資產在地化。 在其他案例中，該公司可能會寧可選擇實行全球性 WAN 解決方案，以透過網路導向的解決方案來因應不同的使用者群。 不論是哪一種情況，移轉策略都可能會受到那些不同終端使用者的使用設定檔所影響。

由於該公司為德國境內的員工、合作夥伴和客戶提供支援，但目前未在該國家/地區擁有資料中心，因此該公司可能已實作某種形式的租用線路解決方案來將該流量路由傳送至其他國家/地區的資料中心。 這個既存的路由機制會對遷移後的應用程式所感受到的效能帶來重大風險。 在已建立並經過調整的全球性 WAN 中另外再插入躍點會讓使用者感覺移轉之後的應用程式效能變差。 尋找和修正這些問題可能會讓專案的進度大幅延遲。 在以下每個程序中，所有必要條件、評估、遷移和最佳化程序都包含可供解決此複雜性的指導方針。 了解每個國家/地區的使用者設定檔對於妥善管理此複雜性至關重要。

### <a name="why-is-the-location-of-datacenters-relevant"></a>為何與資料中心的位置有所關聯？

現有資料中心的位置會影響移轉策略。 幾個最常見的影響如下：

**架構決策：** 目標區域/位置是在設計移轉策略時的首要步驟之一。 這通常會受到現有資產的位置所影響。 此外，是否有可用雲端服務以及這些服務的單位成本都可能會隨不同區域而異。 因此，了解資產現在和未來的位置將會影響架構決策，而且也會影響預算的估計。

**資料中心相依性：** 根據上表的資料，全球各地的資料中心之間可能會互相依存。 許多以此規模類型進行營運的組織可能既未記載也未詳加了解這些相依性。 用來評估使用者設定檔的方法將有助於識別其中某些相依性。 不過，建議您在評估程序進行期間採取其他步驟來降低與此複雜性相關聯的風險。

## <a name="implementing-the-general-approach"></a>實作一般方法

這個方法是由可量化的資訊來驅動。 因此，下列方法會遵循用於解決全球移轉複雜性的資料驅動模型。

當移轉範圍包含多個區域時，雲端採用小組應評估下列整備考量：

- 資料主權可能需要將某些資產在地化，但有許多資產可能不受這些合規性限制所控管。 記錄、報告、網路路由、身分識別和其他核心 IT 服務等項目可能都有資格在多個訂用帳戶或甚至多個區域中裝載為共用服務。 我們的建議是，雲端採用小組應評估這些服務的共用服務模型，如[具有共用服務的中樞和輪輻拓撲參考架構](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/shared-services)所述
- 在為類似環境部署多個執行個體時，環境處理站可能會建立一致性、改善治理並加快部署速度。 [大型企業治理旅程](../../govern/guides/complex/index.md)建立了一個方法，其會產生規模橫跨多個區域的環境。

該小組一旦熟悉基準方法且整備也可配合之後，就必須考慮幾個資料驅動必要條件：

- **一般探索：** 請完成上述的[記載複雜性](#documenting-complexity)資料表。
- **對每個受影響的國家/地區執行使用者設定檔分析：** 請務必及早了解移轉程序中的一般終端使用者路由。 變更全球租用線路並將 ExpressRoute 等連線新增至雲端資料中心會讓網路功能延遲好幾個月。 請盡早在程序中解決這一點。
- **初始數位資產合理化：** 每當移轉策略中增添了新的複雜性時，就應該完成初始的數位資產合理化作業。 如需協助，請參閱[數位資產合理化](../../digital-estate/index.md)的指引。
  - **其他數位資產需求：** 建立標記原則以識別會受到資料主權需求影響的工作負載。 所需的標記應該從數位資產合理化開始，並帶到遷移遷移後的資產。
- **評估中樞和輪輻模型：** 分散式系統通常會共用通用的相依性。 這些相依性通常可透過實作中樞和輪輻模型來加以解決。 雖然這類模型不在移轉程序的涵蓋範圍內，但請將這一點標記為需要在未來反覆進行[就緒程序](../../ready/index.md)期間考慮的事項。
- **移轉待辦項目的優先順序：** 在必須變更網路以支援在生產環境部署可支援多個區域的工作負載時，雲端策略小組必須追蹤和管理有關這些網路變更的升級。 更高層級主管的支援將有助於提升變更速度。 不過，更重要的影響是這會讓策略小組能夠重新安排待辦項目的優先順序，以確保全球性的工作負載不會因為網路變更而遭到封鎖。 只有在網路變更完成後，才可安排這類工作負載的優先順序。

上述必要條件有助於建立程序的進行，以在執行移轉策略期間解決此複雜性。

## <a name="assess-process-changes"></a>評估程序變更

在處理全球資產和使用者群的複雜性時，您必須在任何移轉候選方案的評估程序中新增幾個重要活動。 這些變更各自都會透過資料驅動方法讓您清楚了解全球使用者和資產所會受到的影響。

### <a name="suggested-action-during-the-assess-process"></a>評估程序進行期間的建議動作

**評估跨資料中心的相依性：** [Azure Migrate 中的相依性視覺效果工具](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)可協助您找出相依性。 在移轉之前使用此工具集是不錯的一般最佳做法。 不過，在處理全球複雜性時，這會成為評定程序的必要步驟。 透過[相依性群組](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies)，視覺效果可協助您識別為了支援工作負載所需的資產會有什麼 IP 位址和連接埠。

> [!IMPORTANT]
> 兩個重要事項：首先，必須有了解資產位置和 IP 位址架構的主題專家才能識別位於次要資料中心的資產。 其次，請務必評估視覺效果中的下游相依性和用戶端，以了解雙向相依性。

**識別全球使用者影響：** 得自必要條件使用者設定檔分析的輸出應該能識別會受全球使用者設定檔影響的任何工作負載。 當移轉候選方案位於受影響的工作負載清單中時，在為移轉做準備的架構設計人員應該要諮詢網路和作業主題專家，以驗證網路路由和效能預期。 架構內至少應包含最接近的網路作業中心 (NOC) 與 Azure 之間的 ExpressRoute 連線。 [ExpressRoute 連線的參考架構](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/expressroute)有助於設定所需的連線。

**合規性設計：** 得自必要條件使用者設定檔分析的輸出應該能識別會受資料主權需求影響的任何工作負載。 在評估程序的架構活動進行期間，所指派的架構設計人員應該要諮詢合規性主題專家，以了解跨多個區域進行移轉/部署的任何需求。 這些需求會大幅影響設計策略。 [多區域 Web 應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)和[多區域多層式架構應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)的參考架構有助於設計。

> [!WARNING]
> 在使用上述任一參考架構時，可能必須從複寫程序中排除特定資料元素以符合資料主權需求。 這會在升階程序中添加額外的步驟。

## <a name="migrate-process-changes"></a>遷移程序變更

在遷移必須部署到多個區域的應用程式時，雲端採用小組必須考慮一些事項。 這些考慮包括 Azure Site Recovery 保存庫設計、設定/處理序伺服器設計、網路頻寬設計和資料同步處理。

### <a name="suggested-action-during-the-migrate-process"></a>遷移程序進行期間的建議動作

**Azure Site Recovery 保存庫設計：** 建議您使用 Azure Site Recovery 這個工具來將數位資產以雲端原生的方式複寫和同步處理至 Azure。 Site Recovery 會將資產相關資料複寫到 Site Recovery 保存庫，而該保存庫繫結至特定區域和 Azure 資料中心內的特定訂用帳戶。 將資產複寫到第二個區域時，可能需要有第二個 Site Recovery 保存庫。

**設定/處理序伺服器設計：** Site Recovery 可與設定和處理序伺服器的本機執行個體搭配運作，該伺服器會繫結至單一 Site Recovery 保存庫。 這表示這些伺服器的第二個執行個體可能必須安裝在來源資料中心以輔助複寫。

**網路頻寬設計：** 在複寫和後續進行同步處理期間，二進位資料會透過網路從來源資料中心移至目標 Azure 資料中心內的 Site Recovery 保存庫。 此程序會耗用頻寬。 將工作負載複製到第二個區域將會讓耗用的頻寬量加倍。 當頻寬有限或工作負載涉及大量設定或資料漂移時，便可能會干擾到完成移轉所需的時間。 更重要的是，還可能會影響使用者或應用程式的體驗，只因為其仍依賴來源資料中心的頻寬。

**資料同步處理：** 消耗頻寬最大的因素往往是資料平台在進行同步處理。 如[多區域 Web 應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)和[多區域多層式架構應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)的參考架構所定義，系統往往必須進行資料同步處理才能讓應用程式保持一致。 如果這是應用程式所需的操作狀態，則最好在遷移應用程式和中介層資產之前，先完成來源資料平台與每個雲端平台之間的同步處理工作。
**資料同步處理：** 消耗頻寬最大的因素往往是資料平台在進行同步處理。 如[多區域 Web 應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)和[多區域多層式架構應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)的參考架構所定義，系統往往必須進行資料同步處理才能讓應用程式保持一致。 如果這是應用程式所需的操作狀態，則最好在遷移應用程式和中介層資產之前，先完成來源資料平台與每個雲端平台之間的同步處理工作。

**Azure 至 Azure 災害復原：** 還有一個替代選項可以進一步降低複雜性。 如果時間表和資料同步處理方法採用有兩個步驟的部署，則 [Azure 至 Azure 災害復原](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture)也是可以接受的解決方案。 在此案例中，會先使用單一 Site Recovery 保存庫和設定或處理序伺服器設計將工作負載遷移至第一個 Azure 資料中心。 工作負載經過測試後，即可從遷移遷移後的資產復原到第二個 Azure 資料中心。 這種方法可減少對來源資料中心資源的影響，並利用 Azure 資料中心之間所能提供的更快傳送速率和高頻寬上限。

> [!NOTE]
> 這種方法可能會提高短期移轉成本，因為這可能會導致額外的輸出頻寬費用。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

解決最佳化和升階期間的全球複雜性可能需要在每個額外的區域中進行重複的工作。 有單一部署可為您所接受時，可能仍需要重複的業務測試和業務變更方案。

### <a name="suggested-action-during-the-optimize-and-promote-process"></a>最佳化和升階程序進行期間的建議動作

**預先測試最佳化：** 和移轉工作一樣，初始自動化測試可以識別可能的最佳化機會。 如果是全球性的工作負載，請務必在每個區域獨立測試工作負載，因為網路或目標 Azure 資料中心的設定只要稍有變更就可能會影響效能。

**業務變更方案：** 針對任何複雜的移轉案例，建議您建立業務變更方案以確保能夠清楚傳達業務程序的變更、使用者體驗以及要整合變更所需的工作時間。 如果是全球性的移轉工作，此方案應該考量到每個受影響地理區域中的使用者。

**業務測試：** 在與業務變更方案結合時，每個區域都可能需要進行業務測試以確保效能合宜且符合修改後的網路路由模式。

**升階發行小眾測試版：** 升階通常會以單一活動的形式來進行，以將生產環境的流量重新路由傳送至遷移後的工作負載。 如果是全球性發行工作，建議您在發行小眾測試版 (或預先定義的使用者集合) 中傳遞升階。 這可讓雲端策略小組和雲端採用小組更清楚地觀察效能，並改善對每個區域的使用者所提供的支援。 升階發行小眾測試版通常是在網路層級進行控制，方法是將特定 IP 範圍的路由從來源工作負載資產變更為遷移後的新資產。 在遷移指定的終端使用者集合後，即可重新路由下一個群組。

**發行小眾測試版最佳化：** 升階發行小眾測試版的其中一個優點是，其可讓您更深入地觀察已部署的資產並進一步最佳化。 第一個發行小眾測試版在生產環境中使用了一小段時間後，如果 IT 作業程序允許，則建議您再改善遷移後的資產。
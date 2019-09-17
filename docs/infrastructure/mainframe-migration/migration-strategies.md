---
title: 大型主機移轉：從大型主機切換至 Azure
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 針對目前在大型主機上執行的系統，將應用程式從大型主機環境遷移至 Azure。
author: njray
ms.author: v-nanra
ms.date: 12/26/2018
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 00b2ee80709c36658a58f23b2848fdf1b64d856d
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71024382"
---
# <a name="make-the-switch-from-mainframes-to-azure"></a>從大型主機切換至 Azure

作為執行傳統大型主機應用程式的替代平台，Azure 在高可用性環境中提供超大規模的計算能力和儲存體。 您可以取得現代化雲端式平台的價值和靈活性，而不需要與大型主機環境相關的成本。

本節提供從大型主機平台切換至 Azure 的技術指導。

![大型主機和 Azure](../../_images/mainframe-migration/make-the-switch.png)

## <a name="mips-vs-vcpus"></a>MIPS 與 vCPU

沒有通用對應公式存在，無法判斷執行大型主機工作負載所需的虛擬中央處理器（個 vcpu）數目。 不過，每秒百萬指令數 (MIPS) 計量通常會對應至 Azure 上的 vCPU。 MIPS 透過針對指定電腦提供常數值的每秒循環次數，來測量大型主機的整體計算能力。

小型組織可能需要低於 500 MIPS，而大型組織通常需要超過 5,000 MIPS。 假設每單一 MIPS 需要 $1000 美元的情況下，大型組織每年需花費大約 5 百萬美元來部署 5,000 MIPS 的基礎結構。 針對此規模的一般 Azure 部署，每年的成本評估大約是 MIPS 基礎結構的十分支一。 如需詳細資訊，請參閱揭密[大型主機到 Azure 中的表4。遷移](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration)白皮書。

MIPS 與 Azure 的 vCPU 對應的精確計算，取決於 vCPU 的類型和實際執行的工作負載。 不過，基準測試研究可提供良好的基準，供您預估將需要的 vCPU 數目和類型。 最近的 HPE zREF 基準測試提供下列評估：

- 針對線上 (CICS) 作業，每個在 HP Proliant 伺服器上執行的 Intel 型核心為 288 MIPS。

- 針對 COBOL 批次作業，每個 Intel 核心為 170 MIPS。

本指南評估每個 vCPU 針對線上處理作業為 200 MIPS，針對批次處理作業為 100 MIPS。

> [!NOTE]
> 這些評估可能會因 Azure 中提供新的虛擬機器 (VM) 系列而有所變更。

## <a name="high-availability-and-failover"></a>高可用性和容錯移轉

當使用大型主機耦合與 Parallel Sysplex 時，大型主機系統通常會提供五個 9 的可用性 (99.999%)。 但是系統操作員仍需要為了維護和初始程式載入來排程停機時間。 實際的可用性會接近兩個或三個9，相當於高階的 Intel 伺服器。

相較之下，Azure 提供以服務等級協定 (SLA) 為基礎的承諾，預設便提供多個 9 的可用性，且透過服務的本機或異地複寫最佳化。

Azure 藉由從多個儲存體裝置 (可能是本機或在其他地理區域中) 複寫資料，提供額外的可用性。 萬一發生 Azure 失敗，計算資源可以存取本機或區域層級的複寫資料。

當您使用 Azure 平台作為服務 (PaaS) 資源時 (例如 [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) 和 [Azure Cosmos Database](https://docs.microsoft.com/azure/cosmos-db/introduction))，Azure 可以自動處理容錯移轉。 當您使用 Azure 基礎結構即服務（IaaS）時，容錯移轉會依賴特定的系統功能，例如 SQL Server Always On 功能、容錯移轉叢集實例和可用性群組。

## <a name="scalability"></a>延展性

大型主機通常是相應增加，而雲端環境是相應放大。大型主機可以使用結合設備（CF）向外延展，但是硬體和儲存體的高成本會使大型主機的相應放大變得昂貴。

CF 也提供緊密結合的計算，而 Azure 的相應放大功能則是鬆散結合。 雲端可以透過以使用量為基礎的計費模型，根據需求調整計算能力、儲存體和服務，來相應增加或減少以符合使用者的規格。

## <a name="backup-and-recovery"></a>備份與復原

大型主機客戶通常會保留災害復原網站，或使用獨立的大型主機提供者作為災害應變措施。 與災害復原網站的同步處理，通常是透過離線資料複本來完成。 這兩個選項都會產生高度成本。

透過大型主機耦合工具也可提供自動化異地備援，雖然費用很高，且通常會保留用於任務關鍵性系統。 相反地，Azure 擁有容易實作且符合成本效益的選項，在本機或區域層級 (或透過異地備援) 提供[備份](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup)、[復原](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)和[備援](https://docs.microsoft.com/azure/storage/common/storage-redundancy)。

## <a name="storage"></a>儲存體

了解大型主機的運作如何牽涉解碼各種重疊的詞彙。 例如，中央儲存體、實際記憶體，實際儲存體和主要儲存體，通常都是指直接連接到大型主機處理器的儲存體。

大型主機硬體包含處理器和其他許多裝置，例如直接存取存放裝置（Dasd）、磁片磁碟機，以及數種類型的使用者主控台。 磁帶和 DASD 用於系統功能和使用者程式。

大型主機的實體儲存體類型包括：

- **中央儲存體：** 直接位於大型主機的處理器上，也稱為處理器或實際儲存體。
- **輔助儲存體：** 與大型主機位在不同地方，此類型包含 DASD 上的儲存體，也稱為分頁儲存體。

雲端提供一組有彈性、可擴充的選項，且您只要為所需的那些選項付費。 [Azure 儲存體](https://docs.microsoft.com/azure/storage/common/storage-introduction)提供可大幅調整的資料物件存放區、雲端檔案系統服務、可靠的訊息存放區，以及 NoSQL 存放區。 針對 VM，受控和非受控磁碟可提供安全的永續性磁碟儲存體。

## <a name="mainframe-development-and-testing"></a>大型主機的開發與測試

在大型主機移轉專案中的主要磁碟機，是應用程式開發的變化面。 組織希望開發環境針對業務需求能更靈活且反應更迅速。

大型主機通常會有個別的邏輯分割區 (LPAR) 用於開發和測試，例如 QA 和暫存 LPAR。 大型主機開發解決方案包括編譯器 (COBOL、PL/I、Assembler) 和編輯器。 最常見的情況是在 IBM 大型主機上執行的 z/OS 作業系統的 Interactive System Productivity Facility (ISPF)。 其他包含 ROSCOE Programming Facility (RPF) 和電腦相關工具，例如 CA Librarian 和 CA-Panvalet。

模擬環境和編譯器可在 x86 平台上使用，因此開發和測試通常是從大型主機移轉至 Azure 的第一個工作負載。 [Azure 中的 DevOps 工具](https://azure.microsoft.com/solutions/devops)的可用性和廣泛使用，正在加速開發和測試環境的移轉。

當解決方案已開發完成並在 Azure 上經過測試，且已準備好部署至大型主機，您將需要複製程式碼到大型主機並在該處進行編譯。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [大型主機應用程式移轉](./application-strategies.md)

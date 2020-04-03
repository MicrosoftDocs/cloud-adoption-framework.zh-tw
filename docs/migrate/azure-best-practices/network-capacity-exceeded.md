---
title: 超過網路容量
description: 在遷移工作期間，資料需求超過網路容量。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 2d836e6d5397a5b2ff36ab57ee23712cfe40561a
ms.sourcegitcommit: 88fbc36cd634c3069e1a841a763a5327c737aa84
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2020
ms.locfileid: "80636442"
---
<!-- cSpell:ignore HDFS databox VHDX -->

# <a name="data-requirements-exceed-network-capacity-during-a-migration-effort"></a>資料需求在遷移工作期間超過網路容量

在雲端移轉中，資產會在現有資料中心與雲端之間透過網路進行複寫和同步處理。 不同工作負載的現有資料大小需求超過網路容量的情況並不少見。 在這種情況下，移轉程序的速度會變得很慢，在某些情況下甚至會完全停止。 下列指引將擴充 [Azure 移轉指南](../azure-migration-guide/index.md)的範圍，以提供可解決網路限制的解決方案。

## <a name="general-scope-expansion"></a>一般範圍擴充

此範圍擴充所需的大部分工作都會發生於移轉的必要條件、評估和遷移程序期間。

## <a name="suggested-prerequisites"></a>建議的必要條件

**驗證網路容量風險：** 強烈建議使用[數位資產合理化](../../digital-estate/rationalize.md)，特別是在有造成可用網路容量的顧慮時。 在數位資產合理化期間，系統會收集[數位資產的詳細目錄](../../digital-estate/inventory.md)。 該詳細目錄應包含所有數位資產的現有儲存需求。 如[複寫風險：複寫物理學](../migration-considerations/migrate/replicate.md#replication-risks---physics-of-replication)中所述，該詳細目錄可用來預估**移轉資料大小總計**，從而與**可用移轉頻寬總計**進行比較。 如果該比較不符合所需的**業務變更時間**，則本文可協助加快移轉速度，以減少遷移資料中心所需的時間。

**獨立資料存放區的離線傳輸：** 下圖所示的範例是使用 Azure 資料箱進行線上和離線資料傳輸。 這些方法可用來在移轉工作負載之前，先將大量資料運送至雲端。 在離線資料傳輸中，來源資料會複製到 Azure 資料箱，然後實體運送至 Microsoft，再以檔案或 Blob 的形式傳輸至 Azure 儲存體帳戶。 此程序可用來在進行其他移轉工作之前，先運送未直接繫結至特定工作負載的資料。 這麼做可減少需要透過網路運送的資料量，以便在網路限制內完成移轉工作。

這種方法可用來從 HDFS、備份、封存、檔案伺服器和應用程式傳輸資料。 現有的技術指導方針會說明如何使用此方法，從 [HDFS 存放區](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster)、或是從會使用 [SMB](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data)、[NFS](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs)、[REST](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest) 或[資料複製服務](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-copy-service)的磁碟將資料傳輸至資料箱。

另外也有[第三方合作夥伴解決方案](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)會使用 Azure 資料箱進行「植入和饋送 (Seed and Feed)」移轉，以透過離線傳輸移動大量資料，再於稍後透過網路以較低的規模進行同步處理。

![使用 Azure 資料箱進行離線和線上資料傳輸](../../_images/migrate/databox.png)

## <a name="assess-process-changes"></a>評定程序變更

如果一或多個工作負載的儲存需求超過網路容量，則 Azure 資料箱仍可用於進行離線資料傳輸。

除非網路無法使用，否則網路傳輸是建議的方法。 即使頻寬受到限制，透過網路傳送資料的速度通常會比使用離線傳輸機制（如資料箱）實際傳送相同數量的資料更快。

如果可以連線到 Azure，則應先進行分析再使用資料箱，特別是當工作負載的移轉具有時限的話。 只有在傳輸必要資料的時間超過使用資料箱填入、運送和還原資料的所需時間時，才建議使用資料箱。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

**網路容量分析：** 當工作負載相關的資料傳輸需求有超出網路容量的風險時，雲端採用小組會將額外的分析工作新增至評估程式，稱為網路容量分析。 在此分析期間，具有區域網路和網路連線相關主題專業知識的小組成員，會估計可用的網路容量和所需的資料傳輸時間。 該可用容量會與要在目前發行期間進行遷移的所有資產儲存需求進行比較。 如果儲存需求超過可用頻寬，則會選取支援工作負載的資產來進行離線傳輸。

> [!IMPORTANT]
> 在分析結束時，可能需要更新發行計劃以反映運送、還原和同步處理要離線傳輸的資產所需的時間。

**漂移分析：** 要進行離線傳輸的每個資產都應該分析儲存和設定漂移。 儲存漂移是一段時間內基礎儲存體的變更量。 設定漂移則是一段時間內資產設定的變更。 從儲存體複製時到資產升階到生產環境時，都有可能遺失漂移。 如果該漂移需要反映在遷移後的資產中，則需要在本機資產與遷移後的資產之間進行某種形式的同步處理。 請將這一點標記為需要在移轉執行期間考慮的事項。

## <a name="migrate-process-changes"></a>遷移程序變更

在使用離線傳輸機制時，不太可能需要[複寫程序](../migration-considerations/migrate/replicate.md)。 不過，可能仍需要[同步處理程序](../migration-considerations/migrate/replicate.md)。 了解評估程序進行期間所完成漂移分析的結果，您將會知道移轉期間所需的工作 (如果資產正在進行離線傳輸的話)。

### <a name="suggested-action-during-the-migrate-process"></a>遷移程序進行期間的建議動作

**複製儲存體：** 這種方法可用來傳送 HDFS、備份、封存、檔案伺服器或應用程式的資料。 現有的技術指導方針會說明如何使用此方法，從 [HDFS 存放區](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster)、或是從會使用 [SMB](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data)、[NFS](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs)、[REST](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest) 或[資料複製服務](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-copy-service)的磁碟將資料傳輸至資料箱。

另外也有[第三方合作夥伴解決方案](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)會使用 Azure 資料箱進行「植入和同步 (Seed and Sync)」移轉，以透過離線傳輸移動大量資料，再於稍後透過網路以較低的規模進行同步處理。

**寄送裝置：** 複製資料之後，裝置就可以[寄送到 Microsoft](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up)。 收到資料並匯入之後，資料就可在 Azure 儲存體帳戶中使用。

**還原資產：** 確認儲存體帳戶中有可用[的資料](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up#verify-data-upload-to-azure)。 確認之後，就可以透過 Blob 或在 Azure 檔案儲存體中使用資料。 如果資料是 VHD/VHDX 檔案，則檔案可以轉換為受控磁碟。 隨後，您就可以使用這些受控磁碟來具現化虛擬機器，以建立原始內部部署資產的複本。

**同步處理：** 如果漂移的同步處理是已遷移資產的需求，則可以使用其中一個[協力廠商合作夥伴解決方案](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)來同步處理檔案，直到資產還原為止。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

最佳化活動不太可能受到範圍內的這項變更所影響。

## <a name="secure-and-manage-process-changes"></a>保護和管理程序變更

保護和管理活動不太可能受到範圍內的這項變更所影響。

## <a name="next-steps"></a>後續步驟

返回[遷移最佳做法檢查清單](./index.md)，以確保您的遷移方法完全一致。

> [!div class="nextstepaction"]
> [遷移最佳做法檢查清單](./index.md)

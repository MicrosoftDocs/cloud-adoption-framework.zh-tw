---
title: 在遷移工作期間，資料需求超過網路容量的最佳做法
description: 在遷移工作期間，資料需求超過網路容量的最佳做法
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 28a06749f9681f42ede11813c1477754418236b1
ms.sourcegitcommit: 452e09b543e7b84f943db5b02480ba2d18afd939
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87866208"
---
<!-- cSpell:ignore HDFS databox VHDX -->

# <a name="best-practices-when-data-requirements-exceed-network-capacity-during-a-migration-effort"></a>在遷移工作期間，資料需求超過網路容量的最佳做法

在雲端遷移中，您可以透過網路來複寫和同步處理現有資料中心與雲端之間的資產。 不同工作負載的現有資料大小需求並不會超過網路容量，這種情況並不常見。 在這種情況下，移轉程序的速度會變得很慢，在某些情況下甚至會完全停止。 下列指引會擴充[Azure 遷移指南](../azure-migration-guide/index.md)的範圍，以提供可因應網路限制的解決方法。

## <a name="general-scope-expansion"></a>一般範圍擴充

此範圍擴充所需的大部分工作都是在遷移的必要條件、評估和遷移階段期間發生。

## <a name="suggested-prerequisites"></a>建議的必要條件

**驗證網路容量風險：** 強烈建議使用[數位資產合理化](../../digital-estate/rationalize.md)，特別是在有造成可用網路容量的顧慮時。 在數位資產合理化的過程中，您會收集[數位資產的清查](../../digital-estate/inventory.md)。 該詳細目錄應包含所有數位資產的現有儲存需求。 

如複寫風險中所述[：複寫的物理](../migration-considerations/migrate/replicate.md#replication-risks---physics-of-replication)，您可以使用該清查來預估總遷移資料大小，這可以與總可用的遷移頻寬進行比較。 如果該比較不符合所需的業務變更時間，則本文可協助加快移轉速度，以減少遷移資料中心所需的時間。

**獨立資料存放區的離線傳輸：** 下圖顯示使用 Azure 資料箱進行線上和離線資料傳輸的範例。 在工作負載遷移之前，您可以使用這些方法將大量資料傳送至雲端。 在離線資料傳輸中，您會將來源資料複製到 Azure 資料箱，然後實際寄送至 Microsoft，以作為檔案或 blob 傳輸至 Azure 儲存體帳戶。 在進行其他遷移工作之前，您可以使用此程式來運送未直接系結至特定工作負載的資料。 這麼做可減少需要透過網路傳送的資料量，並支援在網路限制內完成遷移。

您可以使用此方法，從 HDFS、備份、封存、檔案伺服器和應用程式傳輸資料。 現有的技術指導方針說明如何使用此方法，透過[SMB](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data)、 [NFS](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs)、 [rest](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest)或[資料複製服務](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-copy-service)，將資料從[HDFS 存放區](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster)或磁片傳輸到資料箱。

另外還有協力廠商的合作夥伴解決方案，會使用 Azure 資料箱進行遷移。 透過這些解決方案，您可以透過離線傳輸來移動大量資料，但是您之後可以透過網路以較低的規模進行同步處理。

![此圖顯示 Azure 資料箱的離線和線上資料傳輸。](../../_images/migrate/data-box.png)

## <a name="assess-process-changes"></a>評定程序變更

如果工作負載 (或工作負載的儲存需求) 超過網路容量，您仍然可以在離線資料傳輸中使用 Azure 資料箱。

除非網路無法使用，否則網路傳輸是建議的方法。 即使頻寬受到限制，透過網路傳送資料的速度通常會比使用離線傳輸機制實際傳送資料更快。

如果可以連線到 Azure，您應該先進行分析，再使用資料箱，特別是當工作負載的遷移具有時間相關性時。 只有在傳送所需資料的時間超過填入、寄送和還原的時間時，才建議資料箱。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

**網路容量分析：** 當工作負載相關的資料傳輸需求有超出網路容量的風險時，雲端採用小組會將額外的分析工作新增至名為網路容量分析的評估程式。 在此分析期間，小組成員會估計可用的網路容量和所需的資料傳輸時間量。 請注意，這個小組成員應該有關于區域網路和網路連線能力的主題專業知識。

可用容量會與目前版本中要遷移之所有資產的儲存體需求相比較。 如果儲存體需求超過可用的頻寬，則會選取支援工作負載的資產來進行離線傳輸。

> [!IMPORTANT]
> 在分析結束時，您可能需要更新發行計畫，以反映出貨、還原和同步處理要離線傳輸的資產所需的時間。

**漂移分析：** 分析每個要在離線時傳輸的資產，以進行儲存體和設定漂移。 *儲存體漂移*是一段時間內基礎儲存體的變更量。 一段時間後的資產設定會變更設定*漂移*。 從儲存體複製到資產升級到生產的時間，可能會遺失任何漂移。 如果該漂移需要反映在已遷移的資產中，您將需要同步處理本機資產和已遷移的資產。 請在遷移執行期間將此標記為考慮。

## <a name="migration-process-changes"></a>遷移程式變更

當您使用離線傳輸機制時，[通常不需要複寫程式，](../migration-considerations/migrate/replicate.md)而[同步處理](../migration-considerations/migrate/replicate.md)程式可能仍然是必要的。 如果資產正在離線傳輸，瞭解評估程式的漂移分析結果將會通知遷移期間所需的工作。

### <a name="suggested-action-during-the-migration-process"></a>在遷移過程中的建議動作

**複製儲存體：** 您可以使用此方法來傳輸 HDFS、備份、封存、檔案伺服器或應用程式的資料。 現有的技術指導方針說明如何使用此方法，透過[SMB](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data)、 [NFS](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs)、 [rest](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest)或[資料複製服務](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-copy-service)，將資料從[HDFS 存放區](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster)或磁片傳輸到資料箱。

另外還有協力廠商的合作夥伴解決方案，會使用 Azure 資料箱進行遷移。 透過這些解決方案，您可以透過離線傳輸來移動大量資料，但是您之後可以透過網路以較低的規模進行同步處理。

**寄送裝置：** 複製資料之後，您可以將[裝置寄送至 Microsoft](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up)。 資料接收並匯入之後，就會在 Azure 儲存體帳戶中提供。

**還原資產：** 確認儲存體帳戶中有可用[的資料](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up#verify-data-upload-to-azure)。 若是如此，您可以使用資料做為 blob 或在 Azure 檔案儲存體中。 如果資料是 VHD/VHDX 檔案，您可以將檔案轉換成受控磁片。 隨後，您就可以使用這些受控磁碟來具現化虛擬機器，以建立原始內部部署資產的複本。

**同步處理：** 如果漂移的同步處理是已遷移資產的需求，您可以使用其中一個協力廠商合作夥伴解決方案來同步處理檔案，直到資產還原為止。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

優化活動不可能會受到此範圍變更的影響。

## <a name="secure-and-manage-process-changes"></a>保護和管理程序變更

此範圍內的變更不可能會影響安全和管理活動。

## <a name="next-steps"></a>後續步驟

回到檢查清單，確保您的遷移方法完全一致。

> [!div class="nextstepaction"]
> [遷移最佳做法檢查清單](./index.md)

---
title: 在遷移工作期間資料需求超過網路容量的最佳作法
description: 在遷移工作期間資料需求超過網路容量的最佳作法
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 3429f0a47e5d8ed3d5bc88ea95e46df112646d84
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94996905"
---
<!-- cSpell:ignore HDFS databox VHDX -->

# <a name="best-practices-when-data-requirements-exceed-network-capacity-during-a-migration-effort"></a>在遷移工作期間資料需求超過網路容量的最佳作法

在雲端遷移中，您會在現有的資料中心與雲端之間，透過網路複寫和同步處理資產。 不同工作負載的現有資料大小需求不會超過網路容量，這並不常見。 在這種情況下，移轉程序的速度會變得很慢，在某些情況下甚至會完全停止。 下列指導方針將擴充 [Azure 遷移指南](../azure-migration-guide/index.md) 的範圍，以提供解決網路限制的解決方案。

## <a name="general-scope-expansion"></a>一般範圍擴充

此範圍擴充所需的大部分工作都是在遷移的必要條件、評估和遷移階段期間進行。

## <a name="suggested-prerequisites"></a>建議的必要條件

**驗證網路容量風險：** [數位資產合理化](../../digital-estate/rationalize.md) 是強烈建議的必要條件，尤其是在有造成可用網路容量的考慮時。 在數位資產合理化期間，您會收集 [數位資產的清查](../../digital-estate/inventory.md)。 該詳細目錄應包含所有數位資產的現有儲存需求。

如複寫 [風險：複寫的物理](../migration-considerations/migrate/replicate.md#replication-risks---physics-of-replication)中所述，您可以使用該清查來估計遷移資料的總大小，這可能會與可用的總遷移頻寬比較。 如果該比較不符合所需的業務變更時間，則本文可協助加快移轉速度，以減少遷移資料中心所需的時間。

**離線傳輸獨立資料存放區：** 下圖顯示使用 Azure 資料箱進行線上和離線資料傳輸的範例。 您可以使用這些方法，在工作負載遷移之前將大量資料傳送至雲端。 在離線資料傳輸中，您會將來源資料複製到 Azure 資料箱，然後實際寄送至 Microsoft，以作為檔案或 blob 傳輸至 Azure 儲存體帳戶。 在進行其他遷移工作之前，您可以使用此程式來傳送未直接系結至特定工作負載的資料。 這麼做可減少需要透過網路傳送的資料量，並支援在網路條件約束中完成遷移。

您可以使用此方法，從 HDFS、備份、封存、檔案伺服器和應用程式傳輸資料。 現有的技術指引說明如何使用這種方法，從 [HDFS 存放區](/azure/storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster) 或從磁片使用 [SMB](/azure/databox/data-box-deploy-copy-data)、 [NFS](/azure/databox/data-box-deploy-copy-data-via-nfs)、 [rest](/azure/databox/data-box-deploy-copy-data-via-rest)或 [資料複製服務](/azure/databox/data-box-deploy-copy-data-via-copy-service) 將資料傳輸至資料箱。

另外還有協力廠商的合作夥伴解決方案，會使用 Azure 資料箱來進行遷移。 使用這些解決方案時，您可以透過離線傳輸移動大量資料，但稍後透過網路以較低的規模進行同步處理。

![顯示 Azure 資料箱的離線和線上資料傳輸圖表。](../../_images/migrate/data-box.png)

## <a name="assess-process-changes"></a>評定程序變更

如果工作負載 (或工作負載) 超過網路容量的儲存需求，您仍然可以在離線資料傳輸中使用 Azure 資料箱。

除非網路無法使用，否則網路傳輸是建議的方法。 即使頻寬受到限制，透過網路傳輸資料的速度通常會比實際使用離線傳輸機制來傳送資料更快。

如果可以連線到 Azure，您應該在使用資料箱之前進行分析，特別是當工作負載的遷移時間很敏感時。 只有在傳輸必要資料的時間超過擴展、寄送及還原所需的時間時，才建議資料箱。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

**網路容量分析：** 當工作負載相關的資料傳輸需求有超過網路容量的風險時，雲端採用小組會將額外的分析工作新增至名為「網路容量分析」的評估程式。 在這項分析期間，小組的成員會估計可用的網路容量和所需的資料傳輸時間量。 請注意，這個小組成員應該有關于區域網路和網路連線能力的主題專家。

可用容量會與要在目前版本中遷移之所有資產的儲存體需求進行比較。 如果儲存體需求超過可用的頻寬，則會選取支援工作負載的資產來進行離線傳送。

> [!IMPORTANT]
> 分析結束時，您可能需要更新發行計畫，以反映出貨、還原和同步處理要離線傳輸的資產所需的時間。

**漂移分析：** 分析每個要離線傳輸的資產，以進行儲存和設定漂移。 _儲存體漂移_ 是一段時間內基礎儲存體的變更量。 設定 _漂移_ 會隨著時間而變更資產的設定。 從儲存體複製到資產升級到生產環境的時間，可能會遺失任何漂移。 如果該漂移必須反映在已遷移的資產中，則您必須同步處理本機資產和已遷移的資產。 在遷移執行期間，將此旗標標示為考慮。

## <a name="migration-process-changes"></a>遷移流程變更

當您使用離線傳輸機制時，通常不需要複寫 [進程](../migration-considerations/migrate/replicate.md) ，而 [同步處理](../migration-considerations/migrate/replicate.md) 程式可能仍是必要的。 如果資產正在離線傳輸，請瞭解評估程式的漂移分析結果將會通知遷移期間所需的工作。

### <a name="suggested-action-during-the-migration-process"></a>遷移過程中的建議動作

**複製存放裝置：** 您可以使用此方法來傳送 HDFS、備份、封存、檔案伺服器或應用程式的資料。 現有的技術指引說明如何使用這種方法，從 [HDFS 存放區](/azure/storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster) 或從磁片使用 [SMB](/azure/databox/data-box-deploy-copy-data)、 [NFS](/azure/databox/data-box-deploy-copy-data-via-nfs)、 [rest](/azure/databox/data-box-deploy-copy-data-via-rest)或 [資料複製服務](/azure/databox/data-box-deploy-copy-data-via-copy-service) 將資料傳輸至資料箱。

另外還有協力廠商的合作夥伴解決方案，會使用 Azure 資料箱來進行遷移。 使用這些解決方案時，您可以透過離線傳輸移動大量資料，但稍後透過網路以較低的規模進行同步處理。

**寄送裝置：** 複製資料之後，您可以將 [裝置寄送至 Microsoft](/azure/databox/data-box-deploy-picked-up)。 資料收到並匯入之後，就會在 Azure 儲存體帳戶中提供。

**還原資產：** 確認儲存體帳戶中有可用 [的資料](/azure/databox/data-box-deploy-picked-up#verify-data-upload-to-azure) 。 若是如此，您可以使用資料作為 blob 或 Azure 檔案儲存體。 如果資料是 VHD/VHDX 檔案，您可以將檔案轉換成受控磁片。 隨後，您就可以使用這些受控磁碟來具現化虛擬機器，以建立原始內部部署資產的複本。

**同步處理：** 如果已遷移的資產需要同步處理，您可以使用其中一個協力廠商的夥伴解決方案來同步處理檔案，直到資產還原為止。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

優化活動不太可能受到範圍內的這項變更所影響。

## <a name="secure-and-manage-process-changes"></a>保護和管理程序變更

安全性和管理活動不太可能受到範圍內的這項變更所影響。

## <a name="next-steps"></a>下一步

返回檢查清單，以確保您的遷移方法完全一致。

> [!div class="nextstepaction"]
> [遷移最佳做法檢查清單](./index.md)

---
title: 評估工作負載整備程度
description: 瞭解評估工作負載是否就緒以遷移至雲端所需的事項。 您將瞭解如何驗證所有資產和相關聯的相依性。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: ed4587e1028f9adf21ca2ee30365de3ae1114911
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101786799"
---
# <a name="evaluate-workload-readiness"></a>評估工作負載整備程度

此活動著重於評估遷移至雲端的工作負載整備程度。 在此活動期間，雲端採用小組會驗證所有資產和相關聯的相依性是否與所選的部署模型和雲端提供者相容。 在此過程中，小組會記載[補救](../migrate/remediate.md)相容性問題所需的任何付出。

## <a name="evaluation-assumptions"></a>評估假設

在雲端採用架構中討論原則的大部分內容都是雲端中立的。 不過，整備評估程序必須主要專屬於每個特定的雲端平台。 下列指引假設有遷移至 Azure 的意圖。 它也會假設對[複寫活動](../migrate/replicate.md)使用 Azure Migrate (也稱為 Azure Site Recovery)。 如需替代工具，請參閱複寫 [選項](../migrate/replicate-options.md)。

本文不會捕捉所有可能的評估活動。 假設每個環境和業務結果都將規定特定的需求。 為了協助加速建立這些需求，本文的其餘部分會分享一些與[基礎結構](#common-infrastructure-evaluation-activities)、[資料庫](#common-database-evaluation-activities)和[網路](#common-network-evaluation-activities)評估相關的常見評估活動。

## <a name="common-infrastructure-evaluation-activities"></a>一般基礎結構評估活動

- VMware 需求： [檢查 vmware 的 Azure Site Recovery 需求](/azure/site-recovery/vmware-physical-azure-support-matrix)。
- Hyper-v 需求： [檢查 hyper-v 的 Azure Site Recovery 需求](/azure/site-recovery/hyper-v-azure-support-matrix)。

務必記載主機組態、複寫的 VM 組態、儲存體需求或網路組態中的任何差異。

## <a name="common-database-evaluation-activities"></a>一般資料庫評估活動

- 記錄復原點目標 (Rpo) 和復原時間目標 (目前資料庫部署的 Rto) 。 這些會在 [架構活動](./architect.md) 期間用來協助進行決策。
- 記載高可用性設定的任何需求。 如需瞭解 SQL Server 需求的協助，請參閱 [Sql server 高可用性解決方案指南](/sql/database-engine/sql-server-business-continuity-dr)。
- 評估 PaaS 相容性。 [Azure 資料移轉指南](https://datamigration.microsoft.com)會將內部部署資料庫對應到相容的 azure PaaS 解決方案，[例如 azure Cosmos DB](/azure/cosmos-db/)、適用于 MySQL 的[azure SQL Database](/azure/azure-sql/) [azure 資料庫](/azure/mysql/)、 [azure database for 于 postgresql](/azure/postgresql/)或[azure database for 適用于 mariadb](/azure/mariadb/)。
- 當 PaaS 相容性是不需要任何補救的選項時，請洽詢負責[架構活動](./architect.md)的小組。 PaaS 移轉可以大幅節省時間，並降低大部分雲端解決方案的擁有權總成本 (TCO)。
- 當 PaaS 相容性是需要補救的選項時，請洽詢負責[架構活動](./architect.md)和[補救活動](../migrate/remediate.md)的小組。 在許多情況下，資料庫解決方案的 PaaS 移轉優點可能會超越補救時間的增加。
- 記載要遷移的每個資料庫的大小和變動率。
- 可能的話，記載對每個資料庫進行呼叫的任何應用程式或其他資產。

> [!NOTE]
> 任何資產的同步處理都會在複寫期間耗用頻寬。 有個非常常見的缺陷，那就是忽略在複寫點和發行之間保持資產同步所需的頻寬耗用量。 在發行週期內，資料庫是頻寬的常見取用者，而具有大型儲存體磁碟使用量或高變動率的資料庫則特別有關。 在使用者接受度測試 (UAT) 和發行之前，請考慮使用受控制的更新來複寫資料結構的方法。 在此種情況下，Azure Site Recovery 的替代項目可能更適合。 如需詳細資訊，請參閱 [Azure 資料庫移轉指南](https://datamigration.microsoft.com)中的指引。

## <a name="common-network-evaluation-activities"></a>一般網路評估活動

- 計算在導致發行的反覆運算期間所有要複寫 VM 的總儲存體。
- 計算在導致發行的反覆運算期間所有要複寫 VM 的漂移或變動率。
- 將總儲存體和漂移加總，以計算每次反覆運算所需的頻寬需求。
- 計算目前網路上可用的未使用頻寬，以依照反覆運算對齊方式進行驗證。
- 記載要達到預期移轉速度所需的頻寬。 如果需要任何補救措施，以提供所需的頻寬，請通知負責[補救活動](../migrate/remediate.md)的小組。

> [!NOTE]
> 總儲存體會在初始複寫期間直接影響頻寬需求。 不過，儲存體漂移會從複寫點繼續到發行為止。 這表示漂移對可用的頻寬有累積的影響。

## <a name="next-steps"></a>下一步

完成系統評估之後，輸出會饋送新[雲端架構](./architect.md)的開發。

> [!div class="nextstepaction"]
> [在移轉前建構工作負載](./architect.md)

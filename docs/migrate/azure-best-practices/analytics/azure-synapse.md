---
title: Azure Synapse 分析的高可用性
description: 使用 Azure Synapse 功能來解決高可用性和嚴重損壞修復需求。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: fb75af110c980a718d1050441d7e64426ffb30a0
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451728"
---
# <a name="high-availability-for-azure-synapse-analytics"></a>Azure Synapse 分析的高可用性

新式雲端式基礎結構（例如 Microsoft Azure）的主要優點之一，就是內建的高可用性和嚴重損壞修復功能，而且很容易執行和自訂。 這些功能通常會比內部部署環境中的對等功能低。 使用這些內建函數也表示不需要遷移現有舊版資料倉儲中的備份和修復機制。

下列各節說明可解決高可用性和嚴重損壞修復需求的標準 Azure Synapse 分析功能。

## <a name="high-availability-ha"></a>高可用性（HA）

Azure Synapse 分析會使用資料庫快照集來提供倉儲的高可用性。 資料倉儲快照集會建立可用來將資料倉儲復原或複製到先前狀態的還原點。 由於 Azure Synapse 分析是分散式系統，因此資料倉儲快照集是由許多位於 Azure 儲存體中的檔案所組成。 快照集會從資料倉儲中儲存的資料內擷取增量變更。

Azure Synapse 分析會在一天內自動拍攝快照集，以建立七天提供的還原點。 此保留期無法變更。 Azure Synapse 分析支援八小時的復原點目標（RPO）。 您可以從過去七天內所建立的任何快照集，在主要區域中還原資料倉儲。

也支援使用者定義的還原點，允許手動觸發快照集，以在大型修改前後建立資料倉儲的還原點。 這項功能可確保還原點在邏輯上一致，如果有任何工作負載中斷或使用者錯誤，可提供額外的資料保護以快速復原時間。

## <a name="disaster-recovery-dr"></a>災害復原 (DR)

此外，Azure Synapse Analytics 也會以標準的形式，每天備份一次到配對的資料中心。 異地還原的 RPO 為 24 小時。 您可以將異地備份還原到支援 Azure Synapse Analytics 的任何其他區域中的伺服器。 異地備份可確保在主要區域中的還原點無法使用時，可以還原資料倉儲。

---
title: Azure Synapse 分析的高可用性
description: 使用 Azure Synapse 分析功能來解決高可用性和嚴重損壞修復需求。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 93b7a66a1c30ec4975a2cef092b54f56023eed0a
ms.sourcegitcommit: 163e703d9cbf90b26d96087c836a9cbd94fc7e35
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963276"
---
# <a name="high-availability-for-azure-synapse-analytics"></a>Azure Synapse 分析的高可用性

新式雲端式基礎結構（例如 Microsoft Azure 的主要優點之一，就是高可用性 (HA) 和嚴重損壞修復的功能， (DR) 內建且易於執行和自訂。 這些設備的成本通常比內部部署環境中的對等功能低。 使用這些內建函數也表示不需要遷移現有資料倉儲中的備份和復原機制。

下列各節說明標準的 Azure Synapse 分析功能，以解決高可用性和嚴重損壞修復的需求。

## <a name="high-availability"></a>高可用性

Azure Synapse 分析會使用資料庫快照集來提供倉儲的高可用性。 資料倉儲快照集會建立可用來將資料倉儲復原或複製到先前狀態的還原點。 由於 Azure Synapse 分析是分散式系統，因此資料倉儲快照集是由許多位於 Azure 儲存體中的檔案所組成。 快照集會從資料倉儲中儲存的資料內擷取增量變更。

Azure Synapse 分析會自動拍攝每天的快照集，以建立七天內可用的還原點。 無法變更此保留期間。 Azure Synapse Analytics 支援八小時的復原點目標， (RPO) 。 您可以從過去七天內所建立的任何快照集，還原主要區域中的資料倉儲。

此服務也支援使用者定義的還原點。 手動觸發快照集可以在大型修改前後建立資料倉儲的還原點。 這項功能可確保還原點在邏輯上是一致的。 邏輯一致性可針對工作負載中斷或使用者錯誤提供額外的資料保護，以快速復原時間。

## <a name="disaster-recovery"></a>災害復原

除了稍早所述的快照集之外，Azure Synapse 分析會每天執行一次標準異地備份至配對的資料中心。 異地還原的 RPO 為 24 小時。 您可以將異地備份還原到支援 Azure Synapse Analytics 的任何其他區域中的伺服器。 異地備份可確保在主要區域中的還原點無法使用時，可以還原資料倉儲。

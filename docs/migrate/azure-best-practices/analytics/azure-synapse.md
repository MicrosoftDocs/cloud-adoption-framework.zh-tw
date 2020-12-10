---
title: Azure Synapse Analytics 的高可用性
description: 瞭解如何使用 Azure Synapse Analytics 功能來解決高可用性和嚴重損壞修復的需求。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: d63d656864b55ebabfef8913f021ed0d8d1435b1
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97015298"
---
# <a name="high-availability-for-azure-synapse-analytics"></a>Azure Synapse Analytics 的高可用性

新式雲端式基礎結構（例如 Microsoft Azure）的主要優點之一，就是高可用性的功能 (HA) 和嚴重損壞修復 (DR) 是內建的，而且很容易執行和自訂。 這些設施通常會比內部部署環境內的對等功能低。 使用這些內建函數也表示不需要遷移現有資料倉儲中的備份和復原機制。

下列各節說明處理高可用性和嚴重損壞修復需求的標準 Azure Synapse Analytics 功能。

## <a name="high-availability"></a>高可用性

Azure Synapse Analytics 會使用資料庫快照集來提供倉儲的高可用性。 資料倉儲快照集會建立還原點，可用來將資料倉儲復原或複製到先前的狀態。 由於 Azure Synapse Analytics 是分散式系統，因此資料倉儲快照集是由許多位於 Azure 儲存體的檔案所組成。 快照集會從資料倉儲中儲存的資料內擷取增量變更。

Azure Synapse Analytics 會在一天內自動拍攝快照集，以建立可供七天使用的還原點。 此保留期限無法變更。 Azure Synapse Analytics 支援八小時的復原點目標 (RPO) 。 您可以從過去七天內所建立的任何快照集，還原主要區域中的資料倉儲。

服務也支援使用者定義的還原點。 手動觸發快照集時，可以在進行大型修改之前和之後，建立資料倉儲的還原點。 這項功能可確保還原點在邏輯上是一致的。 邏輯一致性可針對工作負載中斷或使用者錯誤提供額外的資料保護，以加快復原時間。

## <a name="disaster-recovery"></a>災害復原

除了稍早所述的快照，Azure Synapse Analytics 每天執行一次標準異地備份至配對的資料中心。 異地還原的 RPO 為 24 小時。 您可以將異地備份還原到支援 Azure Synapse Analytics 的任何其他區域中的伺服器。 異地備份可確保在主要區域中的還原點無法使用時，可以還原資料倉儲。

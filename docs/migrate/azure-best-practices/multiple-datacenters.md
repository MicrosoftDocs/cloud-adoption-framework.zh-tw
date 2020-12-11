---
title: 多個資料中心
description: 多個資料中心
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 4a3ad1c7e49f854f6e56c75d1f8f58ba196459fe
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97025923"
---
# <a name="multiple-datacenters"></a>多個資料中心

移轉範圍通常涉及多個資料中心的轉換。 下列指導方針將擴充 [Azure 遷移指南](../azure-migration-guide/index.md) 的範圍，以處理多個資料中心。

## <a name="general-scope-expansion"></a>一般範圍擴充

此範圍擴充所需的大部分工作都是在必要條件、評估和優化遷移流程的過程中進行。

## <a name="suggested-prerequisites"></a>建議的必要條件

開始遷移之前，您應該針對要遷移的每個資料中心，在專案管理工具中建立 epics。 每個長篇故事都代表一個資料中心。 請務必瞭解這項遷移的業務成果和動機。 使用這些動機來排列 epics (或資料中心) 清單的優先順序。 比方說，如果移轉的驅動力是想要在必須更新租用之前結束資料中心，則每個 Epic 都會根據租用更新日期來設定優先順序。

在每個工作負載中，要評估及遷移的工作負載都會以功能形式來管理。 該工作負載中的每個資產都會作為使用者案例來管理。 評估、遷移、優化、升階、保護和管理每個資產所需的工作，都是以每個資產的工作來表示。

短期衝刺或反復專案包含一系列的工作，以遷移雲端採用小組所認可的資產和使用者案例。 版本接著會包含一或多個要升級至生產環境的工作負載或功能。

## <a name="assess-process-changes"></a>評定程序變更

當您要擴充範圍來處理多個資料中心時，評定程式最大的變更是與工作負載和跨資料中心相依性的精確記錄和優先順序有關。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

**評估跨資料中心的** 相依性：Azure Migrate 中的相依性 [視覺效果工具](/azure/migrate/concepts-dependency-visualization) 有助於找出相依性。 在遷移之前使用此工具組通常是最佳作法。 但是在處理全球複雜性時，這會成為評定程式的必要步驟。 透過[相依性群組](/azure/migrate/how-to-create-group-machine-dependencies)，視覺效果可協助您識別為了支援工作負載所需的資產會有什麼 IP 位址和連接埠。

> [!IMPORTANT]
>
> - 需要瞭解資產位置和 IP 位址架構的主題專家，才能識別位於次要資料中心的資產。
> - 評估視覺效果中的下游相依性和用戶端，以瞭解雙向相依性。

## <a name="migration-process-changes"></a>遷移流程變更

遷移多個資料中心與合併資料中心類似。 移轉之後，雲端會成為多項資產的單一資料中心解決方案。 在移轉過程中，最可能的範圍擴充是 IP 位址的驗證和比對。

### <a name="suggested-action-during-the-migration-process"></a>遷移過程中的建議動作

以下是會大幅影響雲端遷移成功的活動：

- **評估網路衝突：** 當您將資料中心合併到單一雲端提供者時，您可能會建立網路、DNS 或其他衝突。 在遷移期間，請務必測試是否有衝突，以避免因雲端中裝載的生產系統中斷。
- **更新路由表：** 在合併網路或資料中心時，通常需要修改路由表。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

在優化期間，可能需要進行額外的測試。

### <a name="suggested-action-during-the-optimize-and-promote-process"></a>最佳化和升階程序期間的建議動作

在升級之前，請在此範圍擴充期間提供額外的測試層級。 在測試期間，請務必測試路由或其他網路衝突。 此外，請務必隔離已部署的應用程式，並重新測試以驗證所有相依性都已遷移至雲端。 在此情況下，隔離表示將已部署的環境與生產網路分開。 這麼做可攔截仍在內部部署環境中執行的被忽略資產。

## <a name="secure-and-manage-process-changes"></a>保護和管理程序變更

此範圍擴充不太可能變更安全及管理進程。

## <a name="next-steps"></a>後續步驟

返回檢查清單，以確保您的遷移方法完全一致。

> [!div class="nextstepaction"]
> [遷移最佳做法檢查清單](./index.md)

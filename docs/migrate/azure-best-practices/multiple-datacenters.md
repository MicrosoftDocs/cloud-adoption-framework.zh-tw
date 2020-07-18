---
title: 多個資料中心
description: 多個資料中心
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 7a2a2684eb0b23c4a2f0c9b93665de1c52442050
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86450929"
---
# <a name="multiple-datacenters"></a>多個資料中心

移轉範圍通常涉及多個資料中心的轉換。 下列指引將擴充 [Azure 移轉指南](../azure-migration-guide/index.md)的範圍，以處理多個資料中心。

## <a name="general-scope-expansion"></a>一般範圍擴充

此範圍擴充所需的大部分工作都會發生於移轉的必要條件、評定和最佳化處理期間。

## <a name="suggested-prerequisites"></a>建議的必要條件

開始移轉之前，您應該先在專案管理工具中建立 Epic，以代表要遷移的每個資料中心。 然後，務必瞭解業務成果和動機，期可證明此移轉是正確的。 這些動機可用來設定 Epic (或資料中心) 清單的優先順序。 比方說，如果移轉的驅動力是想要在必須更新租用之前結束資料中心，則每個 Epic 都會根據租用更新日期來設定優先順序。

在每個 Epic 內，要評定和遷移的工作負載都會被當作功能來管理。 該工作負載中的每個資產都會當作使用者案例來管理。 評定、遷移、最佳化、升階、保護及管理每個資產所需的工作，都會以每個資產的工作來表示。

短期衝刺或反覆運算接著會由一系列的工作所組成，而這些工作用以遷移雲端採用小組所認可的資產和使用者案例。 然後，發行會由一或多個要升階到生產環境的工作負載或功能所組成。

## <a name="assess-process-changes"></a>評定程序變更

在擴充範圍來處理多個資料中心時，評定程序的最大變更，與各資料中心之的工作負載和相依性的正確記錄和優先順序設定有關。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

**評估跨資料中心**相依性：Azure Migrate 中的相依性[視覺效果工具](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)可以協助找出相依性。 在遷移之前使用此工具組，通常是最佳作法。 但在處理全域複雜性時，它會成為評估程式中的必要步驟。 透過[相依性群組](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies)，視覺效果可協助您識別為了支援工作負載所需的資產會有什麼 IP 位址和連接埠。

> [!IMPORTANT]
>
> - 需要瞭解資產位置和 IP 位址架構的主題專家，才能識別位於次要資料中心的資產。
> - 評估視覺效果中的下游相依性和用戶端，以瞭解雙向相依性。


## <a name="migration-process-changes"></a>遷移程式變更

遷移多個資料中心與合併資料中心類似。 移轉之後，雲端會成為多項資產的單一資料中心解決方案。 在移轉過程中，最可能的範圍擴充是 IP 位址的驗證和比對。

### <a name="suggested-action-during-the-migration-process"></a>在遷移過程中的建議動作

以下是嚴重影響雲端移轉成功與否的活動：

- **評估網路衝突：** 將資料中心合併到單一雲端提供者時，有可能會建立網路、DNS 或其他衝突。 在移轉期間，務必測試衝突，以免中斷在雲端中裝載的生產系統。
- **更新路由表：** 通常，在合併網路或資料中心時，需要對路由表進行修改。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

在最佳化期間，可能需要進行額外的測試。

### <a name="suggested-action-during-the-optimize-and-promote-process"></a>最佳化和升階程序期間的建議動作

在升階之前，請務必在此範圍擴充期間提供額外的測試層級。 在測試期間，請務必測試路由或其他網路衝突。 此外，務必隔離已部署的應用程式，並重新測試以驗證所有相依項目都已遷移至雲端。 在此情況下，隔離表示將已部署的環境與生產網路分開。 這麼做可攔截仍在內部部署環境中執行的被忽略資產。

## <a name="secure-and-manage-process-changes"></a>保護和管理程序變更

此範圍擴充不應變更安全和管理流程。

## <a name="next-steps"></a>後續步驟

返回[遷移最佳做法檢查清單](./index.md)，以確保您的遷移方法完全一致。

> [!div class="nextstepaction"]
> [遷移最佳做法檢查清單](./index.md)

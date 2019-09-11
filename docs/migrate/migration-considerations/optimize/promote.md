---
title: 要將移轉後的資源升階至生產環境需要什麼？
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端移轉內的程序，其著重於將工作負載移轉至雲端的工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: eb025eacb7743f470b15e2714ed65a05c21034a1
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70825467"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-required-to-promote-a-migrated-resource-to-production"></a>要將移轉後的資源升階至生產環境需要什麼？

升階至生產環境後，代表工作負載移轉至雲端的工作已完成。 資產及其所有相依性都升階之後，將會重新路由生產流量。 流量的重新路由會使內部部署資產過時，讓它們可以解除委任。

升階程序會隨著工作負載的架構而有所不同。 不過，有幾個一致的必要條件和一些常見的工作。 本文將分別加以說明，並可作為升階前的檢查清單使用。

## <a name="prerequisite-processes"></a>必要條件程序

下列每個程序都應在生產環境部署之前執行、記載及驗證：

- **[評估](../assess/index.md)：** 已評估工作負載的雲端相容性。
- **[建構](../assess/architect.md)：** 已正確建構工作負載的結構，以配合所選的雲端提供者。
- **[複寫](../migrate/replicate.md)：** 資產已複寫至雲端環境。
- **[階段](../migrate/stage.md)：** 已在雲端環境的暫存執行個體中還原複寫的資產。
- **[業務測試](./business-test.md)：** 商務使用者已完整測試並驗證工作負載。
- **[業務變更方案](./business-change-plan.md)：** 企業已根據生產升階共用預定變更的計劃；其中應包含使用者採用計劃、業務程序的變更、需要訓練的使用者，以及各種活動的時間表。
- **[準備就緒](./ready.md)：** 在升階前通常必須進行一系列的技術變更。

## <a name="best-practices-to-execute-prior-to-promotion"></a>升階前應執行的最佳做法

在升階的過程中，可能需要完成並記載下列技術變更：

- **網域一致性。** 某些公司原則會要求暫存和生產必須要有個別的網域。 請確定所有資產都已加入適當的網域。
- **使用者路由。** 驗證使用者是否透過適當的網路路由存取工作負載；確認一致的效能預期。
- **身分識別一致性。** 驗證要重新路由至應用程式的使用者在網域內有適當的權限可裝載應用程式。
- **效能。** 執行工作負載效能的最後驗證，將意外狀況降到最低。
- **驗證商務持續性和災害復原。** 驗證適當的備份和復原程序是否如預期運作。
- **資料分類。** 驗證資料分類，以確保已實作適當的保護和原則。
- **資訊安全長 (CISO) 驗證。** 驗證資訊安全主管是否已審查工作負載、業務風險、風險承受度和緩和策略。

## <a name="final-step-promote"></a>最後步驟：升階

工作負載將需要不同層級的詳細審查和升階程序。 不過，網路重新調整可作為所有升階發行通用的最後步驟。 當其他一切都準備就緒時，請更新 DNS 記錄或 IP 位址，以將流量路由傳送至已移轉的工作負載。

## <a name="next-steps"></a>後續步驟

工作負載升階後，表示發行已完成。 但在移轉的同時，淘汰的資產必須[解除委任](./decommission.md)以停止運作。

> [!div class="nextstepaction"]
> [解除委任已淘汰的資產](./decommission.md)

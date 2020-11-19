---
title: 手動 Moodle 移轉的概觀
description: 手動 Moodle 移轉的概觀。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 729304a79e26ef5461b2186a14b73d48cd11b45b
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714394"
---
# <a name="overview-of-a-manual-moodle-migration"></a>手動 Moodle 移轉的概觀

本文件會說明如何將 Moodle 應用程式從內部部署環境遷移至 Azure。 為每個步驟提供兩種方法：

- 一種可讓您使用 Azure 入口網站的方法。
- 一種方法可讓您在使用 Azure CLI 時，使用命令列來完成相同的工作。

## <a name="prerequisites"></a>必要條件

如果內部部署的軟體堆疊版本，是根據本指南所支援的版本延遲，則預期會將內部部署版本更新/修補成本指南中所列的版本。

- 必須具有內部部署基礎結構的存取權，才能進行 Moodle 部署和設定 (包括資料庫設定) 的備份。
- 在移轉之前，應該先建立 Azure 訂用帳戶和 Azure Blob 儲存體帳戶。
- 開啟 Azure CLI 和 AzCopy，以便在整個流程中使用。
- 將 Moodle 網站設定為 **維護模式**。

此移轉指南支援下列軟體版本：

- Ubuntu 18.04 LTS
- Nginx 1.14
- MySQL 5.6、5.7 或 8.0 資料庫伺服器。 本指南適用於 MySQL 的 Azure 資料庫。
- PHP 7.2、7.3 或 7.4
- Moodle 3.8 和 3

## <a name="when-to-use-azure-resource-manager-template-for-moodle-migrations"></a>使用 Azure Resource Manager 範本進行 Moodle 移轉的時機

使用 Azure Resource Manager 範本遷移 Moodle，會在 Azure 中建立基礎結構。 建立基礎結構之後，就會遷移 Moodle 軟體堆疊和相關聯的相依性。

## <a name="moodle-migration-tasks"></a>Moodle 移轉工作

如何將 Moodle 應用程式遷移至 Azure 的步驟，分為下列三項工作：

1. 移轉前
1. 遷移應用程式的實際步驟
1. 移轉後

## <a name="next-steps"></a>後續步驟

如需關於 Moodle 移轉流程的初步資訊，請繼續參閱[如何準備 Moodle 移轉](./migration-pre.md)。

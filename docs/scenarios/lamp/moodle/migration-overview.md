---
title: Moodle 手動遷移的總覽
description: 請參閱手動將 Moodle 從內部部署環境遷移至 Azure 的必要條件和整體步驟。
author: UmakanthOS
ms.author: brblanch
ms.date: 11/30/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: internal
ms.openlocfilehash: 1e6ff81c76fc24c6868579268d6e1e05eb4354ec
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97025515"
---
# <a name="overview-of-moodle-manual-migration"></a>Moodle 手動遷移的總覽

[Moodle](https://moodle.org/) 是以 PHP 撰寫的免費開放原始碼學習管理系統。 本指南說明如何將 Moodle 部署從內部部署環境遷移至 Azure。 本指南提供兩種不同方法的步驟，使用 Azure 入口網站或 Azure 命令列介面 (Azure CLI) 。

## <a name="prerequisites"></a>必要條件

開始遷移之前，您需要下列必要條件：

- 已更新並修補下列版本的內部部署軟體：
  - Ubuntu 18.04 LTS
  - Nginx 1.14
  - MySQL 5.6、5.7 或 8.0 資料庫伺服器。 本指南使用適用於 MySQL 的 Azure 資料庫。
  - PHP 7.2、7.3 或 7.4
  - Moodle 3.8 或3
- 您的 Moodle 網站設定為 **維護模式**。
- 存取內部部署基礎結構，以 [備份 Moodle 部署和](migration-pre.md#back-up-on-premises-data)設定，包括資料庫設定。
- [Azure CLI](migration-pre.md#install-the-azure-cli) 和 [AzCopy](migration-pre.md#download-and-install-azcopy) 安裝在內部部署環境。
- 建立 [Azure 訂](migration-pre.md#create-a-subscription) 用 [帳戶和 Azure Blob 儲存體帳戶](migration-pre.md#create-a-storage-account) 。

## <a name="moodle-migration-process"></a>Moodle 遷移程式

使用 Azure Resource Manager (ARM) 範本來遷移 Moodle 會在 Azure 中建立基礎結構，然後遷移 Moodle software stack 和相關聯的相依性。

Azure 的 Moodle 遷移步驟可細分為下列三個階段：

1. [移轉前](migration-pre.md)
1. [應用程式移轉](migration-start.md)
1. [移轉後](migration-post.md)

## <a name="next-steps"></a>後續步驟

繼續 [瞭解如何準備 Moodle 遷移](./migration-pre.md)。

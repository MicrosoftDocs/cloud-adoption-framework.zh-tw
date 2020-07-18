---
title: Windows 虛擬桌面概念證明
description: 使用適用于 Azure 的雲端採用架構來學習虛擬桌面遷移的最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 00419cc14952f2a4b0faf1f986af50e086c8ffa5
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451815"
---
# <a name="windows-virtual-desktop-proof-of-concept"></a>Windows 虛擬桌面概念證明

若要驗證 Azure 登陸區域和使用者網路容量的設定，請先完成並測試概念證明，再進行使用者桌面部署。 下列遷移程式的方法已簡化，以概述概念證明的執行。

1. **評估：** 主機集區應使用預設的 VM 大小進行部署。 評估資料將有助於識別並行使用者會話的預期數目，以及支援這些並行會話所需的虛擬機器數目。
2. **部署：** 使用 Azure Marketplace 的 Windows 10 元件庫映射和步驟1的大小，建立集區桌面的[主機集](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace)區。
3. **部署：** 為已遷移的工作負載[建立 RemoteApp 應用程式群組](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups#create-a-remoteapp-group)。
4. **部署：** [建立 FSLogix 設定檔容器](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile)來儲存使用者設定檔。
5. **版本：** 測試應用程式群組和已部署桌上型電腦的效能和延遲，以進行使用者的取樣。
6. **版本：** 將使用者上線，告訴他們如何透過[Windows 桌面用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-windows-7-and-10)、 [web 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-web)、 [Android 客戶](https://docs.microsoft.com/azure/virtual-desktop/connect-android)端、 [macOS 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-macos)或[iOS 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-ios)連接

## <a name="assumptions"></a>假設

概念證明方法可能符合部分生產需求。 但是，這種方法是以一些假設為基礎。

對於 WVD 的任何企業遷移，下列假設都不太可能全部證明成立。 採用小組應該假設生產部署需要個別的部署，以更密切地配合 Windows 虛擬桌面評估期間所識別的生產需求。

1. 終端使用者與 Azure 中指派的登陸區域具有低延遲的連線。
2. 所有使用者都可以從共用的桌面集區工作。
3. 所有使用者都可以使用 Azure Marketplace 的 Windows 10 企業版多會話映射。
4. 所有使用者設定檔都會遷移至 FSLogix 設定檔容器的 [Azure 檔案儲存體]、[Azure NetApp Files] 或 [以 VM 為基礎的儲存體服務]。
5. 所有使用者都可以由一個通用角色來描述，每 1 vCPU 和 4 GB RAM 的每個使用者都有一個密度，[根據 VM 大小的建議](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/virtual-machine-recs#multi-session-recommendations)
6. 所有工作負載都與 Windows 10 多會話相容。
7. 可以接受虛擬桌面和應用程式群組之間的延遲，以供生產環境使用。

若要根據概念證明設定參考來計算 WVD 案例的成本，請參閱[美國東部](https://azure.com/e/448606254c9a44f88798892bb8e0ef3c)、[西歐](https://azure.com/e/61a376d5f5a641e8ac31d1884ade9e55)或[東南亞](https://azure.com/e/7cf555068922461587d0aa99a476f926)的定價計算機。 注意-這些範例全都使用 Azure 檔案儲存體是儲存體服務來儲存使用者設定檔。

## <a name="next-step-assess-for-windows-virtual-desktop"></a>下一步：評估 Windows 虛擬桌面

下列文章清單會帶您前往在雲端採用旅程圖的特定點上找到的指引。

- [評估 Windows 虛擬桌面遷移或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面實例](./migrate-deploy.md)
- [將您的 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

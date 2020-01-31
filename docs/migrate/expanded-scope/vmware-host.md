---
title: 使用 VMware 主機加速遷移
description: 使用 VMware 主機加速遷移
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/10/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 77d75127a497778056634f8a84a9021fa874a726
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76802917"
---
# <a name="accelerate-migration-with-vmware-hosts"></a>使用 VMware 主機加速遷移

遷移整個 VMware 主機可以在單一遷移工作中移動多個工作負載和數個資產。 下列指引會透過 VMware 主機遷移來擴充[Azure 遷移指南](../azure-migration-guide/index.md)的範圍。 此範圍擴充所需的大部分工作，都是在遷移工作的必要條件和遷移過程中進行。

## <a name="suggested-prerequisites"></a>建議的必要條件

將您的第一個 VMware 主機遷移至 Azure 時，您必須符合幾個必要條件，以準備身分識別、網路和管理需求。 符合這些必要條件之後，每個額外的主機都需要大幅減少遷移的負擔。 下列各節提供有關必要條件的更多詳細資料。

### <a name="secure-your-azure-environment"></a>保護您的 Azure 環境

在您的 Azure 環境中，為角色型存取控制和網路連線能力執行適當的雲端解決方案。 [保護您的環境指南](https://docs.microsoft.com/azure/vmware-cloudsimple/private-cloud-secure?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)可協助進行這種實現。

### <a name="private-cloud-management"></a>私人雲端管理

有兩個必要的工作，以及一個可供建立私人雲端管理的選擇性工作。 [提升私人雲端許可權](https://docs.microsoft.com/azure/vmware-cloudsimple/escalate-privileges?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)和[工作負載 DNS 和 DHCP 設定](https://docs.microsoft.com/azure/vmware-cloudsimple/dns-dhcp-setup?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)是每個必要的最佳作法。

如果目標是[使用第2層延展的網路來遷移工作負載](https://docs.microsoft.com/azure/vmware-cloudsimple/migration-layer-2-vpn?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)，則第三個最佳做法也是必要的。

### <a name="private-cloud-networking"></a>私人雲端網路

建立管理需求之後，您可以使用下列最佳作法來建立私用雲端網路：

- [私人雲端的 VPN 連線](https://docs.microsoft.com/azure/vmware-cloudsimple/set-up-vpn?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)
- [使用 ExpressRoute 的內部部署網路連線](https://docs.microsoft.com/azure/vmware-cloudsimple/on-premises-connection?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)
- [使用 ExpressRoute 的 Azure 虛擬網路連線](https://docs.microsoft.com/azure/vmware-cloudsimple/azure-expressroute-connection?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)
- [設定 DNS 名稱解析](https://docs.microsoft.com/azure/vmware-cloudsimple/on-premises-dns-setup?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)

### <a name="integration-with-the-cloud-adoption-plan"></a>與雲端採用方案整合

符合其他必要條件之後，您應該將每個 VMware 主機納入[雲端採用方案](../../plan/template.md)。 在雲端採用方案中，將每個要遷移的主機新增為[不同的工作負載](../../plan/workloads.md)。 在每個工作負載中，新增要作為[資產](../../plan/workloads.md)遷移的 vm。 若要大量將工作負載和資產新增至採用方案，請參閱[使用 Excel 新增/編輯工作專案](https://docs.microsoft.com/azure/devops/boards/backlogs/office/bulk-add-modify-work-items-excel?view=azure-devops)。

## <a name="migrate-process-changes"></a>遷移程序變更

在每次反覆運算期間，採用小組會透過待處理專案（backlog）來遷移最高優先順序的工作負載。 VMware 主機的程式並不會真正改變。 待處理專案的下一個工作負載是 VMware 主機時，唯一的變更就是所使用的工具。

您可以使用下列工具來進行遷移工作：

- [原生 VMware 工具](https://docs.microsoft.com/azure/vmware-cloudsimple/migrate-workloads?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)
- [Azure 資料箱](https://docs.microsoft.com/azure/vmware-cloudsimple/migration-using-azure-data-box?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)

或者，您可以使用下列工具，透過嚴重損壞修復容錯移轉來遷移工作負載：

- [備份工作負載虛擬機器](https://docs.microsoft.com/azure/vmware-cloudsimple/backup-workloads-veeam?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)
- [使用 Zerto 將私人雲端設定為災難復原網站](https://docs.microsoft.com/azure/vmware-cloudsimple/disaster-recovery-zerto?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)
- [使用 VMware SRM 將私人雲端設定為災難復原網站](https://docs.microsoft.com/azure/vmware-cloudsimple/disaster-recovery-site-recovery-manager?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)

## <a name="next-steps"></a>後續步驟

回到擴充的範圍檢查清單，以確保您的遷移方法完全一致。

> [!div class="nextstepaction"]
> [擴充的範圍檢查清單](./index.md)

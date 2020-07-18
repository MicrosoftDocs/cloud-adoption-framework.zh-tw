---
title: 將工作負載部署到 Azure Stack 中樞
description: 瞭解如何使用 Azure Stack 中樞在資料中心內部署工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 2e29dcf36d0da76d72187fb895f6108e7a9dd2b5
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451824"
---
# <a name="deploy-workloads-to-azure-stack-hub"></a>將工作負載部署到 Azure Stack 中樞

Azure Stack 可讓客戶在其資料中心內執行自己的 Azure 實例。 組織會挑選 Azure Stack 做為其雲端策略的一部分，因為它可協助他們處理公用雲端無法使用的情況。 使用 Azure Stack 的三個最常見原因是因為公用雲端的網路連線不佳、法規或合約需求，或是無法公開到網際網路的後端系統。

## <a name="infrastructure-as-a-service-iaas-deployment"></a>基礎結構即服務（IaaS）部署

不論部署的原因為何，部署至 Azure Stack 中樞都與任何其他 IaaS 部署類似。 人們通常會將 IaaS 視為虛擬機器，但 IaaS 則比較簡單。 當您在 Azure 或 Azure Stack 中部署 VM 時，機器會隨附軟體定義的網路，包括 DNS、公用 Ip、防火牆規則（也稱為網路安全性群組），以及許多其他功能。 VM 部署也會在使用 Azure Blob 儲存體的軟體定義儲存體上，為您的 Vm 建立磁片。

如需將 Vm 部署至 Azure Stack 的更深入指引，請參閱[Azure Stack 計算總覽](https://docs.microsoft.com/azure-stack/user/azure-stack-compute-overview?view=azs-2002)。

## <a name="platform-as-a-service-paas-deployment"></a>平臺即服務（PaaS）部署

在雲端中，所有 PaaS 資源都是以某種形式的基礎結構服務（例如虛擬機器（VM））執行。 不過，Azure 服務會模糊處理這些後端資源，因此您不需要管理它們。 這些基礎結構資源的混淆和協調是由 Azure Resource Manager 來管理。 使用 Azure Resource Manager 範本部署至 Azure 時，您可能會看到 Resource Manager 的其中一個層面。 這些範本會告訴 Azure 您想要叫用的資源提供者，以及您希望如何設定您的資源。

當雲端在您的資料中心內執行時，您的堆疊中樞系統管理員必須有點熟悉混淆層級。 在您的使用者或開發人員可以使用 PaaS 資源之前，Azure Stack 中樞系統管理員必須從 marketplace 安裝資源提供者。 這些資源提供者可讓 Azure Stack Hub 的實例複寫您的堆疊實例中的 Azure 資源提供者功能。 如需有關部署 Azure Stack Hub 資源提供者的詳細資訊，請參閱[Azure Stack IaaS blog 系列](https://azure.microsoft.com/blog/azure-stack-iaas-part-one/)。

## <a name="deploy-workloads"></a>部署工作負載

一旦 Azure Stack Hub 系統管理員已正確設定您的堆疊實例之後，就可以繼續進行遷移，如同大部分其他 Azure 遷移工作一樣。 以下是客戶通常會在 Azure Stack 上執行的幾種特定遷移類型的連結：

- [乙太坊區塊鏈網路](https://docs.microsoft.com/azure-stack/user/azure-stack-ethereum?view=azs-2002)
- [AKS 引擎](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-2002)
- [Azure 認知服務](https://docs.microsoft.com/azure-stack/user/azure-stack-solution-template-cognitive-services?view=azs-2002)
- [C # ASP.NET web 應用程式](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-vm-dotnet?view=azs-2002)
- [Linux VM](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-deploy-linux?view=azs-2002)
- [Java Web 應用程式](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-vm-java?view=azs-2002)

## <a name="additional-considerations-during-migration"></a>遷移期間的其他考慮

下列文章可協助您的小組進行遷移和現代化：

- 擴充[性和可用性](https://azure.microsoft.com/blog/azure-stack-iaas-part-six/)服務，例如依使用量付費、VM 可用性設定組、虛擬機器擴展集、網路介面卡，以及新增和調整 vm 和磁片大小的功能。
- [儲存體容量](https://azure.microsoft.com/blog/azure-stack-iaas-part-3/)，包括上傳和下載功能，同時也可捕捉及部署 VM 映射。
- [Azure Stack 快速入門範本](https://github.com/Azure/AzureStack-QuickStart-Templates)GitHub 存放庫。
- [Azure 快速入門範本](https://github.com/Azure/Azure-QuickStart-Templates)GitHub 存放庫。

## <a name="next-step-integrate-this-strategy-into-your-cloud-adoption-journey"></a>下一步：將此策略整合到您的雲端採用旅程圖

下列文章清單會帶您前往在雲端採用旅程圖的特定點上找到的指引。

- [管理 Azure Stack 中樞](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

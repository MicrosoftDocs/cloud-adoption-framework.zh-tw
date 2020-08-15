---
title: 將工作負載部署到 Azure Stack Hub
description: 瞭解如何使用 Azure Stack 中樞在資料中心內部署工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: dd4b132dea74c9b907cf59dc7e8f5e5c5b5fddfc
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237163"
---
# <a name="deploy-workloads-to-azure-stack-hub"></a>將工作負載部署到 Azure Stack Hub

藉由使用 Azure Stack，您的組織可以在其資料中心內執行自己的 Azure 實例。 組織會將 Azure Stack 納入其雲端策略中，因為它可協助他們處理公用雲端無法使用的情況。 使用 Azure Stack 的三個最常見的原因如下：
* 公用雲端的網路連線不佳。
* 法規或合約需求。
* 無法公開到網際網路的後端系統。

## <a name="infrastructure-as-a-service-deployment"></a>基礎結構即服務部署

無論部署基礎結構即服務 (IaaS) 的原因為何，部署至 Azure Stack 中樞都與任何其他 IaaS 部署類似。 人們通常會將 IaaS 視為虛擬機器 (Vm) ，但 IaaS 則不只是這樣。 當您在 Azure 或 Azure Stack 中部署 VM 時，機器會隨附軟體定義的網路，包括網域名稱系統、公用 Ip、防火牆規則 (也稱為網路安全性群組) ，以及許多其他功能。 VM 部署也會使用 Azure Blob 儲存體，在軟體定義的儲存體上為您的 Vm 建立磁片。

如需將 Vm 部署至 Azure Stack 的更深入指引，請參閱 [Azure Stack 計算總覽](https://docs.microsoft.com/azure-stack/user/azure-stack-compute-overview?view=azs-2002)。

## <a name="platform-as-a-service-deployment"></a>平臺即服務部署

在雲端中，所有平臺即服務 (PaaS) 資源都是以某種形式的基礎結構服務（例如 VM）執行。 不過，Azure 服務會模糊處理這些後端資源，因此您不需要管理它們。 這些基礎結構資源的混淆和協調是由 Azure Resource Manager 來管理。 使用 Azure Resource Manager 範本部署至 Azure 時，您可能會看到 Resource Manager 的其中一個層面。 這些範本會告訴 Azure 您想要叫用的資源提供者，以及您希望如何設定您的資源。

當雲端在您的資料中心內執行時，您的堆疊中樞系統管理員必須有點熟悉混淆的層級。 在您的使用者或開發人員可以使用 PaaS 資源之前，Azure Stack 中樞系統管理員必須從 marketplace 安裝資源提供者。 這些資源提供者可讓 Azure Stack Hub 的實例複寫您的堆疊實例中的 Azure 資源提供者功能。 如需有關部署 Azure Stack Hub 資源提供者的詳細資訊，請參閱 [Azure Stack IaaS blog 系列](https://azure.microsoft.com/blog/azure-stack-iaas-part-one/)。

## <a name="deploy-workloads"></a>部署工作負載

在 Azure Stack Hub 系統管理員已正確設定您的堆疊實例之後，遷移可以繼續進行大部分其他 Azure 遷移工作的作業。 藉由使用 Azure Stack，您的小組可以執行下列任何一種類型的遷移：

- [乙太坊區塊鏈網路](https://docs.microsoft.com/azure-stack/user/azure-stack-ethereum?view=azs-2002)
- [AKS 引擎](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-2002)
- [Azure 認知服務](https://docs.microsoft.com/azure-stack/user/azure-stack-solution-template-cognitive-services?view=azs-2002)
- [C # ASP.NET web 應用程式](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-vm-dotnet?view=azs-2002)
- [Linux VM](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-deploy-linux?view=azs-2002)
- [Java Web 應用程式](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-vm-java?view=azs-2002)

## <a name="additional-considerations-during-migration"></a>遷移期間的其他考慮

下列文章可協助您的小組進行遷移和現代化：

- 擴充[性和可用性](https://azure.microsoft.com/blog/azure-stack-iaas-part-six/)服務，例如依使用量付費、vm 可用性設定組、vm 擴展集、網路介面卡，以及新增和調整 vm 和磁片大小的功能
- [儲存體容量](https://azure.microsoft.com/blog/azure-stack-iaas-part-3/)，包括上傳和下載的功能，並同時也能捕捉及部署 VM 映射
- [Azure Stack 快速入門範本](https://github.com/Azure/AzureStack-QuickStart-Templates) GitHub 存放庫
- [Azure 快速入門範本](https://github.com/Azure/Azure-QuickStart-Templates) GitHub 存放庫

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

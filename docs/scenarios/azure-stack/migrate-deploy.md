---
title: 將工作負載部署到 Azure Stack Hub
description: 瞭解如何使用 Azure Stack Hub 在您的資料中心部署工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 86c1d4b7eaea3c7de15dfd4417a6bcc21d956ea8
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88569632"
---
# <a name="deploy-workloads-to-azure-stack-hub"></a>將工作負載部署到 Azure Stack Hub

藉由使用 Azure Stack，您的組織可以在其資料中心內執行自己的 Azure 實例。 組織會在其雲端策略中包含 Azure Stack，因為它可協助他們處理公用雲端無法運作的情況。 使用 Azure Stack 的三個最常見原因包括：

- 對公用雲端的網路連線能力不佳。
- 法規或契約需求。
- 無法公開到網際網路的後端系統。

## <a name="infrastructure-as-a-service-deployment"></a>基礎結構即服務部署

無論將基礎結構即服務部署 (IaaS) 的原因為何，部署至 Azure Stack Hub 都類似于任何其他 IaaS 部署。 人們通常會將 IaaS 視為 (Vm) 的虛擬機器，但 IaaS 比起。 當您在 Azure 或 Azure Stack 中部署 VM 時，電腦會隨附軟體定義的網路，包括網域名稱系統、公用 Ip、防火牆規則 (也稱為網路安全性群組) ，以及許多其他功能。 VM 部署也會使用 Azure Blob 儲存體，在軟體定義的儲存體上建立 Vm 的磁片。

如需將 Vm 部署至 Azure Stack 的更深入指引，請參閱 [Azure stack 計算總覽](/azure-stack/user/azure-stack-compute-overview?view=azs-2002)。

## <a name="platform-as-a-service-deployment"></a>平臺即服務部署

在雲端中，所有平臺即服務 (PaaS) 資源會以某種形式的基礎結構服務（例如 VM）執行。 不過，Azure 服務會模糊化這些後端資源，因此您不需要管理這些後端資源。 這些基礎結構資源的混淆和協調是由 Azure Resource Manager 所管理。 使用 Azure Resource Manager 範本部署至 Azure 時，您可能會看到 Resource Manager 的其中一個層面。 這些範本會告訴 Azure 您要叫用的資源提供者，以及您希望如何設定資源。

當雲端在您的資料中心內執行時，您的 stack hub 系統管理員必須稍微熟悉模糊化層級。 在您的使用者或開發人員可以使用 PaaS 資源之前，Azure Stack Hub 的系統管理員必須從 marketplace 安裝資源提供者。 這些資源提供者可讓您的 Azure Stack Hub 實例，在您的 Stack 實例中複寫 Azure 的資源提供者功能。 如需部署 Azure Stack Hub 資源提供者的詳細資訊，請參閱 [Azure Stack IaaS blog 系列](https://azure.microsoft.com/blog/azure-stack-iaas-part-one/)。

## <a name="deploy-workloads"></a>部署工作負載

在 Azure Stack Hub 系統管理員已正確設定您的堆疊實例之後，就可以繼續進行遷移，如同其他大部分的 Azure 遷移工作一樣。 藉由使用 Azure Stack，您的小組可以執行下列任何一種類型的遷移：

<!-- cSpell:ignore howto -->

- [乙太坊區塊鏈網路](/azure-stack/user/azure-stack-ethereum?view=azs-2002)
- [AKS 引擎](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-2002)
- [Azure 認知服務](/azure-stack/user/azure-stack-solution-template-cognitive-services?view=azs-2002)
- [C # ASP.NET web 應用程式](/azure-stack/user/azure-stack-dev-start-howto-vm-dotnet?view=azs-2002)
- [Linux VM](/azure-stack/user/azure-stack-dev-start-howto-deploy-linux?view=azs-2002)
- [Java Web 應用程式](/azure-stack/user/azure-stack-dev-start-howto-vm-java?view=azs-2002)

## <a name="additional-considerations-during-migration"></a>遷移期間的其他考慮

下列文章可協助您的小組在遷移和現代化期間：

- 擴充[性和可用性](https://azure.microsoft.com/blog/azure-stack-iaas-part-six/)服務，例如依使用量付費、vm 可用性設定組、vm 擴展集、網路介面卡，以及新增和調整 vm 和磁片大小的能力
- [儲存體容量](https://azure.microsoft.com/blog/azure-stack-iaas-part-3/)，包括上傳及下載的能力，以及捕獲和部署 VM 映射
- [Azure Stack 快速入門範本](https://github.com/Azure/AzureStack-QuickStart-Templates) GitHub 儲存機制
- [Azure 快速入門範本](https://github.com/Azure/Azure-QuickStart-Templates) GitHub 儲存機制

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)

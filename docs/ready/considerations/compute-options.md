---
title: 檢查您的計算選項
description: 使用適用于 Azure 的雲端採用架構，瞭解如何判斷裝載工作負載的計算需求。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: d5a4f593e62772dbd45fa9aea3eebfb6360fbcf4
ms.sourcegitcommit: 9f5b94ff2a57f17541c9bd706245ae23883ad22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2021
ms.locfileid: "98625713"
---
# <a name="review-your-compute-options"></a>檢查您的計算選項

當您準備雲端採用時，決定裝載工作負載的計算需求是一項重要考量。 Azure 計算產品和服務支援各種不同的工作負載計算案例和功能。 要如何設定登陸區域環境來支援計算需求，取決於您的工作負載管理、技術和商務需求。

## <a name="identify-compute-services-requirements"></a>識別計算服務需求

在登陸區域的評估和準備過程中，您需要識別登陸區域必須支援的所有計算資源。 此程序牽涉到評估組成工作負載的每個應用程式和服務，以判斷計算和裝載需求。 在您識別並記錄需求之後，您可以為登陸區域建立原則，以根據您的工作負載需求來控制允許的資源類型。

針對您要部署到登陸區域環境的每個應用程式或服務，請使用下列決策樹作為起點，以協助您判斷計算服務需求：

![Azure 計算服務決策樹的圖表。](../../_images/ready/compute-decision-tree.png)

_圖1： Azure 計算服務決策樹。_

定義：

- 「隨即轉移」是一種將工作負載遷移至雲端，而不需要重新設計應用程式或進行程式碼變更的策略。 也稱為重新裝載。 如需詳細資訊，請參閱 Azure 移轉中心。
- 「雲端優化」是一種將應用程式重構以利用雲端原生特性和功能來遷移至雲端的策略。

此流程圖的輸出是要考慮的起點。 接下來，請執行更詳細的服務評估，以查看是否符合您的需求。

> [!NOTE]
> 在 [Azure 應用程式架構指南](/azure/architecture/guide/technology-choices/compute-decision-tree)中，深入了解如何評估每個應用程式或服務的計算選項。

### <a name="key-questions"></a>重要問題

回答下列有關工作負載的問題，以協助您根據 Azure 計算服務決策樹來做出決策：

- **您是否正在建立全新的應用程式和服務，或從現有的內部部署工作負載遷移？** 開發新的應用程式作為雲端採用工作的一部分，可讓您從設計階段開始，充分利用新式雲端式裝載技術。
- **如果您要遷移現有的工作負載，可以利用新式雲端技術嗎？** 遷移內部部署工作負載需要分析。 您可以輕鬆地將現有的應用程式和服務優化以利用新式雲端技術，也可以讓您的工作負載使用隨即轉移方法更好嗎？
- **您的應用程式或服務可以利用容器嗎？** 如果您的應用程式是容器化裝載的絕佳候選項目，您可以利用 [Azure 中的容器服務](https://azure.microsoft.com/product-categories/containers)所提供的資源效率、擴充性和協調流程功能。 [Azure 受控磁片](/azure/virtual-machines/windows/managed-disks-overview)和[Azure 檔案儲存體](/azure/storage/files/storage-files-introduction)都可用於容器化應用程式中的持續性儲存體。
- **您的應用程式是以 web 或 API 為基礎，而且會使用 PHP、ASP.NET、Node.js 或類似的技術嗎？** Web 應用程式可以部署到受控的 [Azure App Service](/azure/app-service/overview) 執行個體，因此您不需要維護裝載用的虛擬機器。
- **您需要完整控制您工作負載的作業系統和裝載環境嗎？** 如果您需要控制裝載環境 (包括作業系統、磁碟、本機執行的軟體和其他設定)，您可以使用 [Azure 虛擬機器](https://azure.microsoft.com/services/virtual-machines)來裝載您的應用程式和服務。 除了選擇您的虛擬機器大小和效能層級，您對於虛擬磁片儲存體的決策將會影響基礎結構即服務工作負載的相關效能和 Sla。 如需詳細資訊，請參閱 [Azure 磁片儲存體](/azure/virtual-machines/windows/managed-disks-overview) 檔。
- **您的工作負載是否牽涉到高效能運算 (HPC) 功能？** [Azure Batch](/azure/batch/batch-technical-overview) 能以平台服務的形式為電腦資源提供工作排程及自動調整服務，讓您在雲端輕鬆執行大規模平行應用程式和 HPC 應用程式。
- **您的應用程式會使用微服務架構嗎？** 使用微服務架構的應用程式可以利用數個最佳化的計算技術。 由事件驅動的獨立工作負載可以使用 [Azure Functions](/azure/azure-functions/functions-overview) 來建立可調整的無伺服器應用程式，其不需要基礎結構。 對於需要更充分掌控微服務執行所在環境的應用程式，您可以使用容器服務，例如 [Azure 容器](/azure/container-instances/container-instances-overview)、[Azure Kubernetes Service](/azure/aks/intro-kubernetes) 和 [Azure Service Fabric](/azure/service-fabric/service-fabric-overview)。

> [!NOTE]
> 大部分的 Azure 計算服務都會與 Azure 儲存體搭配使用。 如需了解相關的儲存體決策，請參閱[儲存體決策指引](./storage-options.md)。

## <a name="common-compute-scenarios"></a>常見計算案例

下表說明一些常見的使用案例，以及用來處理這些需求的建議計算服務：

| 狀況  | 計算服務 |
| --- | --- |
| 我需要透過選擇的設定，在幾秒內佈建 Linux 和 Windows 虛擬機器。 | [Azure 虛擬機器](https://azure.microsoft.com/services/virtual-machines) |
| 我需要透過自動調整達到高可用性，以用幾分鐘的時間建立數千部 VM。 | [虛擬機器擴展集](https://azure.microsoft.com/services/virtual-machine-scale-sets) |
| 我想簡化 Kubernetes 的部署、管理與作業。 | [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service) |
| 我需要使用事件驅動的無伺服器架構來加快應用程式開發的速度。 | [Azure Functions](https://azure.microsoft.com/services/functions) |
| 我需要在 Windows 或 Linux 上開發微服務及協調容器。 | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric) |
| 我想要使用完全受控的平臺，快速建立 web 和行動裝置的雲端應用程式。 | [Azure App Service](https://azure.microsoft.com/services/app-service) |
| 我想要使用單一命令來將應用程式，並輕鬆地執行容器。 | [Azure 容器執行個體](https://azure.microsoft.com/services/container-instances) |
| 我需要雲端規模的作業排程和計算管理可調整成數十、數百或數千部虛擬機器。 | [Azure Batch](https://azure.microsoft.com/services/batch) |
| 我需要建立高度可用、可擴充的雲端應用程式和 Api，以協助我專注于應用程式，而不是硬體。 | [Azure 雲端服務](https://azure.microsoft.com/services/cloud-services) |

## <a name="regional-availability"></a>區域可用性

Azure 可讓您以所需的規模，將服務提供給客戶和 **合作夥伴。** 規劃雲端部署的關鍵要素是判斷哪個 Azure 區域可託管您的工作負載資源。

某些計算選項（例如 Azure App Service）已在大部分的 Azure 區域中正式運作，而其他計算服務僅在特定區域中受到支援。 某些虛擬機器類型及其相關聯儲存體類型的區域可用性有限。 在您決定要部署計算資源的區域之前，建議您先參閱 [ [區域] 頁面](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=azure-vmware-cloudsimple,cloud-services,batch,container-instances,app-service,service-fabric,functions,kubernetes-service,virtual-machine-scale-sets,virtual-machines) ，以檢查區域可用性的最新狀態。

若要深入瞭解 Azure 全球基礎結構，請參閱 [azure 區域頁面](https://azure.microsoft.com/global-infrastructure/regions)。 您也可以查看 [依區域提供的產品](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=all) ，以取得每個 Azure 區域中可用之整體服務的特定詳細資料。

## <a name="data-residency-and-compliance-requirements"></a>資料落地和合規性需求

您的工作負載中通常會有與資料儲存體相關的法律和合約需求。 這些需求可能會因為您組織的位置、存放和處理檔案和資料的管轄權，以及您適用的商務部門而有所不同。 需要考量的資料責任包括資料分類、資料位置，以及共同責任模式下的個別資料保護責任。 許多計算解決方案取決於連結的儲存體資源。 這項需求也可能會影響您的計算決策。 如需瞭解這些需求的協助，請參閱 [使用 Azure 達成符合規範的資料落地和安全性](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure)的白皮書。

合規性工作的一部分可能包括控計算資源實際所在的位置。 Azure 區域會在稱為 geographies 的群組中進行排列。 [Azure 地理](https://azure.microsoft.com/global-infrastructure/geographies)可確保符合地理及政治界限內的資料落地、主權、合規性及復原需求。 如果您的工作負載受限於資料主權或其他合規性需求，您必須將儲存體資源部署到合規 Azure 地理位置中的區域。

## <a name="establish-controls-for-compute-services"></a>建立計算服務的控制項

當您準備登陸區域環境時，您可以建立控制項來限制每位使用者可以部署的資源。 控制項可協助您管理成本並限制安全性風險，同時仍可讓開發人員和 IT 小組部署及設定支援您工作負載所需的資源。

識別並記下登陸區域的需求之後，您可以使用 [Azure 原則](/azure/governance/policy/overview)來控制允許使用者建立的計算資源。 控制項可以採用[允許或拒絕建立計算資源類型](/azure/governance/policy/samples/allowed-resource-types)的形式。 例如，您可能會限制使用者只能建立 Azure App Service 或 Azure Functions 資源。 您也可以在建立資源時使用原則來控制允許的選項，例如[限制可以佈建的虛擬機器 SKU](/azure/governance/policy/samples/built-in-policies#compute)，或是[只允許特定 VM 映像](/azure/governance/policy/samples/allowed-custom-images)。

原則的範圍可以設定為資源、資源群組、訂用帳戶和管理群組。 您可以將原則包含在 [Azure 藍圖](/azure/governance/blueprints/overview) 定義中，並在整個雲端資產中重複套用。

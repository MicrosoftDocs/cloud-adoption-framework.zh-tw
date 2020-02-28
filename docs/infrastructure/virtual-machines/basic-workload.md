---
title: 在 Azure 中部署基本工作負載
description: 瞭解核心雲端基礎結構元件和基本工作負載，例如基本 web 應用程式、單一虛擬機器和虛擬網路。
author: alexbuckgit
ms.author: abuck
ms.date: 12/31/2018
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: e720fb698e0eecb53942bb5d7be99df5e923451e
ms.sourcegitcommit: 10637acba8c857a6f5aa8c4a80c0649903f60402
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2020
ms.locfileid: "78170693"
---
# <a name="deploy-a-basic-workload-in-azure"></a>在 Azure 中部署基本工作負載

「工作負載」這個詞彙通常被定義為任意的功能單位，例如應用程式或服務。 這有助於以部署到伺服器的程式碼成品，以及應用程式特定的其他服務來思考工作負載。 針對內部部署應用程式或服務，這可能是實用的定義，但對於雲端應用程式而言則需要擴充。

在雲端中，工作負載不僅是包括所有成品，同時也包含雲端資源。 定義當中包含雲端資源，是因為所謂「基礎結構即程式碼」的概念。 如同您在 [Azure 的運作方式？](../../getting-started/what-is-azure.md)說明中所了解的，Azure 中的資源是由協調器服務部署。 此協調器服務會透過 Web API 公開功能，您可以使用數種工具（例如 PowerShell、Azure CLI 和 Azure 入口網站）來呼叫 Web API。 這表示您可以在電腦可讀取的檔案中指定 Azure 資源，該檔案可以與應用程式相關聯的程式碼成品 一起儲存。

這可讓您以程式碼成品和必要雲端資源的角度定義工作負載，進一步讓您隔離工作負載。 您可以依據資源的組織方式、網路拓撲或其他屬性來隔離工作負載。 工作負載隔離的目標是要將工作負載的特定資源關聯至小組，讓小組可以獨立管理這些資源的所有層面。 這可讓多個小組共用 Azure 中的資源管理服務，同時防止不小心刪除或修改彼此的資源。

這個隔離也會實現稱為 DevOps 的另一個概念。 DevOps 包含軟體開發實務，其中包含上述的軟體開發和 IT 作業，且盡可能新增自動化的使用。 DevOps 的其中一個原則稱為持續整合與持續傳遞 (CI/CD)。 持續整合是指每次開發人員認可程式碼變更時，都會執行的自動化建置程序。 持續傳遞是指將這個程式碼部署到各種環境 (例如進行測試的開發環境或進行最終部署的生產環境) 的自動化程序。

## <a name="basic-workload"></a>基本工作負載

「基本工作負載」通常定義為單一 Web 應用程式，或包含虛擬機器 (VM) 的虛擬網路 (VNet)。

> [!NOTE]
> 本指南並未涵蓋應用程式開發。 如需有關在 Azure 上開發應用程式的詳細資訊，請參閱 [Azure 應用程式架構指南](https://docs.microsoft.com/azure/architecture/guide)。

不論工作負載是 Web 應用程式或 VM，每個部署都需要*資源群組*。 具有權限建立資源群組的使用者必須先建立資源群組，然後再遵循下列步驟。

## <a name="basic-web-application-paas"></a>基本 Web 應用程式 (PaaS)

針對基本 Web 應用程式，請從 [Web 應用程式文件](https://docs.microsoft.com/azure/app-service)中選取其中一個 5 分鐘快速入門，並遵循其中的步驟。

> [!NOTE]
> 部分快速入門指南預設會部署資源群組。 在此情況下，不需要明確建立的資源群組。 否則，請將 Web 應用程式部署到先前建立的資源群組。

部署簡單的工作負載之後，您可以深入瞭解將[基本 web 應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)部署至 Azure 的最佳作法。

## <a name="single-windows-or-linux-vm-iaas"></a>單一 Windows 或 Linux VM (IaaS)

針對在 VM 上執行的簡單工作負載，第一個步驟是部署虛擬網路。 Azure 中的所有基礎結構即服務 (IaaS) 資源 (例如虛擬機器、負載平衡器和閘道) 都需要虛擬網路。 請了解 [Azure 虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)，然後遵循步驟以[使用入口網站將虛擬網路部署至 Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)。 當您在 Azure 入口網站中指定虛擬網路的設定時，請務必指定先前建立的資源群組名稱。

下一個步驟是決定要部署單一 Windows 或 Linux VM。 針對 Windows VM，請遵循步驟以[使用入口網站將 Windows VM 部署至 Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)。 同樣地，當您在 Azure 入口網站中指定虛擬機器的設定時，請為先前建立的資源群組指定名稱。

遵循步驟並部署 VM 之後，您可以瞭解[在 Azure 上執行 WINDOWS VM 的最佳作法](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/single-vm)。 針對 Linux VM，請遵循步驟以[使用入口網站將 Linux VM 部署至 Azure](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)。 您也可以深入瞭解[在 Azure 上執行 LINUX VM 的最佳作法](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-linux/single-vm)。

## <a name="next-steps"></a>後續步驟

請參閱[架構決策指南](../../decision-guides/index.md)，了解如何使用 Azure 雲端中的核心基礎結構元件。

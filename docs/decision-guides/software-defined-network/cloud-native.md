---
title: 軟體定義網路：雲端原生
description: 使用適用于 Azure 的雲端採用架構來瞭解將 Vm 部署至雲端時所需的雲端原生虛擬網路。
author: rotycenh
ms.author: abuck
ms.date: 02/11/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: a69c196d76db7f633acdfcf0aff6ad72a11872e5
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83215019"
---
# <a name="software-defined-networking-cloud-native"></a>軟體定義網路：雲端原生

將 IaaS 資源（例如虛擬機器）部署到雲端平臺時，需要雲端原生虛擬網路。 從外部來源 (類似於 Web) 對虛擬網路的存取需要明確佈建。 這些類型的虛擬網路支援建立子網路、路由規則和虛擬防火牆，以及流量管理裝置。

雲端原生虛擬網路不會相依于您組織的內部部署或其他非雲端資源，以支援雲端裝載的工作負載。 所有必要的資源都會佈建在虛擬網路本身，或使用受控 PaaS 供應項目進行佈建。

## <a name="cloud-native-assumptions"></a>雲端原生假設

部署雲端原生虛擬網路會假設下列各項：

- 您對虛擬網路部署的工作負載，與只能從您內部部署網路內部存取的應用程式和密碼，兩者之間並沒有相依性。 除非它們提供可透過公用網際網路存取的端點，否則裝載于內部部署的應用程式和服務無法供雲端平臺上託管的資源使用。
- 您工作負載的身分識別管理和存取控制取決於雲端平台的身分識別服務或雲端環境中裝載的 IssS 伺服器。 您將不需要直接連線到內部部署或其他外部位置裝載的身分識別服務。
- 您的身分識別服務不需要支援單一登入 (SSO) 與內部部署目錄。

雲端原生虛擬網路沒有外部相依性。 這讓它們能夠很容易地部署和設定，因此此架構通常是適用於實驗用途或其他較小型獨立或快速重複部署的最佳選擇。

在討論雲端原生虛擬網路架構時，您的雲端採用小組應考慮的其他問題包括：

- 設計成在內部部署資料中心執行的現有工作負載可能需要大規模修改，才能善用雲端的功能，例如儲存體或驗證服務。
- 雲端原生網路僅透過雲端平臺管理工具進行管理，因此可能會在一段時間後，從您現有的 IT 標準進行管理和原則分歧。

## <a name="next-steps"></a>後續步驟

如需 Azure 中雲端原生虛擬網路的詳細資訊，請參閱：

- [Azure 虛擬網路指南](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)：新建立的虛擬網路預設為雲端原生。 您可以使用這些指南，協助規劃虛擬網路的設計與部署。
- [Azure 網路限制](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits)：每個虛擬網路和連線的資源都存在於單一訂用帳戶中。 這些資源會受到訂用帳戶限制所約束。

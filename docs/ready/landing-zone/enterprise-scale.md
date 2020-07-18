---
title: 在 Azure 中部署 CAF enterprise scale 登陸區域
description: 了解如何在 Azure 中部署移轉登陸區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/27/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 9c2fb6fe013446373e198b9caff033c8d9c8cd6b
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86448243"
---
# <a name="deploy-a-caf-enterprise-scale-landing-zone-in-azure"></a>在 Azure 中部署 CAF enterprise scale 登陸區域

企業級登陸區域是已布建的環境，其中包含裝載更複雜工作負載所需的所有支援資源。 從企業級登陸區域開始，可讓您裝載任務關鍵性且安全的工作負載，以配合第一個部署的安全性、治理、合規性和操作需求。

## <a name="approach"></a>方法

準備好方法中所述的_開始小型_方法從單一登陸區域開始，並整合了更多的企業功能，因為雲端採用方案會朝著企業規模發展。 該方法會建立與每個發行或遷移 wave 的需求一致的結構化實際操作學習路徑。 使用該方法，登陸區域可能會支援生產工作負載，但它們並不是真正符合企業需求的前幾個版本。

當客戶需要更快速地取得企業準備時，需要更複雜的架構。 單一登陸區域無法在企業規模隔離的情況中提供。 企業級架構需要企業就緒的環境，以裝載符合企業需求的登陸區域。 這兩種方法都遵循相同的原則和最佳作法。 但企業級方法會透過單一 JSON 檔案套用多個最佳做法，此檔案會使用 GitHub 動作和部署管線來部署環境的所有層面，包括多個登陸區域。 這種高度固定的執行方式會使用相同的 ARM 範本、Azure 原則和用於較小藍圖的 RBAC 工具。

一旦部署這兩種方法之後，請遵循基礎結構即程式碼和測試導向的開發模型，以擴充登陸區域以符合您未來的登陸區域需求。

## <a name="deploy-the-first-landing-zone"></a>部署第一個登陸區域

在您使用企業級的方法和實施之前，請先參閱下列假設、決策和實施指引。 如果本指南與您的雲端採用方案一致，企業級環境和登陸區域可以使用企業級的[執行](../enterprise-scale/implementation.md)方式進行部署。

> [!div class="nextstepaction"]
> [執行企業級登陸區域解決方案](../enterprise-scale/implementation.md)

## <a name="assumptions"></a>假設

此方法和架構包含下列假設。 如果這些假設符合您的需求和條件約束，您可以使用此實現來設定您的環境，並部署您的第一個登陸區域。 部署完成後，您可以擴充登陸區域和環境以符合您未來的登陸區域需求，以自訂執行。

- **訂用帳戶限制：** 不同于其他登陸區域選項，此架構不受訂用帳戶[限制](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)所限制。
- **合規性：** 此實施不會部署任何協力廠商合規性需求，但可輕鬆新增。
- **架構複雜度：** 不同于其他選項，此登陸區域和環境可以裝載之工作負載的架構複雜性沒有任何限制。
- **共用服務：** 共用服務的設定與 Microsoft 最佳作法一致。
- **有限的生產範圍：** 此登陸區域的預設設定較可能符合您的關鍵性和敏感性資料裝載需求。
- **需要先進的技能：** 採用這種方法的小組會大幅提升技術技能需求。 深入瞭解[企業級登陸區域總覽](../enterprise-scale/index.md)的技術技能需求。

如果這些假設符合您目前的採用需求，則這項執行可能是建立登陸區域和環境的好起點。

## <a name="decisions"></a>決策

如需塑造企業規模執行的完整決策，請參閱[實施指導方針](../enterprise-scale/implementation-guidelines.md)。

## <a name="customize-or-deploy-a-landing-zone"></a>自訂或部署登陸區域

如需部署企業級執行的範例，請參閱[Contoso 企業規模的執行指南](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md)。

若要自訂企業規模的架構，請參閱[設計方針](../enterprise-scale/design-guidelines.md)。

## <a name="next-steps"></a>後續步驟

部署您的第一個登陸區域之後，您就可以開始[擴充您的登陸區域](../considerations/index.md)

> [!div class="nextstepaction"]
> [擴充登陸區域](../considerations/index.md)

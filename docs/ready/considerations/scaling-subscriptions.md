---
title: 使用多個 Azure 訂用帳戶進行調整
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解如何使用多個 Azure 訂用帳戶進行調整。
author: alexbuckgit
ms.author: abuck
ms.date: 05/20/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: be35763ea3beeec5977073dab8ef98c2e441b537
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72548783"
---
# <a name="scaling-with-multiple-azure-subscriptions"></a>使用多個 Azure 訂用帳戶進行調整

組織通常需要一個以上的 Azure 訂用帳戶，做為資源限制和其他治理考慮的結果。 調整訂用帳戶的策略非常重要。

## <a name="production-and-nonproduction-workloads"></a>生產和非生產工作負載

在 Azure 中部署您的第一個生產工作負載時，您應該從兩個訂用帳戶開始：一個用於生產環境，另一個用於您的非生產（開發/測試）環境。

![基本訂用帳戶模型，會在標示為「生產」和「非生產」的方塊旁顯示金鑰](../../_images/ready/basic-subscription-model.png)

我們之所以建議採用這種方法，是因為以下幾個原因：

- Azure 有適用於開發/測試工作負載的特定訂用帳戶供應項目。 這些供應項目會為 Azure 服務和授權提供折扣費率。
- 您的生產和非生產環境可能會有不同組的 Azure 原則。 使用個別訂用帳戶可讓您輕鬆地在訂用帳戶層級上套用每個不同的原則集合。
- 在開發/測試訂用帳戶中，您可能會想要有特定類型的 Azure 資源來進行測試。 透過個別的訂用帳戶，您無須將這些資源類型放到生產環境，即可加以使用。
- 您可以使用開發/測試訂用帳戶作為隔離的沙箱環境。 這類沙箱可讓系統管理員和開發人員快速建立及卸載整組 Azure 資源。 這種隔離也有助於資料保護和安全性考量。
- 生產環境與開發/測試訂用帳戶可接受的成本閾值可能會有差異。

## <a name="other-reasons-for-multiple-subscriptions"></a>使用多個訂用帳戶的其他原因

其他情況也可能需要額外的訂用帳戶。 當您擴充雲端資產時，請記住下列事項。

- 針對不同資源類型，訂用帳戶會有不同的限制。 例如，訂用帳戶中的虛擬網路數目會受限。 當訂用帳戶快達到其任何限制時，您就需要建立另一個訂用帳戶，並將新資源放入該處。

  如需詳細資訊，請參閱 [Azure 訂用帳戶和服務限制、配額與條件約束](https://docs.microsoft.com/azure/azure-subscription-service-limits)。

- 每個訂用帳戶都可以針對可部署的資源類型和支援的區域，執行自己的原則。

- 公用雲端區域和主權或政府雲端區域中的訂用帳戶有不同的限制。 這些通常是由環境間不同的資料分類層級所驅動。

- 如果您因為安全性或合規性而完全隔離不同的使用者集合，您可能需要個別的訂用帳戶。 例如，國家政府組織可能需要將訂用帳戶的存取許可權制為僅限公民。

- 不同的訂用帳戶可能會有不同類型的供應項目，每個供應項目都有自己的條款和權益。

- 訂用帳戶擁有者和要部署的資源擁有者之間可能存在信任問題。 使用具有不同擁有權的另一個訂用帳戶可減輕這些問題，不過，您也必須考慮這項部署將會發生的網路和資料保護問題。

- 固定的財務或地緣政治控制項可能需要針對特定訂用帳戶進行不同的財務安排。 這些考慮可能包括資料主權的考慮、具有多個子公司的公司，或是不同國家/地區和不同貨幣之業務單位的個別會計和計費。

- 使用傳統部署模型所建立的 Azure 資源應該會隔離在自己的訂用帳戶中。 傳統資源的安全性與透過 Azure Resource Manager 部署的資源不同。 Azure 原則無法套用至傳統資源。

- 使用傳統資源的服務管理員與訂用帳戶的角色型存取控制 (RBAC) 擁有者具有相同的權限。 若訂用帳戶中混合傳統資源和 Resource Manager 資源，則很難將這些服務管理員的存取權全部壓縮到此訂用帳戶中。

您也可以選擇針對您組織特有的其他商業或技術原因，來建立其他訂用帳戶。 訂用帳戶之間的資料輸入和輸出可能會有一些額外成本。

您可以將許多資源類型從一個訂用帳戶移至另一個訂用帳戶，或使用自動化部署將資源遷移至另一個訂用帳戶。 如需詳細資訊，請參閱[將 Azure 資源移至另一個資源群組戶或訂用帳](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)。

## <a name="managing-multiple-subscriptions"></a>管理多個訂用帳戶

如果您只有幾個訂用帳戶，獨立管理這些訂用帳戶相當簡單。 但如果您有許多訂用帳戶，您應該考慮建立管理群組階層，以簡化您的訂用帳戶及資源管理。

管理群組可讓您有效率地管理組織訂閱的存取、原則和合規性。 每個管理群組都是一個或多個訂用帳戶的容器。

管理群組會以單一階層的方式排列。 您會在 Azure Active Directory （Azure AD）租使用者中定義此階層，以符合您組織的結構和需求。 最上層稱為「根管理群組」。 您最多可以在階層中定義六個層級的管理群組。 每個訂用帳戶只能包含在一個管理群組中。

Azure 提供四個管理範圍層級：管理群組、訂用帳戶、資源群組和資源。 階層中某一層級所套用的任何存取或原則，都會由其底下的層級繼承。 資源擁有者或訂用帳戶擁有者無法改變繼承的原則。 這項限制有助於改善治理方式。

> [!NOTE]
> 請注意，標記繼承目前無法使用，但很快就會變成可用。

藉由依賴此繼承模型，您可以在階層中安排訂用帳戶，讓每個訂用帳戶都遵循適當的原則和安全性控制項。

![用來組織 Azure 資源的四個範圍層級](../../ready/azure-setup-guide/media/organize-resources/scope-levels.png)

根管理群組上的任何存取權或原則指派，都會套用至目錄中的所有資源。 請仔細考慮要在此範圍上定義的項目。 這應該僅包含您必須擁有的指派。

當您一開始定義管理群組階層時，您會先建立根管理群組。 然後將目錄中所有現有的訂用帳戶移到根管理群組。 新的訂用帳戶一律會建立在根管理群組中。 您之後可以將這些訂用帳戶移至另一個管理群組。

當您將訂用帳戶移至現有的管理群組時，該訂用帳戶就會繼承其上方管理群組階層中的原則和角色指派。 當您為 Azure 工作負載建立多個訂用帳戶之後，您應該建立額外的訂用帳戶來包含其他訂用帳戶共用的 Azure 服務。

![管理群組階層的範例](../../_images/ready/management-group-hierarchy.png)

如需詳細資訊，請參閱[使用 Azure 管理群組來組織資源](https://docs.microsoft.com/azure/governance/management-groups)。

## <a name="tips-for-creating-new-subscriptions"></a>建立新訂用帳戶的秘訣

- 識別負責建立新訂用帳戶的人員。
- 決定訂用帳戶中的預設資源。
- 決定所有標準訂用帳戶的型態。 這些考量包括 RBAC 存取、原則、標記和基礎結構資源。
- 可能的話，請[使用服務主體](https://docs.microsoft.com/azure/azure-resource-manager/grant-access-to-create-subscription)來建立新的訂用帳戶。 定義可透過自動化工作流程要求新訂用帳戶的安全性群組。
- 如果您是 Enterprise 合約 (EA) 客戶，請要求 Azure 支援服務為您的組織封鎖非 EA 訂用帳戶的建立。

## <a name="related-resources"></a>相關資源

- [Azure 基礎概念](./fundamental-concepts.md)。
- [使用 Azure 管理群組來組織資源](https://docs.microsoft.com/azure/governance/management-groups)。
- [提高存取權以管理所有 Azure 訂用帳戶和管理群組](https://docs.microsoft.com/azure/role-based-access-control/elevate-access-global-admin)。
- [將 Azure 資源移至另一個資源群組或訂用帳戶](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)。

## <a name="next-steps"></a>後續步驟

部署 Azure 資源時，檢閱並遵循[建議的命名和標記慣例](./naming-and-tagging.md)。

> [!div class="nextstepaction"]
> [建議的命名和標記慣例](./naming-and-tagging.md)

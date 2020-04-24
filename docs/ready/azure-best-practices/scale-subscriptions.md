---
title: 建立額外的訂用帳戶來調整您的 Azure 環境
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何使用多個 Azure 訂用帳戶開發調整環境的策略。
author: alexbuckgit
ms.author: abuck
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 465f1771232f8df7226773fb8055052a846cacca
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80997953"
---
# <a name="create-additional-subscriptions-to-scale-your-azure-environment"></a>建立額外的訂用帳戶來調整您的 Azure 環境

組織通常會使用多個 Azure 訂用帳戶來避免每個訂用帳戶的資源限制，並更有效地管理和管控其 Azure 資源。 請務必定義調整訂閱的策略。

## <a name="review-fundamental-concepts"></a>回顧基本概念

當您將 Azure 環境擴充到您的[初始](./initial-subscriptions.md)訂用帳戶之外時，請務必瞭解 azure 概念，例如帳戶、租使用者、目錄和訂用帳戶。 如需詳細資訊，請參閱[Azure 基本概念](../considerations/fundamental-concepts.md)。

其他考慮可能需要額外的訂閱。 當您擴充雲端資產時，請記住下列事項。

## <a name="technical-considerations"></a>技術考量

**訂用帳戶限制：** 訂閱已定義某些資源類型的限制。 例如，訂用帳戶中的虛擬網路數目會受限。 當訂用帳戶接近這些限制時，您將需要建立另一個訂用帳戶，並在該處放入其他資源。 如需詳細資訊，請參閱[Azure 訂用帳戶和服務限制](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#general-limits)。

**傳統模型資源：** 如果您已使用 Azure 一段很長的時間，您可能會有使用傳統部署模型建立的資源。 Azure 原則、角色型存取控制、資源群組和標記無法套用至傳統模型資源。 您應該將這些資源移入僅包含傳統模型資源的訂用帳戶。

**成本：** 在訂用帳戶之間的資料輸入和輸出可能會有一些額外的成本。

## <a name="business-priorities"></a>商業優先順序

您的業務優先順序可能會導致您建立其他訂用帳戶。 這些優先順序包括：

- 創新
- 移轉
- 成本
- 作業
- 安全性
- 控管

如需有關調整訂閱的其他考慮，請參閱雲端採用架構中的訂用帳戶[決策指南](../../decision-guides/subscriptions/index.md)。

## <a name="moving-resources-between-subscriptions"></a>在訂用帳戶之間移動資源

當您的訂用帳戶模型成長時，您可能會決定某些資源屬於其他訂閱。 可以在訂用帳戶之間移動許多類型的資源。 您也可以使用自動化部署來重新建立另一個訂用帳戶中的資源。 如需詳細資訊，請參閱[將 Azure 資源移至另一個資源群組戶或訂用帳](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription)。

## <a name="tips-for-creating-new-subscriptions"></a>建立新訂用帳戶的秘訣

- 識別負責建立新訂閱的人員。
- 根據預設，決定訂用帳戶中可用的資源類型。
- 決定所有標準訂用帳戶的型態。 這些考量包括 RBAC 存取、原則、標記和基礎結構資源。
- 可能的話，請以程式設計方式透過服務主體[建立新的訂閱](https://docs.microsoft.com/azure/azure-resource-manager/management/programmatically-create-subscription)。 您必須[授與許可權給服務主體](https://docs.microsoft.com/azure/azure-resource-manager/grant-access-to-create-subscription)，才能建立訂閱。 定義可透過自動化工作流程要求新訂用帳戶的安全性群組。
- 如果您是 Enterprise 合約 (EA) 客戶，請要求 Azure 支援服務為您的組織封鎖非 EA 訂用帳戶的建立。

## <a name="next-steps"></a>後續步驟

建立管理群組階層，以協助[組織和管理您的](./organize-subscriptions.md)訂用帳戶和資源。

> [!div class="nextstepaction"]
> [組織和管理您的訂用帳戶和資源](./organize-subscriptions.md)

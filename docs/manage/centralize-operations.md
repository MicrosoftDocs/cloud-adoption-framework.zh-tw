---
title: 集中化管理作業
description: 瞭解如何使用適用于所有使用者的單一 Azure Active Directory 租使用者，來集中管理作業。 集中式管理可簡化管理作業，並降低維護成本。
author: JnHs
ms.author: jenhayes
ms.date: 09/27/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 8dbfd5324957b7175afc81801873eef8bf5242cf
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88571825"
---
<!-- cSpell:ignore jenhayes -->

# <a name="centralize-management-operations"></a>集中化管理作業

對於大部分的組織而言，使用單一 Azure Active Directory (Azure AD) 租使用者的所有使用者都能簡化管理作業，並降低維護成本。 這是因為所有管理工作都可以透過指定的使用者、使用者群組或該租使用者內的服務主體來進行。

如果可能的話，我們建議您只針對您的組織使用一個 Azure AD 的租使用者。 不過，在某些情況下，可能會因為下列原因而需要組織維護多個 Azure AD 租使用者：

- 它們是完全獨立的分公司。
- 它們是在多個地理位置上獨立運作。
- 適用特定的法律或合規性需求。
- 另外還有其他組織的收購 (有時是暫時性的，直到長期的租使用者合併策略) 定義為止。

當需要多租使用者架構時， [Azure Lighthouse](/azure/lighthouse/overview) 提供了一種方式來集中化和簡化管理作業。 您可以上線多個租使用者的訂用帳戶，以進行 [Azure 委派的資源管理](/azure/lighthouse/concepts/azure-delegated-resource-management)。 此選項可讓管理租使用者中指定的使用者以集中且可調整的方式執行 [跨租使用者管理功能](/azure/lighthouse/concepts/cross-tenant-management-experience) 。

例如，假設您的組織有單一租使用者 `Tenant A` 。 然後，組織會取得兩個額外的租使用者，而且 `Tenant B` `Tenant C` 您有商務原因需要您將其維護為個別的租使用者。

貴組織想要在所有租用戶中使用相同的原則定義、備份做法和安全性程序。 因為您已經有使用者 (包括負責在租使用者 A 中執行這些工作的使用者群組和服務主體) ，所以您可以將租使用者 B 和租使用者 C 內的所有訂用帳戶上架，讓租使用者 A 中的相同使用者可以執行這些工作。 租使用者 A 接著會成為租使用者 B 和租使用者 C 的管理租使用者。

![租用戶 A 中的使用者管理租用戶 B 和租用戶 C 中的資源](../_images/manage/enterprise-azure-lighthouse.jpg)

如需詳細資訊，請參閱 [企業案例中的 Azure Lighthouse](/azure/lighthouse/concepts/enterprise)。

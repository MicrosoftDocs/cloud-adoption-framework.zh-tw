---
title: 集中化管理作業
description: 瞭解如何使用適用于所有使用者的單一 Azure Active Directory 租使用者來集中管理作業。 集中式管理可簡化管理作業並降低維護成本。
author: JnHs
ms.author: jenhayes
ms.date: 09/27/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 312be5ae3b716ad8d6aa609749bcbb615f6ef1c5
ms.sourcegitcommit: 388e32dd4861039149c846c926c0e9230cf28ae3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79140417"
---
<!-- cSpell:ignore jenhayes -->

# <a name="centralize-management-operations"></a>集中化管理作業

對於大部分的組織而言，針對所有使用者使用單一 Azure Active Directory （Azure AD）租使用者，可簡化管理作業並降低維護成本。 這是因為所有管理工作都可以由指定的使用者、使用者群組或該租使用者內的服務主體所組成。

我們建議您盡可能僅針對貴組織使用一個 Azure AD 租使用者。 不過，某些情況可能需要組織維護多個 Azure AD 的租使用者，原因如下：

- 它們是完全獨立的子公司。
- 它們會在多個地理位置中獨立運作。
- 適用特定的法律或合規性需求。
- 還有其他組織的收購（有時是暫時性的，直到定義了長期的租使用者匯總策略為止）。

當需要多租使用者架構時， [Azure 燈塔](https://docs.microsoft.com/azure/lighthouse/overview)會提供一種方式來集中化和簡化管理作業。 可以上架多個租使用者的訂用帳戶，以進行[Azure 委派的資源管理](https://docs.microsoft.com/azure/lighthouse/concepts/azure-delegated-resource-management)。 此選項可讓管理租使用者中的指定使用者以集中且可擴充的方式執行[跨租使用者管理功能](https://docs.microsoft.com/azure/lighthouse/concepts/cross-tenant-management-experience)。

例如，假設您的組織有一個租使用者*a*。然後，組織會取得兩個額外的*租使用者 B*和租使用者*C*，而且您有商業理由要求您將其維護為個別的租使用者。

貴組織想要在所有租用戶中使用相同的原則定義、備份做法和安全性程序。 因為您已經有負責在租使用者 A 中執行這些工作的使用者（包括使用者群組和服務主體），所以您可以將租使用者 B 和租使用者 C 內的所有訂用帳戶上架，讓租使用者 A 中的這些訂用帳戶可以執行這些作業任務. 然後，租使用者 A 會成為租使用者 B 和租使用者 C 的管理租使用者。

![租用戶 A 中的使用者管理租用戶 B 和租用戶 C 中的資源](../_images/manage/enterprise-azure-lighthouse.jpg)

如需詳細資訊，請參閱[企業案例中的 Azure 燈塔](https://docs.microsoft.com/azure/lighthouse/concepts/enterprise)。

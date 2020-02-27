---
title: 什麼是雲端資源治理？
description: 瞭解管理、監視和審核 Azure 資源的程式，以符合您組織的目標和需求。
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.openlocfilehash: fdf63fd86b69a1c42276e5847d53deb71fcd2530
ms.sourcegitcommit: af45c1c027d7246d1a6e4ec248406fb9a8752fb5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "77706569"
---
<!-- markdownlint-disable MD026 -->

# <a name="cloud-resource-governance"></a>雲端資源治理？

在[Azure 如何運作？](../../getting-started/what-is-azure.md)中，您已瞭解 azure 是代表使用者執行虛擬化硬體和軟體的伺服器和網路硬體的集合。 Azure 可讓您的組織應用程式開發和 IT 部門輕鬆地視需要建立、讀取、更新和刪除資源。

不過，雖然不受限制的資源可以讓開發人員更靈活，但也可能導致非預期的成本。 例如，開發小組可能會核准要部署一組測試資源，但是在完成測試之後忘記刪除。 這些資源會繼續產生成本，即使不再核准或不需要也一樣。

解決方案是資源存取管理。 **治理**是持續進行的程式，可讓您管理、監視和審核 Azure 資源的使用，以符合您組織的目標和需求。

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ii94]

<!-- markdownlint-enable MD034 -->

這些需求對每個組織而言都是唯一的，因此，一種適用于治理的單一大小方法並不實用。 相反地，每個組織會使用 Azure 的兩個主要治理工具來設計其治理模型：**角色型存取控制（RBAC）** 和**資源原則**。

RBAC 會定義角色，而角色則會定義指派給該角色的每個使用者所允許的作業。 例如，「**擁有**者」角色允許資源上的所有作業（建立、讀取、更新和刪除），而「**讀取**者」角色則只允許讀取作業。 角色可以廣泛定義並套用至許多資源類型，或定義更狹窄且只套用至幾個資源類型。

資源原則會定義資源建立的規則。 例如，資源原則可以將虛擬機器的 SKU 限制為特定的預先核准大小。 當提出要求來建立資源時，另一個資源原則可以強制為指派的成本中心套用標記。

設定這些工具時，請務必平衡治理和組織靈活性。 治理原則的限制愈多，開發人員和 IT 工作者的敏捷度就越低。 嚴格的治理原則可能需要更多的手動步驟，例如要求開發人員填寫表單，或傳送電子郵件給治理小組的成員，以手動建立資源。 治理小組具有有限的容量，而且可能成為瓶頸，因而導致開發小組等待 unproductively 建立資源，或在刪除之前不需要的資源會產生成本。

## <a name="next-steps"></a>後續步驟

既然您已瞭解雲端資源治理的概念，請深入瞭解如何在 Azure 中管理資源存取。

> [!div class="nextstepaction"]
> [深入了解 Azure 中的資源存取](./resource-access-management.md)

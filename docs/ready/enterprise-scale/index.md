---
title: 開始使用企業級登陸區域
description: 開始使用企業級登陸區域
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: d2218a84cda97c42a98327a9902009a47f8839b5
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84792195"
---
# <a name="start-with-enterprise-scale-landing-zones"></a>開始使用企業級登陸區域

企業級架構代表客戶 Azure 環境的策略性設計路徑和目標技術狀態。 其會隨著 Azure 平台持續發展，最終由組織為了定義其 Azure 旅程所必須做出的各種設計決策來定義。

並非所有企業都會以相同的方式採用 Azure，所以企業級架構可能會因客戶而異。 最終，企業級架構的技術考量和設計建議，可能會根據客戶的案例而導致不同的取捨。 預計會有一些變化，但如果遵循核心建議，則所產生的目標架構會將客戶放在可持續調整的路徑上。

## <a name="prescriptive-guidance"></a>規範性指引

企業級架構會提供與 Azure 最佳做法搭配的規範性指引，並且對客戶的 Azure 環境遵循重要設計區域的設計原則。

## <a name="qualifiers-should-i-start-with-enterprise-scale"></a>限定條件：我應該從企業級架構著手嗎？

企業級架構特意設計成模組化，可讓客戶從支援其應用程式組合的基礎登陸區域著手，而不管應用程式是否正在遷移或是新開發並部署至 Azure。 不論調整點為何，此架構都可以隨著客戶的商務需求調整。

## <a name="start-with-an-enterprise-scale-landing-zone"></a>開始使用企業級登陸區域

用於建構登陸區域的企業級方法包含四組可支援雲端小組的資產：

- [設計指導方針](./design-guidelines.md)：驅動 CAF 企業級登陸區域設計的重要決策指南。
- [架構](./architecture.md)：概念性參考架構，可示範設計區域和最佳做法。
- [實作](./implementation.md)：此架構的 Azure Resource Manager 範本，用於加速採用。

<!-- TODO: Reinstate once template.md is ready. 
- [Template](./template.md): A documentation template to quickly capture decisions and any deviation from the suggested architecture or implementation.
-->

## <a name="community"></a>社群

<!-- docsTest:ignore "Cloud Solutions Unit" -->

本指南主要是由 Microsoft 架構設計人員和更廣大的 Cloud Solutions Unit 技術社群所開發。 此社群積極地提升本指南，以分享在企業級採用過程中所學到的經驗。

雖然本指南共用與標準「就緒」方法相同的設計原則，但會擴充這些原則來整合先前在規劃過程中的一些主題，例如治理和安全性。 擴充標準程序是必要的，因為當採用工作需要大規模企業變更時，可以進行一些自然假設。

## <a name="next-steps"></a>後續步驟

[實作 CAF 企業級登陸區域](./implementation.md)

> [!div class="nextstepaction"]
> [實作 CAF 企業級登陸區域](./implementation.md)

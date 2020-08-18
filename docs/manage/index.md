---
title: 雲端管理
description: 使用「適用於 Azure 的雲端採用架構」，了解如何開發有效管理雲端所需的商務和技術方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: bb6a42562b061fef3176927b3e71641786d8467c
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88283614"
---
# <a name="cloud-management-in-the-cloud-adoption-framework"></a>雲端採用架構中的雲端管理

必須要有紮實的規劃、整備和採用措施，才能提供[雲端策略](../strategy/index.md)。 但要提供實質的商務成果，卻要持續不斷地運作數位資產。 如果沒有為雲端解決方案規劃好可靠、管理完善的作業方法，結果只會徒勞無功。 下列練習可協助您開發所需的商業和技術方法，以便提供能夠實現持續作業的雲端管理措施。

## <a name="get-started"></a>開始使用

為了讓您針對此階段的雲端採用生命週期做好準備，此架構建議您進行以下練習：

1. [建立管理基準](./azure-management-guide/index.md)：定義重要性分類、雲端管理工具和所需流程，以提供能夠進行作業管理的最低承諾。
2. [定義商務承諾](./considerations/business-alignment.md)：記錄所支援的工作負載，以便與企業建立營運承諾，並對要在每個工作負載投入多少雲端管理心力達成協議。
3. [擴展管理基準](./best-practices.md)：根據商務承諾和營運決策，利用內含的最佳做法來實作所需的雲端管理工具。
4. [先進的作業和設計原則](./design-principles.md)：需要更高度商務承諾的平台或工作負載，可能需要更深入地檢閱架構，以提供復原能力和可靠性承諾。

上述步驟會建立可操作的方法，以供提供雲端採用架構的管理方法。

<!-- cSpell:ignore CAF -->

![管理雲端採用架構中的方法](../_images/manage/caf-manage.png)

如[業務配合](./considerations/business-alignment.md)一文所述，並非所有工作負載都具有任務關鍵性。 雲端組合內會有各種程度的營運管理需求。 業務配合工作可協助您掌握業務影響，並與業務協調管理成本，以確保您會有最適當的作業管理流程和工具。

雲端採用架構管理小節中的指引有兩個目的：

- 提供可操作的作業管理方法範例，顯示客戶常有的一般體驗。
- 協助您根據商務承諾來建立個人化的管理解決方案。

此內容適用於雲端作業團隊。 也與開發強大雲端作業或雲端設計原則基礎的雲端架構設計師息息相關。

雲端採用架構的內容會影響企業的業務、技術和文化。 雲端採用架構的這個部分會與 IT 作業、IT 治理、財務、業務主管、網路、身分識別和雲端採用小組進行密切互動。 對於這些人員的各種相依性，需要雲端架構師使用此指引才能達成。 您往往要不只一次尋求這些團隊的協助。

雲端架構設計師可引導並促使這些對象共同合作。 此系列指南的內容旨在協助雲端架構設計師促進與正確對象進行正確的對話，藉以推動必要的決策。 由雲端推動的業務轉型必須由雲端架構設計師居中協助指導整個業務和 IT 的決策。

雲端採用架構的每個小節代表雲端架構設計師角色的不同專業化或樣貌。 雲端採用架構的這一節是專為熱愛操作和管理部署解決方案的雲端架構設計師所設計的。 此架構經常將這些專家稱為「雲端作業」或統稱為「雲端作業團隊」。

如果您想要從頭到尾按照本指南操作，此內容可協助您開發健全的雲端作業策略。 本指南會引導您了解此類策略的理論和實作。

您可以套用方法以[建立清楚的商務承諾](./considerations/business-alignment.md)。

<!-- TODO: For a crash course on the theory and quick access to Azure implementation, get started with the [governance guides overview](TODO). Using this guidance, you can start small and iteratively improve your governance needs in parallel with cloud adoption efforts. -->

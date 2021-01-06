---
title: Windows 虛擬桌面規劃
description: 使用適用于 Azure 的雲端採用架構，利用可降低複雜性並將遷移程式標準化的最佳作法，來規劃您的 Windows 虛擬桌面遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/17/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: dabe9dd4731ba2de263e19c6ebfdc4b994989a21
ms.sourcegitcommit: 86d51757bd34b49ce3b061123a6aaa8c88d3b2cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97909424"
---
# <a name="windows-virtual-desktop-planning"></a>Windows 虛擬桌面規劃

Windows 虛擬桌面和部署案例遵循與其他遷移工作相同的遷移方法。 這種一致的方法可讓遷移 factory 或現有的遷移小組採用幾乎不需要變更非技術性需求的程式。

![遷移雲端採用架構的方法。](../../_images/migrate/methodology.png)

## <a name="plan-your-migration"></a>規劃移轉

如同其他遷移，您的小組會評估工作負載、部署工作負載，然後將它們發行給使用者。 不過，Windows 虛擬桌面包含特定需求，需要在評估工作負載期間審查 Azure 登陸區域。 此程式在第一次部署之前也需要概念證明。

若要建立您的方案，請參閱 Azure DevOps 中現有遷移待處理專案的「 [雲端採用方案 DevOps 範本](../../plan/template.md) 」。 使用範本來建立活動的詳細計畫。

## <a name="business-justification"></a>商業論證

規劃的一部分將需要能夠 articaulte 移至 Windows 虛擬桌面的企業優勢。 下列清單包含要在商務案例中使用的專案。
  
- Windows 虛擬桌面控制平面或管理平面會以服務的形式提供給客戶。 控制平面會管理使用者對桌面的無縫全球連線能力，以及其所需的集中化部署和協調流程。 這是一項 PaaS 服務，因為您不需要購買硬體、部署該硬體、加以修補或支援。 它是您剛剛使用的長達一項服務。 這是免費的服務，您可以透過自己的授權取得您已擁有的授權，因此您可以使用此 PaaS 服務來節省成本。 這也表示在遷移後，您不需要對自己的內部內部部署虛擬桌面管理服務進行管理、饋送和灌溉、疑難排解、中斷修正、修補等作業。 這可讓 IT 人員專注于對企業來說更重要的功能，這會著重于為企業提供更高的價值，通常確保客戶在取用應用程式和資料時，擁有最佳的使用者體驗。
- 無需預付費用。 若要在內部部署環境中執行虛擬桌面，您必須預先支付或輸入符合尖峰負載所需的所有硬體的長度租用協定，即使該硬體未在專案進行時使用，甚至是在專案完成時未使用100% 的時間為止。 Windows 虛擬桌面和 Azure 會根據耗用量收費，因此您只需支付使用的部分。 此外，這也可以根據商務需求來相應增加和減少，讓您能夠滿足商務需求，同時也能在相應減少時降低成本。
- Windows 虛擬桌面和 Azure 在新特性和功能方面有更快速的步調。 內部部署硬體和現成的軟體都有生命週期的存留期，且不會經常收到大型的新功能集或新功能，而且通常需要成本高昂的專案才能執行。 Windows 虛擬桌面和 Azure 會收到一般的新功能，而該部署是由 Microsoft 所管理。 對您而言，這是您剛剛使用的長達一項服務。 您不需要專案就能將新的服務推出給商務使用者，讓他們能夠使用新的服務。 這些新功能可能會提供競爭優勢，或甚至是在最少的情況下，不會讓您以技術債務進行權衡。
- Azure 是超大規模雲端，可提供大規模的規模。 內部部署服務將永遠無法與 Azure 競爭作為運算平臺。 這可為您的組織提供大量的靈活性。 如果他們有新的機會擴充到他們沒有本機資料中心的全球位置，Microsoft 可以將此服務裝載在不斷增加的 Azure 區域中，如此一來，Microsoft 就能將基礎結構或服務放置於比您更接近的終端使用者。
- 以多個會話的成本，Windows 10 使用者預期的豐富使用者體驗。 Windows 虛擬桌面可讓 Windows Server 搭配遠端桌面服務與 Windows 10 的使用者體驗進行調整，而不會危及應用程式的相容性
- Windows 10 多個會話的用戶端存取授權並不需要，因為 Windows 10 多會話不需要 CAL
- 在 Windows 虛擬桌面中，如果您部署 Windows Server (Windows Server 2012 R2、2016或 2019) ，就不需要購買 Windows Server 授權
- 所有 Windows 虛擬桌面虛擬機器都會以基礎計算費率計費。 Windows 虛擬桌面有權獲得另一項授權，您可能已經擁有 (M365 E3 +) ，其中包含 Windows 授權
- Windows 虛擬桌面中的所有 Windows 7 虛擬機器，在2023年1月14日前都會收到免費的延伸安全性更新


## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [檢查您的環境或 Azure 登陸區域](./ready.md)
- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面遷移或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)

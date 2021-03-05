---
title: 雲端組織反模式
description: 避免因組織問題（例如不一致的 IT 部門、合作關係和工程指派）而造成的雲端採用反模式。
author: sarahwendel
ms.author: brblanch
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: think-tank
ms.openlocfilehash: 188520559e408610fa022156ba6d07597dd50b32
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102207872"
---
# <a name="cloud-organizational-antipatterns"></a>雲端組織反模式

客戶通常會遇到其組織結構內的雲端採用反模式。 許多因素可能會導致下列問題：

- 工具
- 合作夥伴
- 工程師
- IT 部門不一致

在成功的雲端採用案例中，請務必瞭解每個因素的角色。

## <a name="antipattern-treat-it-as-a-cost-center"></a>反模式：將它視為成本中心

許多公司將 IT 部門視為成本中心。 這種方法可能會讓您認為它並不會增加公司的價值。 當員工將它視為提供者而非啟用程式時，可能會不建議您這樣做。 公司也很難吸引適當的人才。 降低動機和生命週期時間的結果。 IT 的工作品質可能會受到影響，而定址 [接收器和領域](../organize/fiefdoms-silos.md) 可進行開發。

### <a name="example-treat-it-as-a-cost-center"></a>範例：將它視為成本中心

公司在財務長 (CFO 的) 責任下，將 IT 部門管理為成本中心。 管理面板將它視為一種較慢的服務提供者，這是公司最大的成本驅動因素之一。 管理面板並不知道行動業務單位耗用 IT 部門所訂購的大部分資產。 它會為所有要使用的業務單位購買一個資料中心，但行動業務單位會取得這種超大的資產。 面板不會將它視為啟用程式或合作夥伴。

### <a name="preferred-outcome-view-it-as-an-enabler"></a>慣用結果：以啟用程式形式加以查看

請考慮下列其中一種方法，而不是以成本中心的形式管理您的 IT 部門：

- [退款](../strategy/cloud-accounting.md#chargeback)：業務單位會將 IT 成本視為預算中的營運費用。
- [回報或認知-back](../strategy/cloud-accounting.md#showback-or-awareness-back)：它會作為代理程式。 在回報給業務的情況下，它會針對相關的業務單位，對任何直接成本進行屬性。

使用雲端作為工具來增加成本和業務透明度。 例如，實施 [成本管理專業領域](../govern/cost-management/index.md) 來增加成本透明度。 然後，您會更瞭解不同業務單位的成本。 您將會看到 IT 部門作為這些單位的啟用程式。

若要改善透明度，請在移至雲端時專注于可見度、責任和優化。 如需詳細資訊，請參閱 [建立符合成本的組織](../organize/cost-conscious-organization.md)。

## <a name="antipattern-invest-in-new-technology-without-involving-the-business"></a>反模式：投資新技術而不涉及企業

IT 部門通常會投資大量的人力和財務資源，以建立及部署穩固的平臺和工具組。 但是，有時候在設計和開發階段中，它無法考慮業務單位及其需求。 這種省略會導致對業務單位有基本相關性的新平臺。 接著遲疑員工以接受新的技術。 可能會導致採用不良或緩慢。 當營業單位未使用其平臺時，也會在其中建立挫折。

### <a name="example-set-up-a-platform-without-involving-business-units"></a>範例：設定平臺而不涉及業務單位

資料分析公司的 IT 部門會設定和自訂 Azure 平臺，而不涉及任何業務單位。 在使用平臺時，業務單位開發人員：

- 請注意，他們沒有部署所需的許可權。
- 只能使用有限數目的服務。
- 發出延長核准週期的支援票證。
- 開始不確定新平臺。

最後，有些開發人員會自行購買 Azure 訂用帳戶，以避免 IT 規則和法規的麻煩。 影子它隨即出現。 因為公司對影子 IT 沒有太大的控制權，所以會出現高安全性風險。

### <a name="preferred-outcome-involve-business-units-in-decision-making"></a>慣用結果：參與決策的業務單位

部署符合企業需求的雲端平臺時，請避免建立它的定址 [接收器](../organize/fiefdoms-silos.md) 。 涉及開發人員和技術決策者 (在設計和開發程式中，從營業單位 Tdm) 。 若要改善平臺的採用，請接聽業務單位輸入。

請參閱 [開始使用雲端採用架構企業規模登陸區域](../ready/enterprise-scale/index.md) ，以取得 Azure 最佳做法和設計原則，以提升採用速度並為開發人員量身打造。 在合規性與彈性之間取得適當的平衡。 比方說，您可以尋找滿足治理和安全性原則的方式，同時保持開發環境的敏捷度。

## <a name="antipattern-outsource-core-business-functions"></a>反模式：外包核心商務功能

諮詢夥伴和受控服務提供者 (Msp) 在雲端旅程中扮演著重要的角色。 但是，公司應該要注意的是，夥伴的「和 Msp」工作無法在其業務中提供最大的價值。 將責任外包給 Msp 或雲端顧問的公司，不應該相依于這些提供者。

### <a name="example-outsource-cloud-adoption-and-migration"></a>範例：外包雲端採用和遷移

研究人員有時間緊迫的雲端遷移專案。 為了縮短雲端採用旅程，它會雇用 MSP 來建立 Azure 基礎並實行遷移。 我們不需要瞭解雲端採用階段和建立技能，而是選擇將所有 Azure 責任交給 MSP。 因為此局沒有任何雲端或 Azure 知識，所以 MSP 會將所有決策的潛在客戶導向，讓局相依于 MSP。

### <a name="preferred-outcome-make-critical-design-areas-the-companys-responsibility"></a>慣用結果：讓公司的責任成為重要的設計領域

請將外包視為絕佳的成本節約策略。 但是，在您的公司內參與這些重要的設計領域時，請做出決策：

- 控管
- 風險
- 法規遵循
- 身分識別

在公司內部針對您的安全資產提供重要的其他領域，請務必承擔責任。 使用外部合作夥伴加速採用旅程。 但是，為了避免相依于提供者，請不要外包所有專案。

## <a name="antipattern-hire-tdms-instead-of-developing-cloud-engineers"></a>反模式：雇用 Tdm，而不是開發雲端工程師

公司會對尋找正確的人員帶來重要性。 如此一來，它們通常會在初始雲端採用階段期間雇用或建立 Tdm。 成功的雲端旅程依賴 Tdm。 但更重要的是，雲端採用需要工程師提供全手 mentalities 和深度的技術技能。

### <a name="example-hire-tdms-only"></a>範例：僅限雇用 Tdm

研究部門雇用數個 Tdm 來領導其雲端旅程。 初始高階概念階段結束之後，就會開始執行階段。 然後，此局會發現雲端部署的行為與內部部署不同。 它需要額外的雲端工程工作，才能適當地將 [基礎結構作為程式碼 (IaC) ](/azure/devops/learn/what-is-infrastructure-as-code) 概念和原則導向的治理。

### <a name="preferred-outcome-use-cloud-engineers-for-the-implementation-phase"></a>慣用結果：使用雲端工程師進行實行階段

請記住，工程師對於適當實施雲端自動化和登陸區域概念是不可或缺的。 當您採用服務模型時，責任和工作可能會大幅轉移。 藉由將責任轉移至雲端提供者，您可以更快進入生產環境。 您也可以使用 Tdm 來進行決策。 但是，針對需要深入工程知識的工作，請使用可支援的雲端工程師。 然後您就可以瞭解雲端所提供的優點。

## <a name="next-steps"></a>下一步

- [協調小組間的職責](../organize/raci-alignment.md)
- [組織反模式：定址接收器和領域](../organize/fiefdoms-silos.md)
- [打造成本重視的組織](../organize/cost-conscious-organization.md)

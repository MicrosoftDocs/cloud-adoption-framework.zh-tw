---
title: 選擇您組織的登陸區域
description: 瞭解如何為您的組織選擇正確的登陸區域。 您可以從小規模開始，並展開或實行企業規模的選項。
author: BrianBlanchard
ms.author: brblanch
ms.date: 01/20/2021
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: dfcb3c7c40fafb6343dffed4682eed993c60c3cc
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101785048"
---
# <a name="choose-the-landing-zone-for-your-organization"></a>選擇您組織的登陸區域

在雲端採用架構中實施登陸區域的方法有很多種。 針對您的組織，正確的方法有必要的服務可支援您的商務應用程式，而不需要額外的管理負擔。 從不符合您需求的實作為開始，可以浪費您的時間和精力。

Microsoft 針對登陸區域提供兩種執行選項：

- 從小規模著手並擴充
- 企業規模

觀看下列15分鐘的影片，以深入瞭解如何選擇最符合您需求的 Azure 登陸區域實作選項。

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWwZeg]

您也可以考慮進行協力廠商的實施。 我們的合作夥伴有許多透過其服務提供的實現。 如需詳細資訊，請參閱 [評估 Microsoft 合作夥伴的 Azure 登陸區域](./partner-landing-zone.md)。

## <a name="overview-of-landing-zone-options"></a>登陸區域選項的總覽

下表摘要說明每個登陸區域執行方法的考慮。

:::row:::
    :::column:::

    :::column-end:::
    :::column:::

    :::column-end:::
    :::column:::
        **從小規模著手並擴充**
    :::column-end:::
    :::column:::
        **企業規模**
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **初始考慮**
    :::column-end:::
    :::column:::
        [作業模型對齊](../../operating-model/compare.md#operating-model-comparison)
    :::column-end:::
    :::column:::
        集中式作業
    :::column-end:::
    :::column:::
        企業營運
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::

    :::column-end:::
    :::column:::
        基準參考架構
    :::column-end:::
    :::column:::
        提供簡單的起點，可讓您以最低的訂用帳戶建立自己的解決方案，您只需視需要進行調整。
    :::column-end:::
    :::column:::
        無論您的調整點為何，都能提供整個 Azure 租使用者參考，其中包括雲端原生作業。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **採用計畫考慮**
    :::column-end:::
    :::column:::
        提供長期自我充足性
    :::column-end:::
    :::column:::
        需要「雲端採用架構」治理和管理方法，以達成長期自我充足性。
    :::column-end:::
    :::column:::
        企業級架構登陸區域方法和架構可讓您的組織準備好長期自我充足性。 提供保留實例以開始使用。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::

    :::column-end:::
    :::column:::
        啟用整個組織的採用速度
    :::column-end:::
    :::column:::
        快速啟用低風險採用。 在一段時間內建立安全性、治理和合規性。
    :::column-end:::
    :::column:::
        從安全性、治理和合規性開始，以更快實現符合規範的採用。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::

    :::column-end:::
    :::column:::
        實現卓越的營運
    :::column-end:::
    :::column:::
        需要雲端採用架構治理和管理方法，以達成卓越的營運。
    :::column-end:::
    :::column:::
        透過以原則導向的治理和管理為基礎的平臺和應用程式小組，為其提供卓越的營運。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **相容性考量**
    :::column-end:::
    :::column:::
        達成安全性、治理和合規性的途徑
    :::column-end:::
    :::column:::
        反復的方法。 需要治理和管理方法，以支援敏感性資料或任務關鍵性工作負載。
    :::column-end:::
    :::column:::
        企業規模的架構包含治理、安全性劃分和責任分隔的設計。 這種方法可讓小組在適當的登陸區域內採取行動。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::

    :::column-end:::
    :::column:::
        建立安全性、治理和合規性的相關風險
    :::column-end:::
    :::column:::
        有大量重構或甚至是重新部署，以達成必要需求的風險。
    :::column-end:::
    :::column:::
        啟用可能無法符合您作業模型的雲端原生作業產品的風險。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **部署考慮**
    :::column-end:::
    :::column:::
        雲端提供者的最佳作法？
    :::column-end:::
    :::column:::
        您必須使用雲端採用架構方法來新增更多的最佳作法，以套用安全性、治理和合規性。
    :::column-end:::
    :::column:::
        企業規模包含 Azure 最佳做法，而且是 Azure 環境的目標技術狀態。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::

    :::column-end:::
    :::column:::
        根據身分識別/存取管理、治理、安全性、網路和記錄的建議最佳作法，提供所有重要服務並正確設定
    :::column-end:::
    :::column:::
        部分。 部署一些資源。 符合雲端採用架構方法的其他供應專案，這些方法必須套用最佳作法以支援安全性、治理和合規性。
    :::column-end:::
    :::column:::
        企業規模架構是符合 Azure 平臺藍圖的 Azure 環境目標技術狀態建議。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::

    :::column-end:::
    :::column:::
        基礎結構即程式碼之類的自動化功能 (IaC) 和 Azure DevOps
    :::column-end:::
    :::column:::
        Azure Resource Manager、Azure 原則和 Azure 藍圖可以用來建立您自己的持續整合/持續開發管線。
    :::column-end:::
    :::column:::
        Azure Resource Manager、Azure 原則和 GitHub/Azure DevOps。 CI/CD 管線選項包含在參考實行指引中。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **時間軸考慮**
    :::column-end:::
    :::column:::
        要採用或遷移低風險工作負載的時程表：
    :::column-end:::
    :::column:::
        3到10天
    :::column-end:::
    :::column:::
        3到10天
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::

    :::column-end:::
    :::column:::
        時間軸可達成所有工作負載的安全性、治理和合規性需求：
    :::column-end:::
    :::column:::
        四到六個月
    :::column-end:::
    :::column:::
       六到八周
    :::column-end:::
:::row-end:::

## <a name="initial-considerations"></a>初始考慮

哪些作業模型更能描述您的組織？ 請留意您的組織目前看起來的樣子，以及您想要的結果，並將它在一年或之後的三個月內。

- 集中式作業：在這個小型環境中，IT 作業、安全性和其他角色的集中式小組會管理生產和工作負載。

- 企業營運：在這種情況下，企業營運環境會集中管理穩定穩定的狀態。

集中式作業優先于一種開始-小型和展開的方法。 企業營運的解決方式可能更適合企業規模。

您是否需要基準架構或環境？ 開始-小型和展開的方法提供簡單的起點，讓您可以在其中建立自己的解決方案。 企業規模的方法提供整個 Azure 租使用者的環境，包括雲端原生作業。

如需作業類型的詳細資訊，請參閱 [比較常見的雲端作業模型](../../operating-model/compare.md)。

## <a name="adoption-plan-considerations"></a>採用計畫考慮

以下是這兩種類型的採用計畫的關鍵考慮：

- 長期自我充足性
- 整個組織的採用速度
- 卓越營運

企業規模可立即提供長期的自我充足性和操作能力。 企業規模也有助於加速、在整個組織中採用合規性。 企業規模的方法為您打造基礎。 企業規模包含有關安全性、身分識別和網路的護欄。 此方法包含適用于 DevOps 和自動化的 CI/CD 管線選項。

如果您從小規模開始著手，則有一些方法可取得自我充足性、採用速度和卓越的營運。 在雲端採用架構內使用治理或管理方法，將這些部分反復地建立到登陸區域解決方案中。 您可以使用「 [雲端採用架構](../enterprise-scale/design-guidelines.md)」這八個領域的設計指導方針，以反復改善您的設計。

若要深入瞭解卓越的營運，請參閱 [在數位轉型期間提供卓越的營運](../../get-started/operational-excellence.md)。

## <a name="compliance-considerations"></a>相容性考量

請考慮下列有關貴組織合規性的問題：

- 達成安全性、治理和合規性的途徑
- 建立安全性、治理和合規性的風險

您的組織可能需要在短時間內擁有必須符合規範的特定工作負載或應用程式，而這項需求可能會影響您的選擇。

從小規模開始，擴充是一種反復合規性的方法。 使用「雲端採用架構」管理和管理方法，以支援敏感性資料或重要工作負載。 如需詳細資訊，請參閱管理雲端的 [方法](../../govern/methodology.md) 和雲端 [中的 IT 管理和作業](../../manage/considerations/index.md)。

企業規模架構包含了分割和分隔的設計，以支援合規性目標和 [服務啟用架構](../enterprise-scale/security-governance-and-compliance.md#service-enablement-framework) ，以決定如何達成適當的治理、安全性和合規性層級。

可能的話，請先找出要先執行的低風險工作負載。 這項技術可協助您在一段時間內建立基礎結構和技能。 您可以在瞭解雲端的運作方式時，加入治理和管理方法。

## <a name="deployment-considerations"></a>部署考量因素

部署登陸區域或登陸區域會引發數個選擇執行的考慮：

- 雲端提供者的最佳做法。

- 所有重要的服務都會根據身分識別/存取管理、治理、安全性、網路和記錄的建議最佳作法來呈現並正確設定。

- 自動化功能，例如 IaC 和 Azure DevOps。

這兩個實行都提供最佳作法。 從小規模開始，您可以使用雲端採用架構方法來新增最佳做法，以套用安全性、治理和合規性。

企業規模包含所有已設定的重要服務。 從小規模開始，並擴展一些部署的資源。

如需詳細資訊，請參閱 [Azure 就緒的最佳做法](../azure-best-practices/index.md)。

這兩種方法都提供自動化功能。

- 從小規模開始，並展開： ARM 範本、Azure 原則和 Azure 藍圖。 可以建立您自己的 CI/CD 管線。
- 企業級： ARM 範本、Azure 原則、GitHub/Azure DevOps 和 CI/CD 管線選項都包含在參考實行指引中。

從小規模開始，使用 ARM 範本、Azure 原則和 Azure 藍圖。

- [CAF 基礎藍圖](./foundation-blueprint.md)
- [CAF 移轉登陸區域藍圖](./migrate-landing-zone.md)

企業規模使用 ARM 範本、Azure 原則，並提供三個參考實行和不同的部署。

- [企業規模的基礎](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/wingtip/README.md)
- [企業規模的虛擬 WAN](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md)
- [企業規模的中樞和輪輻](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md)

無論您是以小規模、擴展或企業規模來實行，都可以使用範本和入口網站體驗。 您稍後可以在流程中包含 IaC。 如需詳細資訊，請流覽此 [IaC 總覽](/dotnet/architecture/cloud-native/infrastructure-as-code) 。

## <a name="timeline-considerations"></a>時間軸考慮

登陸區域選項需要不同的時間來執行。 有兩種類型的時間軸：

- 採用或遷移低風險工作負載的時程表
- 針對所有工作負載達到安全性、治理和合規性需求的時程表

使用開始-小型和展開的方法，您可以在3到10天內啟動並執行低風險的工作負載。 針對具有高安全性、治理和合規性需求的工作負載，可能需要四到六個月的時間。

針對企業規模的執行，您仍然可以在3到10天內採用低風險的工作負載，但更詳盡的工作負載可在六到八周內準備就緒。

## <a name="next-steps"></a>下一步

[從小規模著手並擴充](./migrate-landing-zone.md)

[開始使用企業規模](../enterprise-scale/index.md)
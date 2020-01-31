---
title: 遷移環境規劃檢查清單
description: 在移轉之前驗證環境是否已就緒
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 2660c6f09924c907591c8c8635b943125d0ac9a1
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76801404"
---
# <a name="migration-environment-planning-checklist-validate-environmental-readiness-prior-to-migration"></a>遷移環境規劃檢查清單：在遷移前驗證環境是否就緒

在移轉程序的初始步驟中，您必須在雲端建立正確的環境，以接收、裝載及支援資產的移轉。 本文將列載在進行移轉前須在目前的環境中驗證的事項。

下列檢查清單會與雲端採用架構的[準備](../../../ready/index.md)一節中說明的指引一致。 請參閱這一節，以取得執行下列任何作業的相關指引。

## <a name="effort-type-assumption"></a>採用的工作類型

本文和檢查清單均採用_重新裝載_或_雲端轉換_方法來進行雲端移轉。

## <a name="governance-alignment"></a>控管一致性

關於任何可供移轉的環境，首要決策就是控管一致性的選擇。 移轉基礎的控管一致性是否已達成相關共識？ 雲端採用小組至少應該瞭解這項遷移功能是否會在有限的治理、完全受控的環境處理站，或介於之間進行某種變化的單一環境中進行登陸。 如需更多關於控管一致性的選項和指引，請參閱關於[控管和合規性一致性](../../expanded-scope/governance-or-compliance.md)的文章。

## <a name="cloud-readiness-implementation"></a>實作雲端整備

無論您是否選擇要在初始移轉期間與更廣泛的雲端控管策略達成一致，都必須確保您的雲端部署環境已適當設定而可支援工作負載。

如果您打算從一開始就讓移轉與雲端控管策略保持一致，您必須套用[雲端治理的五個專業領域](../../../govern/governance-disciplines.md)，以利傳達可讓雲端環境符合整體公司需求的原則、工具鏈和強制執行機制的決策。 請參閱雲端採用架構的[可採取動作的控管設計指南](../../../govern/guides/index.md)，以取得如何使用 Azure 服務來實作此模型的範例。

如果您的初始移轉並未與更廣泛的雲端控管策略緊密保持一致，則仍需要管理組織、存取和基礎結構規劃的一般問題。 如需這些雲端就緒決策的協助，請參閱[Azure 設定指南](../../../ready/azure-setup-guide/index.md)。

> [!CAUTION]
> 強烈建議您針對初始工作負載移轉以外的任何移轉制定控管策略。

無論控管一致性的程度如何，您都必須做出與下列主題有關的決策。

### <a name="resource-organization"></a>資源組織

根據控管一致性決策，應在移轉之前建立組織和部署資源的方法。

### <a name="nomenclature"></a>命名法

在移轉之前，應先建立命名資源的一致方法，以及一致的命名結構描述。

### <a name="resource-governance"></a>資源管理

在移轉之前，應先決定用來控管資源的相關工具。 這些工具不需要完全實作，但應選取並測試方向。 雲端治理小組應該在進行遷移之前，定義和要求治理工具的最低可行產品（MVP）實行。

## <a name="network"></a>網路

您的雲端式工作負載將需要佈建虛擬網路，以支援終端使用者和系統管理存取權。 根據資源組織和資源控管決策，您應選取符合 IT 安全性需求的網路方法。 此外，您的網路決策應遵循混合式網路的任何必要限制；在執行移轉待處理項目中的工作負載時，以及支援對裝載於內部部署之資源的任何存取時，皆必須符合這些限制。

## <a name="identity"></a>身分識別

雲端式身分識別服務是為您的雲端資源提供身分識別與存取管理 (IAM) 的必要條件。 在繼續進行之前，請先讓您的身分識別管理原則與雲端採用方案達成一致。 例如，在移轉現有的內部部署資產時，請考慮使用[目錄同步](../../../decision-guides/identity/index.md)來支援混合式身分識別方法，讓您的內部部署和雲端環境之間在移轉期間和之後都能有一組一致的使用者認證。

## <a name="next-steps"></a>後續步驟

如果環境符合最低需求，即可將其視為通過移轉整備核准的環境。 [文化複雜性和變更管理](./cultural-complexity.md)有助於調整角色和責任，以確保在計劃執行期間能符合適當預期。

> [!div class="nextstepaction"]
> [文化複雜性和變更管理](./cultural-complexity.md)

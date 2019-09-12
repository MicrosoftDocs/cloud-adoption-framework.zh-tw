---
title: 持續管理和安全性
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 持續管理和安全性
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: c8b6c5085117f965bedd5947bfbc2725aedf96e4
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70833098"
---
# <a name="phase-3-ongoing-management-and-security"></a>階段 3：持續管理和安全性

在上架 Azure 管理服務之後, 您必須專注于可支援您進行中作業的作業和安全性設定。 我們會藉由查看 Azure 資訊安全中心, 開始保護您的環境。 接著, 我們會設定原則, 讓伺服器保持合規性並將一般工作自動化。 本節包含下列主題：

- **[解決安全性建議。](#address-security-recommendations)** Azure 資訊安全中心提供改善環境安全性的建議。 當您執行這些建議時, 您可以看到反映在安全性分數中的影響。
- **[啟用來賓設定原則。](./guest-configuration-policy.md)** 啟用 [Azure 原則來賓設定] 功能, 以在虛擬機器中審查設定。 例如, 您可以檢查是否有任何憑證即將到期。
- **[追蹤重大變更併發出警示。](./enable-tracking-alerting.md)** 當您進行疑難排解時, 首先要考慮的問題是「什麼是已變更？」 在本文中, 您將瞭解如何追蹤變更並建立警示, 以主動監視重要元件。
- **[建立更新排程。](./update-schedules.md)** 排程更新的安裝, 以確保所有伺服器都有最新的版本。
- **[常見的 Azure 原則範例。](./common-policies.md)** 提供一般管理原則的範例。

## <a name="address-security-recommendations"></a>解決安全性建議

Azure 資訊安全中心是為您的環境管理安全性的中心位置。 您會看到整體評估和目標建議。

我們建議您檢查並執行此服務所提供的建議。 如需 Azure 資訊安全中心其他優點的詳細資訊, 請參閱[遵循 Azure 資訊安全中心建議](/azure/migrate/migrate-best-practices-security-management#best-practice-follow-azure-security-center-recommendations)。

## <a name="next-steps"></a>後續步驟

瞭解如何[啟用 Azure 原則來賓](./guest-configuration-policy.md)設定功能。

> [!div class="nextstepaction"]
> [來賓設定原則](./guest-configuration-policy.md)

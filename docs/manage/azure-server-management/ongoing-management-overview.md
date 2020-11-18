---
title: 持續管理和安全性
description: 使用適用于 Azure 的雲端採用架構，瞭解如何將焦點放在可支援您進行中作業的作業和安全性設定。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: e3dc658ec7ba94d44420521d031bcd5bfa90c5c7
ms.sourcegitcommit: 412b945b3492ff3667c74627524dad354f3a9b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94880223"
---
# <a name="phase-3-ongoing-management-and-security"></a>第3階段：持續的管理和安全性

上線 Azure 伺服器管理服務之後，您必須將焦點放在可支援進行中作業的作業和安全性設定。 我們會藉由查看 Azure 資訊安全中心來開始保護您的環境。 接著，我們會設定原則，讓您的伺服器符合規範，並將一般工作自動化。 本節包含下列主題：

- [解決安全性建議](#address-security-recommendations)： Azure 資訊安全中心提供改進環境安全性的建議。 當您執行這些建議時，您會看到反映在安全性分數中的影響。
- [啟用來賓設定原則](./guest-configuration-policy.md)：使用 Azure 原則來賓設定功能來審查虛擬機器中的設定。 例如，您可以檢查是否有任何憑證即將到期。
- [追蹤和警示重大變更](./enable-tracking-alerting.md)：當您進行疑難排解時，首先要考慮的問題是「變更了哪些內容？」 在本文中，您將瞭解如何追蹤變更並建立警示，以主動監視重要的元件。
- [建立更新](./update-schedules.md)排程：排程更新的安裝，以確保所有伺服器都有最新的更新。
- [常見的 Azure 原則範例](./common-policies.md)：本文提供一般管理原則的範例。

## <a name="address-security-recommendations"></a>解決安全性建議

Azure 資訊安全中心是管理環境安全性的集中位置。 您將會看到整體評定和目標建議。

建議您複習並執行此服務所提供的建議。 如需 Azure 資訊安全中心其他優點的詳細資訊，請參閱 [遵循 Azure 資訊安全中心建議](/azure/migrate/migrate-best-practices-security-management#best-practice-follow-azure-security-center-recommendations)。

## <a name="next-steps"></a>後續步驟

瞭解如何 [啟用 Azure 原則來賓](./guest-configuration-policy.md) 設定功能。

> [!div class="nextstepaction"]
> [來賓設定原則](./guest-configuration-policy.md)

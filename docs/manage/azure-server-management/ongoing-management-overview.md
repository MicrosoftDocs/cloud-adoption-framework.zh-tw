---
title: 持續管理和安全性
description: 使用適用于 Azure 的雲端採用架構，瞭解如何將焦點放在可支援您進行中作業的作業和安全性設定。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: 41615efbde7f7b4c32846e73c2493c2d4b47500f
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102113634"
---
# <a name="phase-3-ongoing-management-and-security"></a>第3階段：持續的管理和安全性

上線 Azure 伺服器管理服務之後，您必須將焦點放在可支援進行中作業的作業和安全性設定。 我們會藉由查看 Azure 安全性中心來開始保護您的環境。 接著，我們會設定原則，讓您的伺服器符合規範，並將一般工作自動化。 本節包含下列主題：

- [解決安全性建議](#address-security-recommendations)： Azure 安全性中心提供改進環境安全性的建議。 當您執行這些建議時，您會看到反映在安全性分數中的影響。
- [啟用來賓設定原則](./guest-configuration-policy.md)：使用 Azure 原則來賓設定功能來審查虛擬機器中的設定。 例如，您可以檢查是否有任何憑證即將到期。
- [追蹤和警示重大變更](./enable-tracking-alerting.md)：當您進行疑難排解時，首先要考慮的問題是「變更了哪些內容？」 在本文中，您將瞭解如何追蹤變更並建立警示，以主動監視重要的元件。
- [建立更新](./update-schedules.md)排程：排程更新的安裝，以確保所有伺服器都有最新的更新。
- [常見的 Azure 原則範例](./common-policies.md)：本文提供一般管理原則的範例。

## <a name="address-security-recommendations"></a>解決安全性建議

Azure 安全性中心是管理環境安全性的集中位置。 您將會看到整體評定和目標建議。

建議您複習並執行此服務所提供的建議。 如需 Azure 資訊安全中心其他優點的詳細資訊，請參閱 [遵循 azure 資訊安全中心的建議](../../migrate/azure-best-practices/migrate-best-practices-security-management.md#best-practice-follow-azure-security-center-recommendations)。

## <a name="next-steps"></a>下一步

瞭解如何 [啟用 Azure 原則來賓](./guest-configuration-policy.md) 設定功能。

> [!div class="nextstepaction"]
> [來賓設定原則](./guest-configuration-policy.md)

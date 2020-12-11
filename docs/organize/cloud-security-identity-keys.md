---
title: 雲端中的身分識別和金鑰管理功能
description: 瞭解雲端中身分識別和金鑰管理的功能。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: internal
ms.openlocfilehash: 5a1c603cd118c93679b30104eae6c962b12d1a09
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97024682"
---
# <a name="function-of-identity-and-key-management-in-the-cloud"></a>雲端中的身分識別和金鑰管理功能

負責身分識別管理之安全性小組的主要目標是提供人員、服務、裝置和應用程式的驗證和授權。 金鑰與憑證管理可提供安全的金鑰資料散發和存取，以進行密碼編譯作業， (通常會支援與身分識別管理) 類似的結果。

## <a name="modernization"></a>現代化

資料身分識別和金鑰管理現代化的成形方式如下：

- 身分識別和金鑰/憑證管理專業領域即將緊密聯繫，因為兩者都會提供驗證和授權的保證，以啟用安全通訊。
- 身分識別控制項可作為雲端應用程式的主要安全性周邊
- 因為儲存和安全地提供這些金鑰的存取權，所以會將雲端服務的金鑰型驗證取代為身分識別管理。
- 從內部部署身分識別架構（例如單一身分識別、單一登入 (SSO) 和原生應用程式整合）中學到的正面經驗，相當重要。
- 避免常見的內部部署架構錯誤（通常會 overcomplicated）的重要重要性，讓支援變得更容易。 其中包含：
  -  (Ou) 的擴張群組和組織單位。
  - 擴張協力廠商目錄和身分識別管理系統的集合。
  - 缺乏明確的應用程式身分識別策略的標準化和擁有權。
- 認證竊取攻擊會維持高度的衝擊，而且可能會降低風險。
- 服務帳戶和應用程式帳戶剩下最大的挑戰，但變得更容易解決。 身分識別小組應主動採用開始解決此問題的雲端功能，例如 [Azure AD 受控](/azure/active-directory/managed-identities-azure-resources/overview)身分識別。

## <a name="team-composition-and-key-relationships"></a>小組撰寫和索引鍵關聯性

身分識別和金鑰管理小組必須建立具有下列角色的強式關聯性：

- IT 架構和作業
- 安全性架構和作業
- 開發小組
- 資料安全性小組
- 隱私權小組
- 法律團隊
- 合規性/風險管理小組

## <a name="next-steps"></a>後續步驟

複習[基礎結構和端點安全性](./cloud-security-infrastructure-endpoint.md)的功能

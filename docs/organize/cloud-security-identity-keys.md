---
title: 雲端中身分識別和金鑰管理的功能
description: 瞭解雲端中身分識別和金鑰管理的功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: bd98ff4e52faceea4c7dd48a2363efc5ebe87da2
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83755461"
---
# <a name="function-of-identity-and-key-management-in-the-cloud"></a>雲端中身分識別和金鑰管理的功能

負責身分識別管理之安全性小組的主要目標，是要提供人類、服務、裝置和應用程式的驗證和授權。 金鑰和認證管理為密碼編譯作業（通常支援類似身分識別管理的結果）提供安全的散發和存取金鑰內容。

## <a name="modernization"></a>現代化

資料身分識別和金鑰管理現代化的塑造方式如下：

- 身分識別和金鑰/憑證管理專業領域會更密切合作，因為它們都提供驗證和授權的保證，以啟用安全通訊。
- 身分識別控制項是雲端應用程式的主要安全性周邊
- 雲端服務的金鑰型驗證會被身分識別管理取代，因為儲存和安全地提供這些金鑰的存取權會很困難。
- 從內部部署身分識別架構（例如單一身分識別、單一登入（SSO）和原生應用程式整合）學習的正面課程的關鍵重要性。
- 避免常見的內部部署架構錯誤（通常會 overcomplicated 它們）的重要重要性，讓支援變得更容易和攻擊。 這些包括：
  - 擴張群組和組織單位（Ou）。
  - 擴張協力廠商目錄和身分識別管理系統的集合。
  - 缺乏清楚的標準化和應用程式身分識別策略的擁有權。
- 認證竊取攻擊會持續影響高的風險，而且可能會有高機率的威脅。
- 服務帳戶和應用程式帳戶都是最重要的挑戰，但變得更容易解決。 身分識別小組應主動採用開始解決此問題的雲端功能，例如[Azure AD 受控](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)識別。

## <a name="team-composition-and-key-relationships"></a>小組撰寫和按鍵關聯性

身分識別和金鑰管理小組必須建立具有下列角色的強式關聯性：

- IT 架構和作業
- 安全性架構和作業
- 開發小組
- 資料安全性小組
- 隱私權小組
- 法律團隊
- 合規性/風險管理小組

## <a name="next-steps"></a>後續步驟

檢查[基礎結構和端點安全性](./cloud-security-infrastructure-endpoint.md)的功能

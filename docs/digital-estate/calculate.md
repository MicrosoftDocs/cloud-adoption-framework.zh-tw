---
title: 根據數位資產調整成本模型
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 根據數位資產調整成本模型以預測雲端成本。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/10/2018
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: governance
ms.openlocfilehash: e46c045d51bd4961f1cf355352cb84b110a81d81
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71023526"
---
# <a name="align-cost-models-with-the-digital-estate-to-forecast-cloud-costs"></a>根據數位資產調整成本模型以預測雲端成本

在合理化數位資產之後, 您可以使用所選的雲端提供者, 將它與對等的成本模型對齊。 若未以特定的雲端提供者為基礎，就很難討論成本模型。 為了在本文中提供具體範例，我們將假設雲端提供者是 Azure。

Azure 定價工具可協助您以透明和精確度來管理雲端支出, 讓您可以充分利用 Azure 和其他雲端。 這些工具可用來監視、配置及最佳化雲端成本，讓客戶放心加速未來的投資。

- [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview)：Azure Migrate 可能是成本模型對齊最符合成本效益的方法。 只要使用這項工具，數位資產的[清查](./inventory.md)、[有限合理化](./rationalize.md)和成本計算全都可以完成。

- [擁有權總成本（TCO）計算機](https://azure.microsoft.com/pricing/tco/calculator/)：透過 Azure 雲端平台降低內部部署基礎結構的擁有權總成本 (TCO)。 使用 Azure TCO 計算機，可預估您將應用程式工作負載移至 Azure 所省下的成本。 提供內部部署環境的簡短描述, 以取得立即報告。

- [Azure 定價計算機](https://azure.microsoft.com/pricing)：使用我們的定價計算機來估計您預期的每月帳單。 使用計費入口網站隨時追蹤實際的帳戶使用量及帳單。 設定自動電子郵件計費警示, 以在您的支出超過設定的金額時通知您。

- [Azure 成本管理](https://azure.microsoft.com/services/cost-management)：Azure 成本管理 (由 Microsoft 子公司 Cloudyn 授權) 是多重雲端的成本管理解決方案, 可協助您有效地使用及管理 Azure 和其他雲端資源。 請透過 Azure、Amazon Web Services 和 Google Cloud Platform 的應用程式開發介面 (API)，收集雲端使用量和帳單資料。 這些資料可讓您獲得所有雲端平台之資源取用量及成本的完整可見度，並以整合的單一檢視呈現。 持續監控雲端取用量和成本趨勢。 針對您的預算追蹤實際的雲端費用, 以避免超支。 偵測異常開銷和使用效益不彰的情況。 使用歷程記錄資料，提升您預測雲端使用量和開支的精準度。

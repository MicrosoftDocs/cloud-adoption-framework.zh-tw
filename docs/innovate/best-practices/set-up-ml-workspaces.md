---
title: 如何設定機器學習服務工作區
description: 瞭解機器學習環境，考慮影響您如何設定機器學習服務工作區的因素，並決定每個工作區的最佳結構和控制項。
author: dmichaels713
ms.author: brblanch
ms.date: 01/20/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: think-tank
ms.openlocfilehash: 2915bc2bfedf36d8fb2b5774f6d441901b8e8d53
ms.sourcegitcommit: 9cd2b48fbfee229edc778f8c5deaf2dc39dfe2d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230245"
---
# <a name="how-to-set-up-machine-learning-workspaces"></a>如何設定機器學習服務工作區

## <a name="machine-learning-environments-and-role-based-access-control"></a>機器學習環境和以角色為基礎的存取控制

開發、測試和生產環境都支援機器學習作業程式。

![顯示機器學習環境和角色型存取控制的圖表。](./media/ml-environments-and-rbac.png)

**在開發環境中：** 機器學習管線應該支援資料科學家和資料工程師所執行的資料科學和工程活動。 建議他們具有執行實驗相關擁有權限的完整存取權，例如布建定型叢集或建立模型。 不過，他們不應擁有活動的許可權，例如刪除或建立工作區、新增或移除工作區使用者。

**在測試環境中：** 在模型的環境中會執行各種測試，建議您針對模型使用擁護/挑戰者或 A/B 測試。 測試環境應該模仿部署環境;建議您執行負載、模型回應時間和其他測試等測試。 資料科學家與工程師對此環境的存取權有限，主要是唯讀存取權，以及一些設定許可權。 DevOps 工程師手擁有環境的完整存取權。 建議您盡可能將最多的測試自動化。 完成所有測試之後，就必須在生產環境中部署專案關係人核准。

**在生產環境中：** 在批次或即時推斷期間部署模型。 生產環境通常是唯讀的;不過，DevOps 工程師擁有此環境的完整存取權，負責持續支援和維護。 資料科學家和資料工程師對環境具有有限的存取權，而且是唯讀的。

下圖顯示所有環境的角色型存取控制：

![以角色為基礎的存取控制的圖表，適用于所有環境。](./media/rbac-all-environments.png)

下表顯示資料工程師和資料科學家的存取層級會在 DevOps 工程師的存取量增加時，在較高的環境中降低。 這是因為機器學習作業工程師會建立管線、將專案聯結在一起，並在生產環境中部署模型。 針對每個角色，建議使用這個層級的資料細微性。

## <a name="factors-that-influence-machine-learning-workspaces"></a>影響機器學習服務工作區的因素

有多個因素會影響您設定機器學習工作區的方式，而且可以協助您判斷每個工作區類型的最佳結構和控制項：

![如何設定 Azure Machine Learning 工作區的圖表。](./media/set-up-workspaces.png)

- **Public、受限制：**
  - 開發、測試和生產工作區
  - 自訂角色：資料科學家
  - 適用于版本控制和持續整合/持續開發的 Git 整合 (CI/CD) 

- **Public、無限制：**
  - 開發、測試和生產工作區
  - 角色：參與者
  - 版本控制和 CI/CD 的 Git 整合

- **私用、受限制：**
  - 開發、測試和生產工作區
  - Private Link 已啟用
  - 自訂角色：資料科學家
  - 版本控制和 CI/CD 的 Git 整合

- **私用、不受限制：**
  - 開發、測試和生產工作區
  - Private Link 已啟用
  - 角色：參與者
  - 版本控制和 CI/CD 的 Git 整合

- **所有工作區：**
  - 每個專案一個 Azure Machine Learning studio 工作區
  - 每個資料科學家一個計算實例
  - 每個虛擬機器大小的一個計算叢集，與資料科學家共用以進行開發
  - 每個生產管線一個計算叢集
  - 將計算叢集的最小節點大小設定為0，以降低成本
  - 指示使用者在使用後以手動方式關閉計算實例
  - 具有建立計算實例和叢集之存取權的「工作區 admin 自訂角色」
  - 「資料科學家」自訂角色要求其他使用者必須先設定所有基礎結構，資料科學家才能開始工作

## <a name="next-steps"></a>下一步

- 深入瞭解如何 [在 Azure Machine Learning 中建立和管理工作區](/azure/machine-learning/how-to-manage-workspace)。

- [使用 PYTHON SDK](/azure/machine-learning/tutorial-1st-experiment-sdk-setup-local) 在您的開發環境中建立工作區。

- 在開發生命週期中[使用 Azure 入口網站和 Jupyter 筆記本](/azure/machine-learning/tutorial-1st-experiment-sdk-setup)，以設定您的 Azure Machine Learning 開發環境和定型您的模型。

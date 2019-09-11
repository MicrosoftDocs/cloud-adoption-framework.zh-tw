---
title: 協調小組間的責任
titleSuffix: Microsoft Cloud adoption Framework for Azure
description: 瞭解如何在各小組之間協調責任。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: organize
ms.openlocfilehash: 9c3d6bda2c94ebda47ade8737fe8d60f535887fc
ms.sourcegitcommit: 5846ed4d0bf1b6440f5e87bc34ef31ec8b40b338
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2019
ms.locfileid: "70906129"
---
# <a name="align-responsibilities-across-teams"></a>協調小組間的職責

瞭解如何藉由開發跨小組的矩陣，識別負責、參與、*諮詢和通知*（RACI）的合作物件，來協調各小組的責任。 本文提供[建立小組結構](./organization-structures.md)中所述之組織結構的範例 RACI 矩陣：

- [僅限雲端採用小組](#cloud-adoption-team-only)
- [MVP 最佳做法](#best-practice-minimum-viable-product-mvp)
- [中央 IT](#central-it)
- [策略性對齊](#strategic-alignment)
- [操作對齊](#operational-alignment)
- [卓越的雲端中心 (CCoE)](#cloud-center-of-excellence-ccoe)

若要追蹤經過一段時間的組織結構決策，請下載並修改[RACI 試算表範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)。

本文中的範例會指定這些 RACI 結構:

- *負責*函式的一個小組。
- *負責*結果的小組。
- 在規劃期間應*諮詢*的小組。
- 工作完成時應*通知*的小組。

每個資料表的最後一個資料列（第一個除外）包含最符合之雲端功能的連結，以取得其他資訊。

## <a name="cloud-adoption-team-only"></a>僅限雲端採用小組

|  |解決方案交付  |商務對齊  |變更管理  |解決方案作業  |管理 |平臺成熟度  |平臺作業  |平臺自動化  |
|---------|---------|---------|---------|---------|---------|---------|---------|---------|
|雲端採用小組 |負|負|負|負|負|負|負|負|

## <a name="best-practice-minimum-viable-product-mvp"></a>最佳做法: 最基本的可行產品 (MVP)

|  |解決方案交付  |商務對齊  |變更管理  |解決方案作業  |管理 |平臺成熟度  |平臺作業  |平臺自動化  |
|---------|---------|---------|---------|---------|---------|---------|---------|---------|
|雲端採用小組|負|負|負|負|參考|參考|參考|獲得|
|雲端治理小組|參考|獲得|獲得|獲得|負|負|負|負|
||||||||||
|已調整的雲端功能|[雲端採用](./cloud-adoption.md)|[雲端策略](./cloud-strategy.md)|[雲端策略](./cloud-strategy.md)|[雲端作業](./cloud-operations.md)|[CCoE](./cloud-center-excellence.md)-[雲端治理](./cloud-governance.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端自動化](./cloud-automation.md)|

## <a name="central-it"></a>中央 IT

| |解決方案交付  |商務對齊  |變更管理  |解決方案作業  |管理 |平臺成熟度  |平臺作業  |平臺自動化  |
|---------|---------|---------|---------|---------|---------|---------|---------|---------|
|雲端採用小組  |負|負|承擔    |承擔|獲得   |獲得   |獲得   |獲得   |
|雲端治理小組|參考  |獲得   |獲得   |獲得   |負|參考  |承擔|獲得   |
|中央 IT           |參考  |獲得   |負   |負   |承擔  |負|負|負|
||||||||||
|已調整的雲端功能|[雲端採用](./cloud-adoption.md)|[雲端策略](./cloud-strategy.md)|[雲端策略](./cloud-strategy.md)|[雲端作業](./cloud-operations.md)|[雲端治理](./cloud-governance.md)|[中央 IT](./central-it.md)|[中央 IT](./central-it.md)|[中央 IT](./central-it.md)|

## <a name="strategic-alignment"></a>策略性對齊

|  |解決方案交付  |商務對齊  |變更管理  |解決方案作業  |管理 |平臺成熟度  |平臺作業  |平臺自動化  |
|---------|---------|---------|---------|---------|---------|---------|---------|---------|
|雲端策略小組  |參考  |負|負|參考  |參考  |獲得   |獲得   |獲得   |
|雲端採用小組  |負|參考  |承擔|負|獲得   |獲得   |獲得   |獲得   |
|CCoE 模型 RACI      |參考  |獲得   |獲得   |獲得   |負|負|負|負|
||||||||||
|已調整的雲端功能|[雲端採用](./cloud-adoption.md)|[雲端策略](./cloud-strategy.md)|[雲端策略](./cloud-strategy.md)|[雲端作業](./cloud-operations.md)|[CCoE](./cloud-center-excellence.md)-[雲端治理](./cloud-governance.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端自動化](./cloud-automation.md)|

## <a name="operational-alignment"></a>操作對齊

|  |解決方案交付  |商務對齊  |變更管理  |解決方案作業  |管理 |平臺成熟度  |平臺作業  |平臺自動化  |
|---------|---------|---------|---------|---------|---------|---------|---------|---------|
|雲端策略小組  |參考  |負|負|參考  |參考  |獲得   |獲得   |獲得   |
|雲端採用小組  |負|參考  |承擔|參考  |獲得   |獲得   |獲得   |獲得   |
|雲端營運小組|參考  |參考  |承擔|負|參考  |獲得   |負|參考  |
|CCoE 模型 RACI      |參考  |獲得   |獲得   |獲得   |負|負|承擔|負|
||||||||||
|已調整的雲端功能|[雲端採用](./cloud-adoption.md)|[雲端策略](./cloud-strategy.md)|[雲端策略](./cloud-strategy.md)|[雲端作業](./cloud-operations.md)|[CCoE](./cloud-center-excellence.md)-[雲端治理](./cloud-governance.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端自動化](./cloud-automation.md)|

## <a name="cloud-center-of-excellence-ccoe"></a>卓越的雲端中心 (CCoE)

|  |解決方案交付  |商務對齊  |變更管理  |解決方案作業  |管理 |平臺成熟度  |平臺作業  |平臺自動化  |
|---------|---------|---------|---------|---------|---------|---------|---------|---------|
|雲端策略小組  |參考  |負|負|參考  |參考  |獲得   |獲得   |獲得   |
|雲端採用小組  |負|參考  |承擔|參考  |獲得   |獲得   |獲得   |獲得   |
|雲端營運小組|參考  |參考  |承擔|負|參考  |獲得   |負|參考  |
|雲端治理小組|參考  |獲得   |獲得   |參考  |負|參考  |承擔|獲得   |
|雲端平臺小組  |參考  |獲得   |獲得   |參考  |參考  |負|承擔|承擔|
|雲端自動化團隊|參考  |獲得   |獲得   |獲得   |參考  |承擔|承擔|負|
||||||||||
|已調整的雲端功能|[雲端採用](./cloud-adoption.md)|[雲端策略](./cloud-strategy.md)|[雲端策略](./cloud-strategy.md)|[雲端作業](./cloud-operations.md)|[CCoE](./cloud-center-excellence.md)-[雲端治理](./cloud-governance.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端平臺](./cloud-platform.md)|[CCoE](./cloud-center-excellence.md)-[雲端自動化](./cloud-automation.md)|

## <a name="next-steps"></a>後續步驟

若要追蹤一段時間內有關組織結構的決策，請下載並修改[RACI 試算表範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)。 從本文的 RACI 矩陣複製並修改最緊密對齊的範例。

> [!div class="nextstepaction"]
> [下載 RACI 試算表範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)

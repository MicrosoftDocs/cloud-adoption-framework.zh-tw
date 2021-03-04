---
title: AKS 企業規模儲存體
description: 適用于企業規模儲存體的 AKS 指導方針
author: gbowerman
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 896d327577c790952de9f58002cf4d5b0eee1cda
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794322"
---
# <a name="aks-enterprise-scale-storage"></a>AKS 企業規模儲存體

您的組織或企業必須設計適合的 AKS 平台層級功能，以符合其特定需求。 具體來說，這些應用程式工作負載有可能不同的儲存體需求。 針對這些應用程式選擇儲存體需求時，有多項考慮，例如效能、持續性、協助工具和成本。 Kubernetes 提供不同的儲存體概念來滿足這些需求，特別是在 AKS 中，您必須為每個應用程式選擇正確的服務。

## <a name="design-considerations"></a>設計考量

請考慮下列因素：

- 您的應用程式是否需要具狀態工作負載。
- 應用程式是否需要管理秘密。
- 無論您的應用程式是否需要保證的儲存體輸送量，以及此特性如何與儲存層相關， (standard、premium、ultra) 、儲存體服務 (Azure 磁片、Azure 檔案服務、Azure NetApp Files 或協力廠商服務) 和背景工作角色大小。
- 在叢集中執行的多個寫入讀取應用程式。
- 多組織使用者共用和節點集區隔離。
- 建立符合您組織需求的儲存體類別。

## <a name="design-recommendations"></a>設計建議

以下是您設計的經過證實做法：

- 如果考慮到具狀態的工作負載，請準備動態儲存體 Azure 磁片或 Azure 檔案儲存體。
- 如果 multipod 需要在相同的儲存體中寫入/讀取，則支援 Azure 檔案服務或協力廠商服務，例如 Azure NetApp Files。
- 在大部分情況下，建議使用 premium SSD 儲存體。
- 針對需要不同儲存體和效能特性的不同工作負載，請使用節點集區。
- 建立儲存類別。

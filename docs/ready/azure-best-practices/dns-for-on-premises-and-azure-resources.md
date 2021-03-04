---
title: 適用于內部部署和 Azure 的 DNS
description: 檢查內部部署和 Microsoft Azure DNS 的主要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 5a3793322e66892d314b6291fd83d75a3bcbe932
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794561"
---
# <a name="dns-for-on-premises-and-azure-resources"></a>適用于內部部署和 Azure 資源的 DNS

網域名稱系統 (DNS) 是整個企業規模架構中的重要設計主題。 某些組織可能會想要在 DNS 中使用現有的投資。 其他人可能會看到雲端採用將其內部 DNS 基礎結構現代化，並使用原生 Azure 功能的機會。

**設計考慮：**

- 您可以使用 DNS 解析程式搭配 Azure 私人 DNS 來進行跨單位名稱解析。

- 您可能需要在內部部署與 Azure 之間使用現有的 DNS 解決方案。

- 虛擬網路可與自動註冊連結的私人 DNS 區域數目上限為一個。

- 虛擬網路可以連結的私人 DNS 區域數目上限為1000，但未啟用自動註冊。

**設計建議：**

- 針對 Azure 中的名稱解析是必要的環境，請使用 Azure 私人 DNS 進行解析。 建立委派區域以進行名稱解析 (例如 `azure.contoso.com`) 。

- 針對在 Azure 和內部部署之間進行名稱解析的環境，請使用現有的 DNS 基礎結構 (例如，Active Directory 整合式 DNS) 部署到至少兩部虛擬機器 (Vm) 。 在虛擬網路中設定 DNS 設定，以使用這些 DNS 伺服器。

- 您仍然可以將 Azure 私人 DNS 區域連結至虛擬網路，並使用 DNS 伺服器做為混合式解析程式，搭配使用內部部署 dns 名稱的條件式轉送（例如 `onprem.contoso.com` ），並使用內部部署 dns 伺服器。 您可以使用條件轉寄站來設定內部部署伺服器，以在 azure 中針對 Azure 私人 DNS 區域解析 Vm (例如 `azure.contoso.com`) 。

- 需要和部署自己的 DNS (（例如 Red Hat OpenShift) ）的特殊工作負載應使用其慣用的 DNS 解決方案。

- 啟用 Azure DNS 的自動註冊，自動管理虛擬網路內部署的虛擬機器 DNS 記錄的生命週期。

- 使用虛擬機器作為使用 Azure 私人 DNS 解析跨單位 DNS 的解析程式。

- 在全域連線訂用帳戶內建立 Azure 私人 DNS 區域。 例如，您可以建立其他 Azure 私人 DNS 區域 (例如， `privatelink.database.windows.net` 或 `privatelink.blob.core.windows.net` Azure private Link) 。

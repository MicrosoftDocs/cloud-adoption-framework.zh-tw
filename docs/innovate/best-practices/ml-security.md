---
title: 機器學習安全性
description: 機器學習服務為企業提供獨特的安全性考慮，而且公司在設計和評估機器學習架構時，應該考慮幾個安全準則。
author: shinchan75034
ms.author: brblanch
ms.date: 01/20/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: think-tank
ms.openlocfilehash: a702b08b518919b6f3b1a30bf954e81a5b02aeea
ms.sourcegitcommit: 9cd2b48fbfee229edc778f8c5deaf2dc39dfe2d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230296"
---
# <a name="machine-learning-security"></a>機器學習安全性

機器學習服務為企業提供獨特的安全性考慮，而且公司在設計和評估機器學習架構時，應該考慮幾個安全準則。

- **復原能力：** 機器學習系統應識別異常行為並防止操作或強制型轉。

- **判斷：** 任何牽涉到 AI 的資料存取案例都應該將資料的存取限制為最短的範圍。

- **惡意資料：** 機器學習演算法必須防止惡意引進的資料。

- **透明度和責任：** AI 必須具有可提供透明度和責任的內建取證功能。 這些功能是一種早期的 AI 入侵偵測，可協助工程師瞭解決定的確切時間點，以及使用的資料。

- **安全環境：** 必須保護開發、定型和推斷環境的存取，以保護資料和模型的結果/預測。

## <a name="deploy-azure-kubernetes-service-to-secure-an-inference-environment"></a>部署 Azure Kubernetes Service 以保護推斷環境

建議您在生產環境中使用 Azure Kubernetes Service (AKS) 進行推斷。 您的虛擬網路中有兩個可用選項：

- 使用公用 IP 位址將 AKS 叢集部署或連結至您的虛擬網路。
- 將私人 AKS 叢集附加至您的虛擬網路。

預設 AKS 叢集具有具有公用 IP 位址的控制平面。 如果您的 AKS 叢集需要私人 IP 位址，請使用私人 AKS 叢集。 針對推斷，AKS 叢集和 Azure Machine Learning 工作區必須位於相同的虛擬網路中。

若要保護虛擬網路內的預設 AKS 推斷叢集，請指定三個 IP 位址：

- CIDR 標記法指定 **AKS 位址範圍**。 不得與任何其他子網範圍重迭。

- 系統會為 AKS 內的 DNS 服務指派 **AKS dns 服務 IP 位址** 。 它必須在 AKS 位址範圍內。

- **Docker 橋接器位址** 會指派給 docker 橋接器，該橋接器會以容器的形式執行您的評分腳本。 此位址不得位於您的子網 IP 或 AKS 位址範圍內。

您可以將 AKS 設定為使用內部和私人負載平衡器搭配私人 AKS 叢集。 此案例只允許私人 Ip，而且您可以使用 Python SDK 或 Azure 命令列擴充功能，但不能 Azure Machine Learning studio 執行這項工作。 使用私人負載平衡器時，您必須將「網路參與者」角色授與包含虛擬網路的 AKS 叢集資源群組。

## <a name="next-steps"></a>下一步

- 參考如何 [使用虛擬網路保護 Azure Machine Learning 推斷環境](/azure/machine-learning/how-to-secure-inferencing-vnet?tabs=python#secure-vnet-traffic) ，以查看 IP 位址範圍，以及在虛擬網路中執行推斷的步驟。

- 請參閱 [網路參與者角色一節](/azure/machine-learning/how-to-secure-inferencing-vnet?tabs=python#network-contributor-role) ，以瞭解如何設定私人負載平衡器和設定網路參與者角色。

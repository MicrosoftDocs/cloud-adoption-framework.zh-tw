---
title: 應用程式開發與部署
description: 瞭解如何在應用程式開發和架構的雲端採用架構中使用 Kubernetes。
author: sabbour
ms.author: asabbour
ms.date: 03/20/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: e6b08905ddde67b2fcef2933dd9577e099c5fe8b
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86448770"
---
<!-- cSpell:ignore asabbour sabbour autoscaler Istio Linkerd -->

# <a name="application-development-and-deployment"></a>應用程式開發與部署

檢查應用程式開發的模式和實務、設定 Azure Pipelines，並實行網站可靠性工程（SRE）最佳作法。

## <a name="plan-train-and-proof"></a>規劃、定型和證明

當您開始使用時，以下的檢查清單和資源將可協助您規劃應用程式的開發和部署。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您是否已準備好開發環境和設定工作流程？
> - 您要如何將專案資料夾結構為支援 Kubernetes 應用程式開發？
> - 您是否已識別出應用程式的狀態、設定和儲存需求？

<!-- markdownlint-disable MD033 -->

<! docsTest:ignore "AAD Pod Identity -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單 | 資源 |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **準備您的開發環境。** 使用您建立容器和設定開發工作流程所需的工具來設定您的環境。 | [在 Visual Studio Code 中使用 Docker](https://code.visualstudio.com/docs/azure/docker) <br> [在 Visual Studio Code 中使用 Kubernetes](https://code.visualstudio.com/docs/azure/kubernetes) <br> [Azure Dev Spaces 簡介](https://docs.microsoft.com/azure/dev-spaces/about) |
> | **容器化您的應用程式。** 熟悉端對端 Kubernetes 開發體驗，包括應用程式架構、內部迴圈工作流程、應用程式管理架構、CI/CD 管線、記錄匯總、監視和應用程式計量。 | [使用 Docker 和 Kubernetes 容器化您的應用程式（電子書）](https://azure.microsoft.com/resources/containerize-your-apps-with-docker-and-kubernetes) <br> [Azure 上的端對端 Kubernetes 開發體驗（網路研討會）](https://info.microsoft.com/AU-AzureApp-WBNR-FY20-11Nov-12-ContainerizeYourApplicationswithKubernetesonAzure-SRDEM10557_LP02OnDemandRegistration-ForminBody.html) |
> | **檢查常見的 Kubernetes 案例。** Kubernetes 通常被視為提供微服務的平臺，但它變得更廣泛。 觀看這段影片以瞭解常見的 Kubernetes 案例，例如 batch 分析和工作流程。    | [&nbsp; &nbsp; 使用 Kubernetes 的常見案例 &nbsp; &nbsp; &nbsp; （影片）](https://www.youtube.com/watch?v=zd8vYhrFXp4&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=7) |
> | **準備您的應用程式以進行 Kubernetes。** 準備您的應用程式檔案系統配置，以進行每週或每日版本的 Kubernetes 和組織。 瞭解 Kubernetes 部署程式如何啟用可靠、零停機的升級。 | [成功 Kubernetes 應用程式的專案設計和版面配置（網路研討會）](https://info.microsoft.com/ww-OnDemandRegistration-successful-kubernetes-applications-webinar.html) <br> [Kubernetes 部署的工作方式（影片）](https://www.youtube.com/watch?v=mNK14yXIZF4&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=3) <br> [經歷 AKS 的研討會](https://aka.ms/learn/aksworkshop) |
> | **管理應用程式儲存區。** 瞭解 pod 的效能需求和存取方法，讓您可以提供適當的儲存體選項。 您也應該針對附加的儲存體規劃備份方法並測試還原程序。 | [Kubernetes 中具狀態應用程式的基本概念（影片）](https://www.youtube.com/watch?v=GieXzb91I40&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=9) <br> [Docker 應用程式中的狀態和資料](https://docs.microsoft.com/dotnet/architecture/microservices/architect-microservice-container-applications/docker-application-state-data) <br> [Azure Kubernetes Service 中的儲存體選項](https://docs.microsoft.com/azure/aks/operator-best-practices-storage) |
> | **管理應用程式秘密。** 請勿將認證儲存在您的應用程式代碼中。 金鑰保存庫應用來儲存和取出金鑰和認證。  | [Kubernetes 和設定管理的工作方式（影片）](https://www.youtube.com/watch?v=vRcQOZLnKUk&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=11) <br> [瞭解 Kubernetes 中的秘密管理（影片）](https://www.youtube.com/watch?v=KmhM33j5WYk&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=10) <br> [搭配 Kubernetes 使用 Azure Key Vault](https://github.com/azure/kubernetes-keyvault-flexvol) <br> [使用 Azure AD pod 身分識別來驗證和存取 Azure 資源](https://github.com/azure/aad-pod-identity) |

## <a name="deploy-to-production-and-apply-best-practices"></a>部署到生產環境並套用最佳作法

當您準備應用程式以進行生產時，您應該執行一組最基本的最佳作法。 請在此階段使用以下檢查清單。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您可以監視應用程式的所有層面嗎？
> - 您是否已為您的應用程式定義資源需求？ 如何調整需求？
> - 您是否可以部署新版本的應用程式，而不會影響生產系統？

<!-- -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源                                                                                                     |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **設定準備就緒和作用中的健康狀態檢查。** Kubernetes 會使用就緒和活動檢查來知道您的應用程式何時已準備好接收流量，以及何時需要重新開機。 若未定義這類檢查，Kubernetes 將無法判斷您的應用程式是否已啟動且正在執行。   | [活動和準備檢查](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes) |
> | **設定記錄、應用程式監視和警示。** 監視容器很重要，尤其在您使用多個應用程式大規模執行生產環境叢集時。 容器化應用程式的建議記錄方法是寫入標準輸出（stdout）和標準錯誤（stderr）資料流程。   | [在 Kubernetes 中登入](https://kubernetes.io/docs/concepts/cluster-administration/logging) <br> [開始使用 Kubernetes 的監視和警示（影片）](https://www.youtube.com/watch?v=W7aN_z-cyUw&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=16) <br> [適用於容器的 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) <br> [在 Azure Kubernetes Service (AKS) 中啟用並檢閱 Kubernetes 主要節點記錄](https://docs.microsoft.com/azure/aks/view-master-logs) <br> [即時查看 Kubernetes 記錄、事件和 pod 計量](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-livedata-overview) |
> | **定義應用程式的資源需求。** 在 Kubernetes 叢集中管理計算資源的主要方式是使用 pod 要求和限制。 這些要求和限制會告訴 Kubernetes 排程器應指派 pod 的計算資源。     | [定義 &nbsp; pod &nbsp; 資源 &nbsp; 要求 &nbsp; 和 &nbsp; 限制](https://docs.microsoft.com/azure/aks/developer-best-practices-resource-management) |
> | **設定應用程式調整需求。** Kubernetes 支援水平 Pod 自動調整，可根據 CPU 使用率或其他選取的計量來調整部署中的 Pod 數目。 若要使用自動調整程式，pod 中的所有容器都必須定義 CPU 要求和限制。    | [設定水準 pod 自動調整](https://docs.microsoft.com/azure/aks/tutorial-kubernetes-scale#autoscale-pods) |
> | **使用自動化管線和 DevOps 部署應用程式。**  將程式碼認可到生產部署之間所有步驟的完整自動化，可讓小組專注于建立程式碼，並移除手動操作步驟中的額外負荷和潛在的人為錯誤。 部署新程式碼的速度更快且風險較低，協助小組變得更靈活、更具生產力，以及更有把握其執行中的程式碼。    | [發展您的 DevOps 實務](https://docs.microsoft.com/learn/paths/evolve-your-devops-practices) <br> [設定 Kubernetes 組建管線（影片）](https://www.youtube.com/watch?v=5irsAdKoEBU&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=6) <br> [Azure Kubernetes Service 的部署中心](https://docs.microsoft.com/azure/aks/deployment-center-launcher) <br> [部署至 Azure Kubernetes Service 的 GitHub 動作](https://docs.microsoft.com/azure/aks/kubernetes-action) <br> [使用 Jenkins 的 CI/CD Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/jenkins-continuous-deployment) |

## <a name="optimize-and-scale"></a>優化和調整

應用程式現在已進入生產階段，如何優化您的工作流程，並準備您的應用程式和小組進行調整？ 使用優化和調整檢查清單來準備。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 跨領域應用程式的顧慮是否從您的應用程式中抽象化？
> - 您是否能夠維護系統和應用程式的可靠性，同時仍會逐一查看新功能和版本？

<!-- docsTest:ignore Consul -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源                                                                                                     |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **部署 API 閘道。** API 閘道可做為微服務的進入點、將用戶端與您的微服務分離、增加額外的安全性層級，並藉由消除處理跨領域考慮的負擔，降低微服務的複雜性。     | [將 Azure API 管理與 Azure Kubernetes Service 中部署的微服務搭配使用](https://docs.microsoft.com/azure/api-management/api-management-kubernetes) |
> | **部署服務網格。** 服務網格提供工作負載的功能，例如流量管理、復原、原則、安全性、強身份識別和可檢視性。 您的應用程式與這些作業功能分離，而服務網格會將它們移出應用層，並向下移動到基礎結構層。     | [&nbsp;服務 &nbsp; 網格 &nbsp; &nbsp; 在 Kubernetes 中的使用方式 &nbsp; &nbsp; （影片）](https://www.youtube.com/watch?v=izVWk7rYqWI&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=15&t=0s) <br> [瞭解服務網格](https://docs.microsoft.com/azure/aks/servicemesh-about) <br> [搭配 Azure Kubernetes Service 使用 Istio](https://docs.microsoft.com/azure/aks/servicemesh-istio-about) <br> [搭配 Azure Kubernetes Service 使用 Linkerd](https://docs.microsoft.com/azure/aks/servicemesh-linkerd-about) <br> [搭配 Azure Kubernetes Service 使用 Consul](https://docs.microsoft.com/azure/aks/servicemesh-consul-about) |
> | **實行網站可靠性工程（SRE）實務。**  網站可靠性工程（SRE）是一種經過證實的方法，可維護重要的系統和應用程式可靠性，同時以 marketplace 所要求的速度來進行反覆運算。   | [網站可靠性工程（SRE）簡介](https://docs.microsoft.com/learn/modules/intro-to-site-reliability-engineering) <br> [Microsoft DevOps：遊戲串流 SRE](https://azure.microsoft.com/resources/devops-at-microsoft-game-streaming-sre) |

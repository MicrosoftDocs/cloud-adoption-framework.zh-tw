---
title: 適用于 AKS 的企業級網路拓朴和連線能力
description: 瞭解此企業規模案例如何改善 Azure Kubernetes Service (AKS) 的網路拓朴和連線能力。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: d04811ba3f0aa034362cb1683d82074dfd5eb776
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102111985"
---
# <a name="network-topology-and-connectivity-for-azure-kubernetes-service-aks-enterprise-scale-scenario"></a>適用于 Azure Kubernetes Service (AKS) 企業規模案例的網路拓撲和連線能力

<!-- docutune:casing "Basic and Standard" -->

## <a name="design-considerations"></a>設計考量

- AKS 支援兩種網路模型： kubenet 和 Azure 容器網路介面 (CNI) 。
  - CNI 需要針對 IP 位址進行額外的規劃。
  - 只有 CNI 支援 Windows Server 節點和網路原則集區。
  - kubenet 需要使用者定義的路由， (Udr) 要手動套用。
  - 確認每個 CNI 外掛程式目前支援的功能 [清單](/azure/aks/concepts-network#compare-network-models) 。
- IP 位址和虛擬網路子網的大小必須謹慎規劃，以支援叢集的調整。 例如，您可以新增更多節點。
- 虛擬節點可以用來進行快速叢集調整，但有一些 [已知的限制](/azure/aks/virtual-nodes-portal)。
- AKS 叢集支援基本和標準的 Azure 負載平衡器 Sku。
- 您可以使用公用或內部負載平衡器來公開 AKS 服務。 內部負載平衡器可以設定在與 Kubernetes 節點相同的子網中，或在專用子網中設定。
- 適用于 AKS 的 azure 原則和 [Azure 原則附加](/azure/governance/policy/concepts/policy-for-kubernetes) 元件可以控制和限制在 AKS 叢集中建立的物件，例如拒絕在叢集中建立公用 IP 位址。
- AKS 使用 CoreDNS 為叢集中執行的 pod 提供名稱解析。
  - CoreDNS 會直接解析叢集內部網域。
  - 其他網域將轉送至 Azure 虛擬網路中所設定的 DNS 伺服器，這會是預設的 Azure DNS 解析程式，或在虛擬網路層級設定的任何自訂 DNS 伺服器。
- 輸出 (輸出) 可透過 Azure 防火牆或網路虛擬裝置叢集傳送網路流量。
  - 根據預設，AKS 叢集具有不受限制的輸出網際網路存取。
  - AKS 叢集的輸出流量可以透過 Azure 防火牆或網路虛擬裝置叢集，藉由在 AKS 子網中設定 Udr 來傳送。
- 根據預設，AKS 叢集中的所有 Pod 都可以無限制地傳送及接收流量。 Kubernetes 網路原則可以用來改善安全性，並篩選 AKS 叢集中 pod 之間的網路流量。 有兩個 [網路原則模型](/azure/aks/use-network-policies#network-policy-options-in-aks) 可供 AKS。
- 服務網格提供流量管理、復原、原則、安全性、強式身分識別和可檢視性等功能。 如需詳細資訊，請參閱 [選取準則](/azure/aks/servicemesh-about#selection-criteria)。
- 全球負載平衡機制（例如 [Azure 流量管理員](/azure/traffic-manager/traffic-manager-overview) 和 [azure 前端](/azure/frontdoor/front-door-overview) ）會將流量路由傳送到多個叢集（可能位於不同的 Azure 區域），以提高復原能力。

### <a name="private-clusters"></a>私人叢集

AKS 叢集 IP 可見度可以是公用或私用。 [私人](/azure/aks/private-clusters) 叢集會透過私人 IP 位址（而不是公用 IP 位址）公開 Kubernetes API。 此私人 IP 位址實際上是透過 [私人端點](/azure/private-link/private-endpoint-overview)在 AKS 虛擬網路中表示。 Kubernetes API 不應透過其 IP 位址來存取，而是透過其完整功能變數名稱)  (FQDN。 從 Kubernetes API FQDN 到其 IP 位址的解析通常會由 [Azure 私人 DNS 區域](/azure/dns/private-dns-overview)執行。 此 DNS 區域可由 Azure 在 [AKS 節點資源群組](/azure/aks/faq#why-are-two-resource-groups-created-with-aks)中建立，您也可以指定現有的 [dns 區域](/azure/aks/private-clusters#no-private-dns-zone-prerequisites)。

遵循企業級的經證實的作法，Azure 工作負載的 DNS 解析是由連線訂用帳戶中部署的集中式 DNS 伺服器所提供，其位於中樞虛擬網路或連接到 Azure 虛擬 WAN 的共用服務虛擬網路中。 這些伺服器會有條件地使用 Azure DNS (IP 位址) 來解析 Azure 特定和公用名稱 `168.63.129.16` ，以及使用公司 dns 伺服器來解析私人名稱。 不過，這些集中式 DNS 伺服器將無法解析 AKS API FQDN，直到它們與針對 AKS 叢集建立的 DNS 私人區域連線為止。 請注意，每個 AKS 都會有唯一的 DNS 私人區域，因為區功能變數名稱稱前面會加上隨機的 GUID。 因此，針對每個新的 AKS 叢集，其對應的私人 DNS 區域應連接至中央 DNS 伺服器所在的虛擬網路。

所有虛擬網路都應該設定為使用這些中央 DNS 伺服器進行名稱解析。 但是，如果 AKS 的虛擬網路設定為使用中央 DNS 伺服器，而且這些伺服器尚未連線至私人 DNS 區域，則 AKS 節點將無法解析 Kubernetes API 的 FQDN，且建立 AKS 叢集將會失敗。 AKS 虛擬網路應設定為在叢集建立之後才使用中央 DNS 伺服器。

一旦建立叢集之後，就會在 DNS 私人區域與部署中央 DNS 伺服器的虛擬網路之間建立連線。 AKS 虛擬網路也已設定為使用連線訂用帳戶中的中央 DNS 伺服器，而 AKS Kubernetes API 的系統管理員存取權將會遵循此流程：

![私人叢集](./media/network-private-cluster.png)

> [!NOTE]
> 本文中的影像會使用傳統的中樞和輪輻連接模型來反映設計。 企業規模的登陸區域可以選擇虛擬 WAN 連線模型，其中中央 DNS 伺服器會在連線到虛擬 WAN 中樞的共用服務虛擬網路中。

1. 系統管理員將會解析 Kubernetes API 的 FQDN。 內部部署 DNS 伺服器會將要求轉送到授權伺服器，也就是 Azure 中的 DNS 解析程式。 這些伺服器會將要求轉送至 Azure DNS 伺服器 (`168.63.129.16`) ，以找出來自 Azure 私人 DNS 區域的 IP 位址。
2. 解析 IP 位址之後，Kubernetes API 的流量會從內部部署路由傳送至 Azure 中的 VPN 或 ExpressRoute 閘道，視連線模型而定。
3. 私人端點會 `/32` 在中樞虛擬網路中引進路由，因此 VPN 和 ExpressRoute 閘道會將流量直接傳送到部署在 AKS 虛擬網路中的 KUBERNETES API 私人端點。

### <a name="traffic-from-application-users-to-the-cluster"></a>從應用程式使用者到叢集的流量

傳入的 (輸入) 控制器可用來公開在 AKS 叢集中執行的應用程式。

- 輸入控制器可提供應用層級的路由，代價是稍微增加複雜性。
- 輸入控制器可將 Web 應用程式防火牆併入 (WAF) 功能。
- 輸入控制器可在叢集和叢集中執行：
  - 叢集輸入控制器會將計算 (（例如 HTTP 流量路由或 TLS 終止) ）卸載至 AKS 以外的其他服務，例如 [Azure 應用程式閘道輸入控制器 (AGIC) 附加](/azure/application-gateway/ingress-controller-overview)元件。
  - 叢集內解決方案會使用 AKS 叢集資源進行計算 (例如 HTTP 流量路由或 TLS 終止) 。 叢集中輸入控制器可提供較低的成本，但需要謹慎的資源規劃和維護。
- 基本的 HTTP 應用程式路由附加元件很容易使用，但有一些限制，如 [HTTP 應用程式路由](/azure/aks/http-application-routing)中所述。

輸入控制器可以公開具有公用或私人 IP 位址的應用程式和 Api。

- 設定應該與輸出篩選設計一致，以避免非對稱式路由。
- 如果需要 TLS 終止，則必須考慮 TLS 憑證的管理。

應用程式流量可以來自內部部署或公用網際網路。 下圖說明 [Azure 應用程式閘道](/azure/application-gateway/overview) 設定為從內部部署和從公用網際網路反向 proxy 連線到叢集的範例。

![應用程式流量](./media/network-app-access.png)

來自內部部署的流量會遵循上圖中編號藍色標注的流程。

1. 用戶端將會使用連接訂用帳戶或內部部署 DNS 伺服器中部署的 DNS 伺服器，來解析指派給應用程式的 FQDN。
2. 將應用程式 FQDN 解析為 IP 位址之後 (應用程式閘道) 的私人 IP 位址，流量會透過 VPN 或 ExpressRoute 閘道路由傳送。
3. 閘道子網中的路由設定為將要求傳送至 Web 應用程式防火牆。
4. Web 應用程式防火牆會將有效要求傳送給在 AKS 叢集中執行的工作負載。

此範例中的 Azure 應用程式閘道可以部署在與 AKS 叢集相同的訂用帳戶中，因為其設定與 AKS 中部署的工作負載非常密切相關，因此由相同的應用程式小組管理。 從網際網路存取時，會遵循上圖中編號綠色標注的流程。

1. 來自公用網際網路的用戶端會使用 [Azure 流量管理員](/azure/traffic-manager/traffic-manager-overview)來解析應用程式的 DNS 名稱。 或者，您也可以使用其他全球負載平衡技術，例如 [Azure Front 大門](/azure/frontdoor/front-door-overview) 。
2. 流量管理員會將應用程式公用 FQDN 解析為應用程式閘道的公用 IP 位址，用戶端會透過公用網際網路進行存取。
3. 應用程式閘道將會存取 AKS 中部署的工作負載。

> [!NOTE]
> 這些流程僅適用于 web 應用程式。 非 web 應用程式不在本文的討論範圍內，而且可以透過中樞虛擬網路中的 Azure 防火牆來公開，或是在使用虛擬 WAN 連線模型的情況下，透過安全虛擬中樞來公開。

或者，您可以建立 web 應用程式的流量，以跨越連線訂用帳戶中的 Azure 防火牆，以及 AKS 虛擬網路中的 WAF。 這種方法的優點是提供一些額外的保護，例如使用 [Azure 防火牆智慧型篩選](/azure/firewall/threat-intel) 來捨棄網際網路上已知惡意 IP 位址的流量。 不過，它也有一些缺點。 例如，在公開應用程式時，遺失原始用戶端 IP 位址，以及防火牆和應用程式小組之間所需的額外協調。 這是因為 Azure 防火牆將需要 (DNAT) 規則的目的地網路位址轉譯。

### <a name="traffic-from-the-aks-pods-to-backend-services"></a>從 AKS pod 到後端服務的流量

在 AKS 叢集中執行的 pod 可能需要存取後端服務，例如 Azure 儲存體、Azure SQL 資料庫或 Azure Cosmos DB NoSQL 資料庫。 [虛擬網路服務端點](/azure/virtual-network/virtual-network-service-endpoints-overview) 和 [私用連結](/azure/private-link/private-link-overview) 可以用來保護這些 Azure 受管理服務的連線能力。

如果您使用 Azure 私人端點來處理後端流量，則可以使用 Azure 私人 DNS 區域來執行 Azure 服務的 DNS 解析。 由於整個環境的 DNS 解析程式都位於中樞虛擬網路 (或共用服務虛擬網路（如果使用虛擬 WAN 連線模型) ），因此應該在連線訂用帳戶中建立這些私人區域。 若要建立解析私用服務之 FQDN 所需的 A 記錄，您可以將連線訂用帳戶) 中的私人 DNS 區域 (與應用程式訂用帳戶) 中的私人端點 (產生關聯。 這項作業需要每個訂用帳戶中的特定許可權。

您可以手動建立 A 記錄，但是將私人 DNS 區域與私人端點相關聯，會導致安裝程式較不容易設定。

從 AKS pod 到透過私人端點公開的 Azure PaaS 服務的後端連線將遵循下列順序：

![後端流量](./media/network-backend-access.png)

1. AKS pod 將會使用連線訂用帳戶中的中央 DNS 伺服器（定義為 AKS 虛擬網路中的自訂 DNS 伺服器），將 Azure 平臺即服務的 FQDN 解析為 (PaaS) 。
2. 解析的 IP 將是私人端點的私人 IP 位址，可直接從 AKS pod 存取。

AKS pod 和私人端點之間的流量如果使用虛擬 WAN) ，則不會通過中樞虛擬網路 (或安全虛擬中樞內的 Azure 防火牆，即使 AKS 叢集已設定為 [使用 Azure 防火牆](/azure/aks/limit-egress-traffic)進行輸出篩選也一樣。 原因是私人端點會 `/32` 在應用程式虛擬網路的子網（AKS 部署所在）中建立路由。

## <a name="design-recommendations"></a>設計建議

- 如果您的安全性原則要求 Kubernetes API 具有私人 IP 位址 (而不是公用 IP 位址) ，請 [部署私人 AKS](/azure/aks/private-clusters)叢集。
  - 建立私人叢集時，請使用自訂的私人 DNS 區域，而不是讓建立程式使用 [系統私人 dns 區域](/azure/aks/private-clusters#configure-private-dns-zone)。
- 除非您有可指派給 AKS 叢集的有限 IP 位址範圍，否則請使用 Azure 容器網路介面 (CNI) 作為網路模型。
  - 遵循與 CNI 進行 [IP 位址規劃](/azure/aks/configure-azure-cni#plan-ip-addressing-for-your-cluster) 相關的檔。
  - 若要使用 Windows Server 節點集區和虛擬節點來確認最終的限制，請參閱 [WINDOWS AKS 支援常見問題](/azure/aks/windows-faq)。
- 使用 Azure DDoS 保護標準來保護用於 AKS 叢集的虛擬網路。
- 使用連結到整體網路設定的 DNS 設定與 Azure 虛擬 WAN、中樞和輪輻架構、Azure DNS 區域，以及您自己的 DNS 基礎結構。
- 使用 Private Link 來保護網路連線，並使用以私人 IP 為基礎的連線來連接到其他支援 Private Link 的受控 Azure 服務，例如 Azure 儲存體、Azure Container Registry、Azure SQL Database 和 Azure Key Vault。
- 使用輸入控制器來提供先進的 HTTP 路由和安全性，並為應用程式提供單一端點。
- 若要節省 AKS 叢集的計算和儲存資源，請使用叢集內輸入控制器。
  - 使用 [Azure 應用程式閘道輸入控制器 (AGIC) ](/azure/application-gateway/ingress-controller-overview) 附加元件，也就是第一方管理的 Azure 服務。
  - 使用 AGIC 時，請為每個 AKS 叢集部署專用的 Azure 應用程式閘道，而不會在多個 AKS 叢集之間共用相同的應用程式閘道。
  - 如果沒有資源或作業條件約束，或 AGIC 未提供必要的功能，請使用叢集輸入控制器解決方案，例如 NGINX、Traefik 或任何其他 Kubernetes 支援的解決方案。
- 針對網際網路面向和安全性關鍵的內部 web 應用程式，請使用具有輸入控制器的 Web 應用程式防火牆。
  - Azure 應用程式閘道和 Azure Front 大門都會整合 [AZURE WAF](/azure/web-application-firewall/ag/ag-overview) 來保護 web 應用程式。
- 如果您的安全性原則要求檢查 AKS 叢集中產生的所有網際網路輸出流量，請使用 Azure 防火牆或協力廠商網路虛擬裝置來保護輸出網路流量， (NVA) 部署在 (受控) 中樞虛擬網路中。 如需詳細資訊，請參閱 [限制輸出流量](/azure/aks/limit-egress-traffic)。

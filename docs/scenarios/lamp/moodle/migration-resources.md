---
title: Moodle 遷移資源
description: 瞭解 Moodle 遷移在 Azure 中建立的資源。 範例包括 Azure 虛擬網路、網路安全性群組和子網。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: cffcac0ca8b06608971637ed75aac1ba9cb3eff3
ms.sourcegitcommit: 1d7b16eb710bed397280fb8f862912c78f2254ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95812176"
---
# <a name="moodle-migration-resources"></a>Moodle 遷移資源

當您使用 Azure Resource Manager (ARM) 範本來遷移 Moodle 時，部署會在 Azure 中建立資源。 在此部署程式中，其他部署會自動透過子範本執行。 下列各節說明這些部署以及它們所建立的資源。

## <a name="network-template"></a>網路範本

網路範本部署會建立下列資源：

- [Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview)：在雲端中表示您自己的網路。 虛擬網路是專屬於您訂用帳戶的 Azure 雲端邏輯隔離。 當您建立虛擬網路時，您內的服務和虛擬機器可以直接且安全地在雲端中進行通訊。 網路範本建立的虛擬網路包括虛擬網路名稱、API 版本、位置、DNS 伺服器名稱和 AddressSpace。 AddressSpace 包含子網可使用的 IP 位址範圍。

- [網路安全性群組 (NSG) ](/azure/virtual-network/network-security-groups-overview)：網路篩選器或防火牆，其中包含安全性規則的清單。 這些規則可允許或拒絕連線至虛擬網路之資源的網路流量。

- [網路介面](/azure/virtual-network/virtual-network-network-interface)： Azure 虛擬機器可用於與網際網路、Azure 和內部部署資源通訊的介面。

- [子網](/azure/virtual-network/virtual-network-manage-subnet)：大型網路內的較小網路。 子網也稱為子網。 根據預設，子網中的 IP 位址可以與虛擬網路內的任何其他 IP 位址通訊。

- [公用 ip 位址](/azure/virtual-network/public-ip-addresses#:~:text=Public%20IP%20addresses%20enable%20Azure,IP%20assigned%20can%20communicate%20outbound)： Azure 資源用來與網際網路通訊的 ip 位址。 此位址是 Azure 資源專屬的位址。

- [Azure Load Balancer](/azure/virtual-machines/windows/tutorial-load-balancer#:~:text=An%20Azure%20load%20balancer%20is,traffic%20to%20an%20operational%20VM)：負載平衡器，可有效率地將網路或應用程式流量分散到伺服器陣列中的多部伺服器。 Load Balancer 只能將要求傳送到線上的伺服器，以確保高可用性和可靠性。

- [Azure 應用程式閘道](/azure/application-gateway/overview)： Load Balancer 的替代方案。 所有四個預先定義的 ARM 範本都會部署 Load Balancer。 如果您使用完全可設定的部署，而不是 ARM 範本，您可以選擇應用程式閘道，而不是 Load Balancer。 應用程式閘道是網路流量 Load Balancer，可用來管理 web 應用程式的流量。 應用程式閘道可以根據 HTTP 要求的其他屬性（例如 URI 路徑或主機標頭）進行路由決策。

- [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview)：以開放原始碼軟體 Redis 為基礎的記憶體中資料存放區。 Redis 可改善大量儲存後端資料之應用程式的效能和擴充性。 它可以藉由將經常存取的資料保存在伺服器記憶體中，來處理大量的應用程式要求。 這種資料可以快速寫入和讀取。

## <a name="storage-template"></a>儲存體範本

儲存體帳戶範本部署會建立 FileStorage 類型的 Azure 儲存體帳戶。 此帳戶具有 premium 效能、本機冗余儲存體 (LRS) 複寫，以及 1 tb (TB 的儲存體) 。 預先定義的範本會設定成讓具有 Azure 檔案儲存體的儲存體帳戶建立檔案共用。

[Azure 儲存體帳戶](/azure/storage/common/storage-account-overview)包含 Azure 儲存體的資料物件，例如 blob、檔案、佇列、資料表和磁片。 儲存體帳戶會為您的 Azure 儲存體資料提供唯一的命名空間，此命名空間可透過 HTTP 或 HTTPS 從世界各地存取。 以下是可用的 Azure 儲存體帳戶類型：一般用途 v1、一般用途 v2、BlockBlobStorage、FileStorage 和 Blob 儲存體。 複寫類型可以是異地冗余或 LRS 和區域多餘的儲存體。 效能類型為 standard 和 premium，而個別儲存體帳戶最多可儲存 500 TB 的資料，就像任何其他 Azure 服務一樣。

ARM 範本支援下列儲存體帳戶類型：

- [網路檔案系統 (NFS) ](/windows-server/storage/nfs/nfs-overview)：遠端主機可用來透過網路掛接檔案系統的帳戶類型。 遠端主機可以與這些檔案系統互動，就好像它們是在本機掛接一樣。 透過這項設計，系統管理員可以將資源合併到網路中的集中式伺服器。

- [GlusterFS](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)：開放原始碼分散式檔案系統，可在建立區塊的方式中向外延展，以儲存多達數 pb 的資料。

- [Azure 檔案儲存體](/azure/storage/files/storage-files-introduction)：唯一可提供安全、SMB 和完全受控雲端檔案共用的公用雲端檔案儲存體，也可以在內部部署中快取以獲得效能和相容性。 針對 NFS 和 GlusterFS，複寫是標準 LRS，儲存體類型是一般用途 v1。 針對 Azure 檔案儲存體，複寫是 premium LRS，而類型為 FileStorage。

根據您選擇的部署而定，這些儲存機制會有所不同。 NFS 和 GlusterFS 會建立容器，Azure 檔案儲存體建立檔案共用。 針對最小和最短的 Moodle 大小，此範本支援 NFS。 針對大型和最大的大小，範本支援 Azure 檔案儲存體。 若要存取容器和檔案共用，請移至 Azure 入口網站，然後選取資源群組中的儲存體帳戶。

![Azure 入口網站的螢幕擷取畫面。 儲存體帳戶的頁面是可見的，而按鈕可用於存取容器和檔案共用。](./images/storage-account.png)

## <a name="database-template"></a> 資料庫範本

資料庫範本部署會建立 [適用於 MySQL 的 Azure 資料庫](/azure/mysql/) 伺服器。 適用於 MySQL 的 Azure 資料庫可以輕鬆地設定、管理和調整。 它會將基礎結構和資料庫伺服器的管理和維護自動化，包括例行更新、備份和安全性。 適用於 MySQL 的 Azure 資料庫是以最新的 MySQL 版本所建立，包括5.6、5.7 和8.0 版。 若要存取範本所建立的資料庫伺服器，請移至 Azure 入口網站，然後開啟部署程式所提供的資源群組。 然後移至 **適用於 MySQL 的 Azure 資料庫 server**。 此範本會為資料庫伺服器提供伺服器名稱、伺服器管理員登入名稱、MySQL 版本，以及效能設定。

## <a name="virtual-machine-template"></a>虛擬機器範本

虛擬機器範本部署會將虛擬機器指定為控制器虛擬機器。 控制器虛擬機器的作業系統是 Ubuntu 18.04。

虛擬機器擴充功能是小型的應用程式，可在 [Azure 虛擬機器](/azure/virtual-machines/extensions/overview)上提供部署後設定和自動化工作。 虛擬機器擴充功能會執行 shell 腳本，以在控制器虛擬機器上安裝 Moodle 並捕獲記錄檔。 它會 `stderr` `stdout` 在資料夾中建立和記錄檔 `/var/lib/waagent/custom-script/download/0/` 。 您可以使用根使用者的形式來查看這些檔案。

## <a name="scale-set-template"></a>擴展集範本

擴展集範本部署會建立 [虛擬機器擴展集](/azure/virtual-machine-scale-sets/overview)。 您可以使用虛擬機器擴展集來部署和管理一組自動調整虛擬機器。 您可以手動調整擴展集中的虛擬機器數目，或定義規則以根據資源使用量（例如 [CPU](/visualstudio/profiling/average-cpu-utilization)、記憶體需求或網路流量）自動調整。 當實例擴大時，它會部署虛擬機器。 然後執行會安裝 Moodle 必要條件並設定 cron 作業的 shell 腳本。 擴展集中的虛擬機器具有私人 IP 位址。 遵循 [如何建立虛擬網路閘道，並透過私人 ip](./vpn-gateway.md) 連線來連線到擴展集中具有私人 ip 位址的虛擬機器的步驟。

## <a name="next-steps"></a>後續步驟

繼續 [瞭解如何建立虛擬網路閘道，並透過私人 IP 連接](./vpn-gateway.md)。

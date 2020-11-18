---
title: Moodle 遷移資源
description: 瞭解 Moodle 遷移的資源。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: a4b57c31e1b8d3fd489498eb36cf36f259c8e560
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714495"
---
# <a name="moodle-migration-resources"></a>Moodle 遷移資源

使用 Azure Resource Manager 範本時，會在 Azure 中建立下列資源：

- **網路範本：** 網路範本將會建立虛擬網路、網路安全性群組、網路介面、子網、公用 IP 位址、Azure Load Balancer/應用程式閘道、Redis 快取等等。

網路範本也會建立一個虛擬網路，並以字串作為名稱、apiVersion、位置和 DNS 伺服器名稱。 `AddressSpace`包含子網可使用的 IP 位址範圍。

- **虛擬網路：** Azure 虛擬網路代表您在雲端中的網路。 它是專為您的訂用帳戶專屬的 Azure 雲端邏輯隔離。 當您建立虛擬網路時，您內的服務和虛擬機器可以直接且安全地在雲端中進行通訊。 如需詳細資訊，請參閱 [Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview)。

- **網路安全性群組：** 網路安全性群組 (NSG) 是一種網路篩選 (防火牆) 包含一份安全性規則清單，可允許或拒絕連線至 Azure 虛擬網路之資源的網路流量。 探索 [網路安全性群組](/azure/virtual-network/network-security-groups-overview) 以取得詳細資訊。

- **網路介面：** 網路介面可讓 Azure 虛擬機器與網際網路、Azure 和內部部署資源進行通訊。 探索 [網路介面](/azure/virtual-network/virtual-network-network-interface) 以取得詳細資訊。

- **子網：** 子網或子網是大型網路中較小的網路。 根據預設，子網中的 IP 可以與虛擬網路內的任何其他 IP 進行通訊。 探索 [子網](/azure/virtual-network/virtual-network-manage-subnet) 以取得詳細資訊。

- **公用 IP：** 公用 IP 位址可用來將 Azure 資源傳達給網際網路。 此位址是 Azure 資源專屬的位址。 探索 [公用 IP](/azure/virtual-network/public-ip-addresses#:~:text=Public%20IP%20addresses%20enable%20Azure,IP%20assigned%20can%20communicate%20outbound) 以取得詳細資訊。

- **Azure Load Balancer：** 在伺服器陣列中的多部伺服器上有效率地散發網路或應用程式流量。 只將要求傳送到線上的伺服器，以確保高可用性和可靠性。 探索 [Azure Load Balancer](/azure/virtual-machines/windows/tutorial-load-balancer#:~:text=An%20Azure%20load%20balancer%20is,traffic%20to%20an%20operational%20VM) 以取得詳細資訊。

四個預先定義範本中的任一個都會部署 Azure Load Balancer。 在完全可設定的部署中，使用者可以選擇 Azure 應用程式閘道，而不是 Load Balancer。

- **Azure 應用程式閘道：** Web 流量 Azure Load Balancer，可讓您管理 web 應用程式的流量。 應用程式閘道可以根據 HTTP 要求的其他屬性（例如 URI 路徑或主機標頭）進行路由決策。 探索 [Azure 應用程式閘道](/azure/application-gateway/overview) 以取得詳細資訊。

- **Redis cache：** Azure Cache for Redis 會根據開放原始碼軟體 Redis 提供記憶體中的資料存放區。 Redis 可改善應用程式的效能和擴充性，以大量儲存後端資料。 它可以藉由將經常存取的資料保留在伺服器記憶體中，來處理大量的應用程式要求，且此資料可以快速寫入和讀取。 探索 [Redis cache](/azure/azure-cache-for-redis/cache-overview) 以取得詳細資訊。

- **儲存體範本：** 儲存體帳戶範本將會建立具有 FileStorage 種類和 Premium 本地儲存體的儲存體帳戶 (LRS) 複寫，也就是 1 tb (TB) 。 根據預先定義的範本，具有 Azure 檔案儲存體的儲存體帳戶會建立檔案共用。

Azure 儲存體帳戶包含 blob、檔案、佇列、資料表和磁片，這些都是 Azure 儲存體資料物件。 儲存體帳戶會為您的 Azure 儲存體資料提供唯一的命名空間，此命名空間可透過 HTTP 或 HTTPS 從世界各地存取。 Azure 儲存體帳戶的類型是一般用途 V1、一般用途 V2、BlockBlobStorage、FileStorage 和 Blob 儲存體。 複寫類型為 LRS 和區域冗余或地理位置多餘的儲存體。 效能類型為 standard 和 premium，而個別儲存體帳戶最多可儲存 500 TB 的資料，就像任何其他 Azure 服務一樣。

下列儲存體帳戶類型功能 Azure Resource Manager 範本支援：

- NFS：網路檔案系統 (NFS) 可讓遠端主機透過網路裝載檔案系統，並與這些檔案系統互動，就像是在本機掛接一樣。 這可讓系統管理員將資源合併到網路上的集中式伺服器上。 探索 [NFS](/windows-server/storage/nfs/nfs-overview) 以取得詳細資訊。

- GluserFS：開放原始碼分散式檔案系統，可在建立區塊的方式中向外延展，以儲存多達數 pb 的資料。 探索 [GLUSTER FS](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs) 以取得詳細資訊。

- Azure 檔案儲存體：唯一可提供安全、SMB 和完全受控雲端檔案共用的公用雲端檔案儲存體，也可以在內部部署中快取以獲得效能和相容性。 探索 [Azure](/azure/storage/files/storage-files-introduction) 檔案以取得詳細資訊。 針對 NFS 和 glusterFS，複寫是標準 LRS，儲存體類型是一般用途 v1。 針對 Azure 檔案儲存體，複寫會在本機上進行複寫-多餘的儲存體、LRS，且類型為 FileStorage。

這些儲存機制會根據所選的部署而有所不同。 NFS 和 glusterFS 會建立容器，Azure 檔案儲存體將會建立檔案共用。 針對最基本和 short2mid，範本將支援 NFS，而針對大型和最大的範本，範本將支援 Azure 檔案儲存體。 若要存取容器和檔案共用，請流覽至入口網站，然後選取資源群組中的儲存體帳戶。

![儲存體帳戶。](images/storage-account.png)

探索 [儲存體帳戶](/azure/storage/common/storage-account-overview) ，以深入瞭解儲存體帳戶。

- **資料庫範本：** 資料庫範本將會建立 [適用於 MySQL 的 Azure 資料庫的伺服器](/azure/mysql/)。 您可以輕鬆地設定、管理和調整適用於 MySQL 的 Azure 資料庫 server。 它會將基礎結構和資料庫伺服器的管理和維護自動化，包括例行更新、備份和安全性。 使用最新的 MySQL 版本建立，包括5.6、5.7 和8.0 版。 若要存取所建立的資料庫伺服器，請流覽至部署期間提供的 **資源群組** ，然後移至 **適用於 MySQL 的 Azure 資料庫 server。** 資料庫伺服器會有伺服器名稱、伺服器管理員登入名稱、MySQL 版本，以及效能設定。

- **虛擬機器範本：** 此範本會將虛擬機器區別為控制器虛擬機器。 控制器虛擬機器的作業系統是 Ubuntu 18.04。

- **虛擬機器擴充功能：** 虛擬機器擴充功能可以是小型應用程式，可在 [Azure 虛擬機器](/azure/virtual-machines/extensions/overview)上提供部署後設定和自動化工作。 虛擬機器擴充功能將會執行 shell 腳本檔案，此檔案會在控制器虛擬機器上安裝 Moodle 並捕獲記錄檔。 記錄檔 `stderr` 和 `stdout` 是在中建立 `/var/lib/waagent/custom-script/download/0/` ，而使用者可以將它們視為根使用者來加以查看。

- **擴展集範本：** 此範本會建立 [虛擬機器擴展集](/azure/virtual-machine-scale-sets/overview)。 虛擬機器擴展集可讓您部署和管理一組自動調整的虛擬機器。 您可以手動調整擴展集中的虛擬機器數目，或定義規則以根據資源使用量（例如 CPU、記憶體需求或網路流量）自動調整。 自動調整虛擬機器實例取決於 [CPU 使用率](/visualstudio/profiling/average-cpu-utilization)。 在擴充實例時，會部署虛擬機器，並執行 shell 腳本來安裝 Moodle 必要條件和設定 cron 作業。 虛擬機器實例具有私人 IP。 遵循 [如何建立虛擬網路閘道，並透過私人 ip](./vpn-gateway.md) 連線來連線至具有私人 ip 之擴展集上的虛擬機器的步驟。

## <a name="next-steps"></a>後續步驟

繼續瞭解 [如何建立虛擬網路閘道，並透過私人 IP](./vpn-gateway.md) 連線，以取得 Moodle 遷移程式的詳細資訊。

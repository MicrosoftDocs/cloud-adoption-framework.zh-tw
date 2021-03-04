---
title: 大規模的私人連結和 DNS 整合
description: 大規模的私人連結和 DNS 整合
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 2d7afeca3547f5880e743b82c221f1ab58fdab11
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794557"
---
# <a name="private-link-and-dns-integration-at-scale"></a>大規模的私人連結和 DNS 整合

> 本文說明如何整合適用于 PaaS 服務的 Azure Private Link 與中樞和輪輻網路架構中的 Azure 私人 DNS 區域。

## <a name="introduction"></a>簡介

許多客戶會使用中樞和輪輻網路架構，在 Azure 中建立其網路基礎結構，其中：

- 網路共用服務 (例如網路虛擬裝置、ExpressRoute/VPN 閘道或 DNS 伺服器) 會部署在 **中樞** 虛擬網路 (VNet) 
- **輪輻** Vnet 使用 VNet 對等互連來取用這些共用服務。

在中樞和輪輻網路架構中，應用程式擁有者通常會隨附于 Azure 訂用帳戶，其中包含 VNet (連接至 *中樞* VNet 的 *輪輻*) 。 在此架構中，他們可以部署其虛擬機器，並透過 ExpressRoute 或 VPN 與其他 Vnet 或內部部署網路具有私人連線能力。

網際網路輸出連線是透過中央網路虛擬裝置提供 (NVA) 例如 Azure 防火牆。

許多應用程式小組會使用 Azure IaaS 和 PaaS 資源的組合來建立解決方案。 某些 Azure PaaS 服務 (例如 SQL 受控實例) 可部署在客戶 Vnet 中。 因此，流量會在 Azure 網路內保持私密，並且可從內部部署完全路由傳送。

不過，某些 Azure PaaS 服務 (例如 Azure 儲存體或 Azure Cosmos DB) 無法部署在客戶的 Vnet 中，並可透過其公用端點存取。 在某些情況下，這可能會導致與客戶安全性原則的競爭，因為公司流量可能不允許部署或存取公司資源 (例如，透過公用端點的 SQL database) 。

[Azure Private Link][link-1] 可讓您透過私人端點存取 [Azure 服務清單][link-2] ，但需要在對應的 [私人 DNS 區域][link-3]中註冊這些私人端點記錄。

本文說明應用程式小組如何在其訂用帳戶中部署 Azure PaaS 服務，而這些服務只能透過私人端點來存取。

本文也會說明應用程式小組如何透過 Azure 私人 DNS，來確保服務會自動與私人 DNS 區域整合。 移除在 DNS 中手動建立或刪除記錄的需求。

## <a name="private-link-and-dns-integration-in-hub-and-spoke-network-architectures"></a>中樞和輪輻網路架構中的私人連結和 DNS 整合

私人 DNS 區域通常會集中託管在部署中樞 VNet 的相同 Azure 訂用帳戶中。 這項中央主機實務是由 [跨單位 dns 名稱解析][link-4] 和其他對中央 DNS 解析（例如 Active Directory）的需求所驅動。 在大多數情況下，只有網路/身分識別管理員有權管理這些區域中的 DNS 記錄。

應用程式小組具有在自己的訂用帳戶中建立 Azure 資源的許可權。 它們在中央網路連線訂用帳戶中沒有任何許可權，包括管理私人 DNS 區域中的 DNS 記錄。 這項存取限制表示他們無法建立部署具有私人端點的 Azure PaaS 服務時 [所需的 DNS 記錄][link-4] 。

下圖顯示使用中央 DNS 解析之企業環境的典型高階架構，以及私人連結資源的名稱解析是透過 Azure 私人 DNS 進行的：

![影像-1][image-1]

從上圖中，請務必強調：

- 內部部署 DNS 伺服器針對每個私人端點 [公用 DNS 區域][link-3] 轉寄站設定條件轉寄站，指向 DNS 轉寄站 (`10.100.2.4` ，並 `10.100.2.5`) 裝載于中樞 VNet 中。
- 所有 Azure Vnet 都有 DNS 轉寄站 (`10.100.2.4` ，並 `10.100.2.5`) 設定為主要和次要 DNS 伺服器。

有兩個條件必須為 true，才能讓應用程式小組自由地在其訂用帳戶中建立任何必要的 Azure PaaS 資源：

- 中央網路和/或中央平臺小組必須確保應用程式小組只能透過私人端點部署和存取 Azure PaaS 服務。
- 中央網路及/或中央平臺小組必須確保每次建立私人端點時，會自動在集中式私人 DNS 區域中建立對應的記錄，以符合所建立的服務。
  - DNS 記錄必須遵循私人端點的生命週期，並在刪除私人端點時自動移除 DNS 記錄。

下列各節將說明應用程式小組如何使用 [Azure 原則][link-10]來啟用這些條件。 我們會使用 Azure 儲存體作為應用程式小組在下列範例中所需部署的 Azure 服務，但相同的原則可以套用至大部分 [支援][link-2] Private Link 的 azure 服務。

## <a name="configuration-required-by-platform-team"></a>平臺小組需要的設定

### <a name="create-private-dns-zones"></a>建立私人 DNS 區域

依據 [檔][link-4]，在中央連線訂用帳戶中為支援的 private Link 服務建立私人 DNS 區域。

在我們的案例中，因為我們會在範例中使用 **儲存體帳戶與 blob** ，所以會轉譯為我們在連線訂用帳戶 `privatelink.blob.core.windows.net` 中建立私人 DNS 區域。

![影像-2][image-2]

### <a name="policy-definitions"></a>原則定義

除了私人 DNS 區域，我們也需要 [建立一組自訂的 Azure 原則定義][link-5] ，以強制使用私用端點，並在我們所建立的 dns 區域中自動建立 dns 記錄：

1. 適用于 PaaS 服務原則的 **拒絕** 公用端點

   此原則可防止使用者建立具有公用端點的 Azure PaaS 服務，並在建立資源時未選取私人端點時，提供錯誤訊息給他們。

   ![影像-3][image-3]

   ![影像-4][image-4]

   ![影像-5][image-5]

   PaaS 服務之間的確切原則規則可能會有所不同。 針對 Azure 儲存體帳戶，我們會查看 **networkAcls. defaultAction** 屬性，此屬性會定義是否允許來自公用網路的要求。 在我們的案例中，如果屬性 **networkAcls. defaultAction** 不是，我們會設定條件來拒絕建立 **Microsoft. Storage/storageAccounts** 資源類型 `Deny` 。 此原則定義如下所示：

   ```json
   {
     "mode": "All",
     "policyRule": {
       "if": {
         "allOf": [
           {
             "field": "type",
             "equals": "Microsoft.Storage/storageAccounts"
           },
           {
             "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
             "notequals": "Deny"
           }
         ]
       },
       "then": {
         "effect": "Deny"
       }
     }
   }
   ```

2. **拒絕** 使用首碼原則建立私人 DNS 區域 `privatelink`

   由於我們使用集中式 DNS 架構搭配由平臺小組管理的訂用帳戶所裝載的條件轉寄站和私人 DNS 區域，因此我們必須防止應用程式小組擁有者建立自己的私人連結私人 DNS 區域，並將服務連結至其訂用帳戶。

   若要達成此目的，我們必須確定當應用程式小組建立私人端點時， `Integrate with private DNS zone` `No` 使用 Azure 入口網站時必須將選項設為。

   ![影像-6][image-6]

   如果 `Yes` 選取了，Azure 原則將會防止建立私人端點。 在我們的原則定義中，如果區域有前置詞，則會拒絕建立 **Microsoft. Network/privateDnsZones** 資源類型 `privatelink` 。 此原則定義如下所述：

   ```json
   {
     "description": "This policy restricts creation of private DNS zones with the `privatelink` prefix",
     "displayName": "Deny-PrivateDNSZone-PrivateLink",
     "mode": "All",
     "parameters": null,
     "policyRule": {
       "if": {
         "allOf": [
           {
             "field": "type",
             "equals": "Microsoft.Network/privateDnsZones"
           },
           {
             "field": "name",
             "like": "privatelink*"
           }
         ]
       },
       "then": {
         "effect": "Deny"
       }
     }
   }
   ```

3. **DeployIfNotExists** 原則，以在中央私人 DNS 區域中自動建立所需的 DNS 記錄

   如果使用特定服務來建立私人端點資源，則會觸發此原則 `groupId` 。 `groupId`是從遠端資源取得的群組識別碼， (服務) 此私人端點應連接到此群組。 接著 [`privateDNSZoneGroup`][link-6] ，我們會在私人端點內觸發的部署，以用來將私人端點與我們的私人 DNS 區域產生關聯。 在我們的範例中，您 `groupId` `blob` `groupId` 可以在本文的 **Subresource** 資料行) 下找到其他 azure [][link-4]服務的 azure 儲存體 blob (。 當原則發現 `groupId` 在建立的私人端點中，它會在 [`privateDNSZoneGroup`][link-6] 私人端點內部署，而它會連結至指定為參數的私人 DNS 區域資源識別碼。 在我們的範例中，私人 DNS 區域資源識別碼會是：

   `/subscriptions/<subscription-id>/resourceGroups/<resourceGroupName>/providers/Microsoft.Network/privateDnsZones/privatelink.blob.core.windows.net`

   此原則定義如下所示：

   ```json
   {
     "mode": "Indexed",
     "policyRule": {
       "if": {
         "allOf": [
           {
             "field": "type",
             "equals": "Microsoft.Network/privateEndpoints"
           },
           {
             "count": {
               "field": "Microsoft.Network/privateEndpoints/privateLinkServiceConnections[*].groupIds[*]",
               "where": {
                 "field": "Microsoft.Network/privateEndpoints/privateLinkServiceConnections[*].groupIds[*]",
                 "equals": "blob"
               }
             },
             "greaterOrEquals": 1
           }
         ]
       },
       "then": {
         "effect": "deployIfNotExists",
         "details": {
           "type": "Microsoft.Network/privateEndpoints/privateDnsZoneGroups",
           "roleDefinitionIds": [
             "/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
           ],
           "existenceCondition": {
             "allOf": [
               {
                 "field": "Microsoft.Network/privateEndpoints/privateDnsZoneGroups/privateDnsZoneConfigs[*].privateDnsZoneId",
                 "equals": "[parameters('privateDnsZoneId')]"
               }
             ]
           },
           "deployment": {
             "properties": {
               "mode": "incremental",
               "template": {
                 "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                 "contentVersion": "1.0.0.0",
                 "parameters": {
                   "privateDnsZoneId": {
                     "type": "string"
                   },
                   "privateEndpointName": {
                     "type": "string"
                   },
                   "location": {
                     "type": "string"
                   }
                 },
                 "resources": [
                   {
                     "name": "[concat(parameters('privateEndpointName'), '/deployedByPolicy')]",
                     "type": "Microsoft.Network/privateEndpoints/privateDnsZoneGroups",
                     "apiVersion": "2020-03-01",
                     "location": "[parameters('location')]",
                     "properties": {
                       "privateDnsZoneConfigs": [
                         {
                           "name": "storageBlob-privateDnsZone",
                           "properties": {
                             "privateDnsZoneId": "[parameters('privateDnsZoneId')]"
                           }
                         }
                       ]
                     }
                   }
                 ]
               },
               "parameters": {
                 "privateDnsZoneId": {
                   "value": "[parameters('privateDnsZoneId')]"
                 },
                 "privateEndpointName": {
                   "value": "[field('name')]"
                 },
                 "location": {
                   "value": "[field('location')]"
                 }
               }
             }
           }
         }
       }
     },
     "parameters": {
       "privateDnsZoneId": {
         "type": "String",
         "metadata": {
           "displayName": "privateDnsZoneId",
           "strongType": "Microsoft.Network/privateDnsZones"
         }
       }
     }
   }
   ```

### <a name="policy-assignments"></a>原則指派

部署原則定義之後，請在管理群組階層中的所需範圍 [指派原則][link-7] 。 請確定原則指派是以應用程式小組將使用的 Azure 訂用帳戶為目標，以獨佔方式部署具有私人端點存取權的 PaaS 服務。

> [!IMPORTANT]
> 請記得將私人 dns 區域裝載于訂用帳戶/資源群組中的 [私人 Dns 區域參與者角色][link-8] 角色，此角色 [ `DeployIfNotExists` 指派][link-9] 會負責建立及管理私人 dns 區域中的私人端點 DNS 記錄。 這是因為私人端點位於應用程式擁有者的 Azure 訂用帳戶中，而私人 DNS 區域位於不同的訂用帳戶中 (例如中央連線訂用帳戶) 。

當平臺團隊完成這項設定之後，來自應用程式小組的 Azure 訂用帳戶已準備好建立具有專用私人端點存取權的 Azure PaaS 服務，並確保私人端點的 DNS 記錄會自動註冊 (，並在) 從對應的私人 DNS 區域中刪除私人端點時移除。

## <a name="application-owner-experience"></a>應用程式擁有者體驗

一旦平臺小組已將平臺基礎結構元件部署 (私人 DNS 區域和原則，) 上一節中所述。當應用程式擁有者嘗試將 Azure PaaS 服務部署至 Azure 訂用帳戶時，如果他們透過 Azure 入口網站或其他用戶端（例如 PowerShell 或 CLI）進行活動，因為它們的訂用帳戶受 Azure 原則控管，應用程式擁有者將會有下列體驗。

1. 透過 Azure 入口網站建立儲存體帳戶。 在 [基本] 索引標籤中，提供名稱和您想要的設定，然後按 **[下一步]**。

   ![影像-7][image-7]

2. 在 [網路功能] 區段中，確定已選取 [ **私人端點** ]。 如果選取了 **私人端點** 以外的選項，Azure 入口網站將不允許在部署嚮導的 [審核] + [ **建立** ] 區段中建立儲存體帳戶，因為原則會防止在公用端點啟用時建立此服務。

   ![影像-8][image-8]

3. 您可以在此畫面上建立私人端點，也可以在建立儲存體帳戶之後完成。 在此練習中，我們將在建立儲存體帳戶之後，建立私人端點。 按一下 [ **審核] + [建立** ]，並完成儲存體帳戶的建立。

4. 建立儲存體帳戶之後，請透過 Azure 入口網站建立私人端點

   ![影像-9][image-9]

5. 在 [ **資源** ] 區段中，找出在上一個步驟中建立的儲存體帳戶，並針對 [目標 subresource] 選取 [ **Blob**]，然後選取 **[下一步]**。

   ![影像-10][image-10]

6. **在 [設定**] 區段中，選取您的 VNet 和子網之後，請確定 [**與私人 DNS 區域整合**] 設定為 [**否**]。 否則，Azure 入口網站將會防止建立私人端點，因為 Azure 原則將不允許建立具有前置詞的私人 DNS 區域 `privatelink` 。

   ![影像-11][image-11]

7. 選取 [ **審核 + 建立**]，然後選取 [ **建立** ] 以部署私人端點。

8. 幾分鐘之後，將會 `DeployIfNotExists` 觸發原則，而後續 `dnsZoneGroup` 部署將會在集中管理的 DNS 區域中，新增私人端點所需的 DNS 記錄。

9. 一旦建立私人端點，請選取它，然後檢查其 FQDN 和私人 IP：

   ![影像-12][image-12]

10. 檢查已建立私人端點之資源群組的活動記錄，或您可以檢查私人端點本身的活動記錄。 您會注意到，在幾分鐘之後，就會 `DeployIfNotExist` 執行原則動作，以在私人端點上設定 DNS 區域群組：

    ![影像-13][image-13]

11. 如果中央網路團隊移至 `privatelink.blob.core.windows.net` 私人 DNS 區域，他們將確認已為我們建立的私人端點建立 DNS 記錄，並將名稱和 IP 位址與私人端點內的值相符。

    ![影像-14][image-14]

此時，應用程式小組可以透過來自中樞和輪輻網路環境中任何 VNet 的私人端點，以及從內部部署使用儲存體帳戶，因為 DNS 記錄已自動記錄在私人 DNS 區域中。

如果應用程式擁有者刪除私人端點，則會自動移除私人 DNS 區域中的對應記錄。

[link-1]: /azure/private-link/private-link-overview
[link-2]: /azure/private-link/private-link-overview#availability
[link-3]: /azure/private-link/private-endpoint-dns#azure-services-dns-zone-configuration
[link-4]: /azure/private-link/private-endpoint-dns
[link-5]: /azure/governance/policy/tutorials/create-custom-policy-definition
[link-6]: /azure/templates/microsoft.network/2020-05-01/privateendpoints/privatednszonegroups
[link-7]: /azure/governance/policy/assign-policy-portal
[link-8]: /azure/dns/dns-protect-private-zones-recordsets
[link-9]: /azure/governance/policy/how-to/remediate-resources
[link-10]: /azure/governance/policy/overview
[image-1]: ./media/private-link-example-central-dns.png
[image-2]: ./media/private-link-storage-with-blob.jpg
[image-3]: ./media/private-link-storage-policy-step-1.jpg
[image-4]: ./media/private-link-storage-policy-step-2.jpg
[image-5]: ./media/private-link-storage-policy-step-3.jpg
[image-6]: ./media/private-link-private-dns-set-no.jpg
[image-7]: ./media/private-link-create-storage.jpg
[image-8]: ./media/private-link-network-private.jpg
[image-9]: ./media/private-link-create-private.jpg
[image-10]: ./media/private-link-target-blob.jpg
[image-11]: ./media/private-link-integrate-with-private.jpg
[image-12]: ./media/private-link-fqdn-private-ip.jpg
[image-13]: ./media/private-link-activity-log.jpg
[image-14]: ./media/private-link-check-private-dns.jpg

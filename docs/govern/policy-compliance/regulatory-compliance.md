---
title: 法規合規性簡介
description: 瞭解可能會影響雲端治理之各種產業和地理位置的合規性法規。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: ca690c8137a8059febbd8f444f7397ad061c650a
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97019650"
---
# <a name="introduction-to-regulatory-compliance"></a>法規合規性簡介

這是關於法規合規性的簡介文章，因此不適用於實作合規性策略。 如需 [Azure 合規性供應](/microsoft-365/compliance/offering-home) 專案的詳細資訊，請參閱 [Microsoft 信任中心](https://www.microsoft.com/trust-center)。 此外，所有可下載的檔都可供 [Microsoft 服務信任入口網站](https://servicetrust.microsoft.com)中的某些 Azure 客戶使用。

法規合規性是指確保公司遵循其地理位置中治理主體或主動採用產業標準所需規則的規範和流程。 針對 IT 法規合規性，人員和程式會監視公司系統，以偵測並防止違反這些治理法規和標準所建立的原則和程式。 這接著會套用至各式各樣的監視和強制執行程式。 根據產業和地理位置，這些程式可能會變得冗長且複雜。

對跨國組織來說，合規性相當困難，特別是在醫療保健和金融服務等大量管制的產業中。 標準和法規為數眾多，而且在某些情況下可能會經常變更，因此企業難以掌握不斷變化的國際電子資料處理法律。

在安全性控制方面，組織應該了解有關雲端法規合規性的職責劃分。 雲端提供者致力於確保其平台和服務符合規範。 組織也需要確認其應用程式、應用程式所依賴的基礎結構，以及協力廠商提供的服務是否也符合規範。

以下是各種產業和地理區域的合規性法規說說明：

<!-- docutune:casing PHI "Health Information Portability and Accountability Act" -->

## <a name="hipaa"></a>HIPAA

處理受保護健康資訊 (PHI) 的醫療保健應用程式，受限於健康資訊可攜性和責任法案 (HIPAA) 所包含的隱私權規則和安全性規則。 HIPAA 最少可能會要求醫療保健企業必須從雲端提供者收到書面保證，以保護所收到或建立的任何 PHI。

<!-- docutune:ignore Discover -->
<!-- cSpell:ignore Visa Mastercard -->

## <a name="pci"></a>PCI

支付卡產業資料安全性標準 (PCI DSS) 是處理來自主要信用卡付款系統（包括簽證、Mastercard、美洲 Express、探索及 JCB）之品牌信用卡的公司專屬資訊安全標準。 PCI 標準是由卡片品牌委任並由支付卡產業安全標準協會管理。 建立這項標準的目的在於提高對於持卡人資料的控制權，以減少信用卡詐騙情形。 相容性驗證是每年執行，可能是由外部合格的安全性評估者 (QSA) 或由公司特定的內部安全性評估者 (ISA) 負責為處理大量交易的組織建立合規性 (ROC) ，或由自我評量問卷 (公司的自我評量問卷) SAQ。

## <a name="personal-data"></a>個人資料

個人資料是可用來識別取用者、員工、合作夥伴或任何其他生活或法律實體的資訊。 許多新興的法律（特別是處理隱私權和個人資料的法律）都需要企業本身遵守並報告合規性，以及可能發生的任何缺口。

## <a name="gdpr"></a>GDPR

此領域最重要的發展之一是一般資料保護規定 (GDPR) ，其設計目的是為了強化歐盟內個人的資料保護。 GDPR 需要個人相關資料 (例如「名稱、首頁位址、相片、電子郵件地址、銀行詳細資料、社交網路網站上的貼文、醫療資訊或電腦的 IP 位址」，) 在 EU 內的伺服器上維護，而不會將其傳送出去。 此外，公司也需要公司通知個人任何資料缺口，並規定公司有資料保護長 (DPO) 。 其他國家/地區具有 (或正在開發) 相似類型的法規。

## <a name="compliant-foundation-in-azure"></a>Azure 中的符合規範基礎

為了協助客戶符合其在全球受管制產業和市場的合規性義務，Azure 會維持產業中最大的合規性組合（廣泛 (供應專案總數）) 以及評量範圍) 之客戶面向服務的深度 (數目。 Azure 合規性供應專案分為四個區段：

- 全球
- 美國政府
- 產業
- 地區

Azure 合規性供應項目會以各種類型的保證為基礎，包括由獨立第三方稽核公司所產生的正式認證、證明、驗證、授權及評量，以及由 Microsoft 產生的契約修訂、自我評量及客戶指引文件。 本文件中的每個供應項目描述都會提供最新的範圍聲明，指出評量範圍包含哪些 Azure 客戶面向服務，並提供可下載的資源連結，以協助客戶履行自己的合規性義務。

Microsoft 信任中心提供更多有關 [Azure 合規性供應](https://www.microsoft.com/trust-center/compliance/compliance-overview)專案的詳細資訊。 此外，在下列各節中， [Microsoft 服務信任入口網站](https://servicetrust.microsoft.com) 中的某些 Azure 客戶可以取得所有可下載的檔：

- **Audit reports：** 包含適用于 FedRAMP、GRC 評量、ISO、PCI DSS 和 SOC 報表的區段。
- **資料保護資源：** 包含合規性指南、常見問題和技術白皮書，以及畫筆測試和安全性評估章節。

## <a name="next-steps"></a>後續步驟

深入瞭解雲端安全性的就緒程度。

> [!div class="nextstepaction"]
> [雲端安全性整備程度](./cloud-security-readiness.md)

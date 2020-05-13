---
title: 法規合規性簡介
description: 深入瞭解可能會影響雲端治理的各種產業和地理位置中的合規性法規。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 0860dfc137b8aaa9ad39beeebb3856786eee1318
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83218300"
---
# <a name="introduction-to-regulatory-compliance"></a>法規合規性簡介

這是關於法規合規性的簡介文章，因此不適用於實作合規性策略。 如需[Azure 合規性供應](https://aka.ms/allcompliance)專案的更多詳細資訊，請至[Microsoft 信任中心](https://www.microsoft.com/trust-center)。 此外，所有可下載的檔都可供[Microsoft 服務信任入口網站](https://servicetrust.microsoft.com)中的特定 Azure 客戶使用。

法規合規性是指確保公司遵循其地理位置中的規定，或主動採用的產業標準所需之規則的專業和程式。 針對 IT 法規合規性，人員和程式會監視公司系統，以偵測並防止違反這些治理法規和標準所建立的原則和程式。 這也適用于廣泛的監視和強制執行程式。 根據產業和地理位置而定，這些程式可能會變得冗長且複雜。

對於跨國組織而言，相容性是一項挑戰，特別是在醫療保健和金融服務等嚴格管制的產業中。 標準和法規為數眾多，而在某些情況下，可能會經常變更，讓企業難以掌握不斷改變的國際電子資料處理法規。

在安全性控制方面，組織應該了解有關雲端法規合規性的職責劃分。 雲端提供者致力於確保其平台和服務符合規範。 但組織也需要確認其應用程式、應用程式所依賴的基礎結構，以及協力廠商所提供的服務，也會通過合規性認證。

以下是各種產業和地理區域的合規性法規說說明：

<!-- docsTest:ignore PHI "Health Information Portability and Accountability Act" -->

## <a name="hipaa"></a>HIPAA

負責處理受保護健康資訊（PHI）的醫療保健應用程式，受限於健康狀態資訊可攜性和責任法案（HIPAA）內所包含的隱私權規則和安全性規則。 HIPAA 最少可能會要求醫療保健企業必須從雲端提供者收到書面保證，以保護任何已接收或建立的 PHI。

<!-- cSpell:ignore Visa Mastercard -->
<!-- docsTest:ignore "American Express" Discover JCB QSA ISA ROC SAQ DPO GRC -->

## <a name="pci"></a>PCI

支付卡產業資料安全標準（PCI DSS）是公司專屬的資訊安全標準，適用于處理來自主要卡片配置的品牌信用卡，包括簽證、Mastercard、美國 Express、探索和 JCB。 PCI 標準是由卡片品牌委任並由支付卡產業安全標準協會管理。 建立這項標準的目的在於提高對於持卡人資料的控制權，以減少信用卡詐騙情形。 合規性驗證的執行每年是由外部合格安全性評估者（QSA）或特定公司內部安全性評估者（ISA）所建立，負責處理大量交易的組織，或由公司的自我評估問卷（SAQ）。

## <a name="personal-data"></a>個人資料

個人資料是可用來識別取用者、員工、合作夥伴或任何其他生活或法律實體的資訊。 許多新興的法則，特別是處理隱私權和個人資料的法律，企業本身必須符合並回報合規性和可能發生的任何缺口。

## <a name="gdpr"></a>GDPR

在此領域中最重要的其中一項發展是一般資料保護規定（GDPR），其設計目的是為了加強歐盟內個人的資料保護。 GDPR 需要將有關個人的資料（例如「姓名、家庭位址、相片、電子郵件地址、銀行細節、社交網路網站上的貼文、醫療資訊或電腦的 IP 位址）保留在 eu 的伺服器上，而不是從 it 傳出。 這也需要公司通知個人任何資料缺口，並規定公司擁有資料保護長（DPO）。 其他國家/地區具有 (或正在開發) 相似類型的法規。

## <a name="compliant-foundation-in-azure"></a>Azure 中的符合規範基礎

為了協助客戶符合其在全球受管制產業和市場的合規性義務，Azure 保有業界最大的合規性組合、廣度（供應專案總數）和深度（評量範圍內的客戶面向服務數目）。 Azure 合規性供應專案分為四個區段：

- 全域
- 美國政府
- 業界
- 地區

Azure 合規性供應項目會以各種類型的保證為基礎，包括由獨立第三方稽核公司所產生的正式認證、證明、驗證、授權及評量，以及由 Microsoft 產生的契約修訂、自我評量及客戶指引文件。 本文件中的每個供應項目描述都會提供最新的範圍聲明，指出評量範圍包含哪些 Azure 客戶面向服務，並提供可下載的資源連結，以協助客戶履行自己的合規性義務。

Microsoft 信任中心提供更多關於[Azure 合規性供應](https://www.microsoft.com/trust-center/compliance/compliance-overview)專案的詳細資訊。 此外，在下列各節中，所有可下載的檔可供[Microsoft 服務信任入口網站](https://servicetrust.microsoft.com)中的特定 Azure 客戶使用：

- **審查報告：** 包含 FedRAMP、GRC 評估、ISO、PCI DSS 和 SOC 報告的章節。
- **資料保護資源：** 包含合規性指南、常見問題和白皮書，以及畫筆測試和安全性評估章節。

## <a name="next-steps"></a>後續步驟

深入瞭解雲端安全性的就緒程度。

> [!div class="nextstepaction"]
> [雲端安全性整備程度](./cloud-security-readiness.md)

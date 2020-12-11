---
title: 重大變更的追蹤和警示
description: 使用 Azure 變更追蹤和清查，在混合式環境中啟用重大變更的追蹤和警示。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: eb784eb557c6296e454e1f79bc8151152645dd0c
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97017151"
---
<!-- cSpell:ignore HKEY kusto -->

# <a name="enable-tracking-and-alerting-for-critical-changes"></a>啟用重大變更的追蹤和警示

Azure 變更追蹤和清查提供混合式環境的設定狀態警示，以及對該環境所做的變更。 它可以報告重要的檔案、服務、軟體和登錄變更，這些變更可能會影響您部署的伺服器。

Azure 自動化清查服務預設不會監視檔案或登錄設定。 解決方案會提供建議用來監視的登錄機碼清單。 若要查看此清單，請移至 Azure 入口網站中的 Azure 自動化帳戶，然後選取 [**清查**  >  **編輯設定**]。

![Azure 入口網站中 Azure 自動化清查視圖的螢幕擷取畫面](./media/change-tracking1.png)

如需每個登錄機碼的詳細資訊，請參閱登錄機 [碼變更追蹤](/azure/automation/automation-change-tracking#registry-key-change-tracking)。 選取任何要評估的金鑰，然後加以啟用。 此設定會套用至目前工作區中啟用的所有 Vm。

您也可以使用服務來追蹤重要的檔案變更。 例如，您可能想要追蹤檔案， `C:\windows\system32\drivers\etc\hosts` 因為 OS 會使用它來將主機名稱對應至 IP 位址。 對此檔案所做的變更可能會導致連線問題，或將流量重新導向至危險的網站。

若要啟用主機檔案的檔案內容追蹤，請依照 [啟用檔案內容追蹤](/azure/automation/change-tracking-file-contents#enable-file-content-tracking)中的步驟執行。

您也可以針對正在追蹤的檔案新增變更的警示。 例如，您可能想要設定主機檔案變更的警示。 若要這樣做，請在命令列上選取 **Log analytics** ，或針對連結的 log Analytics 工作區選取 **記錄搜尋** 。 在 Log Analytics 中，使用下列查詢來搜尋主機檔案的變更：

  ```kusto
  ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"
  ```

![Azure 入口網站中 Log Analytics 查詢編輯器的螢幕擷取畫面](./media/change-tracking2.png)

此查詢會搜尋檔案內容的變更，這些檔案的路徑包含「主機」一詞。 您也可以藉由變更 path 參數來搜尋特定的檔案。 (例如：`FileSystemPath ==  "c:\\windows\\system32\\drivers\\etc\\hosts"`)。

查詢傳回結果之後，請選取 [ **新增警示規則** ] 以開啟 [警示規則編輯器]。 您也可以透過 Azure 入口網站中的 Azure 監視器來取得此編輯器。

在 [警示規則] 編輯器中，檢查查詢並視需要變更警示邏輯。 在此情況下，我們想要在環境中的任何電腦上偵測到任何變更時引發警示。

![Azure 入口網站中 Log Analytics 警示規則編輯器的螢幕擷取畫面](./media/change-tracking3.png)

設定條件邏輯之後，您可以指派動作群組來執行動作，以回應警示。 在此範例中，當警示引發時，會傳送電子郵件並建立 ITSM 票證。 您可以採取許多其他有用的動作，例如觸發 Azure 函式、Azure 自動化 runbook、webhook 或邏輯應用程式。

![Azure 入口網站中的範例警示規則摘要的螢幕擷取畫面](./media/change-tracking4.png)

設定所有參數和邏輯之後，請將警示套用至環境。

## <a name="tracking-and-alerting-examples"></a>追蹤和警示範例

本節說明您可能想要使用之追蹤和警示的其他常見案例。

### <a name="driver-file-changed"></a>驅動程式檔已變更

使用下列查詢來偵測是否變更、新增或移除驅動程式檔案。 它很適合用來追蹤重要系統檔案的變更。

  ```kusto
  ConfigurationChange | where ConfigChangeType == "Files" and FileSystemPath contains " c:\\windows\\system32\\drivers\\"
  ```

### <a name="specific-service-stopped"></a>特定服務已停止

使用下列查詢來追蹤系統關鍵服務的變更。

  ```kusto
  ConfigurationChange | where SvcState == "Stopped" and SvcName contains "w3svc"
  ```

### <a name="new-software-installed"></a>已安裝新軟體

針對需要鎖定軟體設定的環境，請使用下列查詢。

  ```kusto
  ConfigurationChange | where ConfigChangeType == "Software" and ChangeCategory == "Added"
  ```

### <a name="specific-software-version-is-or-isnt-installed-on-a-machine"></a>特定軟體版本是或未安裝在電腦上

使用下列查詢來評估安全性。 此查詢會參考 `ConfigurationData` 包含清查記錄的查詢，並提供最後報告的設定狀態，而不是變更。

  ```kusto
  ConfigurationData | where SoftwareName contains "Monitoring Agent" and CurrentVersion != "8.0.11081.0"
  ```

### <a name="known-dll-changed-through-the-registry"></a>透過登錄變更的已知 DLL

使用下列查詢來偵測已知登錄機碼的變更。

  ```kusto
  ConfigurationChange | where RegistryKey == "HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Control\\Session Manager\\KnownDlls"
  ```

## <a name="next-steps"></a>後續步驟

瞭解 Azure 自動化如何 [建立更新](./update-schedules.md) 排程，以管理伺服器的更新。

> [!div class="nextstepaction"]
> [建立更新排程](./update-schedules.md)

---
title: 重大變更的追蹤和警示
description: 使用 Azure 變更追蹤和清查，針對混合式環境中的重大變更啟用追蹤和警示。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: c973dfbdf7cb4fede3520465b2192b7f821cec1d
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80434136"
---
<!-- cSpell:ignore HKEY kusto -->

# <a name="enable-tracking-and-alerting-for-critical-changes"></a>啟用重大變更的追蹤和警示

Azure 變更追蹤和清查提供混合式環境設定狀態的警示，以及該環境的變更。 它可以報告可能影響已部署伺服器的重要檔案、服務、軟體和登錄變更。

Azure 自動化清查服務預設不會監視檔案或登錄設定。 解決方案會提供建議的登錄機碼清單，以供監視。 若要查看這份清單，請移至 Azure 入口網站中的自動化帳戶，然後選取 **清查** > **編輯設定**。

![Azure 入口網站中 Azure 自動化清查視圖的螢幕擷取畫面](./media/change-tracking1.png)

如需每個登錄機碼的詳細資訊，請參閱登錄機[碼變更追蹤](https://docs.microsoft.com/azure/automation/automation-change-tracking#registry-key-change-tracking)。 選取任何金鑰進行評估，然後加以啟用。 此設定會套用至目前工作區中啟用的所有 Vm。

您也可以使用服務來追蹤重要的檔案變更。 例如，您可能會想要追蹤 C:\windows\system32\drivers\etc\hosts 檔案，因為 OS 會使用它來將主機名稱對應到 IP 位址。 對此檔案所做的變更可能會造成連線問題，或將流量重新導向至危險的網站。

若要啟用主機檔案的檔案內容追蹤，請遵循[啟用檔案內容追蹤](https://docs.microsoft.com/azure/automation/change-tracking-file-contents#enable-file-content-tracking)中的步驟。

您也可以針對要追蹤之檔案的變更新增警示。 例如，假設您想要設定主機檔案變更的警示。 選取命令列上的**Log analytics**或已連結之 log Analytics 工作區的記錄搜尋。 在 Log Analytics 中，使用下列查詢來搜尋主機檔案的變更：

```kusto
ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"
```

![Azure 入口網站中 Log Analytics 查詢編輯器的螢幕擷取畫面](./media/change-tracking2.png)

此查詢會搜尋路徑包含「主機」一詞之檔案內容的變更。 您也可以藉由變更 path 參數來搜尋特定檔案。 (例如，`FileSystemPath ==  "c:\\windows\\system32\\drivers\\etc\\hosts"`)。
  
查詢傳回結果之後，請選取 [**新增警示規則**] 以開啟 [警示-規則編輯器]。 您也可以透過 Azure 入口網站中的 Azure 監視器，進入此編輯器。

在警示規則編輯器中，檢查查詢，並視需要變更警示邏輯。 在此情況下，我們想要在環境中的任何電腦上偵測到任何變更時，產生警示。

![Azure 入口網站中 Log Analytics 警示規則編輯器的螢幕擷取畫面](./media/change-tracking3.png)

設定條件邏輯之後，您可以指派動作群組來執行動作，以回應警示。 在此範例中，當產生警示時，會傳送電子郵件並建立 ITSM 票證。 您可以採取許多其他有用的動作，例如觸發 Azure 函式、Azure 自動化 runbook、webhook 或邏輯應用程式。

![Azure 入口網站中的範例警示規則摘要的螢幕擷取畫面](./media/change-tracking4.png)

設定所有參數和邏輯之後，請將警示套用至環境。

## <a name="tracking-and-alerting-examples"></a>追蹤和警示範例

本節說明您可能想要使用之追蹤和警示的其他常見案例。

### <a name="driver-file-changed"></a>驅動程式檔案已變更

使用下列查詢來偵測是否已變更、新增或移除驅動程式檔案。 這適用于追蹤重要系統檔案的變更。

  ```kusto
  ConfigurationChange | where ConfigChangeType == "Files" and FileSystemPath contains " c:\\windows\\system32\\drivers\\"
  ```

### <a name="specific-service-stopped"></a>特定服務已停止

使用下列查詢來追蹤系統重要服務的變更。

  ```kusto
  ConfigurationChange | where SvcState == "Stopped" and SvcName contains "w3svc"
  ```

### <a name="new-software-installed"></a>已安裝新軟體

針對需要鎖定軟體設定的環境，請使用下列查詢。

  ```kusto
  ConfigurationChange | where ConfigChangeType == "Software" and ChangeCategory == "Added"
  ```

### <a name="specific-software-version-is-or-isnt-installed-on-a-machine"></a>特定軟體版本是或未安裝在電腦上

使用下列查詢來評估安全性。 此查詢會參考 `ConfigurationData`，其中包含清查的記錄，並提供上次回報的設定狀態，而不會變更。

  ```kusto
  ConfigurationData | where SoftwareName contains "Monitoring Agent" and CurrentVersion != "8.0.11081.0"
  ```

### <a name="known-dll-changed-through-the-registry"></a>已知的 DLL 已透過登錄變更

使用下列查詢來偵測已知登錄機碼的變更。

  ```kusto
  ConfigurationChange | where RegistryKey == "HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Control\\Session Manager\\KnownDlls"
  ```

## <a name="next-steps"></a>後續步驟

瞭解如何使用 Azure 自動化來[建立更新](./update-schedules.md)排程，以管理伺服器的更新。

> [!div class="nextstepaction"]
> [建立更新排程](./update-schedules.md)

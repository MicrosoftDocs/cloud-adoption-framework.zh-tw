---
title: 啟用重大變更的追蹤和警示
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 啟用重大變更的追蹤和警示
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 93449f754e3908e092fa64c55ad62fc604b4ba5b
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71027101"
---
# <a name="enable-tracking-and-alerting-for-critical-changes"></a>啟用重大變更的追蹤和警示

Azure 變更追蹤和清查會針對混合式環境的設定狀態以及對該環境的任何變更, 提供警示。 您可以監視可能會影響已部署伺服器的重要檔案、服務、軟體和登錄變更。

Azure 自動化清查服務預設不會監視檔案或登錄設定。 解決方案會提供建議的登錄機碼清單, 以供監視。 若要查看這份清單, 請移至 Azure 入口網站中的自動化帳戶, 然後選取 [**清查** > **編輯設定**]:

![Azure 入口網站中 Azure 自動化清查視圖的螢幕擷取畫面](./media/change-tracking1.png)

如需每個登錄機碼的詳細資訊, 請參閱登錄機[碼變更追蹤](https://docs.microsoft.com/azure/automation/automation-change-tracking#registry-key-change-tracking)。 您可以藉由選取每個金鑰來進行評估並加以啟用。 此設定會套用至目前工作區中啟用的所有 Vm。

您也可以追蹤重要的檔案變更。 例如, 您可能會想要追蹤 C:\windows\system32\drivers\etc\hosts 檔案, 因為 OS 會使用它來將主機名稱對應到 IP 位址。 對此檔案所做的任何變更可能會造成連線問題, 或將流量重新導向至危險的網站。

若要啟用主機檔案的檔案內容追蹤, 請遵循啟用檔案[內容追蹤](https://docs.microsoft.com/azure/automation/change-tracking-file-contents#enable-file-content-tracking)中的步驟。

您也可以針對要追蹤之檔案所做的變更, 加入警示。 例如, 假設您想要設定對 hosts 檔案所做變更的警示。 首先, 請在命令列上選取**Log analytics** , 或開啟連結的 log analytics 工作區的記錄搜尋, 以前往 log analytics。 在 Log Analytics 中之後, 使用下列查詢, 搜尋主機檔案的內容變更:

```kusto
ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"
```

![Azure 入口網站中 Log Analytics 查詢編輯器的螢幕擷取畫面](./media/change-tracking2.png)

此查詢會搜尋路徑包含「主機」一詞之檔案內容的變更。 您也可以藉由變更 path 參數來搜尋特定檔案。 (例如，`FileSystemPath ==  "c:\\windows\\system32\\drivers\\etc\\hosts"`)。
  
查詢傳回結果之後, 請選取 [**新增警示規則**] 以開啟 [警示規則編輯器]。 您也可以透過 Azure 入口網站中的 Azure 監視器, 進入此編輯器。

在 [警示規則編輯器] 中, 檢查查詢, 並視需要變更警示邏輯。 在此情況下, 我們想要在環境中的任何電腦上偵測到任何變更時, 產生警示。

![Azure 入口網站中 Log Analytics 警示規則編輯器的螢幕擷取畫面](./media/change-tracking3.png)

設定條件邏輯之後, 您可以指派動作群組來執行動作, 以回應警示。 在此範例中, 當產生警示時, 會傳送電子郵件並建立 ITSM 票證。 您可以採取許多其他有用的動作, 例如觸發 Azure 函式、Azure 自動化 runbook、webhook 或邏輯應用程式。

![Azure 入口網站中的範例警示規則摘要的螢幕擷取畫面](./media/change-tracking4.png)

設定所有參數和邏輯之後, 請將警示套用至環境。

## <a name="more-tracking-and-alerting-examples"></a>更多追蹤和警示範例

以下是您可能想要考慮的一些追蹤和警示的常見案例:

### <a name="driver-file-changed"></a>驅動程式檔案已變更

偵測是否已變更、新增或移除驅動程式檔案。 適用于追蹤重要系統檔案的變更。

  ```kusto
  ConfigurationChange | where ConfigChangeType == "Files" and FileSystemPath contains " c:\\windows\\system32\\drivers\\"
  ```

### <a name="specific-service-stopped"></a>特定服務已停止

適用于追蹤系統重要服務的變更。

  ```kusto
  ConfigurationChange | where SvcState == "Stopped" and SvcName contains "w3svc"
  ```

### <a name="new-software-installed"></a>已安裝新軟體

適用于需要鎖定軟體設定的環境。

  ```kusto
  ConfigurationChange | where ConfigChangeType == "Software" and ChangeCategory == "Added"
  ```

### <a name="specific-software-version-is-or-isnt-installed-on-a-machine"></a>特定軟體版本是或未安裝在電腦上

有助於評估安全性。 請注意, 此查詢`ConfigurationData`會參考, 其中包含清查的記錄並報告上次回報的設定狀態, 而不會變更。

  ```kusto
  ConfigurationData | where SoftwareName contains "Monitoring Agent" and CurrentVersion != "8.0.11081.0"
  ```

### <a name="known-dll-changed-through-registry"></a>已知的 DLL 已透過登錄變更

適用于偵測已知登錄機碼的變更。

  ```kusto
  ConfigurationChange | where RegistryKey == "HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Control\\Session Manager\\KnownDlls"
  ```

## <a name="next-steps"></a>後續步驟

瞭解如何使用 Azure 自動化[建立更新](./update-schedules.md)排程, 來管理伺服器的更新。

> [!div class="nextstepaction"]
> [建立更新排程](./update-schedules.md)

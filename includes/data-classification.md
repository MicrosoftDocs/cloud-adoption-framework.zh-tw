<!-- TEMPLATE FILE - DO NOT ADD METADATA -->
<!-- markdownlint-disable MD026 MD041 -->
資料分類可讓您判斷並指派組織資料的價值，並提供共同管理的共同起點。 資料分類程式會依敏感性和業務影響來分類資料，以找出風險。 分類資料時，您可以透過保護敏感或重要資料免于遭竊或遺失的方式進行管理。

## <a name="understand-data-risks-then-manage-them"></a>瞭解資料風險，然後管理它們

在可以管理任何風險之前，您必須先瞭解。 對於資料安全性缺口責任，必須先了解資料分類。 資料分類是一種程式，可將中繼資料特性與數位資產中的每個資產產生關聯，以識別與該資產相關聯的資料類型。

任何識別為可能適合遷移或部署至雲端的資產都應該有記載的中繼資料，以記錄資料分類、業務重要性和帳單責任。 這三個分類點對於了解和降低風險很有幫助。

## <a name="classifications-microsoft-uses"></a>Microsoft 使用的分類

下列是 Microsoft 使用的分類清單。 視您的產業或現有的安全性需求而定，您的組織中可能已經有資料分類標準。 如果沒有任何標準，您可能會想要使用此範例分類，以進一步瞭解您自己的數位資產和風險設定檔。

- **非企業：** 您個人生活中不屬於 Microsoft 的資料。
- **公用：** 免費提供且已核准可供公開使用的商務資料。
- **一般：** 適用于公眾物件的商務資料。
- **機密：** 如果 overshared，可能會對 Microsoft 造成損害的商務資料。
- **高度機密：** 如果 overshared，可能會對 Microsoft 造成大量損害的商務資料。

## <a name="tagging-data-classification-in-azure"></a>在 Azure 中標記資料分類

資源標記適合用來儲存中繼資料，而且您可以使用這些標記將資料分類資訊套用至已部署的資源。 雖然依分類來標記雲端資產不是正式資料分類程式的替代方案，但它提供了一個重要的工具來管理資源及套用原則。 [Azure 資訊保護](/azure/information-protection/what-is-information-protection) 是一個絕佳的解決方案，可協助您將資料本身分類，不論其 (位於內部部署、Azure 或其他) 的地方。 請將它視為整體分類策略的一部分。

## <a name="take-action"></a>採取動作

使用定義的資料分類來定義和標記資產，以採取動作。

- [請選擇其中一個可操作的治理指南](../docs/govern/guides/index.md) ，以取得在您的組合中套用標記的範例。
- 請參閱 [建議的命名和標記慣例](../docs/ready/azure-best-practices/resource-tagging.md) ，以定義更完整的標記標準。
- 如需 Azure 中資源標記的詳細資訊，請參閱 [使用標記來組織您的 azure 資源和管理](/azure/azure-resource-manager/management/tag-resources)階層。

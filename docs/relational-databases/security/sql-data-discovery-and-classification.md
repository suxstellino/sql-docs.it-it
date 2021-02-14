---
title: Individuazione dati e classificazione SQL | Microsoft Docs
description: Individuazione dati e classificazione SQL
documentationcenter: ''
ms.reviewer: vanto
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.prod_service: sql-database,sql
ms.custom: security
ms.topic: conceptual
ms.date: 02/05/2021
ms.author: datrigan
author: DavidTrigano
ms.openlocfilehash: 74478f363ac4c6fac4ab684fde1bafd0a2bb8551
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340177"
---
# <a name="sql-data-discovery-and-classification"></a>Individuazione dati e classificazione SQL
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La funzionalità Individuazione dati e classificazione costituisce un nuovo strumento incorporato in [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) per l'**individuazione**, la **classificazione**, l'**assegnazione di etichette** & **la creazione di report** di dati sensibili nei database.
L'individuazione e la classificazione dei dati più sensibili, come i dati aziendali, finanziari, medici e così via, può avere un ruolo fondamentale nella protezione delle informazioni dell'organizzazione. Possono costituire l'infrastruttura per:
* Contribuire a soddisfare gli standard per la privacy dei dati.
* Monitoraggio dell'accesso a database/colonne contenenti dati altamente sensibili.

> [!NOTE]
> Individuazione dati e classificazione è una funzionalità **supportata per SQL Server 2012 e versioni successive e può essere usata con [SSMS 17.5](../../ssms/download-sql-server-management-studio-ssms.md) o versioni successive**. Per database SQL di Azure, vedere [Individuazione dati e classificazione nel database SQL di Azure](/azure/sql-database/sql-database-data-discovery-and-classification/).

## <a name="overview"></a><a id="Overview"></a>Panoramica
Individuazione dati & classificazione introduce un set di servizi, costituendo un nuovo paradigma SQL Information Protection mirato alla protezione dei dati, non solo al database:

* **Individuazione e consigli**: il motore di classificazione esegue l'analisi del database e identifica le colonne che contengono dati potenzialmente sensibili. In seguito offre un modo semplice per verificare e applicare i consigli di classificazione appropriati, nonché per classificare manualmente le colonne.
* **Assegnazione di etichette**: è possibile contrassegnare le colonne con etichette di classificazione di riservatezza in modo permanente.
* **Visibilità**: è possibile visualizzare lo stato di classificazione del database in un report dettagliato che può essere stampato o esportato a scopo di controllo e conformità, nonché per altre esigenze.

## <a name="discovering-classifying--labeling-sensitive-columns"></a><a id="Discovering-classifying-labeling-sensitive-columns"></a>Individuazione, classificazione e assegnazione di etichette a colonne di dati sensibili
Nella sezione seguente vengono descritti i passaggi per l'individuazione, la classificazione e l'assegnazione di etichette a colonne del database contenenti dati sensibili, nonché per visualizzare lo stato di classificazione corrente del database e per esportare report.

La classificazione include due attributi di metadati:
* Etichette: gli attributi di classificazione principali, usati per definire il livello di riservatezza dei dati archiviati nella colonna.  
* Tipi di informazioni: offrono maggiore granularità del tipo di dati archiviati nella colonna.

**Per classificare il database di SQL Server:**

1. in SQL Server Management Studio (SSMS) connettersi a SQL Server.

2. In Esplora oggetti di SSMS fare clic con il pulsante destro del mouse sul database che si vuole classificare e scegliere **Attività** > **Individuazione dati e classificazione** > **Classifica dati**.

   ![Screenshot che mostra Esplora oggetti di SSMS con le opzioni Attività > Individuazione dati e classificazione > Classifica dati selezionate.][0]

3. Il motore di classificazione esegue l'analisi del database per identificare le colonne che contengono dati potenzialmente sensibili e specifica un elenco di **classificazioni di colonne consigliate**:

    * Per visualizzare l'elenco delle classificazioni delle colonne consigliate, fare clic sulla casella di notifica dei consigli in alto oppure sul pannello dei consigli nella parte inferiore della finestra:

        ![Screenshot che mostra la notifica che sono state trovate 39 colonne con consigli per la classificazione. Fare clic qui per visualizzarli.][2]

        ![Screenshot che mostra la notifica che sono state trovate 39 colonne con consigli per la classificazione (fare clic per visualizzarli).][3]

    * Esaminare l'elenco dei consigli:
        * Per accettare un consiglio per una colonna specifica, selezionare la casella di controllo nella colonna sinistra della riga pertinente. È anche possibile contrassegnare *tutti i consigli* come accettati selezionando la casella di controllo nell'intestazione della tabella dei consigli.

        * È anche possibile modificare i valori di Tipo di informazioni e di Etichetta riservatezza mediante le caselle a discesa.        

        ![Screenshot che mostra l'elenco dei consigli.][4]

    * Per applicare i consigli selezionati, fare clic sul pulsante blu **Accettare i consigli selezionati**.

        ![Screenshot del pulsante Accettare i consigli selezionati.][5]

4. È anche possibile **classificare manualmente** le colonne in alternativa o in aggiunta alla classificazione basata sui consigli:

    * Fare clic su **Aggiungi classificazione** nel menu superiore della finestra.

        ![Screenshot che mostra il menu principale con l'opzione Aggiungi classificazione evidenziata.][6]

    * Nella finestra di contesto che si apre selezionare lo schema, la tabella e la colonna che si vuole classificare, nonché il tipo di informazioni e l'etichetta di riservatezza. In seguito fare clic sul pulsante blu **Aggiungi classificazione** nella parte inferiore della finestra di contesto.

        ![Screenshot che mostra la finestra di contesto Aggiungi classificazione.][7]

5. Per completare la classificazione e assegnare in modo permanente le etichette alle colonne del database con i nuovi metadati di classificazione, fare clic su **Salva** nel menu superiore della finestra.

    ![Screenshot che mostra il menu principale con l'opzione Salva evidenziata.][8]


6. Per generare un report con un riepilogo completo dello stato di classificazione del database, fare clic su **Visualizza Report** nel menu superiore della finestra. È anche possibile generare un report usando SSMS. Fare clic con il pulsante destro del mouse sul database in cui si vuole generare il report e scegliere **Attività** > **Individuazione dati e classificazione** > **Genera report**.

    ![Screenshot che mostra il menu principale con l'opzione Visualizza report evidenziata.][9]

    ![Screenshot che mostra il report di classificazione dei dati SQL.][10]

## <a name="manage-information-protection-policy-with-ssms"></a><a id="Manage-information-protection-policy-with-SSMS"></a>Gestire i criteri di Information Protection con SSMS

[SSMS 18.4](../../ssms/download-sql-server-management-studio-ssms.md) o versione successiva consente di gestire i criteri di Information Protection:

1. in SQL Server Management Studio (SSMS) connettersi a SQL Server.

2. In Esplora oggetti di SSMS fare clic con il pulsante destro del mouse su uno dei database e scegliere **Attività** > **Individuazione dati e classificazione**.

   Le seguenti opzioni di menu consentono di gestire i criteri di Information Protection:

* **Imposta file dei criteri di Information Protection**: usa i criteri di Information Protection come definito nel file JSON selezionato.

* **Esporta criteri di Information Protection**: esporta i criteri di Information Protection in un file JSON.

* **Ripristina criteri di Information Protection**: reimposta i criteri di Information Protection sui criteri predefiniti di Information Protection.

> [!IMPORTANT]
> Il file dei criteri di Information Protection non è archiviato in SQL Server.
> SSMS usa i criteri di Information Protection predefiniti. Se un criterio di Information Protection personalizzato non riesce, SSMS non è in grado di usare il criterio predefinito. La classificazione dei dati ha esito negativo. Per risolvere il problema, fare clic su **Ripristina criteri di Information Protection** per usare i criteri predefiniti e riabilitare la classificazione dei dati.

## <a name="accessing-the-classification-metadata"></a><a id="sAccessing-the-classification-metadata"></a>Accesso ai metadati di classificazione

SQL Server 2019 introduce la [`sys.sensitivity_classifications`](../system-catalog-views/sys-sensitivity-classifications-transact-sql.md) vista del catalogo di sistema. Questa vista restituisce i tipi di informazioni e le etichette di riservatezza.

Per le istanze di SQL Server 2019, eseguire una query su `sys.sensitivity_classifications` per esaminare tutte le colonne classificate e le classificazioni corrispondenti. Ad esempio: 

```sql
SELECT 
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    label,
    rank,
    rank_desc
FROM sys.sensitivity_classifications sc
    JOIN sys.objects O
    ON  sc.major_id = O.object_id
    JOIN sys.columns C 
    ON  sc.major_id = C.object_id  AND sc.minor_id = C.column_id
```

Prima di SQL 2019 i metadati di classificazione per i tipi di informazioni e le etichette di riservatezza sono archiviati nelle seguenti Proprietà estese: 

* `sys_information_type_name`
* `sys_sensitivity_label_name`

Per le istanze di SQL Server 2017 e versioni precedenti, l'esempio seguente restituisce tutte le colonne classificate e le classificazioni corrispondenti:

```sql
SELECT
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    sensitivity_label 
FROM
    (
        SELECT
            IT.major_id,
            IT.minor_id,
            IT.information_type,
            L.sensitivity_label 
        FROM
        (
            SELECT
                major_id,
                minor_id,
                value AS information_type 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_information_type_name'
        ) IT 
        FULL OUTER JOIN
        (
            SELECT
                major_id,
                minor_id,
                value AS sensitivity_label 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_sensitivity_label_name'
        ) L 
        ON IT.major_id = L.major_id AND IT.minor_id = L.minor_id
    ) EP
    JOIN sys.objects O
    ON  EP.major_id = O.object_id 
    JOIN sys.columns C 
    ON  EP.major_id = C.object_id AND EP.minor_id = C.column_id
```

## <a name="permissions"></a><a id="Permissions"></a>Autorizzazioni

Nelle istanze SQL Server 2019, la classificazione di visualizzazione richiede l'autorizzazione **View any Sensitivity Classification** . Per altre informazioni, vedere [Metadata Visibility Configuration](./metadata-visibility-configuration.md).

Prima di SQL Server 2019, è possibile accedere ai metadati utilizzando la vista del catalogo delle proprietà estese [`sys.extended_properties`](../system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md) .

Per la gestione della classificazione è necessaria l'autorizzazione ALTER ANY SENSITIVity CLASSIFICAtion. L'autorizzazione ALTER ANY SENSITIVITY CLASSIFICATION è implicita nell'autorizzazione del database ALTER o nell'autorizzazione del server CONTROL SERVER.

## <a name="manage-classifications"></a><a id="subheading-5"></a>Gestire le classificazioni

# <a name="t-sql"></a>[T-SQL](#tab/t-sql)
È possibile utilizzare T-SQL per aggiungere o rimuovere le classificazioni di colonna, nonché recuperare tutte le classificazioni per l'intero database.

- Aggiungere o aggiornare la classificazione di una o più colonne: [AGGIUNGI CLASSIFICAZIONE DI RISERVATEZZA](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)
- Rimuovere la classificazione da una o più colonne: [ELIMINA CLASSIFICAZIONE DI RISERVATEZZA](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)

# <a name="powershell-cmdlet"></a>[Cmdlet di PowerShell](#tab/sql-powelshell)
È possibile utilizzare un cmdlet di PowerShell per aggiungere o rimuovere le classificazioni di colonna, nonché recuperare tutte le classificazioni e ottenere le raccomandazioni per l'intero database.

- [Get-SqlSensitivityClassification](/powershell/module/sqlserver/Get-SqlSensitivityClassification)
- [Get-SqlSensitivityRecommendations](/powershell/module/sqlserver/Get-SqlSensitivityRecommendations)
- [Set-SqlSensitivityClassification](/powershell/module/sqlserver/Set-SqlSensitivityClassification)
- [Remove-SqlSensitivityClassification](/powershell/module/sqlserver/Remove-SqlSensitivityClassification)

## <a name="next-steps"></a><a id="Next-steps"></a>Passaggi successivi

Per database SQL di Azure, vedere [Individuazione dati e classificazione nel database SQL di Azure](/azure/azure-sql/database/data-discovery-and-classification-overview).

Potrebbe essere consigliabile proteggere le colonne sensibili applicando meccanismi di sicurezza a livello di colonna:

* [Dynamic Data Masking](./dynamic-data-masking.md) per l'offuscamento delle colonne sensibili in uso.
* [Always Encrypted](./encryption/always-encrypted-database-engine.md) per la crittografia delle colonne sensibili inattive.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Manage information protection policy with SSMS]: #subheading-3
[Accessing the classification metadata]: #subheading-4
[Manage classifications]: #subheading-5
[Next Steps]: #subheading-6

<!--Image references-->

[0]: ./media/sql-data-discovery-and-classification/0-data-classification-explorer.png
[2]: ./media/sql-data-discovery-and-classification/2-recommendations-notification-box.png
[3]: ./media/sql-data-discovery-and-classification/3-recommendations-panel.png
[4]: ./media/sql-data-discovery-and-classification/4-recommendations.png
[5]: ./media/sql-data-discovery-and-classification/5-accept-recommendations-button.png
[6]: ./media/sql-data-discovery-and-classification/6-add-classification-button.png
[7]: ./media/sql-data-discovery-and-classification/7-manual-classification.png
[8]: ./media/sql-data-discovery-and-classification/8-save.png
[9]: ./media/sql-data-discovery-and-classification/9-view-report.png
[10]: ./media/sql-data-discovery-and-classification/10-report.png

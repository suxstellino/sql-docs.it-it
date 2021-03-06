---
title: Eseguire una valutazione della migrazione SQL Server
titleSuffix: Data Migration Assistant
description: Informazioni su come usare Data Migration Assistant per valutare una SQL Server locale prima di eseguire la migrazione a un altro SQL Server o al database SQL di Azure
ms.date: 01/15/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 300d88b66c2105235ab04ff616d9fcf81b24e944
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247337"
---
# <a name="perform-a-sql-server-migration-assessment-with-data-migration-assistant"></a>Eseguire una valutazione della migrazione di SQL Server con Data Migration Assistant

Le istruzioni dettagliate seguenti consentono di eseguire la prima valutazione per la migrazione a SQL Server locali, SQL Server in esecuzione in una macchina virtuale di Azure o un database SQL di Azure usando Data Migration Assistant.

Data Migration Assistant v 5.0 introduce il supporto per l'analisi della connettività del database e delle query SQL incorporate nel codice dell'applicazione. Per altre informazioni, vedere il post di Blog relativo [all'uso di Data Migration Assistant per valutare il livello di accesso ai dati di un'applicazione](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Using-Data-Migration-Assistant-to-assess-an-application-s-data/ba-p/990430).

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="create-an-assessment"></a>Creare una valutazione

1. Selezionare l'icona **nuovo** (+), quindi selezionare il tipo di progetto di **valutazione** .

2. Impostare il tipo di server di origine e destinazione.

    Se si sta aggiornando l'istanza di SQL Server locale a un'istanza di SQL Server locale moderna o a SQL Server ospitata in una macchina virtuale di Azure, impostare il tipo di server di origine e di destinazione su **SQL Server**. Se si esegue la migrazione al database SQL di Azure, impostare invece il tipo di server di destinazione sul **database SQL di Azure**.

3. Fare clic su **Crea**.

   ![Creare una valutazione](../dma/media/dma-assesssqlonprem/new-assessment.png)

## <a name="choose-assessment-options"></a>Scegliere le opzioni di valutazione

1. Selezionare la versione SQL Server di destinazione in cui si intende eseguire la migrazione.

2. Consente di selezionare il tipo di report.

   Quando si valuta l'istanza di SQL Server di origine per la migrazione a SQL Server locale o a SQL Server ospitata in destinazioni di macchine virtuali di Azure, è possibile scegliere uno o entrambi i tipi di report di valutazione seguenti:

    - **Problemi di compatibilità**
    - **Raccomandazione per le nuove funzionalità**

   ![Selezionare un tipo di report di valutazione per SQL Server destinazione](../dma/media/dma-assesssqlonprem/assessment-types.png)

   Quando si valuta l'istanza di SQL Server di origine per la migrazione al database SQL di Azure, è possibile scegliere uno o entrambi i tipi di report di valutazione seguenti:

    - **Check database compatibility (Verificare la compatibilità del database)**
    - **Check feature parity (Verificare la parità di funzionalità)**

    ![Selezionare il tipo di report di valutazione per destinazione database SQL](../dma/media/dma-assesssqlonprem/assessment-types-azure.png)

## <a name="add-databases-and-extended-events-trace-to-assess"></a>Aggiungere database ed eventi estesi traccia per la valutazione

1. Selezionare **Aggiungi origini** per aprire il menu a comparsa connessione.

2. Immettere il nome dell'istanza di SQL Server, scegliere il tipo di autenticazione, impostare le proprietà di connessione corrette, quindi selezionare **Connetti**.

3. Selezionare i database da valutare, quindi selezionare **Aggiungi**.

    > [!NOTE]
    > È possibile rimuovere più database selezionandola tenendo premuto il tasto MAIUSC o CTRL e quindi facendo clic su **Rimuovi origini**. È anche possibile aggiungere database da più istanze di SQL Server selezionando **Aggiungi origini**.

4. Se sono presenti query SQL ad hoc o dinamiche o istruzioni DML avviate tramite il livello dati dell'applicazione, immettere il percorso della cartella in cui sono stati inseriti tutti i file di sessione degli eventi estesi raccolti per acquisire il carico di lavoro nel SQL Server di origine.

     Nell'esempio seguente viene illustrato come creare una sessione di eventi estesi nel SQL Server di origine per acquisire il carico di lavoro livello dati applicazione.  Acquisire il carico di lavoro per la durata che rappresenta il carico di lavoro di picco.

    ```
    DROP EVENT SESSION [DatalayerSession] ON SERVER
    go
    CREATE EVENT SESSION [DatalayerSession] ON SERVER  
    ADD EVENT sqlserver.sql_batch_completed( 
        ACTION (sqlserver.sql_text,sqlserver.client_app_name,sqlserver.client_hostname,sqlserver.database_id))
    ADD TARGET package0.asynchronous_file_target(SET filename=N'C:\temp\Demos\DataLayerAppassess\DatalayerSession.xel')  
    WITH (MAX_MEMORY=2048 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=3 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
    go
    ---Start the session
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = START;
    ---Wait for few minutes
    
    ---Query events
        
        SELECT 
        object_name,
        CAST(event_data as xml) as event_data,
        file_name, 
        file_offset
    FROM sys.fn_xe_file_target_read_file('C:\temp\Demos\DataLayerAppassess\DatalayerSession*xel', 
                'C:\\temp\\Demos\\DataLayerAppassess\\DatalayerSession*xem', 
                null,
                null)
    ---Stop the session after capturing the peak load.
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = STOP;
        
        go
    ```

5. Fare clic su **Next** (Avanti) per avviare la valutazione.

    ![Aggiungere origini e avviare la valutazione](../dma/media/dma-assesssqlonprem/select-database1.png)

> [!NOTE]
> È possibile eseguire più valutazioni contemporaneamente e visualizzarne lo stato aprendo la pagina **All Assessments** (Tutte le valutazioni).

## <a name="view-results"></a>Visualizzazione dei risultati

La durata della valutazione dipende dal numero di database aggiunti e dalle dimensioni dello schema di ogni database. I risultati vengono visualizzati per ogni database non appena sono disponibili.

1. Selezionare il database che ha completato la valutazione, quindi passare tra **problemi di compatibilità** e **suggerimenti sulle funzionalità** usando lo strumento di selezione.

2. Esaminare i problemi di compatibilità tra tutti i livelli di compatibilità supportati dalla versione di SQL Server di destinazione selezionata nella pagina **Opzioni** .

È possibile esaminare i problemi di compatibilità analizzando l'oggetto interessato, i relativi dettagli e potenzialmente una correzione per ogni problema identificato in caso di **modifiche di rilievo**, **modifiche del comportamento** e **funzionalità deprecate**.

![Visualizza risultati valutazione](../dma/media/dma-assesssqlonprem/review-results.png)

Analogamente, è possibile esaminare le indicazioni sulle funzionalità nelle aree **prestazioni**, **archiviazione** e **sicurezza** .

Le raccomandazioni sulle funzionalità coprono diversi tipi di funzionalità, ad esempio In-Memory OLTP, columnstore, Stretch Database, Always Encrypted, Dynamic Data Masking e Transparent Data Encryption.

![Visualizzare le raccomandazioni sulle funzionalità](../dma/media/dma-assesssqlonprem/feature-recommendations.png)

Per il database SQL di Azure, le valutazioni forniscono problemi di blocco della migrazione e problemi di parità delle funzionalità. Esaminare i risultati per entrambe le categorie selezionando le opzioni specifiche.

- La categoria di **parità delle funzionalità SQL Server** offre un set completo di indicazioni, approcci alternativi disponibili in Azure e procedure di mitigazione. Consente di pianificare questa operazione nei progetti di migrazione.

  ![Visualizzare le informazioni per la parità di funzionalità SQL Server](../dma/media/dma-assesssqlonprem/sql-feature-parity.png)

- La categoria **problemi di compatibilità** fornisce funzionalità parzialmente supportate o non supportate che bloccano la migrazione dei database di SQL Server locali ai database SQL di Azure. Fornisce quindi consigli per risolvere tali problemi.

  ![Visualizzare i problemi di compatibilità](../dma/media/dma-assesssqlonprem/compatibility-issues.png)

## <a name="assess-a-data-estate-for-target-readiness"></a>Valutare un patrimonio di dati per la conformità della destinazione

Se si vuole estendere ulteriormente queste valutazioni all'intera proprietà dei dati e trovare la conformità relativa di SQL Server istanze e database per la migrazione al database SQL di Azure, caricare i risultati nell'hub Azure Migrate selezionando **carica per Azure migrate**.

In questo modo è possibile visualizzare i risultati consolidati nel progetto Hub Azure Migrate.

Istruzioni dettagliate per la valutazione della conformità della destinazione sono disponibili [qui](./dma-assess-sql-data-estate-to-sqldb.md).

   ![Carica i risultati in Azure Migrate](../dma/media/dma-assesssqlonprem/upload-to-azure-migrate.png)

## <a name="export-results"></a>Esportare i risultati

Al termine della valutazione di tutti i database, selezionare **Esporta report** per esportare i risultati in un file JSON o in un file CSV. È quindi possibile analizzare i dati con la propria praticità.

## <a name="save-and-load-assessments"></a>Salvare e caricare valutazioni

Oltre ad esportare i risultati di una valutazione, è possibile salvare i dettagli di valutazione in un file e caricare un file di valutazione per una revisione successiva.  Per ulteriori informazioni, vedere l'articolo [salvare e caricare valutazioni con Data Migration Assistant](../dma/dma-save-load-assessments.md).

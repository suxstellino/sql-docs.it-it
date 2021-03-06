---
title: Valutazione della conformità SQL Server per eseguire la migrazione al database SQL di Azure
titleSuffix: Data Migration Assistant
description: Informazioni su come usare Data Migration Assistant per eseguire la migrazione di un SQL Server Data estate per la migrazione al database SQL di Azure
ms.date: 12/19/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 1094f10eedc819123cd446c0933ec14df511ac75
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247343"
---
# <a name="assess-the-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql-database-using-the-data-migration-assistant"></a>Valutare la conformità di un SQL Server di dati di cui si esegue la migrazione al database SQL di Azure usando il Data Migration Assistant

La migrazione di centinaia di istanze di SQL Server e di migliaia di database nel database SQL di Azure o in Azure SQL Istanza gestita, le nostre offerte di piattaforma distribuita come servizio (PaaS), è un'attività considerevole. Per semplificare il processo il più possibile, è necessario avere la certezza della disponibilità relativa per la migrazione. L'identificazione dei frutti a basso costo, inclusi i server e i database che sono completamente pronti o che richiedono un impegno minimo per preparare la migrazione, semplifica e accelera le attività.

In questo articolo vengono fornite istruzioni dettagliate per sfruttare i [Data Migration Assistant](./dma-overview.md) per riepilogare i risultati di conformità ed esporli nell'hub [Azure migrate](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview) .

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Migration-Assistant/player?WT.mc_id=dataexposed-c9-niner]

## <a name="create-a-project-and-add-a-tool"></a>Creare un progetto e aggiungere uno strumento

Configurare un nuovo progetto di Azure Migrate in una sottoscrizione di Azure e quindi aggiungere uno strumento.

Il progetto Azure Migrate viene usato per archiviare i metadati di individuazione, valutazione e migrazione raccolti dall'ambiente da valutare o trasferire. Si usa un progetto anche per tenere traccia delle risorse individuate e per orchestrare la valutazione e la migrazione.

1. Accedere al portale di Azure, selezionare tutti i **Servizi** e quindi cercare Azure migrate.
2. In **Servizi** selezionare **Azure Migrate**.

   ![Azure Migrate-selezionare il servizio](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-services.png)

3. Nella pagina **Panoramica** selezionare **valuta ed Esegui la migrazione dei database**.

   ![Valutazione dell'avvio Azure Migrate](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

4. Nei **database**, in **operazioni preliminari**, selezionare **Aggiungi strumento/i**.

   ![Azure Migrate-aggiungere strumenti](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tools.png)

5. Nella scheda **migrazione progetto** selezionare la sottoscrizione e il gruppo di risorse di Azure (se non si dispone già di un gruppo di risorse, crearne uno).
6. In **Dettagli progetto** specificare il nome del progetto e l'area geografica in cui si desidera creare il progetto.

    ![Azure Migrate-aggiungere una finestra di dialogo dello strumento](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tool-dialog.png)

    È possibile creare un progetto di Azure Migrate in una delle aree geografiche seguenti.

    | **Area geografica**  | **Area località di archiviazione** |
    | ------------- | ------------- |
    | Asia | Asia sud-orientale o Asia orientale |
    | Europa | Europa meridionale o Europa occidentale |
    | Regno Unito | Regno Unito meridionale o Regno Unito occidentale |
    | Stati Uniti | Stati Uniti centrali o Stati Uniti occidentali 2 |

    L'area geografica specificata per il progetto viene usato solo per archiviare i metadati raccolti da macchine virtuali locali. Per la migrazione effettiva è possibile selezionare qualsiasi area di destinazione.

7. Selezionare **Avanti**, quindi aggiungere uno strumento di valutazione.

   > [!NOTE]
   > Quando si crea un progetto, è necessario aggiungere almeno uno strumento di valutazione o migrazione.

8. Nella scheda **Seleziona strumento di valutazione** **Azure migrate: database Assessment** viene visualizzato come strumento di valutazione da aggiungere. Se non è attualmente necessario uno strumento di valutazione, selezionare la casella **di controllo Ignora l'aggiunta di uno strumento di valutazione per il momento** . Selezionare **Avanti**.

    ![Azure Migrate selezionare la scheda strumento di valutazione](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-assessment-tool.png)

9. Nella scheda **Seleziona strumento di migrazione** **Azure migrate: la migrazione del database** viene visualizzata come strumento di migrazione da aggiungere. Se non è attualmente necessario uno strumento di migrazione, selezionare il **Skip aggiungere uno strumento di migrazione per il momento**. Selezionare **Avanti**.

    ![Azure Migrate-selezionare la scheda strumento di migrazione](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-migration-tool.png)

10. In **Verifica + Aggiungi strumenti**, rivedere le impostazioni e selezionare **Aggiungi strumenti**.

    ![Azure Migrate-Review + Aggiungi strumento/i scheda](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-tools.png)

    Dopo aver creato il progetto, è possibile selezionare strumenti aggiuntivi per la valutazione e la migrazione di server e carichi di lavoro, database e app Web.

## <a name="assess-and-upload-assessment-results"></a>Valutare e caricare i risultati della valutazione

Dopo aver creato un progetto di migrazione, in **strumenti di valutazione**, nella casella **Azure migrate: database Assessment** , le istruzioni per il download e l'uso dello strumento Data Migration Assistant display.

   ![Aggiunta dello strumento di valutazione Azure Migrate](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessment-tool-added.png)

1. Scaricare Data Migration Assistant utilizzando il collegamento fornito, quindi installarlo in un computer con accesso alle istanze di SQL Server di origine.
2. Avviare Data Migration Assistant.

### <a name="create-an-assessment"></a>Creare una valutazione

1. A sinistra, selezionare l' **+** icona e quindi selezionare il **tipo di progetto** di valutazione
2. Specificare il nome del progetto, quindi selezionare il server di origine e i tipi di server di destinazione.

    Se si sta aggiornando l'istanza di SQL Server locale a una versione successiva di SQL Server o a SQL Server ospitata in una macchina virtuale di Azure, impostare il tipo di server di origine e di destinazione su **SQL Server**. Impostare il tipo di server di destinazione su **Azure sql istanza gestita** per una valutazione della conformità della destinazione del database SQL di Azure (PaaS).

3. Selezionare **Crea**.

   ![Interfaccia Azure Migrate-Data Migration Assistant](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-interface.png)

### <a name="choose-assessment-options"></a>Scegliere le opzioni di valutazione

1. Consente di selezionare il tipo di report.

    È possibile scegliere uno o entrambi i tipi di report seguenti:
    * Check database compatibility (Verificare la compatibilità del database)
    * Check feature parity (Verificare la parità di funzionalità)

   ![Schermata delle opzioni di Azure Migrate-Data Migration Assistant-Assessment](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-options-screen.png)

2. Selezionare **Avanti**.

### <a name="add-databases-to-assess"></a>Aggiungere i database da valutare

1. Selezionare **Aggiungi origini** per aprire il menu di scelta rapida della connessione.
2. Immettere il nome dell'istanza di SQL Server, scegliere il tipo di autenticazione, impostare le proprietà di connessione corrette, quindi selezionare **Connetti**.
3. Selezionare i database da valutare, quindi selezionare **Aggiungi**.

   > [!NOTE]
   > È possibile rimuovere più database selezionandola tenendo premuto il tasto MAIUSC o CTRL e quindi facendo clic su Rimuovi origini. È anche possibile aggiungere database da più istanze di SQL Server usando il pulsante Aggiungi origini.

4. Selezionare **Avanti** per avviare la valutazione.

   ![Schermata di Azure Migrate-Data Migration Assistant-seleziona origini](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-select-sources-screen.png)

5. Al termine della valutazione, selezionare **carica per Azure migrate**.

   ![Screenshot che mostra il Data Migration Assistant con l'opzione carica in Azure Migrate denominata out.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-review-results-screen.png)

6. Accedere al portale di Azure.

   ![Screenshot del Data Migration Assistant che mostra la finestra di accesso portale di Azure.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-signin.png)

7. Selezionare la sottoscrizione e il progetto Azure Migrate in cui si desidera caricare i risultati della valutazione, quindi selezionare **carica**.

   Attendere la conferma del caricamento della valutazione.

## <a name="view-target-readiness-assessment-results"></a>Visualizzare i risultati della valutazione della conformità della destinazione

1. Accedere al portale di Azure, cercare Azure migrate e selezionare **Azure migrate**.

   ![Ricerca Azure Migrate-portale di Azure-Service](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-search.png)

2. Selezionare **valuta ed Esegui la migrazione dei database** per ottenere i risultati della valutazione.

   ![Azure Migrate esaminare i risultati della valutazione](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

    È possibile visualizzare il riepilogo del SQL Server di conformità, assicurarsi di trovarsi nel progetto di migrazione corretto. in caso contrario, usare l'opzione cambia per selezionare un progetto di migrazione diverso.

    Ogni volta che si aggiornano i risultati della valutazione al progetto Azure migrate, l'hub Azure migrate consolida tutti i risultati e fornisce il report di riepilogo.  È possibile eseguire più valutazioni Data Migration Assistant in parallelo e caricare i risultati nel progetto di migrazione singolo per ottenere il report di conformità consolidato.

   ![Azure Migrate-verificare i risultati di conformità](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-readiness.png)

    **Istanze di database valutate**: numero di istanze di SQL Server valutate finora.
    **Database valutati**: numero totale di database valutati in una o più istanze di SQL Server la valutazione dei database è **pronta per il database SQL**: numero di database pronti per la migrazione al database SQL di Azure (PaaS).
    **Database pronti per la macchina virtuale SQL di Azure**: il numero di database è costituito da uno o più blocchi di migrazione nel database SQL di Azure (PaaS), ma è pronto per la migrazione ad Azure SQL Server macchine virtuali.

3. Selezionare **istanze di database valutate** per ottenere SQL Server visualizzazione a livello di istanza.

   ![Verifica della conformità dell'istanza di Azure Migrate](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-instances.png)

    È possibile trovare lo stato di conformità percentuale di ogni istanza di SQL Server che esegue la migrazione al database SQL di Azure (PaaS).

4. Selezionare un'istanza specifica per ottenere la visualizzazione di conformità del database.

   ![Azure Migrate verificare la conformità del database](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-databases.png)

    È possibile visualizzare il numero di blocchi di migrazione per ogni database, la destinazione consigliata per ogni database nella vista precedente.

5. Esaminare i risultati della valutazione DMA per ottenere altre informazioni sui blocchi di migrazione.

   ![Azure Migrate rivedere i blocchi di migrazione](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-migration-blockers.png)

## <a name="see-also"></a>Vedi anche

* [Data Migration Assistant (DMA)](../dma/dma-overview.md)
* [Data Migration Assistant: impostazioni di configurazione](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedure consigliate](../dma/dma-bestpractices.md)

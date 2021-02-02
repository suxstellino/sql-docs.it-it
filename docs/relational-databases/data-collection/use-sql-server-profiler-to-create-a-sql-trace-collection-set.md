---
description: Utilizzare SQL Server Profiler per creare un set di raccolta Traccia SQL
title: Creare un set di raccolta Traccia SQL con Profiler
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL Trace collector set
ms.assetid: b6941dc0-50f5-475d-82eb-ce7c68117489
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 4b2f20965b3bdff081368c2e1fcaa2bc255aca47
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250096"
---
# <a name="use-sql-server-profiler-to-create-a-sql-trace-collection-set"></a>Utilizzare SQL Server Profiler per creare un set di raccolta Traccia SQL
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] è possibile sfruttare le funzionalità di traccia sul lato server di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per esportare una definizione della traccia da usare per creare un set di raccolta che usa il tipo di agente di raccolta Traccia SQL generico. Questo processo è costituito da due operazioni:  
  
1.  Creazione ed esportazione di una traccia di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
2.  Creazione di uno script per un nuovo set di raccolta basato su una traccia esportata.  

 Lo scenario per le procedure seguenti comporta la raccolta di dati su tutte le stored procedure il cui completamento richiede almeno 80 millisecondi. Per completare queste procedure è necessario essere in grado di effettuare le operazioni seguenti:  
  
-   Utilizzare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per creare e configurare una traccia.  
  
-   Utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per aprire, modificare ed eseguire una query.  
  
### <a name="create-and-export-a-sql-server-profiler-trace"></a>Creazione ed esportazione di una traccia di SQL Server Profiler  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]aprire [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Scegliere **SQL Server Profiler** dal menu **Strumenti**.  
  
2.  Nella finestra di dialogo **Connetti al server** fare clic su **Annulla**.  
  
3.  Per questo scenario, assicurarsi che i valori di durata siano configurati per essere visualizzati in millisecondi (impostazione predefinita). A tale scopo, attenersi alla seguente procedura:  
  
    1.  Scegliere **Opzioni** dal menu **Strumenti**.  
  
    2.  Nell'area **Opzioni di visualizzazione** assicurarsi che la casella di controllo Mostra i valori nella colonna Durata in microsecondi sia deselezionata.  
  
    3.  Fare clic su **OK** per chiudere la finestra di dialogo **Opzioni generali** .  
  
4.  Scegliere **Nuova traccia** dal menu **File**.  
  
5.  Nella finestra di dialogo **Connetti al server** selezionare il server con cui stabilire la connessione, quindi fare clic su **Connetti**.  
  
     Verrà visualizzata la finestra di dialogo **Proprietà traccia** .  
  
6.  Nella scheda **Generale** eseguire le operazioni seguenti:  
  
    1.  Nella casella **Nome traccia** digitare il nome da usare per la traccia. Per questo esempio, il nome della traccia è **SPgt80**.  
  
    2.  Nell'elenco **Modello** selezionare il modello da usare per la traccia. Per questo esempio, fare clic su **TSQL_SPs**.  
  
7.  Nella scheda **Selezione eventi** eseguire queste operazioni:  
  
    1.  Identificare gli eventi da utilizzare per la traccia. Per questo esempio, deselezionare tutte le caselle di controllo nella colonna **Eventi** , tranne **ExistingConnection** e **SP:Completed**.  
  
    2.  Nell'angolo in basso a destra selezionare la casella di controllo **Mostra tutte le colonne** .  
  
    3.  Fare clic sulla riga **SP:Completed** .  
  
    4.  Scorrere la riga fino alla colonna **Durata** , quindi selezionare la casella di controllo **Durata** .  
  
8.  Nell'angolo in basso a destra fare clic su **Filtri colonne** per aprire la finestra di dialogo **Modifica filtro** . Nella finestra di dialogo **Modifica filtro** eseguire queste operazioni:  
  
    1.  Nell'elenco di filtri fare clic su **Durata**.  
  
    2.  Nella finestra di operatori booleani espandere il nodo **Maggiore o uguale a** , digitare **80** come valore, quindi fare clic su **OK**.  
  
9. Fare clic su **Esegui** per avviare la traccia.  
  
10. Sulla barra degli strumenti fare clic su **Arresta traccia selezionata** o **Sospendi traccia selezionata**.  
  
11. Scegliere **Esporta** dal menu **File**, fare clic su **Crea script per definizione traccia**, quindi su **Per set di raccolta Traccia SQL**.  
  
12. Nella casella **Nome file** della finestra di dialogo **Salva con nome** digitare il nome da usare per la definizione della traccia, quindi salvarlo nel percorso desiderato. Per questo esempio, il nome del file è identico al nome della traccia, ovvero SPgt80.  
  
13. Fare clic su **OK** quando si riceve un messaggio indicante che il file è stato salvato correttamente, quindi chiudere [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="script-a-new-collection-set-from-a-sql-server-profiler-trace"></a>Creazione di uno script per un nuovo set di raccolta da una traccia di SQL Server Profiler  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]scegliere **Apri** dal menu **File** , quindi fare clic su **File**.  
  
2.  Nella finestra di dialogo **Apri file** trovare e aprire il file creato nella procedura precedente (SPgt80).  
  
     Le informazioni sulla traccia salvate in precedenza verranno aperte in una finestra di query e unite in uno script che è possibile eseguire per creare il nuovo set di raccolta.  
  
3.  Scorrere lo script ed eseguire le sostituzioni seguenti, indicate nel testo di commento dello script:  
  
    -   Sostituire **SQLTrace Collection Set Name Here** con il nome che si vuole usare per il set di raccolta. Per questo esempio, assegnare al set di raccolta il nome **SPROC_CollectionSet**.  
  
    -   Sostituire **SQLTrace Collection Item Name Here** con il nome che si vuole usare per l'elemento della raccolta. Per questo esempio, assegnare all'elemento della raccolta il nome **SPROC_Collection_Item**.  
  
4.  Fare clic su **Esegui** per eseguire la query e creare il set di raccolta.  
  
5.  In Esplora oggetti verificare che il set di raccolta sia stato creato. A tale scopo, attenersi alla seguente procedura:  
  
    1.  Fare clic con il pulsante destro del mouse su **Gestione**, quindi scegliere **Aggiorna**.  
  
    2.  Espandere **Gestione**, quindi **Raccolta dati**.  
  
     Il set di raccolta **SPROC_CollectionSet** viene visualizzato allo stesso livello del nodo **Set di raccolta dati di sistema** . Per impostazione predefinita, il set di raccolta è disabilitato.  
  
6.  Utilizzare Esplora oggetti per modificare le proprietà di SPROC_CollectionSet, ad esempio la modalità di raccolta e la pianificazione di caricamento. Seguire le stesse procedure utilizzate per i set di raccolta dati di sistema forniti con l'agente di raccolta dati.  
  
## <a name="example"></a>Esempio  
 L'esempio di codice seguente rappresenta lo script finale risultante dai passaggi descritti nelle procedure precedenti.  
  
```sql
/*************************************************************/  
-- SQL Trace collection set generated from SQL Server Profiler  
-- Date: 11/19/2007  12:55:31 AM  
/*************************************************************/  
  
USE msdb;
GO  
  
BEGIN TRANSACTION  
BEGIN TRY  
  
-- Define collection set  
-- **_  
-- _*_ Replace 'SqlTrace Collection Set Name Here' in the   
-- _*_ following script with the name you want  
-- _*_ to use for the collection set.  
-- _*_  
DECLARE @collection_set_id int;  
EXEC [dbo].[sp_syscollector_create_collection_set]  
    @name = N'SPROC_CollectionSet',  
    @schedule_name = N'CollectorSchedule_Every_15min',  
    @collection_mode = 0, -- cached mode needed for Trace collections  
    @logging_level = 0, -- minimum logging  
    @days_until_expiration = 5,  
    @description = N'Collection set generated by SQL Server Profiler',  
    @collection_set_id = @collection_set_id output;  
SELECT @collection_set_id;  
  
-- Define input and output variables for the collection item.  
DECLARE @trace_definition xml;  
DECLARE @collection_item_id int;  
  
-- Define the trace parameters as an XML variable  
SELECT @trace_definition = convert(xml,   
N'<ns:SqlTraceCollector xmlns:ns"DataCollectorType" use_default="0">  
<Events>  
  <EventType name="Sessions">  
    <Event id="17" name="ExistingConnection" columnslist="1,2,14,26,3,35,12" />  
  </EventType>  
  <EventType name="Stored Procedures">  
    <Event id="43" name="SP:Completed" columnslist="1,2,26,34,3,35,12,13,14,22" />  
  </EventType>  
</Events>  
<Filters>  
  <Filter columnid="13" columnname="Duration" logical_operator="AND" comparison_operator="GE" value="80000L" />  
</Filters>  
</ns:SqlTraceCollector>  
');  
  
-- Retrieve the collector type GUID for the trace collector type.  
DECLARE @collector_type_GUID uniqueidentifier;  
SELECT @collector_type_GUID = collector_type_uid
  FROM [dbo].[syscollector_collector_types]
  WHERE name = N'Generic SQL Trace Collector Type';  
  
-- Create the trace collection item.  
-- _*_  
-- _*_ Replace 'SqlTrace Collection Item Name Here' in   
-- _*_ the following script with the name you want to  
-- _*_ use for the collection item.  
-- _**  
EXEC [dbo].[sp_syscollector_create_collection_item]  
   @collection_set_id = @collection_set_id,  
   @collector_type_uid = @collector_type_GUID,  
   @name = N'SPROC_Collection_Item',  
   @frequency = 900, -- specified the frequency for checking to see if trace is still running  
   @parameters = @trace_definition,  
   @collection_item_id = @collection_item_id output;  
SELECT @collection_item_id;  
  
COMMIT TRANSACTION;  
END TRY  
  
BEGIN CATCH  
ROLLBACK TRANSACTION;  
DECLARE @ErrorMessage nvarchar(4000);  
DECLARE @ErrorSeverity int;  
DECLARE @ErrorState int;  
DECLARE @ErrorNumber int;  
DECLARE @ErrorLine int;  
DECLARE @ErrorProcedure nvarchar(200);  
SELECT @ErrorLine = ERROR_LINE(),  
       @ErrorSeverity = ERROR_SEVERITY(),  
       @ErrorState = ERROR_STATE(),  
       @ErrorNumber = ERROR_NUMBER(),  
       @ErrorMessage = ERROR_MESSAGE(),  
       @ErrorProcedure = ISNULL(ERROR_PROCEDURE(), '-');  
RAISERROR (14684, @ErrorSeverity, 1 , @ErrorNumber,
  @ErrorSeverity, @ErrorState, @ErrorProcedure, @ErrorLine, @ErrorMessage);  
END CATCH;  
GO  
```  
  
  

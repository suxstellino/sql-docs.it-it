---
description: Novità&#39 di Integration Services in SQL Server 2016
title: Novità&#39 di Integration Services in SQL Server 2016 | Microsoft Docs
ms.custom:
- SQL2016_New_Updated
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, what's new
- what's new [Integration Services]
ms.assetid: da6999c7-e5e3-4a59-a284-1da635995af1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa2736f06ad57ec6fca8ff0af8a07625265e296a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237260"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2016"></a>Novità&#39 di Integration Services in SQL Server 2016

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Questo argomento descrive le funzionalità che sono state aggiunte o aggiornate in SQL Server 2016 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Contiene anche le funzionalità aggiunte o aggiornate nel [Feature Pack di Azure per Integration Services & #40; SSIS & #41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md) durante l'intervallo di tempo di SQL Server 2016.  

## <a name="new-for-ssis-in-azure-data-factory"></a>Novità di SSIS in Azure Data Factory

Con l'anteprima pubblica di Azure Data Factory versione 2 del mese di settembre 2017, è ora possibile eseguire le operazioni seguenti:
-   Distribuire i pacchetti al database del catalogo SSIS (SSISDB) nel database SQL di Azure.
-   Eseguire i pacchetti distribuiti in Azure in SSIS Integration Runtime di Azure, un componente di Azure Data Factory versione 2.

Per altre informazioni, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Per queste nuove funzionalità è necessario SQL Server Data Tools (SSDT) versione 17.2 o successiva. Non è invece richiesto SQL Server 2017 o SQL Server 2016. Quando i pacchetti vengono distribuiti in Azure, la distribuzione guidata aggiorna sempre i pacchetti al formato del pacchetto più recente.

## <a name="2016-improvements-by-category"></a>Miglioramenti 2016 raggruppati per categoria  
  
-   **Gestione**  
  
    -   Migliore distribuzione  
  
        -   [Aggiornamento guidato SSISDB](#ssisdbupgrwiz)  
  
        -   [Supporto per Always On nel catalogo SSIS](#AlwaysOn)  
  
        -   [Distribuzione di pacchetti incrementale](#IncrementalDeployment)  
  
        -   [Supporto per Crittografia sempre attiva nel Catalogo SSIS](#encrypted)  
  
    -   Debug migliore  
  
        -   [Nuovo ruolo a livello di database ssis_logreader nel catalogo SSIS](#LogReader)  
  
        -   [Nuovo livello di registrazione RuntimeLineage nel catalogo SSIS](#RuntimeLineage)  
  
        -   [Nuovo livello di registrazione personalizzato nel catalogo SSIS](#CustomLogging)  
  
        -   [Nomi di colonna per gli errori nel flusso di dati](#ErrorColumn)  
  
        -   [Supporto esteso per i nomi delle colonne errore](#getidstring)  
  
        -   [Supporto per il livello di registrazione predefinito per l'intero server](#ServerLogLevel)  
  
        -   [Nuova interfaccia IDTSComponentMetaData130 nell'API](#CMD130)  
  
    -   Migliore gestione dei pacchetti  
  
        -   [Migliore esperienza per l'aggiornamento di progetto](#ProjectUpgrade)  
  
        -   [La proprietà AutoAdjustBufferSize calcola automaticamente le dimensioni del buffer per il flusso di dati](#BufferSize)  
  
        -   [Modelli del flusso di controllo riutilizzabili](#Templates)  
  
        -   [Nuovi modelli rinominati come parti](#Parts)  
  
-   **Connettività**  
  
    -   Connettività estesa in locale  
  
        -   [Supporto per le origini di dati OData v4](#ODatav4)  
  
        -   [Supporto esplicito per le origini dati Excel 2013](#Excel2013)  
  
        -   [Supporto per il file system Hadoop (HDFS)](#HDFS)  
  
        -   [Supporto esteso per Hadoop e HDFS](#more_hadoop)  
  
        -   [La destinazione file HDFS ora supporta il formato file ORC](#hdfsORC)  
  
        -   [Componenti ODBC aggiornati per SQL Server 2016](#odbc2016)  
  
        -   [Supporto esplicito per le origini dati di Excel 2016](#Excel2016)  
  
        -   [Rilasciato Connector for SAP BW per SQL Server 2016](#SAPBW)
        
        -   [Rilasciato Connector v4.0 per Oracle e Teradata](#oracleteradata)
        
        -   [Rilasciati i connettori per l'appliance del sistema della piattaforma (PDW), aggiornamento 5](#pdwau5)
  
    -   Connettività estesa al cloud  
  
        -   Connettori di archiviazione Azure e attività Hive e Pig per HDInsight - [Azure Feature Pack per SSIS rilasciato per SQL Server 2016](#AFP2016)
        
        -   [Supporto per le risorse online di Microsoft Dynamics rilasciato nel Service Pack 1](#dynamics)
        
        -   [Supporto per la versione rilasciata di Azure Data Lake Store](#datalakestore)
        
        -   [Supporto per la versione rilasciata di Azure Synapse Analytics](#sqldwupload)
  
-   **Usabilità e produttività**  
  
    -   Migliore esperienza di installazione  
  
        -   [Aggiornamento bloccato quando SSISDB appartiene a un gruppo di disponibilità](#Upgrade)  
  
    -   Migliore esperienza di progettazione  
  
        -   [Progettazione SSIS crea e gestisce i pacchetti per SQL Server 2016, 2014 o 2012](#OneDesigner)  
  
        -   Numerosi miglioramenti e correzioni di bug della finestra di progettazione  
  
    -   Migliore esperienza di gestione in SQL Server Management Studio
  
        -   [Miglioramento delle prestazioni per le viste del catalogo SSIS](#CatViews)  
  
    -   Altri miglioramenti  
  
        -   [Trasformazione del database di distribuzione di dati bilanciati fa ora parte di SSIS](#BDDinbox)  
  
        -   [I componenti di pubblicazione dei feed di dati fanno ora parte di SSIS](#ComplexFeedinbox)  
  
        -   [Supporto per Archiviazione BLOB di Azure nell'Importazione/Esportazione guidata SQL Server](#AzureBlob)  
  
        -   [Rilascio di Change Data Capture Designer and Service per Oracle per Microsoft SQL Server 2016](#CDCOracle)  
  
        -   [Componenti CDC aggiornati per SQL Server 2016](#cdc2016)  
  
        -   [Attività Esegui DDL Analysis Services aggiornata](#ASDDL)  
  
        -   [Le attività di Analysis Services supportano modelli tabulari](#ssasrc0)  
  
        -   [Supporto per il client R Services predefinito](#builtinR)  
  
        -   [Output di convalida XML avanzato in Attività XML](#ValidateXML)  
  
## <a name="manageability"></a>Gestione  

### <a name="better-deployment"></a>Migliore distribuzione

####  <a name="ssisdb-upgrade-wizard"></a><a name="ssisdbupgrwiz"></a> Aggiornamento guidato del database SSISDB  
 Eseguire Aggiornamento guidato del database SSISDB per aggiornare il database di catalogo SSIS, SSISDB, quando il database è antecedente alla versione corrente dell'istanza di SQL Server. Ciò si verifica quando viene soddisfatta una delle condizioni seguenti.  
  
-   Il database è stato ripristinato da una versione precedente di SQL Server.  
  
-   Il database non è stato rimosso da un gruppo di disponibilità AlwaysOn prima di aggiornare l'istanza di SQL Server. In questo modo si impedisce l'aggiornamento automatico del database. Per ulteriori informazioni, vedere [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  
  
 Per altre informazioni, vedere [SSIS Catalog &#40;SSISDB&#41;](../integration-services/catalog/ssis-catalog.md) (Catalogo SSIS &#40;SSISDB&#41;). 

####  <a name="support-for-always-on-in-the-ssis-catalog"></a><a name="AlwaysOn"></a> Supporto per Always On nel catalogo SSIS  
 I gruppi di disponibilità Always On sono una soluzione di disponibilità elevata e recupero di emergenza che offre un'alternativa di livello enterprise al mirroring del database. Un gruppo di disponibilità supporta un ambiente di failover per un set discreto di database utente, noti come database di disponibilità, su cui si verifica il failover. Per altre informazioni, vedere [Gruppi di disponibilità Always On](../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 In SQL Server 2016 SSIS introduce nuove funzionalità che consentono di distribuire facilmente a un catalogo SSIS centralizzato (ad esempio, il database utente SSISDB). Per garantire una disponibilità elevata per il database SSISDB e il relativo contenuto (progetti, pacchetti, log di esecuzione e così via) è possibile aggiungere il database SSISDB a un gruppo di disponibilità AlwaysOn, esattamente come qualsiasi altro database utente. Quando si verifica un failover, uno dei nodi secondari diventa automaticamente il nuovo nodo primario.  
  
 Per una panoramica e istruzioni dettagliate sull'abilitazione di Always On per SSISDB, vedere [SSIS Catalog](../integration-services/catalog/ssis-catalog.md) (Catalogo SSIS).  

####  <a name="incremental-package-deployment"></a><a name="IncrementalDeployment"></a> Distribuzione di pacchetti incrementale  
La funzionalità di distribuzione dei pacchetti incrementale consente di distribuire uno o più pacchetti in un progetto nuovo o esistente senza distribuire l'intero progetto. È possibile distribuire i pacchetti in modo incrementale usando gli strumenti seguenti.  
  
-   Distribuzione guidata  
  
-   SQL Server Management Studio (che usa la Distribuzione guidata)  
  
-   SQL Server Data Tools (Visual Studio; anch'essi usano la Distribuzione guidata)  
  
-   Stored procedure  
  
-   API del modello a oggetti di gestione (MOM, Management Object Model)  
  
 Per altre informazioni, vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  

####  <a name="support-for-always-encrypted-in-the-ssis-catalog"></a><a name="encrypted"></a> Supporto per Crittografia sempre attiva nel catalogo SSIS  
 SSIS supporta già la funzionalità Crittografia sempre attiva in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per altre informazioni, vedere i post di blog seguenti.  
  
-   [SSIS con Crittografia sempre attiva](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-always-encrypted/ba-p/388272)  
  
-   [Trasformazione Ricerca con Crittografia sempre attiva](https://techcommunity.microsoft.com/t5/sql-server-integration-services/lookup-transformation-with-always-encrypted/ba-p/388282)  

### <a name="better-debugging"></a>Debug migliore

####  <a name="new-ssis_logreader-database-level-role-in-the-ssis-catalog"></a><a name="LogReader"></a> Nuovo ruolo a livello di database ssis_logreader nel catalogo SSIS  
 Nelle versioni precedenti del catalogo SSIS, solo gli utenti nel ruolo **ssis_admin** possono accedere alle viste che contengono l'output di registrazione. È ora disponibile un nuovo ruolo a livello di database **ssis_logreader** che è possibile usare per concedere le autorizzazioni per accedere alle viste che contengono l'output di registrazione agli utenti che non sono amministratori.  
  
 È anche disponibile un nuovo ruolo **ssis_monitor** . Questo ruolo supporta Always On ed è per uso interno solo da parte del catalogo SSIS.  

####  <a name="new-runtimelineage-logging-level-in-the-ssis-catalog"></a><a name="RuntimeLineage"></a> Nuovo livello di registrazione RuntimeLineage nel catalogo SSIS  
 Il nuovo livello di registrazione **RuntimeLineage** nel catalogo SSIS raccoglie i dati necessari a tenere traccia delle informazioni di derivazione nel flusso di dati. È possibile analizzare queste informazioni di derivazione per mappare la relazione di derivazione tra attività. Gli ISV e sviluppatori possono creare strumenti personalizzati di mapping della derivazione con queste informazioni. 

####  <a name="new-custom-logging-level-in-the-ssis-catalog"></a><a name="CustomLogging"></a> Nuovo livello di registrazione personalizzato nel catalogo SSIS  
 Nelle versioni precedenti del catalogo SSIS era possibile scegliere tra quattro livelli di registrazione predefiniti quando si eseguiva un pacchetto: **Nessuno, Base, Prestazioni o Dettagliato**. SQL Server 2016 aggiunge il livello di registrazione **RuntimeLineage**. È ora possibile anche creare e salvare più livelli di registrazione personalizzati nel catalogo SSIS e selezionare il livello di registrazione da usare ogni volta che si esegue un pacchetto. Per ogni livello di registrazione personalizzato, selezionare solo le statistiche e gli eventi da acquisire. Includere facoltativamente il contesto dell'evento per visualizzare i valori delle variabili, le stringhe di connessione e le proprietà dell'attività. Per ulteriori informazioni, vedere [Abilitare la registrazione per l'esecuzione di pacchetti nel server SSIS](../integration-services/performance/integration-services-ssis-logging.md#server_logging). 

####  <a name="column-names-for-errors-in-the-data-flow"></a><a name="ErrorColumn"></a> Nomi di colonna per gli errori nel flusso di dati  
 Quando si reindirizza le righe nel flusso di dati contenenti errori a un output degli errori, l'output contiene un identificatore numerico per la colonna in cui si è verificato l'errore, ma non viene visualizzato il nome della colonna. Esistono diversi modi per trovare o visualizzare il nome della colonna in cui si è verificato l'errore.  
  
-   Quando si configura la registrazione, selezionare l'evento **DiagnosticEx** per la registrazione. Questo evento scrive una mappa delle colonne del flusso di dati nel log. È quindi possibile cercare il nome della colonna in questa mappa usando l'identificatore della colonna acquisito da un output degli errori. Per altre informazioni, vedere [Gestione degli errori nei dati](../integration-services/data-flow/error-handling-in-data.md).  
  
-   Nell'Editor avanzato, è possibile visualizzare il nome della colonna per la colonna a monte quando si visualizzano le proprietà di una colonna di input o di output di un componente flusso di dati.  
  
-   Per visualizzare i nomi delle colonne in cui si è verificato l'errore, collegare un visualizzatore dati a un output degli errori.  Il visualizzatore dati ora mostra sia la descrizione dell'errore sia il nome della colonna in cui si è verificato l'errore.  
  
-   Nel componente script o in un componente flusso di dati personalizzato, chiamare il nuovo metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> dell'interfaccia IDTSComponentMetadata100.  
  
 Per altre informazioni su questo miglioramento, vedere il post del blog scritto da Bo Fan, sviluppatore di SSIS, sui [miglioramenti della colonna errori per il flusso di dati SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)  
  
> [!NOTE]  
>  (questo supporto è stato esteso alle versioni successive. Per altre informazioni, vedere [Supporto esteso per i nomi delle colonne errore](#getidstring) e [Nuova interfaccia IDTSComponentMetaData130 nell'API](#CMD130)).  

####  <a name="expanded-support-for-error-column-names"></a><a name="getidstring"></a> Supporto esteso per i nomi delle colonne errore  
 L'evento **DiagnosticEx** ora registra informazioni sulla colonna per tutte le colonne di input e di output, non solo per le colonne di derivazione. Di conseguenza, l'output è ora definito come mappa delle colonne pipeline anziché mappa di derivazione della pipeline.  
  
 Il metodo GetIdentificationStringByLineageID è stato rinominato in <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A>. Per ulteriori informazioni, vedere [Nomi di colonna per gli errori nel flusso di dati](#ErrorColumn).  
  
 Per altre informazioni su questa modifica e sui miglioramenti apportati alla colonna errore, vedere il seguente post di blog aggiornato. [Miglioramenti della colonna errore per il flusso di dati SSIS (aggiornato per la versione CTP 3.3)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)  
  
> [!NOTE]  
>  (in RC0, questo metodo è stato spostato nella nuova interfaccia di <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> . Per altre informazioni, vedere [Nuova interfaccia IDTSComponentMetaData130 nell'API](#CMD130)).  

####  <a name="support-for-server-wide-default-logging-level"></a><a name="ServerLogLevel"></a> Supporto per il livello di registrazione predefinito per l'intero server  
 In **Proprietà server** di SQL Server, nella proprietà **Livello di registrazione** del server, è ora possibile selezionare un livello di registrazione predefinito per l'intero server. È possibile scegliere uno dei livelli di registrazione predefinita (base, nessuno, dettagliato, prestazioni o RuntimeLineage) oppure è possibile selezionare un livello di registrazione personalizzato esistente. Il livello di registrazione selezionato viene applicato a tutti i pacchetti distribuiti nel catalogo SSIS. Si applica anche per impostazione predefinita a un passaggio del processo di SQL Agent che esegue un pacchetto SSIS.  

####  <a name="new-idtscomponentmetadata130-interface-in-the-api"></a><a name="CMD130"></a> Nuova interfaccia IDTSComponentMetaData130 nell'API  
 Il nuovo livello di registrazione <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> aggiunge nuove funzionalità in SQL Server 2016 all'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> esistente, in particolare il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> . (Il metodo **GetIdentificationStringByID** viene spostato nella nuova interfaccia dall'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> ). Sono inoltre disponibili le nuove interfacce <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn130> e <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn130> , che offrono entrambe la proprietà **LineageIdentificationString** . Per ulteriori informazioni, vedere [Nomi di colonna per gli errori nel flusso di dati](#ErrorColumn).  

### <a name="better-package-management"></a>Migliore gestione dei pacchetti

####  <a name="improved-experience-for-project-upgrade"></a><a name="ProjectUpgrade"></a> Migliore esperienza per l'aggiornamento di progetto  
 Quando si aggiornano i progetti SSIS da versioni precedenti alla versione corrente, le gestioni connessioni a livello di progetto continuano a funzionare come previsto e il layout del pacchetto e le annotazioni vengono mantenute.  

####  <a name="autoadjustbuffersize-property-automatically-calculates-buffer-size-for-data-flow"></a><a name="BufferSize"></a> La proprietà AutoAdjustBufferSize calcola automaticamente le dimensioni del buffer per il flusso di dati  
 Quando si imposta il valore della nuova proprietà **AutoAdjustBufferSize** su **true**, il motore flusso di dati calcola automaticamente le dimensioni del buffer del flusso di dati. Per ulteriori informazioni, vedere [Data Flow Performance Features](../integration-services/data-flow/data-flow-performance-features.md).  

####  <a name="reusable-control-flow-templates"></a><a name="Templates"></a> Modelli del flusso di controllo riutilizzabili  
 Salvare un'attività o un contenitore di flusso di controllo di uso comune in un file di modello autonomo e riusarla più volte in uno o più pacchetti in un progetto usando i modelli di flusso di controllo. Questa riusabilità semplifica la progettazione e la manutenzione dei pacchetti SSIS. Per altre informazioni, vedere [Riusare il flusso di controllo dei pacchetti tramite le parti del pacchetto del flusso di controllo](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

####  <a name="new-templates-renamed-as-parts"></a><a name="Parts"></a> Nuovi modelli rinominati come parti  
 I nuovi modelli di flusso di controllo riutilizzabili rilasciati nella versione CTP 3.0 sono stati rinominati come parti del flusso di controllo o parti del pacchetto. Per altre informazioni su questa funzionalità, vedere [Riusare il flusso di controllo dei pacchetti tramite le parti del pacchetto del flusso di controllo](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

## <a name="connectivity"></a>Connettività  

### <a name="expanded-connectivity-on-premises"></a>Connettività estesa in locale

####  <a name="support-for-odata-v4-data-sources"></a><a name="ODatav4"></a> Supporto per le origini dati OData v4  
 Origine OData e Gestione connessione OData ora supportano i protocolli v3 e v4 di OData.  
  
-   Per il protocollo OData V3, il componente supporta i formati di dati ATOM e JSON.  
  
-   Per il protocollo OData V4, il componente supporta il formato dati JSON.  
  
 Per ulteriori informazioni, vedere [OData Source](../integration-services/data-flow/odata-source.md).  

####  <a name="explicit-support-for-excel-2013-data-sources"></a><a name="Excel2013"></a> Supporto esplicito per le origini dati Excel 2013  
 Gestione connessione Excel, Origine Excel, Destinazione Excel e Importazione/Esportazione guidata SQL Server ora offrono il supporto esplicito per le origini dati di Excel 2013. 

####  <a name="support-for-the-hadoop-file-system-hdfs"></a><a name="HDFS"></a> Supporto per il file system Hadoop (HDFS)  
 Il supporto per HDFS contiene le gestioni connessioni per connettersi ai cluster e alle attività Hadoop per eseguire operazioni comuni di HDFS. Per altre informazioni, vedere [Supporto di Hadoop e HDFS in Integration Services &#40;SSIS&#41;](../integration-services/hadoop-and-hdfs-support-in-integration-services-ssis.md).  

####  <a name="expanded-support-for-hadoop-and-hdfs"></a><a name="more_hadoop"></a> Supporto esteso per Hadoop e HDFS  
  
-   La gestione connessione Hadoop Il connettore Hadoop ora supporta l'autenticazione Kerberos e di base. Per ulteriori informazioni, vedere [Hadoop Connection Manager](../integration-services/connection-manager/hadoop-connection-manager.md).  
  
-   L'origine e la destinazione file HDFS ora supportano i formati Testo e Avro. Per altre informazioni, vedere  [HDFS File Source](../integration-services/data-flow/hdfs-file-source.md) e  [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  
  
-   L'attività File system di Hadoop ora supporta l'opzione CopyWithinHadoop oltre alle opzioni CopyToHadoop e CopyFromHadoop. Per ulteriori informazioni, vedere [Hadoop File System Task](../integration-services/control-flow/hadoop-file-system-task.md).  

####  <a name="hdfs-file-destination-now-supports-orc-file-format"></a><a name="hdfsORC"></a> La destinazione file HDFS ora supporta il formato file ORC  
 La destinazione file HDFS ora supporta il formato file ORC oltre a Testo e Avro. (l'origine file HDFS supporta solo testo e Avro). Per altre informazioni su questo componente, vedere [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  

####  <a name="odbc-components-updated-for-sql-server-2016"></a><a name="odbc2016"></a> Componenti ODBC aggiornati per SQL Server 2016  
 I componenti di origine e destinazione ODBC sono stati aggiornati in modo da offrire la completa compatibilità con SQL Server 2016. Non vi è alcuna nuova funzionalità e non sono state apportate modifiche al comportamento.  

####  <a name="explicit-support-for-excel-2016-data-sources"></a><a name="Excel2016"></a> Supporto esplicito per le origini dati di Excel 2016  
 Gestione connessione Excel, Origine Excel e Destinazione Excel ora offrono il supporto esplicito per le origini dati di Excel 2016.  

####  <a name="connector-for-sap-bw-for-sql-server-2016-released"></a><a name="SAPBW"></a> Rilasciato Connector for SAP BW per SQL Server 2016  
 MicrosoftÂ® Connector per SAP BW per Microsoft SQL ServerÂ® 2016 è stato rilasciato come parte di SQL Server 2016 Feature Pack. Per scaricare i componenti del Feature Pack, vedere[MicrosoftÂ® SQL ServerÂ® 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833).
 
#### <a name="connectors-v40-for-oracle-and-teradata-released"></a><a name="oracleteradata"></a> Rilasciato Connector v4.0 per Oracle e Teradata
Rilasciato Microsoft Connectors v4.0 per Oracle e Teradata. Per il download, vedere [Microsoft Connectors v4.0 for Oracle and Teradata](https://www.microsoft.com/download/details.aspx?id=52950)(Microsoft Connectors v4.0 per Oracle e Teradata).

### <a name="connectors-for-analytics-platform-system-pdw-appliance-update-5-released"></a><a name="pdwau5"></a> Rilasciati i connettori per l'appliance del sistema della piattaforma (PDW), aggiornamento 5
Sono stati rilasciati gli adattatori di destinazione per il caricamento dei dati in PDW con AU5. Per il download degli adattatori, vedere [Aggiornamento 5 della piattaforma di strumenti analitici Microsoft - Documentazione e strumenti client](https://www.microsoft.com/download/details.aspx?id=51610).

### <a name="expanded-connectivity-to-the-cloud"></a>Connettività estesa al cloud

####  <a name="azure-feature-pack-for-ssis-released-for-sql-server-2016"></a><a name="AFP2016"></a> Azure Feature Pack per SSIS rilasciato per SQL Server 2016  
 Il Feature Pack di Azure per Integration Services è stato rilasciato per SQL Server 2016. Il Feature Pack contiene le gestioni connessioni per connettersi alle origini dati e alle attività di Azure per eseguire operazioni comuni di Azure. Per altre informazioni, vedere [Azure Feature Pack for Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

#### <a name="support-for-microsoft-dynamics-online-resources-released-in-service-pack-1"></a><a name="dynamics"></a> Supporto per le risorse online di Microsoft Dynamics rilasciato nel Service Pack 1

Con SQL Server 2016 Service Pack 1 installato, l'origine OData e la gestione connessione OData supportano ora la connessione ai feed OData di Microsoft Dynamics AX Online e Microsoft Dynamics CRM Online.

#### <a name="support-for-azure-data-lake-store-released"></a><a name="datalakestore"></a> Supporto per la versione rilasciata di Azure Data Lake Store

La versione più recente del Feature Pack di Azure include una gestione connessione, un'origine e una destinazione per spostare i dati da e verso Azure Data Lake Store. Per altre informazioni, vedere [Feature Pack di Integration Services &#40;SSIS&#41; per Azure](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

#### <a name="support-for-azure-synapse-analytics-released"></a><a name="sqldwupload"></a> Supporto per la versione rilasciata di Azure Synapse Analytics

La versione più recente del Feature Pack di Azure include l'attività di caricamento di Azure SQL DW per l'inserimento di dati in Azure Synapse Analytics. Per altre informazioni, vedere [Feature Pack di Integration Services &#40;SSIS&#41; per Azure](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

## <a name="usability-and-productivity"></a>Usabilità e produttività  
 
### <a name="better-install-experience"></a>Migliore esperienza di installazione

####  <a name="upgrade-blocked-when-ssisdb-belongs-to-an-availability-group"></a><a name="Upgrade"></a> Aggiornamento bloccato quando SSISDB appartiene a un gruppo di disponibilità  
 Se il database del catalogo SSIS (SSISDB) appartiene a un gruppo di disponibilità Always On, è necessario rimuovere il database SSISDB dal gruppo di disponibilità, eseguire l'aggiornamento di SQL Server e quindi aggiungere nuovamente SSISDB al gruppo di disponibilità. Per ulteriori informazioni, vedere [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  

### <a name="better-design-experience"></a>Migliore esperienza di progettazione

####  <a name="multi-targeting-and-multi-version-support-in-ssis-designer"></a><a name="OneDesigner"></a> Supporto di più destinazioni e versioni in Progettazione SSIS  
 È ora possibile usare Progettazione SSIS in SQL Server Data Tools (SSDT) per Visual Studio 2015 per creare, gestire ed eseguire pacchetti destinati a SQL Server 2016, SQL Server 2014 o SQL Server 2012. Per ottenere SSDT, vedere [Scaricare la versione più recente di SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md). 

 In Esplora soluzioni fare clic con il pulsante destro del mouse su un progetto di Integration Services e scegliere **Proprietà** per aprire le pagine delle proprietà per il progetto. Nella scheda **Generale** di **Proprietà di configurazione** selezionare la proprietà **TargetServerVersion** , quindi scegliere SQL Server 2016, SQL Server 2014 o SQL Server 2012.  
   
 ![Proprietà TargetServerVersion nella finestra di dialogo Proprietà progetto](../integration-services/media/targetserverversion2.png "Proprietà TargetServerVersion nella finestra di dialogo Proprietà progetto")  

> [!IMPORTANT]
> Se si sviluppano estensioni personalizzate di SSIS, vedere [Supporto multitargeting nei componenti personalizzati](../integration-services/extending-packages-custom-objects/support-multi-targeting-in-your-custom-components.md) e [Ottenere estensioni SSIS personalizzate supportate da più versioni di SSDT 2015 per SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/).  

### <a name="better-management-experience-in-sql-server-management-studio"></a>Migliore esperienza di gestione in SQL Server Management Studio

####  <a name="improved-performance-for-ssis-catalog-views"></a><a name="CatViews"></a> Miglioramento delle prestazioni per le viste del catalogo SSIS  
 La maggior parte delle viste del catalogo SSIS viene eseguita meglio da un utente che non è membro del ruolo ssis_admin.  

### <a name="other-enhancements"></a>Altri miglioramenti

####  <a name="balanced-data-distributor-transformation-is-now-part-of-ssis"></a><a name="BDDinbox"></a> Trasformazione del database di distribuzione di dati bilanciati fa ora parte di SSIS  
 Trasformazione del server di distribuzione di dati bilanciati, che richiedeva un download separato nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], viene ora installato quando si installa [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Per ulteriori informazioni, vedere [Balanced Data Distributor Transformation](../integration-services/data-flow/transformations/balanced-data-distributor-transformation.md).  
  
####  <a name="data-feed-publishing-components-are-now-part-of-ssis"></a><a name="ComplexFeedinbox"></a> I componenti di pubblicazione dei feed di dati fanno ora parte di SSIS  
 I componenti di pubblicazione dei feed di dati, che richiedevano un download separato nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vengono ora installati quando si installa [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Per ulteriori informazioni, vedere [Data Streaming Destination](../integration-services/data-flow/data-streaming-destination.md).  

####  <a name="support-for-azure-blob-storage-in-the-sql-server-import-and-export-wizard"></a><a name="AzureBlob"></a> Supporto per Archiviazione BLOB di Azure nell'Importazione/Esportazione guidata SQL Server  
 L'Importazione/Esportazione guidata SQL Server può ora importare dati da (e salvare dati in) Archiviazione BLOB Azure. Per altre informazioni, vedere [Scelta origine dati &#40;Importazione/Esportazione guidata SQL Server&#41;](../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md) e [Scelta destinazione &#40;SQL Importazione/Esportazione guidata SQL Server&#41;](../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md). 

####  <a name="change-data-capture-designer-and-service-for-oracle-for-microsoft-sql-server-2016-released"></a><a name="CDCOracle"></a> Rilascio di Change Data Capture Designer and Service per Oracle per Microsoft SQL Server 2016  
 MicrosoftÂ® Change Data Capture Designer and Service per Oracle di Attunity per Microsoft SQL ServerÂ® 2016 è stato rilasciato come parte di SQL Server 2016 Feature Pack.  Questi componenti ora supportano Oracle 12c nell'installazione classica (l'installazione multi-tenant non è supportata) Per scaricare i componenti del Feature Pack, vedere [MicrosoftÂ® SQL ServerÂ® 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833).  
  
####  <a name="cdc-components-updated-for-sql-server-2016"></a><a name="cdc2016"></a> Componenti CDC aggiornati per SQL Server 2016  
 I componenti Attività di controllo, Origine e Trasformazione Splitter di CDC (Change Data Capture) sono stati aggiornati in modo da offrire la completa compatibilità con SQL Server 2016. Non vi è alcuna nuova funzionalità e non sono state apportate modifiche al comportamento.  
  
####  <a name="analysis-services-execute-ddl-task-updated"></a><a name="ASDDL"></a> Attività Esegui DDL Analysis Services aggiornata  
 L'Attività Esegui DDL Analysis Services è stata aggiornata in modo da accettare i comandi del linguaggio di scripting del modello tabulare.

####  <a name="analysis-services-tasks-support-tabular-models"></a><a name="ssasrc0"></a> Le attività di Analysis Services supportano modelli tabulari  
 È ora possibile usare tutte le attività e le destinazioni SSIS che supportano SQL Server Analysis Services (SSAS) con i modelli tabulari di SQL Server 2016. Le attività SSIS sono state aggiornate in modo da rappresentare oggetti tabulari anziché oggetti multidimensionali. Ad esempio, quando si selezionano oggetti da elaborare, l'attività Elaborazione Analysis Services rileva automaticamente un modello tabulare e visualizza un elenco di oggetti tabulari anziché mostrare gruppi e dimensioni di misure. La destinazione elaborazione partizione ora mostra anche gli oggetti tabulari e supporta il push dei dati in una partizione.  
  
 La destinazione elaborazione dimensione non funziona per i modelli tabulari con il livello di compatibilità di SQL 2016.  L'attività Elaborazione Analysis Services e la destinazione elaborazione partizione sono tutto ciò che serve per l'elaborazione tabulare. 

####  <a name="support-for-built-in-r-services"></a><a name="builtinR"></a> Supporto per Servizi R predefinito  
 SSIS supporta già il client R Services predefinito in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. È possibile usare SSIS non solo per estrarre i dati e caricare l'output dell'analisi, ma per compilare, eseguire e periodicamente ripetere il training dei modelli R. Per altre informazioni, vedere il post di blog su come [rendere operativo il progetto di Machine Learning usando SSIS e R Services di SQL Server 2016](https://techcommunity.microsoft.com/t5/sql-server-integration-services/operationalize-your-machine-learning-project-using-sql-server/ba-p/388296). 

####  <a name="rich-xml-validation-output-in-the-xml-task"></a><a name="ValidateXML"></a> Output di convalida XML avanzato in Attività XML  
 È possibile convalidare documenti XML e ottenere output avanzato degli errori abilitando la proprietà **ValidationDetails** dell'Attività XML. Prima che fosse disponibile la proprietà **ValidationDetails** , la convalida XML da parte dell'Attività XML restituiva solo un risultato di tipo True o False, senza informazioni sugli errori o sulle rispettive posizioni. Attualmente, quando si imposta **ValidationDetails** su True, il file di output contiene informazioni dettagliate su ogni errore, inclusi il numero di riga e la posizione. È possibile usare questa informazione per comprendere, individuare e risolvere gli errori nei documenti XML. Per ulteriori informazioni, vedere [Validate XML with the XML Task](../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 [!INCLUDE[ssIS](../includes/ssis-md.md)] ha introdotto la proprietà **ValidationDetails** in [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Service Pack 2. Questa nuova proprietà non è stata annunciata o documentata in quel momento. La proprietà **ValidationDetails** è disponibile anche in [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] e [!INCLUDE[sssql15-md](../includes/sssql16-md.md)].   

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

## <a name="see-also"></a>Vedere anche  
 [Novità di SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)   
 [Edizioni e funzionalità supportate per SQL Server 2016](../sql-server/editions-and-components-of-sql-server-2016.md)
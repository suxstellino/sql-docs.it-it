---
description: Inserimento bulk - attività
title: Attività Inserimento bulk | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.bulkinserttask.f1
- sql13.dts.designer.bulkinserttask.connection.f1
- sql13.dts.designer.bulkinserttask.general.f1
- sql13.dts.designer.bulkinserttask.options.f1
helpviewer_keywords:
- Bulk Insert task
- copying data [Integration Services]
ms.assetid: c5166156-6b4c-4369-81ed-27c4ce7040ae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 739086916d6b7bdea630e8327aa0ede0fb899c87
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348977"
---
# <a name="bulk-insert-task"></a>Inserimento bulk - attività

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Inserimento bulk rappresenta un modo efficiente per copiare grandi quantità di dati in una tabella o in una vista di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si supponga, ad esempio, che nella propria società venga usato un mainframe per archiviare l'elenco prodotti, che include un milione di righe, ma che il sistema e-commerce dell'azienda usi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per popolare le pagine Web. È necessario aggiornare la tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dei prodotti durante la notte utilizzando l'elenco master dei prodotti del mainframe. A tale scopo è possibile salvare l'elenco prodotti in un file delimitato da tabulazione e utilizzare l'attività Inserimento bulk per copiare i dati direttamente nella tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Per garantire la massima velocità di copia, non è consentito eseguire trasformazioni durante lo spostamento dei dati dal file di origine alla tabella o alla vista.  
  
## <a name="usage-considerations"></a>Considerazioni sull'utilizzo  
 Prima di utilizzare l'attività Inserimento bulk, considerare gli aspetti seguenti:  
  
-   Con l'attività Inserimento bulk i dati possono essere trasferiti solo da un file di testo a una tabella o a una vista di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per usare l'attività Inserimento bulk per trasferire dati da altri sistemi di gestione di database (DBMS, Database Management System), è necessario esportare i dati dall'origine a un file di testo e quindi importarli dal file di testo a una tabella o vista di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   La destinazione deve essere una tabella o vista di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se nella tabella o vista di destinazione sono già presenti dati, durante l'esecuzione dell'attività Inserimento bulk i nuovi dati verranno aggiunti a quelli esistenti. Se si desidera sostituire i dati, prima dell'attività Inserimento bulk avviare un'attività Esegui SQL che esegue un'istruzione DELETE o TRUNCATE. Per altre informazioni, vedere [Attività Esegui SQL](../../integration-services/control-flow/execute-sql-task.md).  
  
-   Nell'oggetto attività Inserimento bulk è possibile utilizzare un file di formato. Se il file di formato è stato creato dall'utilità **bcp** , è possibile specificarne il percorso nell'attività Inserimento bulk. L'attività Inserimento bulk supporta file di formato sia XML che non XML. Per altre informazioni sui file di formato, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
-   I pacchetti che contengono attività Inserimento bulk possono essere eseguiti solo dai membri del ruolo predefinito del server sysadmin.  
  
## <a name="bulk-insert-task-with-transactions"></a>Attività Inserimento bulk con le transazioni  
 Se le dimensioni del batch non sono impostate, l'intera operazione di copia bulk verrà considerata come un'unica transazione. Se il valore delle dimensioni del batch è **0** , i dati verranno inseriti in un solo batch. Se le dimensioni del batch sono impostate, ogni batch rappresenterà una transazione di cui verrà eseguito il commit alla fine dell'esecuzione.  
  
 Il comportamento dell'attività Inserimento bulk in relazione alle transazioni varia a seconda che l'attività partecipi o meno alla transazione del pacchetto. Se l'attività Inserimento bulk non partecipa alla transazione del pacchetto, per ogni batch privo di errori verrà eseguito il commit prima di procedere con il batch successivo. Se l'attività Inserimento bulk partecipa alla transazione, al termine dell'attività i batch privi di errori rimarranno nella transazione e saranno soggetti alle operazioni di commit o di rollback del pacchetto.  
  
 In caso di errore nell'attività Inserimento bulk, i batch già caricati non verranno sottoposti automaticamente a rollback. Analogamente l'esito positivo dell'attività non comporta automaticamente l'esecuzione del commit per tali batch. Le operazioni di commit e di rollback vengono eseguite solo se le proprietà del flusso di lavoro o del pacchetto sono state impostate in questo senso.  
  
## <a name="source-and-destination"></a>Origine e destinazione  
 Quando si specifica il percorso del file di testo di origine, prendere in considerazione i fattori seguenti:  
  
-   Il server deve disporre delle autorizzazioni necessarie per accedere sia al file che al database di destinazione.  
  
-   Poiché l'attività Inserimento bulk viene eseguita dal server, tutti i file di formato utilizzati dall'attività devono trovarsi sul server.  
  
-   Il file di origine caricato dall'attività Inserimento bulk può trovarsi sullo stesso server del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui vengono inseriti i dati oppure su un server remoto. Se il file si trova in un server remoto, è necessario specificarne il percorso in formato UNC (Universal Naming Convention).  
  
## <a name="performance-optimization"></a>Ottimizzazione delle prestazioni  
 Per ottimizzare le prestazioni, prendere in considerazione i fattori seguenti:  
  
-   Se il file di testo si trova nello stesso computer del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui vengono inseriti i dati, l'operazione di copia avviene a una velocità superiore, in quanto i dati non devono essere trasferiti in rete.  
  
-   L'attività Inserimento bulk non registra le righe che determinano errori. Se è necessario acquisire tali informazioni, utilizzare gli output degli errori dei componenti flusso di dati per registrare in un file di eccezioni le righe che provocano errori.  
  
## <a name="custom-log-entries-available-on-the-bulk-insert-task"></a>Voci di log personalizzate disponibili nell'attività Inserimento bulk  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Inserimento bulk. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**BulkInsertTaskBegin**|Indica che l'inserimento bulk è iniziato.|  
|**BulkInsertTaskEnd**|Indica che l'inserimento bulk è terminato.|  
|**BulkInsertTaskInfos**|Offre informazioni descrittive sull'attività.|  
  
## <a name="bulk-insert-task-configuration"></a>Configurazione dell'attività Inserimento bulk  
 Per configurare l'attività Inserimento bulk, procedere nel modo seguente:  
  
-   Specificare la gestione connessione OLE DB da usare per la connessione al database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione e la tabella o la vista in cui inserire i dati. L'attività Inserimento bulk supporta solo le connessioni OLE DB per il database di destinazione.  
  
-   Specificare la gestione connessione file o file flat da utilizzare per accedere al file di origine. L'attività Inserimento bulk utilizza la gestione connessione solo per il percorso del file di origine. L'attività ignora le altre opzioni selezionate nell'editor gestione connessione.  
  
-   Definire il formato utilizzato dall'attività Inserimento bulk, utilizzando un file di formato o definendo i delimitatori di colonna e di riga dei dati di origine. Se si utilizza un file di formato, specificare la gestione connessione file da utilizzare per accedere a tale file.  
  
-   Specificare le azioni da eseguire nella tabella o nella vista di destinazione quando i dati vengono inseriti dall'attività. È possibile specificare se verificare i vincoli, abilitare IDENTITY_INSERT, mantenere i valori Null, attivare trigger o bloccare la tabella.  
  
-   Fornire informazioni sul batch di dati da inserire, ad esempio le dimensioni del batch, la prima e l'ultima riga del file da inserire, il numero massimo di errori oltre il quale arrestare l'inserimento delle righe e i nomi delle colonne che verranno ordinate.  
  
 Se l'attività Inserimento bulk utilizza una gestione connessione file flat per accedere al file di origine, invece del formato specificato nella gestione connessione file flat usa il formato specificato in un file di formato o i valori delle proprietà RowDelimiter e ColumnDelimiter dell'attività.  
  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per ulteriori informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per altre informazioni sull'impostazione di queste proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="programmatic-configuration-of-the-bulk-insert-task"></a>Configurazione a livello di codice dell'attività Inserimento bulk  
 Per ulteriori informazioni sull'impostazione di queste proprietà a livello di codice, fare clic sull'argomento seguente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>  
  
## <a name="related-tasks"></a>Attività correlate  
 [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   Articolo tecnico relativo alla possibile [visualizzazione dell'errore "Impossibile preparare l'attività Inserimento bulk SSIS per l'inserimento dei dati" nei sistemi con Controllo dell'account utente abilitato](https://go.microsoft.com/fwlink/?LinkId=233693)sul sito support.microsoft.com.  
  
-   Articolo tecnico relativo alla [guida alle prestazioni del caricamento dati](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100))sul sito msdn.microsoft.com.  
  
-   Articolo tecnico relativo all' [utilizzo di SQL Server Integration Services per il caricamento bulk dei dati](https://go.microsoft.com/fwlink/?LinkId=233701)sul sito Web simple-talk.com.  
  
## <a name="bulk-insert-task-editor-connection-page"></a>Editor attività Inserimento bulk (pagina Connessione)
  Usare la pagina **Connessione** della finestra di dialogo **Editor attività Inserimento bulk** per specificare l'origine e la destinazione dell'operazione di inserimento bulk e il formato da usare.  
  
 Per altre informazioni sulle operazioni di inserimento bulk, vedere [Attività Inserimento bulk](../../integration-services/control-flow/bulk-insert-task.md) e [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
### <a name="options"></a>Opzioni  
 **Connection**  
 Selezionare una gestione connessione OLE DB nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova connessione.  
  
 **Argomenti correlati:** [Gestione connessione OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **DestinationTable**  
 Consente di digitare il nome della tabella o della vista di destinazione o di selezionare una tabella o una vista nell'elenco.  
  
 **Formato**  
 Consente di selezionare l'origine del formato per l'inserimento bulk. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Usa file**|Consente di selezionare un file contenente la specifica di formato. Selezionando questa opzione viene visualizzata l'opzione dinamica **FormatFile**.|  
|**Specifica**|Consente di specificare il formato. Selezionando questa opzione vengono visualizzate le opzioni dinamiche **RowDelimiter** e **ColumnDelimiter**.|  
  
 **File**  
 Selezionare una gestione connessione file o file flat nell'elenco o fare clic su \<**New connection...**> per creare una nuova connessione.  
  
 Il percorso del file è relativo al Motore di database di SQL Server specificato nella gestione connessione per questa attività. Il file di testo deve essere accessibile dal Motore di database di SQL Server in un disco rigido locale sul server oppure tramite un'unità condivisa o di cui è stato eseguito il mapping a SQL Server. Non è possibile accedere al file tramite SSIS Runtime.  
  
 Se si accede al file di origine utilizzando una gestione connessione file flat, l'attività Inserimento bulk non utilizzerà il formato specificato nella gestione connessione file flat, ma userà il formato specificato in un file di formato o i valori delle proprietà RowDelimiter e ColumnDelimiter dell'attività.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Gestione connessione file flat](../../integration-services/connection-manager/flat-file-connection-manager.md) 
  
 **Aggiorna tabelle**  
 Consente di aggiornare l'elenco di tabelle e di viste.  
  
### <a name="format-dynamic-options"></a>Opzioni dinamiche di Format  
  
#### <a name="format--use-file"></a>Format = Usa file  
 **FormatFile**  
 Digitare il percorso del file di formato oppure fare clic sui puntini di sospensione **(...)** per trovare il file di formato.  
  
#### <a name="format--specify"></a>Format = Specifica  
 **RowDelimiter**  
 Consente di specificare il delimitatore di riga nel file di origine. Il valore predefinito è **{CR}{LF}** .  
  
 **ColumnDelimiter**  
 Consente di specificare il delimitatore di colonna nel file di origine. Il valore predefinito è **Tabulazione**.  
  
## <a name="bulk-insert-task-editor-general-page"></a>Editor attività Inserimento bulk (pagina Generale)
  Utilizzare la pagina **Generale** della finestra di dialogo **Editor attività Inserimento bulk** per assegnare un nome e una descrizione all'attività Inserimento bulk.  
  
### <a name="options"></a>Opzioni  
 **Nome**  
 Consente di specificare un nome univoco per l'attività Inserimento bulk. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività Inserimento bulk.  
 
## <a name="bulk-insert-task-editor-options-page"></a>Editor attività Inserimento bulk (pagina Opzioni)
  Utilizzare la pagina **Opzioni** della finestra di dialogo **Editor attività Inserimento bulk** per impostare le proprietà relative all'operazione di inserimento bulk. L'attività Inserimento bulk consente di copiare grandi quantità di dati all'interno di una vista o tabella di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per altre informazioni sull'uso degli inserimenti di massa, vedere [Attività Inserimento bulk](../../integration-services/control-flow/bulk-insert-task.md) e [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
### <a name="options"></a>Opzioni  
 **CodePage**  
 Consente di specificare la tabella codici dei dati contenuti nel file.  
  
 **DataFileType**  
 Consente di specificare il valore di tipo di dati da utilizzare nell'operazione di caricamento.  
  
 **BatchSize**  
 Consente di specificare il numero di righe di un batch. Il valore predefinito è l'intero file di dati. Se si imposta **BatchSize** su zero, i dati vengono caricati in un singolo batch.  
  
 **LastRow**  
 Consente di specificare l'ultima riga da copiare.  
  
 **FirstRow**  
 Consente di specificare la riga dalla quale iniziare la copia.  
  
 **Opzioni**  
 |Termine|Definizione|  
|----------|----------------|  
|**Vincoli CHECK**|Selezionare questa opzione per verificare i vincoli di colonna e tabella.|  
|**Mantieni valori Null**|Selezionare questa opzione per mantenere i valori Null durante l'operazione di inserimento bulk anziché inserire tutti i valori predefiniti per le colonne vuote.|  
|**Consenti IDENTITY_INSERT**|Selezionare questa opzione per inserire valori esistenti in una colonna Identity.|  
|**Blocco a livello di tabella**|Selezionare questa opzione per bloccare la tabella durante l'inserimento bulk.|  
|**Attive trigger**|Selezionare questa opzione per attivare tutti i trigger di eliminazione, aggiornamento o inserimento nella tabella.|  
  
 **SortedData**  
 Consente di specificare la clausola ORDER BY nell'istruzione di inserimento bulk. Il nome della colonna deve corrispondere a una colonna valida della tabella di destinazione. Il valore predefinito è **false**. Questo valore implica che i dati non vengono ordinati da una clausola ORDER BY.  
  
 **MaxErrors**  
 Consente di specificare il numero massimo di errori che possono verificarsi prima dell'annullamento dell'operazione di inserimento bulk. Un valore pari a 0 indica che è consentito un numero infinito di errori.  
  
> [!NOTE]  
>  Ogni riga che non è possibile importare tramite l'operazione di caricamento bulk viene considerata un errore.  

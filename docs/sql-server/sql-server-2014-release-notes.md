---
title: Note sulla versione di SQL Server 2014 | Microsoft Docs
description: Questo documento descrive i problemi noti di cui è necessario essere a conoscenza prima di installare o risolvere i problemi relativi alle versioni di Microsoft SQL Server 2014 (12.x).
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: bf4c4922-80b3-4be3-bf71-228247f97004
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 5881a0c5d3e4a9ce17a2f8ead0656ce549d41064
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081136"
---
# <a name="sql-server-2014-release-notes"></a>SQL Server 2014 Release Notes
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]
In questo articolo vengono descritti i problemi noti delle versioni di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], inclusi i relativi Service Pack.

## <a name="sql-server-2014-service-pack-2-sp2"></a>SQL Server 2014 Service Pack 2 (SP2)

SQL Server 2014 SP2 contiene rollup di correzioni rilasciate per SQL Server 2014 SP1 CU7. Include miglioramenti incentrati su prestazioni, scalabilità e diagnostica in base ai suggerimenti dei clienti e della community di SQL.

### <a name="performance-and-scalability-improvements-in-sp2"></a>Miglioramenti delle prestazioni e della scalabilità in SP2

|Funzionalità|Descrizione|Per ulteriori informazioni|
|---|---|---|
|Partizionamento automatico di Soft-NUMA|È possibile configurare automaticamente Soft-NUMA sui sistemi che segnalano 8 o più CPU per ogni nodo NUMA.|[Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md)|
|Estensione pool di buffer|Consente al pool di buffer di SQL Server di gestire una scalabilità superiore a 8 TB.|[Estensione pool di buffer](../database-engine/configure-windows/buffer-pool-extension.md)|
|Scalabilità dell'oggetto memoria dinamica| Partizionamento dinamico dell'oggetto memoria basato sul numero di nodi e di core. Questo miglioramento elimina la necessità del flag di traccia 8048 dopo SQL 2014 SP2.|[Scalabilità dell'oggetto memoria dinamica](/archive/blogs/sql_server_team/dynamic-memory-object-scaling)|
|Hint MAXDOP per i comandi DBCC CHECK *|Questo miglioramento è utile per eseguire DBCC CHECKDB con un'impostazione di MAXDOP diversa dal valore sp_configure.|[Hints (Transact-SQL) - Query](../t-sql/queries/hints-transact-sql-query.md)|
|Miglioramento dello spinlock per SOS_RWLock|Elimina la necessità di uno spinlock per SOS_RWLock sostituendolo con tecniche senza blocco simili a OLTP in memoria. |[Riprogettazione SOS_RWLock](/archive/blogs/psssql/sql-2016-it-just-runs-faster-sos_rwlock-redesign)|
|Implementazione spaziale nativa|Miglioramento significativo delle prestazioni delle query spaziali.|[Miglioramenti delle prestazioni spaziali in SQL Server 2012 e 2014](https://support.microsoft.com/help/3107399/spatial-performance-improvements-in-sql-server-2012-and-2014)

### <a name="supportability-and-diagnostics-improvements-in-sp2"></a>Miglioramenti al supporto e alla diagnostica in SP2

|Funzionalità|Descrizione|Per ulteriori informazioni|
|---|---|---|
|Registrazione timeout di AlwaysON|Aggiunta una nuova funzionalità di registrazione per i messaggi di timeout del lease in modo che vengano registrate l'ora corrente e le ore previste per il rinnovo. |[Migliorata la diagnostica del timeout di lease per i gruppi di disponibilità AlwaysOn](/archive/blogs/alwaysonpro/improved-alwayson-availability-group-lease-timeout-diagnostics)
|Contatori di prestazioni e XEvent di AlwaysOn|Nuovi contatori di prestazioni ed eventi di AlwaysON per migliorare la diagnostica durante la risoluzione dei problemi di latenza con AlwaysON. |[KB 3107172](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve) e [KB 3107400](https://support.microsoft.com/help/3107400/improved-tempdb-spill-diagnostics-in-showplan-xml-schema-in-sql-server)
|Pulizia rilevamento modifiche|Una nuova stored procedure sp_flush_CT_internal_table_on_demand consente di pulire su richiesta le tabelle interne di rilevamento delle modifiche.|[KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)
|Clonazione dei database|Usare il nuovo comando DBCC per risolvere i problemi relativi ai database di produzione esistenti clonando lo schema, i metadati e le statistiche ma senza i dati. I database clonati non sono destinati agli ambienti di produzione.|[KB 3177838](https://support.microsoft.com/help/3177838/how-to-use-dbcc-clonedatabase-to-generate-a-schema-and-statistics-only)
|Aggiunte di funzioni a gestione dinamica (DMF)|Nuove DMF sys.dm_db_incremental_stats_properties espongono informazioni per ogni partizione riguardo alle statistiche incrementali.|[KB 3170114](https://support.microsoft.com/help/3170114/update-to-add-dmf-sys-dm-db-incremental-stats-properties-in-sql-server)
|DMF per il recupero del buffer di input in SQL Server|Disponibile una nuova DMF per il recupero del buffer di input per una sessione o una richiesta (sys.dm_exec_input_buffer). Dal punto di vista funzionale equivale a DBCC INPUTBUFFER.|[sys.dm_exec_input_buffer](../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md)
|Supporto DROP DDL per la replica|Consente di eliminare dal database e dalla pubblicazione una tabella inclusa come articolo nella pubblicazione della replica transazionale.|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)
|Privilegio IFI per l'account di servizio SQL|Determinare se l'inizializzazione immediata dei file (IFI) è attiva all'avvio del servizio SQL Server.|[Inizializzazione di file di database](../relational-databases/databases/database-instant-file-initialization.md)
|Concessioni di memoria: gestione dei problemi|È possibile sfruttare gli hint di diagnostica durante l'esecuzione delle query limitando le concessioni di memoria per evitare contese.|[KB 3107401](https://support.microsoft.com/help/3107401/new-query-memory-grant-options-are-available-min-grant-percent-and-max)
|Profilatura lightweight dell'esecuzione di query per operatore |Consente di ottimizzare la raccolta per ogni operatore delle statistiche di esecuzione delle query, ad esempio il numero effettivo di righe.|[Developers Choice: Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere) (Scelta degli sviluppatori: Avanzamento delle query, sempre e dovunque)
|Diagnostica di esecuzione delle query|Le righe effettive lette vengono ora segnalate nei piani di esecuzione della query per migliorare la risoluzione dei problemi di prestazioni della query.|[KB 3107397](https://support.microsoft.com/help/3107397/improved-diagnostics-for-query-execution-plans-that-involve-residual-p)
|Diagnostica di esecuzione delle query per spill tempdb|Gli avvisi relativi ad hash e ordinamento ora hanno colonne aggiuntive per tenere traccia delle statistiche di I/O fisico, memoria in uso e righe interessate. |[Migliorare la diagnostica per spill temptdb](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve)
|Facilità di supporto per tempdb |Usare un nuovo messaggio del log degli errori per il numero di file tempdb e le modifiche dei file dei dati tempdb all'avvio del server.|[KB 2963384](https://support.microsoft.com/help/2963384/fix-sql-server-crashes-when-the-log-file-of-tempdb-database-is-full-in)


Inoltre, tenere presenti le correzioni seguenti:
- Lo stack di chiamate Xevent ora include i nomi dei moduli e l'offset anziché gli indirizzi assoluti.
- Migliore correlazione tra la diagnostica di XEvent e delle DMV: query_hash e query_plan_hash vengono usati per identificare una query in modo univoco. Nelle DMV sono definiti come varbinary (8), mentre in XEvent come UINT64. Poiché in SQL Server non esiste un "bigint senza segno", il cast non sempre funziona. Questo miglioramento introduce nuove colonne azione/filtro di XEvent equivalenti a query_hash e query_plan_hash ad eccezione del fatto che tali valori sono definiti come INT64. Questa correzione consente di correlare le query tra XEvent e DMV.
- Supporto per UTF-8 in BULK INSERT e BCP: il supporto per l'esportazione e l'importazione di dati codificati in set di caratteri UTF-8 è ora abilitato in BULK INSERT e BCP.

### <a name="download-pages-and-more-information-for-sp2"></a>Scaricare le pagine e altre informazioni per SP2

- [Download del Service Pack 2 per Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=53168)
- [SQL Server 2014 Service Pack 2 disponibile](https://www.microsoft.com/download/details.aspx?id=53167)
- [SQL Server 2012 SP2 Express](https://www.microsoft.com/download/details.aspx?id=53167)
- [SQL Server 2014 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=53164)
- [Generatore report di SQL Server 2014 SP2](https://www.microsoft.com/download/details.aspx?id=53163)
- [Componente aggiuntivo SQL Server 2014 SP2 Reporting Services per Microsoft SharePoint](https://www.microsoft.com/download/details.aspx?id=53162)
- [SQL Server 2014 SP2 Semantic Language Statistics](https://www.microsoft.com/download/details.aspx?id=53165)
- [Informazioni sulla versione di SQL Server 2014 Service Pack 2](https://support.microsoft.com/help/3171021/sql-server-2014-service-pack-2-release-information)

## <a name="sql-server-2014-service-pack-1-sp1"></a>SQL Server 2014 Service Pack 1 (SP1)

SQL Server 2014 SP1 contiene correzioni disponibili nell'aggiornamento cumulativo 1 di SQL Server 2014 fino all'aggiornamento cumulativo 5 incluso, nonché un rollup di correzioni accluse in precedenza a SQL Server 2012 SP2.

> [!NOTE]
> Se per l'istanza di SQL Server è abilitato il catalogo SSISDB e si verifica un errore di installazione quando si esegue l'aggiornamento a SP1, seguire le istruzioni relative a questo problema descritte in [Errore 912 o 3417 quando si installa SQL Server 2014 SP1 (build 12.0.4050.0)](https://support.microsoft.com/topic/kb3018269-error-912-or-3417-when-you-install-sql-server-2014-sp1-build-12-0-4050-0-7f34525c-d517-6399-a31b-e7033db13706).

### <a name="download-pages-and-more-information-for-sp1"></a>Scaricare le pagine e altre informazioni per SP1

- [Download del Service Pack 1 per Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=46694)
- [Informazioni aggiornate sul rilascio di SQL Server 2014 Service Pack 1](/archive/blogs/sqlreleaseservices/sql-server-2014-service-pack-1-has-released-updated)
- [Microsoft SQL Server 2014 SP1 Express](https://www.microsoft.com/download/details.aspx?id=42299)
- [Microsoft SQL Server 2014 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=46694)


## <a name="before-you-install-sql-server-2014-rtm"></a>Prima di installare SQL Server 2014 RTM

### <a name="limitations-and-restrictions-in-sql-server-2014-rtm"></a>Limitazioni e restrizioni in SQL Server 2014 RTM

1.  L'aggiornamento da SQL Server 2014 CTP 1 a SQL Server 2014 RTM NON è supportato.  
2.  L'installazione side-by-side di SQL Server 2014 CTP 1 e SQL Server 2014 RTM NON è supportata.  
3.  Il collegamento o il ripristino di un database SQL Server 2014 CTP 1 a SQL Server 2014 RTM NON è supportato.  

**Soluzione alternativa:** No.

#### <a name="upgrading-from-sql-server-2014-ctp-2-to-sql-server-rtm"></a>Aggiornamento da SQL Server 2014 CTP 2 a SQL Server RTM
L'aggiornamento è completamente supportato. In particolare, è possibile:

1.  Collegare un database SQL Server 2014 CTP 2 a un'istanza di SQL Server 2014 RTM.    
2.  Ripristinare in un'istanza di SQL Server 2014 RTM un backup di database creato in SQL Server 2014 CTP 2.    
3.  Aggiornamento sul posto a SQL Server 2014 RTM.
4.  Aggiornamento in sequenza a SQL Server 2014 RTM. È necessario passare alla modalità di failover manuale prima di avviare l'aggiornamento in sequenza. Per informazioni dettagliate, fare riferimento alla pagina [Aggiornamento dei server dei gruppi di disponibilità con tempi di inattività e perdita dei dati minimi](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).    
5.  Non è possibile visualizzare i dati raccolti dai set di raccolta prestazioni delle transazioni installati in SQL Server 2014 CTP 2 tramite SQL Server Management Studio in SQL Server 2014 RTM e viceversa.
  
#### <a name="downgrading-from-sql-server-2014-rtm-to-sql-server-2014-ctp-2"></a>Downgrade da SQL Server 2014 RTM a SQL Server 2014 CTP 2  
Questa azione non è supportata.  
  
**Soluzione alternativa:** per il downgrade non sono disponibili soluzioni alternative. Si consiglia di eseguire il backup del database prima di eseguire l'aggiornamento a SQL Server 2014 RTM.  
  
#### <a name="incorrect-version-of-streaminsight-client-on-sql-server-2014-mediaisocab"></a>Versione non corretta di StreamInsight Client su supporto/ISO/CAB di SQL Server 2014  
La versione errata di StreamInsight.msi e StreamInsightClient.msi si trova nel percorso seguente nel supporto /ISO/CAB di SQL Server (StreamInsight\\\<Architecture\>\\\<Language ID\>).  
  
**Soluzione alternativa:** Scaricare e installare la versione corretta dalla [pagina per il download di SQL Server 2014 Feature Pack](https://www.microsoft.com/download/details.aspx?id=57474).  
  
### <a name="product-documentation-rtm"></a><a name="ProdDoc"></a>Documentazione del prodotto RTM
  
Il contenuto di Generatore report e PowerPivot non è disponibile in alcune lingue. 

**Problema:** il contenuto di Generatore report non è disponibile nelle lingue seguenti:  
  
-   Greco (el-GR)  
-   Norvegese (Bokmål) (nb-NO)  
-   Finlandese (fi-FI)  
-   Danese (da-DK)  
  
In [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]il contenuto è disponibile in un file CHM fornito con il prodotto in queste lingue. I file CHM non vengono più forniti con il prodotto e il contenuto di Generatore report è disponibile solo su MSDN. MSDN non supporta queste lingue. Generatore report è stato rimosso da TechNet e non è più disponibile nelle lingue supportate.  
  
**Soluzione alternativa:** No.  
  
**Problema:** il contenuto di Power Pivot non è disponibile nelle lingue seguenti:
  
-   Greco (el-GR)  
-   Norvegese (Bokmål) (nb-NO)  
-   Finlandese (fi-FI)  
-   Danese (da-DK)  
-   Ceco (cs-CZ)  
-   Ungherese (hu-HU)  
-   Olandese (Paesi Bassi) (nl-NL)  
-   Polacco (pl-PL)  
-   Svedese (sv-SE)  
-   Turco (tr-TR)  
-   Portoghese (Portogallo) (pt-PT)  
  
In [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], il contenuto era disponibile in TechNet ed era disponibile in queste lingue. Il contenuto è stato rimosso da TechNet e non è più disponibile in queste lingue supportate.  
  
**Soluzione alternativa:** No.  
  
### <a name="database-engine-rtm"></a><a name="DBEngine"></a>Motore di database (RTM)
  
#### <a name="changes-made-for-standard-edition-in-sql-server-2014-rtm"></a>Modifiche apportate all'edizione Standard in SQL Server 2014 RTM  
SQL Server 2014 Standard presenta le seguenti modifiche:  
  
-   La funzionalità di estensione del pool di buffer consente di utilizzare la dimensione massima pari a fino 4 volte la memoria configurata.    
-   La memoria massima è aumentata da 64 GB a 128 GB.  
 
#### <a name="memory-optimization-advisor-flags-default-constraints-as-incompatible"></a>L'Ottimizzazione guidata per la memoria contrassegna i vincoli predefiniti come incompatibili  
**Problema:** l'Ottimizzazione guidata per la memoria in SQL Server Management Studio contrassegna tutti i vincoli predefiniti come incompatibili. Non tutti i vincoli predefiniti sono supportati in una tabella ottimizzata per la memoria; l'ottimizzazione guidata non opera una distinzione tra i tipi di vincoli predefiniti supportati e quelli non supportati. Tra i vincoli predefiniti supportati sono incluse tutte le costanti, le espressioni e le funzioni predefinite supportate all'interno di stored procedure compilate in modo nativo. Per visualizzare l'elenco delle funzioni supportate nelle stored procedure compilate in modo nativo, fare riferimento alla pagina [Costrutti supportati in stored procedure compilate in modo nativo](../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md?viewFallbackFrom=sql-server-2014).  
  
**Soluzione alternativa:** per usare l'ottimizzazione guidata per identificare i blocchi, ignorare i vincoli predefiniti compatibili. Per utilizzare l'Ottimizzazione guidata per la memoria per la migrazione delle tabelle con vincoli predefiniti compatibili, ma senza altri blocchi, attenersi alla procedura seguente:  
  
1.  Rimuovere i vincoli predefiniti dalla definizione della tabella.    
2.  Utilizzare l'Ottimizzazione guidata per produrre uno script di migrazione nella tabella.    
3.  Aggiungere nuovamente i vincoli predefiniti allo script di migrazione.    
4.  Eseguire lo script di migrazione.  
  
#### <a name="informational-message-file-access-denied-incorrectly-reported-as-an-error-in-the-sql-server-2014-error-log"></a>Il messaggio informativo "accesso file negato" viene erroneamente restituito come errore nel log degli errori di SQL Server 2014  
**Problema:** quando si riavvia un server con database che contengono tabelle ottimizzate per la memoria, è possibile che venga visualizzato il tipo seguente di messaggi di errore nel log degli errori di SQL Server 2014:  
  
```  
[ERROR]Unable to delete file C:\Program Files\Microsoft SQL   
Server\....old.dll. This error may be due to a previous failure to unload   
memory-optimized table DLLs.  
```  
In realtà si tratta di un messaggio informativo e non è richiesto alcun intervento da parte dell'utente.  
  
**Soluzione alternativa:** No. È un messaggio informativo.  
  
#### <a name="missing-index-details-incorrectly-report-included-columns-for-memory-optimized-table"></a>Nei dettagli sugli indici mancanti sono erroneamente segnalate colonne incluse per la tabella ottimizzata per la memoria  
**Problema:** se SQL Server 2014 rileva un indice mancante per una query in una tabella ottimizzata per la memoria, segnala un indice mancante in SHOWPLAN_XML e nelle DMV dell'indice mancante, ad esempio sys.dm_db_missing_index_details. In alcuni casi, i dettagli sugli indici mancanti contengono le colonne incluse. Poiché tutte le colonne sono incluse in modo implicito in tutti gli indici nelle tabelle ottimizzate per la memoria, non è consentito specificare in modo esplicito le colonne incluse con gli indici ottimizzati per la memoria.  
  
**Soluzione alternativa:** non specificare la clausola INCLUDE con gli indici nelle tabelle ottimizzate per la memoria.  
  
#### <a name="missing-index-details-omit-missing-indexes-when-a-hash-index-exists-but-is-not-suitable-for-the-query"></a>Nei dettagli sugli indici mancanti vengono omessi gli indici mancanti se esiste un indice hash che non è adatto per la query  
**Problema:** se si dispone di un indice HASH sulle colonne di una tabella ottimizzata per la memoria a cui fa riferimento una query, ma l'indice non può essere usato per la query, SQL Server 2014 non segnalerà sempre un indice mancante in SHOWPLAN_XML e nella DMV sys.dm_db_missing_index_details.  
  
In particolare, se una query contiene predicati di uguaglianza che interessano un subset delle colonne chiave dell'indice o se contiene predicati di disuguaglianza che interessano colonne chiave dell'indice, l'indice HASH non può essere utilizzato così com'è ed è necessario un altro indice per eseguire la query in modo efficace.  
  
**Soluzione alternativa:** se si usano gli indici hash, controllare le query e i piani di query per determinare se le query possono trarre vantaggio dalle operazioni Index Seek su un subset della chiave di indice o sui predicati di disuguaglianza. Se è necessario eseguire la ricerca su un subset della chiave di indice, utilizzare un indice NON CLUSTER oppure un indice HASH esattamente sulle colonne in cui eseguire la ricerca. Se è necessario eseguire la ricerca in un predicato di disuguaglianza, utilizzare un indice NON CLUSTER anziché HASH.  
  
#### <a name="failure-when-using-a-memory-optimized-table-and-memory-optimized-table-variable-in-the-same-query-if-the-database-option-read_committed_snapshot-is-set-to-on"></a>Si verifica un errore quando si utilizza una tabella ottimizzata per la memoria e una variabile di tabella ottimizzata per la memoria nella stessa query, se l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON  
**Problema:** se l'opzione di database READ_COMMITTED_SNAPSHOT è impostata su ON e si accede sia a una tabella ottimizzata per la memoria sia a una variabile di tabella ottimizzata per la memoria nella stessa istruzione al di fuori del contesto di una transazione utente, è possibile che venga visualizzato il messaggio di errore seguente:  
  
```  
Msg 41359  
A query that accesses memory optimized tables using the READ COMMITTED  
isolation level, cannot access disk based tables when the database option  
READ_COMMITTED_SNAPSHOT is set to ON. Provide a supported isolation level  
for the memory optimized table using a table hint, such as WITH (SNAPSHOT).  
```  
  
**Soluzione alternativa:** usare l'hint di tabella WITH (SNAPSHOT) con la variabile di tabella o impostare l'opzione di database MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT su ON, tramite l'istruzione seguente:  
  
```  
ALTER DATABASE CURRENT   
SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
```  
  
#### <a name="procedure-and-query-execution-statistics-for-natively-compiled-stored-procedures-record-worker-time-in-multiples-of-1000"></a>Nelle statistiche di esecuzione di stored procedure e query per le stored procedure compilate in modo nativo il tempo del processo viene registrato in multipli di 1000  
**Problema:** dopo avere abilitato la raccolta delle statistiche di esecuzione di stored procedure o query per le stored procedure compilate in modo nativo usando sp_xtp_control_proc_exec_stats o sp_xtp_control_query_exec_stats, *_worker_time verrà visualizzato in multipli di 1000 nelle DMV sys.dm_exec_procedure_stats e sys.dm_exec_query_stats. Le esecuzioni di query con un tempo del processo inferiore a 500 microsecondi verranno segnalate con un valore worker_time pari a 0.  
  
**Soluzione alternativa:** No. È opportuno non fare affidamento sul valore worker_time segnalato nelle DMV delle statistiche di esecuzione per le query con esecuzione rapida nelle stored procedure compilate in modo nativo.  
  
#### <a name="error-with-showplan_xml-for-natively-compiled-stored-procedures-that-contain-long-expressions"></a>Si verifica un errore con SHOWPLAN_XML per le stored procedure compilate in modo nativo che contengono espressioni lunghe  
**Problema:** se una stored procedure compilata in modo nativo contiene un'espressione lunga, l'acquisizione di SHOWPLAN_XML per la stored procedure, tramite l'opzione T-SQL SET SHOWPLAN_XML ON o l'opzione "Visualizza piano di esecuzione stimato" in Management Studio, potrebbe causare l'errore seguente:  
  
```  
Msg 41322. MAT/PIT export/import encountered a failure for memory  
optimized table or natively compiled stored procedure with object ID  
278292051 in database ID 6. The error code was  
0xc00cee81.  
```  
  
**Soluzione alternativa:** sono disponibili due soluzioni:  
  
1.  Aggiungere le parentesi all'espressione, in modo analogo all'esempio seguente:  
  
    Anziché:  
  
    ```  
    SELECT @v0 + @v1 + @v2 + ... + @v199  
    ```  
  
    Scrittura:  
  
    ```  
    SELECT((@v0 + ... + @v49) + (@v50 + ... + @v99)) + ((@v100 + ... + @v149) + (@v150 + ... + @v199))  
    ```  
  
2.  Creare una seconda stored procedure con un'espressione leggermente semplificata per l'uso con lo showplan. La forma generale del piano deve essere la stessa. Ad esempio, anziché:  
  
    ```  
    SELECT @v0 +@v1 +@v2 +...+@v199  
    ```  
  
    Scrittura:  
  
    ```  
    SELECT @v0 +@v1  
    ```  
  
#### <a name="using-a-string-parameter-or-variable-with-datepart-and-related-functions-in-a-natively-compiled-stored-procedure-results-in-an-error"></a>L'utilizzo di un parametro di stringa o di una variabile con DATEPART e funzioni correlate in una stored procedure compilata in modo nativo genera un errore  
**Problema:** quando si usa una stored procedure compilata in modo nativo che usa un parametro o una variabile di stringa con le funzioni predefinite DATEPART, DAY, MONTH e YEAR, viene visualizzato un messaggio di errore che indica che il tipo di dati datetimeoffset non è supportato con le stored procedure compilate in modo nativo.  
  
**Soluzione alternativa:** assegnare la variabile o il parametro di stringa a una nuova variabile di tipo datetime2 e usare tale variabile nella funzione DATEPART, DAY, MONTH o YEAR. Ad esempio:  
  
```  
DECLARE @d datetime2 = @string  
DATEPART(weekday, @d)  
```  
  
#### <a name="native-compilation-advisor-flags-delete-from-clauses-incorrectly"></a>Assistente compilazione nativa contrassegna in modo errato le clausole DELETE FROM  
**Problema:** Assistente compilazione nativa contrassegna in modo errato le clausole DELETE FROM all'interno di una stored procedure come incompatibili.  
  
**Soluzione alternativa:** No.  
  
#### <a name="register-through-ssms-adds-dac-meta-data-with-mismatched-instance-ids"></a>La registrazione con SSMS aggiunge metadati DAC con ID istanza non corrispondenti  
**Problema:** quando si esegue la registrazione o l'eliminazione di un pacchetto di applicazione livello dati (con estensione dacpac) tramite SQL Server Management Studio, le tabelle sysdac* non vengono aggiornate correttamente per consentire a un utente di eseguire query nella cronologia di dacpac per il database.  I valori id_instance per sysdac_history_internal e sysdac_instances_internal non corrispondono, quindi non è possibile creare un join.  
  
**Soluzione alternativa:** questo problema è stato corretto con la ridistribuzione del Feature Pack di [Data-Tier Application Framework](https://www.microsoft.com/download/details.aspx?id=100297).  Dopo l'applicazione dell'aggiornamento, tutte le nuove voci della cronologia useranno il valore elencato per id_instance nella tabella sysdac_instances_internal.  
  
Se si è già verificato il problema relativo ai valori instance_id non corrispondenti, per correggere la mancata corrispondenza dei valori è necessario connettersi al server come utente con privilegi di scrittura per il database MSDB e aggiornare i valori instance_id in modo che corrispondano.  Se si verificano diversi eventi di registrazione e annullamento della registrazione dello stesso database, può essere necessario esaminare data e ora per vedere quali record corrispondono al valore instance_id corrente.  
  
1.  Connettersi al server in SQL Server Management Studio utilizzando un account di accesso con autorizzazioni di aggiornamento per MSDB.    
2.  Aprire una nuova query utilizzando il database MSDB.    
3.  Eseguire la query per visualizzare tutte le istanze di applicazione livello dati attive.  Individuare l'istanza che si vuole correggere e annotare il valore instance_id:  
  
    `select * from` sysdac_instances_internal  
  
4.  Eseguire la query per visualizzare tutte le voci della cronologia:  
  
    `select * from` sysdac_history_internal  
  
5.  Identificare le righe che devono corrispondere all'istanza da correggere. 
6.  Aggiornare il valore sysdac_history_internal.instance_id con il valore annotato nel passaggio 3 (dalla tabella sysdac_instances_internal):  
  
    `update` sysdac_history_internal `set` instance_id = '\<value from step 3\>' `where` \<expression that matches the rows you want to update\>  
  
### <a name="reporting-services-rtm"></a><a name="SSRS"></a>Reporting Services (RTM)
  
#### <a name="the-sql-server-2012-reporting-services-native-mode-report-server-cannot-run-side-by-side-with-sql-server-2014-reporting-services-sharepoint-components"></a>Il server di report in modalità nativa SQL Server 2012 Reporting Services non può essere eseguito side-by-side con componenti di SharePoint di SQL Server 2014 Reporting Services  
**Problema:** il servizio Windows in modalità nativa di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SQL Server Reporting Services (ReportingServicesService.exe) non viene avviato quando sono presenti componenti di SharePoint di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] installati nello stesso server.  
  
**Soluzione alternativa:** disinstallare i componenti di SharePoint di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] e riavviare il servizio Windows di Microsoft SQL Server 2012 Reporting Services.  
  
**Altre informazioni:**  
  
La modalità nativa di [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] non può essere eseguita side-by-side in nessuna delle seguenti condizioni:  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] - Componente aggiuntivo per prodotti SharePoint    
-   Servizio SharePoint Shared di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]  
  
L'installazione side-by-side impedisce l'avvio del servizio Windows in modalità nativa di [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Nel log degli eventi di Windows saranno presenti messaggi di errore simili a quelli descritti qui:  
  
```  
Log Name:   Application  
Source:          Report Server (<SQL instance ID>)  
Event ID:        117  
Task Category:   Startup/Shutdown  
Level:           Error  
Keywords:        Classic  
Description:     The report server database is an invalid version.  
  
Log Name:      Application  
Source:        Report Server (<SQL instance ID>)  
Event ID:      107  
Task Category: Management  
Level:         Error  
Keywords:      Classic  
Description:   Report Server (DENALI) cannot connect to the report server database.  
```  
  
Per ulteriori informazioni, vedere [Suggerimenti e risoluzione dei problemi relativi a SQL Server 2014 Reporting Services](https://go.microsoft.com/fwlink/?LinkID=391254).  
  
#### <a name="required-upgrade-order-for-multi-node-sharepoint-farm-to-sql-server-2014-reporting-services"></a>Ordine obbligatorio per l'aggiornamento della farm di SharePoint a nodi multipli a SQL Server 2014 Reporting Services  
**Problema:** il rendering del report in una farm a nodi multipli ha esito negativo se le istanze del servizio SharePoint Shared di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] vengono aggiornate prima di tutte le istanze del componente aggiuntivo di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] per prodotti SharePoint.  
  
**Soluzione alternativa:** in una farm di SharePoint a nodi multipli,  
  
1.  aggiornare prima tutte le istanze del componente aggiuntivo di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] per prodotti SharePoint.    
2.  quindi, aggiornare tutte le istanze del servizio SharePoint Shared di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .  
  
Per ulteriori informazioni, vedere [Suggerimenti e risoluzione dei problemi relativi a SQL Server 2014 Reporting Services](https://go.microsoft.com/fwlink/?LinkID=391254).  
  
### <a name="sql-server-2014-rtm-on-azure-virtual-machines"></a><a name="AzureVM"></a>SQL Server 2014 RTM in macchine virtuali di Microsoft Azure  
  
#### <a name="the-add-azure-replica-wizard-returns-an-error-when-configuring-an-availability-group-listener-in-azure"></a>La procedura guidata Aggiungi replica Azure restituisce un errore durante la configurazione di un listener del gruppo di disponibilità in Azure  
**Problema:** se un gruppo di disponibilità ha un listener, la procedura guidata Aggiungi replica Azure restituisce un errore durante il tentativo di configurare il listener in Azure.  
  
Ciò è dovuto al fatto che i listener del gruppo di disponibilità richiedono l'assegnazione di un indirizzo IP in ogni subnet che ospita repliche del gruppo di disponibilità, inclusa la subnet di Azure.  
  
**Soluzione alternativa:**  
  
1.  Nella pagina Listener, assegnare un indirizzo IP statico libero al listener del gruppo di disponibilità nella subnet di Azure che ospiterà la replica del gruppo di disponibilità.  
  
    Questa soluzione alternativa consente alla procedura guidata di completare l'aggiunta della replica in Azure.  
  
2.  Al termine della procedura guidata, sarà necessario completare la configurazione del listener in Azure come descritto in [Configurazione del listener per i gruppi di disponibilità AlwaysOn in Azure](/previous-versions/azure/dn376546(v=azure.100))  
  
### <a name="analysis-services-rtm"></a><a name="SSAS"></a>Analysis Services (RTM)
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2010-new-farm-configured-with-sql-server-2014"></a>È necessario scaricare, installare e registrare MSOLAP.5 per una nuova farm SharePoint 2010 configurata con SQL Server 2014  
**Problema:**  
  
-   Per una farm di SharePoint 2010 configurata con una distribuzione di SQL Server 2014 RTM non è possibile connettere le cartelle di lavoro di PowerPivot ai modelli di dati perché il provider a cui si fa riferimento nella stringa di connessione non è installato.  
  
**Soluzione alternativa:**  
  
1.  Scaricare il provider MSOLAP.5 dal Feature Pack di [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] . Installare il provider nei server applicazioni in cui viene eseguito Excel Services. Per altre informazioni, vedere la sezione "Provider OLE DB Microsoft Analysis Services per Microsoft SQL Server 2012 SP1" di [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Registrare MSOLAP.5 come provider attendibile con Excel Services di SharePoint. Per ulteriori informazioni, vedere [Aggiungere MSOLAP.5 come provider di dati attendibile in Excel Services](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15).  
  
**Altre informazioni:**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] include MSOLAP.6. [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] e [!INCLUDE[ssSQL14](../includes/sssql14-md.md)][!INCLUDE[ssGemini](../includes/ssgemini-md.md)] usano MSOLAP.5. Se MSOLAP.5 non è installato nel computer in cui viene eseguito Excel Services, non sarà possibile caricare i modelli di dati tramite Excel Services.  
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2013-new-farm-configured-with-sql-server-2014"></a>È necessario scaricare, installare e registrare MSOLAP.5 per una nuova farm di SharePoint 2013 configurata con SQL Server 2014  
**Problema:**  
  
-   Per una farm SharePoint 2013 configurata con una distribuzione di [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] , non è possibile connettere le cartelle di lavoro di Excel che fanno riferimento al provider MSOLAP.5 ai modelli di dati tabulari perché il provider a cui si fa riferimento nella stringa di connessione non è installato.  
  
**Soluzione alternativa:**  
  
1.  Scaricare il provider MSOLAP.5 dal Feature Pack di [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] . Installare il provider nei server applicazioni in cui viene eseguito Excel Services. Per altre informazioni, vedere la sezione "Provider OLE DB Microsoft Analysis Services per Microsoft SQL Server 2012 SP1" di [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Registrare MSOLAP.5 come provider attendibile con Excel Services di SharePoint. Per ulteriori informazioni, vedere [Aggiungere MSOLAP.5 come provider di dati attendibile in Excel Services](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15).  
  
**Altre informazioni:**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] include MSOLAP.6. ma le cartelle di lavoro di PowerPivot di SQL Server 2014 utilizzano MSOLAP.5. Se MSOLAP.5 non è installato nel computer in cui viene eseguito Excel Services, non sarà possibile caricare i modelli di dati tramite Excel Services.  
  
#### <a name="corrupt-data-refresh-schedules-rtm"></a>Pianificazione dell'aggiornamento dati danneggiata (RTM)
**Problema:**  
  
-   Viene aggiornata una pianificazione dell'aggiornamento e la pianificazione viene danneggiata ed è inutilizzabile.  
  
**Soluzione alternativa:**  
  
1.  In Microsoft Excel cancellare le proprietà avanzate personalizzate. Vedere la sezione "Soluzione alternativa" dell'articolo della Knowledge Base [KB 2927748](https://support.microsoft.com/kb/2927748).  
  
**Altre informazioni:**  
  
-    Se la lunghezza serializzata della pianificazione dell'aggiornamento è inferiore alla pianificazione originale, quando si aggiorna una pianificazione dell'aggiornamento dati per una cartella di lavoro le dimensioni del buffer non vengono aggiornate correttamente e le informazioni sulla nuova pianificazione vengono unite a quelle della pianificazione precedente con il conseguente danneggiamento della pianificazione.  
  
### <a name="data-quality-services-rtm"></a><a name="DQS"></a>Data Quality Services (RTM)
  
#### <a name="no-cross-version-support-for-data-quality-services-in-master-data-services"></a>Nessun supporto tra versioni di Data Quality Services in Master Data Services  
**Problema:** Non sono supportati gli scenari seguenti:  
  
-   Master Data Services 2014 ospitato in un database del motore di database di SQL Server in SQL Server 2012 con Data Quality Services 2012 installato.  
  
-   Master Data Services 2012 ospitato in un database del motore di database di SQL Server in SQL Server 2014 con Data Quality Services 2014 installato.  
  
**Soluzione alternativa:** usare la stessa versione di Master Data Services del database del motore di database e di Data Quality Services.  
  
### <a name="upgrade-advisor-issues-rtm"></a><a name="UA"></a>Problemi di Gestione spazio aggiornamenti (RTM)
  
#### <a name="sql-server-2014-upgrade-advisor-reports-irrelevant-upgrade-issues-for-sql-server-reporting-services"></a>Gestione spazio aggiornamenti di SQL Server 2014 segnala problemi di aggiornamento irrilevanti per SQL Server Reporting Services  
**Problema:** Preparazione aggiornamento di SQL Server fornito con i supporti di SQL Server 2014 segnala erroneamente più errori durante l'analisi del server SQL Server Reporting Services.  
  
**Soluzione alternativa:** questo problema è stato risolto in Gestione spazio aggiornamenti di SQL Server incluso in [SQL Server 2014 Feature Pack per SSUA](https://www.microsoft.com/download/details.aspx?id=57474).  
  
#### <a name="sql-server-2014-upgrade-advisor-reports-an-error-when-analyzing-sql-server-integration-services-server"></a>Gestione spazio aggiornamenti di SQL Server 2014 segnala un errore durante l'analisi del server SQL Server Integration Services  
**Problema:** Gestione spazio aggiornamenti di SQL Server fornito con i supporti di SQL Server 2014 segnala un errore durante l'analisi del server SQL Server Integration Services.  L'errore visualizzato all'utente è il seguente:  
  
```  
The installed version of Integration Services does not support Upgrade Advisor.   
The assembly information is "Microsoft.SqlServer.ManagedDTS, Version=11.0.0.0,   
Culture=neutral, PublicKeyToken=89845dcd8080cc91  
```  
  
**Soluzione alternativa:** questo problema è stato risolto in Gestione spazio aggiornamenti di SQL Server incluso in [SQL Server 2014 Feature Pack per SSUA](https://www.microsoft.com/download/details.aspx?id=57474).  
  
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

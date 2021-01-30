---
description: sp_trace_setevent (Transact-SQL)
title: sp_trace_setevent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setevent_TSQL
- sp_trace_setevent
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setevent
ms.assetid: 7662d1d9-6d0f-443a-b011-c901a8b77a44
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fffcbb5ec35fb329fa0ed3dac4782f0f1417ce82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209725"
---
# <a name="sp_trace_setevent-transact-sql"></a>sp_trace_setevent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge o rimuove un evento o colonna di evento in una traccia. **sp_trace_setevent** possono essere eseguite solo su tracce esistenti interrotte (*lo stato* è **0**). Viene restituito un errore se questa stored procedure viene eseguita su una traccia che non esiste o il cui *stato* è diverso da **0**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_trace_setevent [ @traceid = ] trace_id   
          , [ @eventid = ] event_id  
          , [ @columnid = ] column_id  
          , [ @on = ] on  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @traceid = ] trace_id` ID della traccia da modificare. *trace_id* è di **tipo int** e non prevede alcun valore predefinito. L'utente utilizza questo *trace_id* valore per identificare, modificare e controllare la traccia.  
  
`[ @eventid = ] event_id` ID dell'evento da attivare. *event_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
 Nella tabella seguente vengono descritti gli eventi che è possibile aggiungere o rimuovere in una traccia.  
  
|Numero evento|Nome evento|Descrizione|  
|------------------|----------------|-----------------|  
|0-9|Riservato|Riservato|  
|10|RPC:Completed|Viene generato al completamento di una chiamata di procedura remota (RPC).|  
|11|RPC:Starting|Viene generato all'avvio di una chiamata RPC.|  
|12|SQL:BatchCompleted|Viene generato al completamento di un batch [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|13|SQL:BatchStarting|Viene generato all'avvio di un batch [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|14|Audit Login|Viene generato quando un utente esegue correttamente l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|15|Audit Logout|Viene generato quando un utente si disconnette da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|16|Attention|Viene generato per eventi di attenzione, come le richieste di interrupt dei client o l'interruzione di connessioni client.|  
|17|ExistingConnection|Rileva tutte le attività degli utenti connessi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prima dell'avvio della traccia.|  
|18|Audit Server Starts and Stops|Viene generato alla modifica dello stato del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|19|DTCTransaction|Tiene traccia delle transazioni coordinate [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator tra due o più database.|  
|20|Audit Login Failed|Indica l'esito negativo di un tentativo di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da parte di un client.|  
|21|EventLog|Indica che sono stati registrati eventi nel registro applicazioni di Windows.|  
|22|ErrorLog|Indica che sono stati registrati eventi di errore nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|23|Lock:Released|Indica il rilascio di un blocco su una risorsa, ad esempio una pagina.|  
|24|Lock:Acquired|Indica l'acquisizione di un blocco su una risorsa, ad esempio una pagina di dati.|  
|25|Lock:Deadlock|Indica che due transazioni simultanee si sono bloccate a vicenda con un deadlock, in seguito al tentativo di una transazione di ottenere blocchi incompatibili sulle risorse di proprietà dell'altra transazione.|  
|26|Lock:Cancel|Indica l'annullamento dell'acquisizione di un blocco su una risorsa, ad esempio in seguito a un deadlock.|  
|27|Lock:Timeout|Indica il timeout di una richiesta di blocco su una risorsa, come una pagina, dovuto alla presenza di un blocco su tale risorsa mantenuto attivo da un'altra transazione. Il timeout è determinato dalla @LOCK_TIMEOUT funzione @ e può essere impostato con l'istruzione set LOCK_TIMEOUT.|  
|28|Degree of Parallelism Event (7.0 Insert)|Viene generato prima dell'esecuzione di un'istruzione SELECT, INSERT o UPDATE.|  
|29-31|Riservato|Utilizzare l'evento 28 in alternativa.|  
|32|Riservato|Riservato|  
|33|Eccezione|Indica la generazione di un'eccezione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|34|SP:CacheMiss|Indica che la stored procedure specificata non è stata trovata nella cache delle procedure.|  
|35|SP:CacheInsert|Indica l'inserimento di un elemento nella cache delle procedure.|  
|36|SP:CacheRemove|Indica la rimozione di un elemento dalla cache delle procedure.|  
|37|SP:Recompile|Indica la ricompilazione di una stored procedure.|  
|38|SP:CacheHit|Indica l'individuazione di una stored procedure nella cache delle procedure.|  
|39|Deprecato|Deprecato|  
|40|SQL:StmtStarting|Viene generato all'avvio dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|41|SQL:StmtCompleted|Viene generato al completamento dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|42|SP:Starting|Indica l'avvio della stored procedure.|  
|43|SP:Completed|Indica il completamento della stored procedure.|  
|44|SP:StmtStarting|Indica l'avvio dell'esecuzione di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in una stored procedure.|  
|45|SP:StmtCompleted|Indica il completamento dell'esecuzione di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in una stored procedure.|  
|46|Object:Created|Indica la creazione di un oggetto, ad esempio tramite le istruzioni CREATE INDEX, CREATE TABLE e CREATE DATABASE.|  
|47|Object:Deleted|Indica l'eliminazione di un oggetto, ad esempio tramite le istruzioni DROP INDEX e DROP TABLE.|  
|48|Riservato||  
|49|Riservato||  
|50|SQL Transaction|Tiene traccia delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] BEGIN, COMMIT, SAVE e ROLLBACK TRANSACTION.|  
|51|Scan:Started|Indica l'avvio dell'analisi di una tabella o di un indice.|  
|52|Scan:Stopped|Indica l'arresto dell'analisi di una tabella o di un indice.|  
|53|CursorOpen|Indica l'apertura di un cursore in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] tramite ODBC, OLE DB o DB-Library.|  
|54|TransactionLog|Tiene traccia del momento in cui le transazioni vengono scritte nel log delle transazioni.|  
|55|Hash Warning|Indica l'utilizzo di un piano alternativo per un'operazione di hashing, ad esempio hash join, hash aggregate, hash union e hash distinct, che non viene elaborata in una partizione di buffer. Ciò può verificarsi a causa del livello di nidificazione della ricorsione, di asimmetrie dei dati, di flag di traccia o del conteggio dei bit.|  
|56-57|Riservato||  
|58|Auto Stats|Indica un aggiornamento automatico delle statistiche dell'indice.|  
|59|Lock:Deadlock Chain|Viene generato per ogni evento che ha portato al deadlock.|  
|60|Lock:Escalation|Indica la conversione di un blocco con granularità fine in un blocco con granularità grossolana, come nel caso di un blocco a livello di pagina convertito (escalation) in un blocco TABLE o HoBT.|  
|61|OLE DB Errors|Indica la generazione di un errore OLE DB.|  
|62-66|Riservato||  
|67|Execution Warnings|Indica gli avvisi generati durante l'esecuzione di un'istruzione o una stored procedure di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|68|Showplan Text (Unencoded)|Visualizza l'albero del piano dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguita.|  
|69|Sort Warnings|Indica le operazioni di ordinamento per cui la memoria disponibile risulta insufficiente. Si riferisce solo alle operazioni di ordinamento eseguite nell'ambito di una query, ad esempio una clausola ORDER BY in un'istruzione SELECT. Non include le operazioni di ordinamento che implicano la creazione di indici.|  
|70|CursorPrepare|Indica quando un cursore in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] viene preparato per l'utilizzo tramite ODBC, OLE DB o DB-Library.|  
|71|Prepare SQL|Indica che una o più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono state preparate per l'utilizzo da ODBC, OLE DB o DB-Library.|  
|72|Exec Prepared SQL|Indica che una o più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] preparate sono state eseguite da ODBC, OLE DB o DB-Library.|  
|73|Unprepare SQL|Indica che una o più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] preparate sono state eliminate (annullamento della preparazione) da ODBC, OLE DB o DB-Library.|  
|74|CursorExecute|Indica l'esecuzione di un cursore precedentemente preparato in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] tramite ODBC, OLE DB o DB-Library.|  
|75|CursorRecompile|Indica che un cursore aperto in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] tramite ODBC o DB-Library è stato ricompilato direttamente o in seguito a una modifica dello schema.<br /><br /> Evento generato per cursori ANSI e non ANSI.|  
|76|CursorImplicitConversion|Indica che un cursore in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] è stato convertito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un tipo a un altro.<br /><br /> Evento generato per cursori ANSI e non ANSI.|  
|77|CursorUnprepare|Indica l'eliminazione (annullamento della preparazione) tramite ODBC, OLE DB o DB-Library di un cursore preparato in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|78|CursorClose|Indica la chiusura di un cursore precedentemente aperto in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] tramite ODBC, OLE DB o DB-Library.|  
|79|Missing Column Statistics|Indica che le statistiche di colonna utili per Query Optimizer non sono disponibili.|  
|80|Missing Join Predicate|Indica che è in esecuzione una query senza predicato di join. Ciò può comportare tempi di esecuzione della query prolungati.|  
|81|Server Memory Change|Indica che l'utilizzo di memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è aumentato o diminuito di 1 megabyte (MB) o del 5% della quantità di memoria massima del server, a seconda del valore maggiore.|  
|82-91|User Configurable (0-9)|Dati di evento definiti dall'utente.|  
|92|Data File Auto Grow|Indica che le dimensioni di un file di dati sono state aumentate automaticamente dal server.|  
|93|Log File Auto Grow|Indica che le dimensioni di un file di log sono state aumentate automaticamente dal server.|  
|94|Data File Auto Shrink|Indica che le dimensioni di un file di dati sono state compattate automaticamente dal server.|  
|95|Log File Auto Shrink|Indica che le dimensioni di un file di log sono state compattate automaticamente dal server.|  
|96|Showplan Text|Visualizza l'albero del piano della query dell'istruzione SQL da Query Optimizer. Si noti che la colonna **TextData** non contiene lo Showplan per questo evento.|  
|97|Showplan All|Visualizza il piano della query con dettagli completi sulla fase di compilazione dell'istruzione SQL eseguita. Si noti che la colonna **TextData** non contiene lo Showplan per questo evento.|  
|98|Showplan Statistics Profile|Visualizza il piano della query con dettagli completi sulla fase di esecuzione dell'istruzione SQL eseguita. Si noti che la colonna **TextData** non contiene lo Showplan per questo evento.|  
|99|Riservato||  
|100|RPC Output Parameter|Genera valori di output dei parametri per ogni chiamata RPC.|  
|101|Riservato||  
|102|Audit Database Scope GDR|Viene generato ogni volta che un qualsiasi utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue un'istruzione GRANT, REVOKE o DENY in relazione a un'autorizzazione per azioni relative esclusivamente a database, ad esempio la concessione di autorizzazioni in un database.|  
|103|Audit Object GDR Event|Viene generato ogni volta che un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue GRANT, DENY o REVOKE per un'autorizzazione per gli oggetti.|  
|104|Audit AddLogin Event|Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene aggiunto o rimosso un account di accesso, per **sp_addlogin** e **sp_droplogin**.|  
|105|Audit Login GDR Event|Si verifica quando viene aggiunto o rimosso un diritto di accesso di Windows. per **sp_grantlogin**, **sp_revokelogin** e **sp_denylogin**.|  
|106|Audit Login Change Property Event|Si verifica quando viene modificata una proprietà di un account di accesso, ad eccezione delle password. per **sp_defaultdb** e **sp_defaultlanguage**.|  
|107|Audit Login Change Password Event|Viene generato alla modifica della password di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Le password non vengono registrate.|  
|108|Audit Add Login to Server Role Event|Si verifica quando un account di accesso viene aggiunto o rimosso da un ruolo predefinito del server. per **sp_addsrvrolemember** e **sp_dropsrvrolemember**.|  
|109|Audit Add DB User Event|Si verifica quando un account di accesso viene aggiunto o rimosso come utente di database (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) in un database. per **sp_grantdbaccess**, **sp_revokedbaccess**, **sp_adduser** e **sp_dropuser**.|  
|110|Audit Add Member to DB Role Event|Si verifica quando un account di accesso viene aggiunto o rimosso come utente del database (fisso o definito dall'utente) in un database. per **sp_addrolemember**, **sp_droprolemember** e **sp_changegroup**.|  
|111|Audit Add Role Event|Si verifica quando un account di accesso viene aggiunto o rimosso come utente del database in un database. per **sp_addrole** e **sp_droprole**.|  
|112|Audit App Role Change Password Event|Viene generato per la modifica di una password di un ruolo applicazione.|  
|113|Audit Statement Permission Event|Viene generato quando si utilizza un'autorizzazione per le istruzioni, ad esempio CREATE TABLE.|  
|114|Audit Schema Object Access Event|Viene generato quando si utilizza un'autorizzazione per gli oggetti (ad esempio SELECT) con esito sia positivo che negativo.|  
|115|Audit Backup/Restore Event|Viene generato per l'esecuzione di un comando BACKUP o RESTORE.|  
|116|Audit DBCC Event|Viene generato per l'esecuzione di comandi DBCC.|  
|117|Audit Change Audit Event|Viene generato per modifiche apportate alla traccia di controllo.|  
|118|Audit Object Derived Permission Event|Viene generato per l'esecuzione dei comandi per gli oggetti CREATE, ALTER e DROP.|  
|119|OLEDB Call Event|Viene generato per l'esecuzione di chiamate del provider OLE DB per query distribuite e stored procedure remote.|  
|120|OLEDB QueryInterface Event|Si verifica quando vengono effettuate OLE DB chiamate **QueryInterface** per query distribuite e stored procedure remote.|  
|121|OLEDB DataRead Event|Viene generato per l'esecuzione di una chiamata di richiesta di dati al provider OLE DB.|  
|122|Showplan XML|Viene generato per l'esecuzione di un'istruzione SQL. Includere questo evento per identificare gli operatori Showplan. Ogni evento viene archiviato in un documento XML ben formato. Si noti che la colonna **binaria** per questo evento contiene lo Showplan codificato. Utilizzare SQL Server Profiler per aprire la traccia e visualizzare il piano Showplan.|  
|123|SQL:FullTextQuery|Viene generato per l'esecuzione di una query full-text.|  
|124|Broker:Conversation|Indica lo stato di una conversazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|125|Deprecation Announcement|Viene generato quando si utilizza una funzionalità che verrà rimossa da una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|126|Deprecation Final Support|Viene generato quando si utilizza una funzionalità che verrà rimossa dalla successiva versione principale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|127|Exchange Spill Event|Si verifica quando i buffer di comunicazione in un piano di query parallele sono stati scritti temporaneamente nel database **tempdb** .|  
|128|Audit Database Management Event|Viene generato per la creazione, modifica o eliminazione di un database.|  
|129|Audit Database Object Management Event|Viene generato per l'esecuzione di un'istruzione CREATE, ALTER o DROP sugli oggetti di database, come gli schemi.|  
|130|Audit Database Principal Management Event|Viene generato quando in un database vengono create, modificate o eliminate entità, come gli utenti.|  
|131|Audit Schema Object Management Event|Viene generato per la creazione, modifica o eliminazione di oggetti del server.|  
|132|Audit Server Principal Impersonation Event|Viene generato in presenza di una rappresentazione nell'ambito del server, come per EXECUTE AS LOGIN.|  
|133|Audit Database Principal Impersonation Event|Viene generato in presenza di una rappresentazione nell'ambito del database, come per EXECUTE AS USER o SETUSER.|  
|134|Audit Server Object Take Ownership Event|Viene generato per la modifica del proprietario degli oggetti nell'ambito del server.|  
|135|Audit Database Object Take Ownership Event|Viene generato per la modifica del proprietario degli oggetti nell'ambito del database.|  
|136|Broker:Conversation Group|Viene generato quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un nuovo gruppo di conversazioni o elimina un gruppo di conversazioni esistente.|  
|137|Blocked Process Report|Viene generato quando un processo rimane bloccato per un periodo di tempo maggiore di quello specificato. Non include processi di sistema o processi in attesa di risorse per le quali i deadlock non sono rilevabili. Utilizzare **sp_configure** per configurare la soglia e la frequenza con cui vengono generati i report.|  
|138|Broker:Connection|Indica lo stato di una connessione di trasporto gestita da [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|139|Broker:Forwarded Message Sent|Viene generato quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] inoltra un messaggio.|  
|140|Broker:Forwarded Message Dropped|Viene generato quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] elimina un messaggio destinato all'inoltro.|  
|141|Broker:Message Classify|Viene generato quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] determina la modalità di recapito di un messaggio.|  
|142|Broker:Transmission|Indica che si sono verificati errori nel livello trasporto di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Il numero di errore e i valori di stato indicano l'origine dell'errore.|  
|143|Broker:Queue Disabled|Indica che è stato rilevato un messaggio non elaborabile in seguito alla presenza di cinque rollback di transazioni consecutivi in una coda di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. L'evento include l'ID del database e l'ID della coda contenente il messaggio non elaborabile.|  
|144-145|Riservato||  
|146|Showplan XML Statistics Profile|Viene generato per l'esecuzione di un'istruzione SQL. Identifica gli operatori Showplan e visualizza dati completi della fase di compilazione. Si noti che la colonna **binaria** per questo evento contiene lo Showplan codificato. Utilizzare SQL Server Profiler per aprire la traccia e visualizzare il piano Showplan.|  
|148|Deadlock Graph|Viene generato in seguito all'annullamento di un tentativo di acquisizione di un blocco, perché il tentativo fa parte di un deadlock ed è stato scelto come vittima del deadlock. Include una descrizione XML di un deadlock.|  
|149|Broker:Remote Message Acknowledgement|Viene generato quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] invia o riceve l'acknowledgement di un messaggio.|  
|150|Trace File Close|Viene generato quando un file di traccia viene chiuso durante il rollover di un file di traccia.|  
|151|Riservato||  
|152|Audit Change Database Owner|Viene generato quando l'istruzione ALTER AUTHORIZATION viene utilizzata per modificare il proprietario di un database e vengono controllate le autorizzazioni a tale scopo.|  
|153|Audit Schema Object Take Ownership Event|Viene generato quando l'istruzione ALTER AUTHORIZATION viene utilizzata per assegnare un proprietario a un oggetto e vengono controllate le autorizzazioni a tale scopo.|  
|154|Riservato||  
|155|FT:Crawl Started|Viene generato all'avvio di una ricerca per indicizzazione (popolamento) full-text. Utilizzare questo evento per verificare che una richiesta di ricerca per indicizzazione venga effettivamente accolta dalle attività di lavoro.|  
|156|FT:Crawl Stopped|Viene generato all'arresto di una ricerca per indicizzazione (popolamento) full-text. L'arresto può essere causato dal corretto completamento dell'operazione o da un errore irreversibile.|  
|157|FT:Crawl Aborted|Viene generato quando si rileva un'eccezione durante una ricerca per indicizzazione full-text. In genere l'eccezione causa l'arresto della ricerca per indicizzazione full-text.|  
|158|Audit Broker Conversation|Segnala i messaggi di controllo correlati alla sicurezza del dialogo di [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|159|Audit Broker Login|Segnala i messaggi di controllo correlati alla sicurezza del trasporto di [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|160|Broker:Message Undeliverable|Viene generato quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] non è in grado di memorizzare un messaggio ricevuto che avrebbe dovuto essere recapitato a un servizio.|  
|161|Broker:Corrupted Message|Viene generato quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] riceve un messaggio danneggiato.|  
|162|User Error Message|Indica i messaggi di errori visualizzati agli utenti in caso di errore o eccezione.|  
|163|Broker:Activation|Viene generato quando un monitoraggio di coda avvia una stored procedure di attivazione o invia una notifica QUEUE_ACTIVATION oppure al termine di una stored procedure di attivazione avviata da un monitoraggio di coda.|  
|164|Object:Altered|Viene generato per la modifica di un oggetto di database.|  
|165|Performance statistics|Viene generato quando un piano di query compilato viene memorizzato nella cache per la prima volta, ricompilato o rimosso dalla cache dei piani.|  
|166|SQL:StmtRecompile|Viene generato quando si verifica una ricompilazione a livello di istruzione.|  
|167|Database Mirroring State Change|Viene generato per la modifica dello stato di un database con mirroring.|  
|168|Showplan XML For Query Compile|Viene generato per la compilazione di un'istruzione SQL. Vengono visualizzati dati completi della fase di compilazione. Si noti che la colonna **binaria** per questo evento contiene lo Showplan codificato. Utilizzare SQL Server Profiler per aprire la traccia e visualizzare il piano Showplan.|  
|169|Showplan All For Query Compile|Viene generato per la compilazione di un'istruzione SQL. Vengono visualizzati dati completi della fase di compilazione. Utilizzare questo evento per identificare gli operatori Showplan.|  
|170|Audit Server Scope GDR Event|Indica che è stato generato un evento di concessione, negazione o revoca di autorizzazioni nell'ambito del server, come la creazione di un account di accesso.|  
|171|Audit Server Object GDR Event|Indica che è stato generato un evento di concessione, negazione o revoca per un oggetto dello schema, come una tabella o una funzione.|  
|172|Audit Database Object GDR Event|Indica che è stato generato un evento di concessione, negazione o revoca per oggetti di database, come assembly e schemi.|  
|173|Audit Server Operation Event|Viene generato quando si utilizzano operazioni di controllo della sicurezza, come la modifica di impostazioni, risorse, opzioni per l'accesso esterno o autorizzazioni.|  
|175|Audit Server Alter Trace Event|Viene generato quando un'istruzione verifica la presenza dell'autorizzazione ALTER TRACE.|  
|176|Audit Server Object Management Event|Viene generato per la creazione, modifica o eliminazione di oggetti del server.|  
|177|Audit Server Principal Management Event|Viene generato per la creazione, modifica o eliminazione di entità del server.|  
|178|Audit Database Operation Event|Viene generato quando si eseguono operazioni di database, come la creazione di checkpoint o la sottoscrizione di notifica delle query.|  
|180|Audit Database Object Access Event|Viene generato per l'accesso a oggetti di database, come gli schemi.|  
|181|TM: Begin Tran starting|Viene generato all'avvio di una richiesta BEGIN TRANSACTION.|  
|182|TM: Begin Tran completed|Viene generato al completamento di una richiesta BEGIN TRANSACTION.|  
|183|TM: Promote Tran starting|Viene generato all'avvio di una richiesta PROMOTE TRANSACTION.|  
|184|TM: Promote Tran completed|Viene generato al completamento di una richiesta PROMOTE TRANSACTION.|  
|185|TM: Commit Tran starting|Viene generato all'avvio di una richiesta COMMIT TRANSACTION.|  
|186|TM: Commit Tran completed|Viene generato al completamento di una richiesta COMMIT TRANSACTION.|  
|187|TM: Rollback Tran starting|Viene generato all'avvio di una richiesta ROLLBACK TRANSACTION.|  
|188|TM: Rollback Tran completed|Viene generato al completamento di una richiesta ROLLBACK TRANSACTION.|  
|189|Lock: timeout (timeout > 0)|Viene generato quando si verifica il timeout di una richiesta di blocco su una risorsa, ad esempio una pagina.|  
|190|Progress Report: Online Index Operation|Indica lo stato di un'operazione di compilazione di un indice online durante l'esecuzione del processo di compilazione.|  
|191|TM: Save Tran starting|Viene generato all'avvio di una richiesta SAVE TRANSACTION.|  
|192|TM: Save Tran completed|Viene generato al completamento di una richiesta SAVE TRANSACTION.|  
|193|Background Job Error|Viene generato quando un processo in background termina in modo anomalo.|  
|194|OLEDB Provider Information|Viene generato quando si esegue una query distribuita e tale query raccoglie informazioni corrispondenti alla connessione del provider.|  
|195|Mount Tape|Viene generato alla ricezione di una richiesta di montaggio nastro.|  
|196|Assembly Load|Viene generato in presenza di una richiesta di caricamento di un assembly CLR.|  
|197|Riservato||  
|198|XQuery Static Type|Viene generato quando si esegue un'espressione XQuery. Tramite questa classe di evento viene fornito il tipo statico dell'espressione XQuery.|  
|199|QN: subscription|Viene generato quando non è possibile sottoscrivere una registrazione di query. La colonna **TextData** contiene informazioni sull'evento.|  
|200|QN: parameter table|Le informazioni sulle sottoscrizioni attive vengono archiviate in tabelle di parametri interne. Questa classe di evento viene generata per la creazione o l'eliminazione di una tabella di parametri. In genere, queste tabelle vengono create o eliminate in caso di riavvio del database. La colonna **TextData** contiene informazioni sull'evento.|  
|201|QN: template|Un modello di query rappresenta una classe di query di sottoscrizione. In genere, le query della stessa classe sono identiche con l'eccezione dei valori dei parametri. Questa classe di evento viene generata quando una nuova richiesta di sottoscrizione rientra in una classe già esistente (Match), in una nuova classe (Create) o in una classe Drop, che indica la pulizia dei riferimenti ai modelli per le classi di query senza sottoscrizioni attive. La colonna **TextData** contiene informazioni sull'evento.|  
|202|QN: dynamics|Tiene traccia delle attività interne di notifica delle query. La colonna **TextData** contiene informazioni sull'evento.|  
|212|Avviso bitmap|Indica quando i filtri bitmap sono stati disabilitati in una query.|  
|213|Database Suspect Data Page|Indica quando una pagina viene aggiunta alla tabella **suspect_pages** in **msdb**.|  
|214|CPU threshold exceeded|Indica quando Resource Governor rileva che una query ha superato il valore soglia della CPU (REQUEST_MAX_CPU_TIME_SEC).|  
|215|PreConnect:Starting|Indica quando un trigger LOGON o la funzione di classificazione di Resource Governor avvia l'esecuzione.|  
|216|PreConnect:Completed|Indica quando un trigger LOGON o la funzione di classificazione di Resource Governor completa l'esecuzione.|  
|217|Plan Guide Successful|Indica che in SQL Server è stato correttamente eseguito un piano di esecuzione per una query o un batch contenente una guida di piano.|  
|218|Plan Guide Unsuccessful|Indica che in SQL Server non è stato possibile creare un piano di esecuzione per una query o un batch contenente una guida di piano. SQL Server ha tentato di generare un piano di esecuzione per questa query o batch senza applicare la guida di piano. Una guida di piano non valida potrebbe essere la causa di questo problema. È possibile convalidare la guida di piano utilizzando la funzione di sistema sys.fn_validate_plan_guide.|  
|235|Audit Fulltext||  
  
`[ @columnid = ] column_id` ID della colonna da aggiungere per l'evento. *column_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
 Nella tabella seguente sono incluse le colonne che è possibile aggiungere per un evento.  
  
|Numero di colonna|Nome colonna|Descrizione|  
|-------------------|-----------------|-----------------|  
|1|**TextData**|Valore di testo che dipende dalla classe di evento acquisita nella traccia.|  
|2|**BinaryData**|Valore binario che dipende dalla classe di evento acquisita nella traccia.|  
|3|**DatabaseID**|ID del database specificato nell'istruzione USE *database* oppure il database predefinito se per una determinata connessione non viene eseguita un'istruzione USE *database* .<br /><br /> È possibile determinare l'ID di un database utilizzando la funzione DB_ID.|  
|4|**TransactionID**|ID della transazione assegnato dal sistema.|  
|5|**LineNumber**|Contiene il numero della riga contenente l'errore. Per gli eventi associati a istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] , come **SP:StmtStarting**, **LineNumber** contiene il numero di riga dell'istruzione nella stored procedure o nel batch.|  
|6|**NTUserName**|Nome utente di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|  
|7|**NTDomainName**|Dominio Windows di appartenenza dell'utente.|  
|8|**HostName**|Nome del computer client che ha eseguito la richiesta.|  
|9|**ClientProcessID**|ID assegnato dal computer client al processo in cui è in esecuzione l'applicazione client.|  
|10|**ApplicationName**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|  
|11|**LoginName**|Nome dell'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del client.|  
|12|**SPID**|ID del processo server assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al processo associato al client.|  
|13|**Duration**|Durata dell'evento in microsecondi. Questa colonna di dati non viene popolata dall'evento Hash Warning.|  
|14|**StartTime**|Ora di inizio dell'evento, se disponibile.|  
|15|**EndTime**|Ora di fine dell'evento. Questa colonna non viene popolata per le classi degli eventi di avvio, come **SQL:BatchStarting** o **SP:Starting**. Inoltre, non viene popolata dall'evento **Hash Warning** .|  
|16|**Reads**|Numero di letture logiche del disco eseguite dal server per conto dell'evento. Questa colonna non viene popolata dall'evento **Lock: Released** .|  
|17|**Writes**|Numero di scritture fisiche su disco eseguite dal server per conto dell'evento.|  
|18|**CPU**|Tempo della CPU in millisecondi utilizzato dall'evento.|  
|19|**Autorizzazioni**|Rappresenta la mappa di bit delle autorizzazioni e viene utilizzata per il controllo di sicurezza.|  
|20|**Gravità**|Livello di gravità di un'eccezione.|  
|21|**EventSubClass**|Tipo di sottoclasse di evento. Questa colonna di dati non viene popolata per tutte le classi di evento.|  
|22|**ObjectID**|ID dell'oggetto assegnato dal sistema.|  
|23|**Success**|Esito del tentativo di utilizzo delle autorizzazioni; valore utilizzato per il controllo.<br /><br /> **1** = esito positivo **0** = esito negativo|  
|24|**IndexID**|ID dell'indice dell'oggetto interessato dall'evento. Per determinare l'ID di indice di un oggetto, utilizzare la colonna **indid** della tabella di sistema **sysindexes** .|  
|25|**IntegerData**|Valore integer che dipende dalla classe di evento acquisita nella traccia.|  
|26|**ServerName**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ovvero *ServerName* o *nomeserver\nomeistanza*, tracciata.|  
|27|**EventClass**|Tipo di classe di evento che viene registrato.|  
|28|**ObjectType**|Tipo di oggetto, ad esempio tabella, funzione o stored procedure.|  
|29|**NestLevel**|Livello di nidificazione in cui viene eseguita la stored procedure. Vedere [@ @NESTLEVEL &#40;&#41;Transact-SQL ](../../t-sql/functions/nestlevel-transact-sql.md).|  
|30|**State**|Stato del server in caso di errore.|  
|31|**Error (Errore) (Error (Errore)e)**|Numero di errore.|  
|32|**Modalità**|Modalità del blocco acquisito. Questa colonna non viene popolata dall'evento **Lock: Released** .|  
|33|**Handle**|Handle dell'oggetto a cui si fa riferimento nell'evento.|  
|34|**ObjectName**|Nome dell'oggetto a cui si accede.|  
|35|**DatabaseName**|Nome del database specificato nell'istruzione USE *database* .|  
|36|**FileName**|Nome logico del nome di file modificato.|  
|37|**OwnerName**|Nome del proprietario dell'oggetto a cui si fa riferimento.|  
|38|**RoleName**|Nome del ruolo del database o del server a cui viene applicata un'istruzione.|  
|39|**TargetUserName**|Nome utente della destinazione di un'azione.|  
|40|**DBUserName**|Nome utente del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del client.|  
|41|**LoginSid**|ID di sicurezza (SID) dell'utente connesso.|  
|42|**TargetLoginName**|Nome dell'account di accesso della destinazione di un'azione.|  
|43|**TargetLoginSid**|SID dell'account di accesso che rappresenta la destinazione di un'azione.|  
|44|**ColumnPermissions**|Stato delle autorizzazioni a livello di colonna; valore utilizzato per il controllo di sicurezza.|  
|45|**LinkedServerName**|Nome del server collegato.|  
|46|**ProviderName**|Nome del provider OLE DB.|  
|47|**MethodName**|Nome del metodo OLE DB.|  
|48|**RowCounts**|Numero di righe nel batch.|  
|49|**RequestID**|ID della richiesta contenente l'istruzione.|  
|50|**XactSequence**|Token utilizzato per descrivere la transazione corrente.|  
|51|**EventSequence**|Numero di sequenza dell'evento.|  
|52|**BigintData1**|valore **bigint** , che dipende dalla classe di evento acquisita nella traccia.|  
|53|**BigintData2**|valore **bigint** , che dipende dalla classe di evento acquisita nella traccia.|  
|54|**GUID**|Valore GUID che dipende dalla classe di evento acquisita nella traccia.|  
|55|**IntegerData2**|Valore intero che dipende dalla classe di evento acquisita nella traccia.|  
|56|**ObjectID2**|ID dell'entità o dell'oggetto correlato, se disponibile.|  
|57|**Tipo**|Valore intero che dipende dalla classe di evento acquisita nella traccia.|  
|58|**OwnerID**|Tipo di oggetto proprietario del blocco. Solo per gli eventi di blocco.|  
|59|**ParentName**|Nome dello schema in cui è incluso l'oggetto.|  
|60|**IsSystem**|Indica se l'evento è stato generato per un processo di sistema o un processo utente.<br /><br /> **1** = sistema<br /><br /> **0** = utente.|  
|61|**Offset**|Offset iniziale dell'istruzione nella stored procedure o nel batch.|  
|62|**SourceDatabaseID**|ID del database in cui esiste l'origine dell'oggetto.|  
|63|**SqlHandle**|Hash a 64 bit basato sul testo di una query ad hoc oppure ID del database e dell'oggetto di un oggetto SQL. È possibile passare questo valore a **sys.dm_exec_sql_text()** per recuperare il testo SQL associato.|  
|64|**SessionLoginName**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se, ad esempio, si esegue la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso **Login1** e si esegue un'istruzione con l'account di accesso **Login2**, **SessionLoginName** indica **Login1**, mentre **LoginName** indica **Login2**. In questa colonna vengono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|  
  
 **[ @on =]** *in*  
 Specifica se impostare l'evento su ON (1) oppure OFF (0). *on* è di **bit** e non prevede alcun valore predefinito.  
  
 Se *on* è impostato su **1** e *column_id* è null, l'evento viene impostato su on e tutte le colonne vengono cancellate. Se *column_id* non è null, la colonna viene impostata su on per l'evento.  
  
 Se *on* è impostato su **0** e *column_id* è null, l'evento viene disattivato e tutte le colonne vengono cancellate. Se *column_id* non è null, la colonna viene disattivata.  
  
 Questa tabella illustra l'interazione tra **\@ in** e **\@ ColumnID**.  
  
|\@in|\@ColumnID|Risultato|  
|---------|---------------|------------|  
|ON (**1**)|NULL|L'evento viene abilitato.<br /><br /> Tutte le colonne vengono cancellate.|  
||NOT NULL|La colonna viene abilitata per l'evento specificato.|  
|OFF (**0**)|NULL|L'evento viene disabilitato.<br /><br /> Tutte le colonne vengono cancellate.|  
||NOT NULL|La colonna viene disabilitata per l'evento specificato.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nella tabella seguente vengono descritti i possibili valori di codice visualizzati al completamento della stored procedure.  
  
|Codice restituito|Descrizione|  
|-----------------|-----------------|  
|0|Nessun errore.|  
|1|Errore sconosciuto.|  
|2|La traccia è in esecuzione. Se si modifica la traccia mentre è in esecuzione, verrà generato un errore.|  
|3|L'evento specificato non è valido, in quanto non esiste oppure non è appropriato per la stored procedure.|  
|4|La colonna specificata non è valida.|  
|9|L'handle di traccia specificato non è valido.|  
|11|La colonna specificata viene utilizzata internamente e non può essere rimossa.|  
|13|Memoria insufficiente. Restituito quando la quantità di memoria disponibile non è sufficiente per eseguire l'azione specificata.|  
|16|Funzione non valida per la traccia.|  
  
## <a name="remarks"></a>Commenti  
 **sp_trace_setevent** esegue molte delle azioni eseguite in precedenza dalle stored procedure estese disponibili nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Usare **sp_trace_setevent** anziché quanto segue:  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_eventclassrequired**  
  
-   **xp_trace_seteventclassrequired**  
  
 Gli utenti devono eseguire **sp_trace_setevent** per ogni colonna aggiunta per ogni evento. Durante ogni esecuzione, se **\@ on** è impostato su **1**, **sp_trace_setevent** aggiunge l'evento specificato all'elenco di eventi della traccia. Se è impostato **\@ su** **0**, **sp_trace_setevent** rimuove l'evento specificato dall'elenco.  
  
 I parametri di tutte le stored procedure di traccia SQL (**sp_trace_xx**) sono fortemente tipizzati. Se questi parametri non vengono chiamati con i tipi di dati corretti per i parametri di input, come indicato nella descrizione dell'argomento, la stored procedure restituirà un errore.  
  
 Per un esempio dell'uso di stored procedure relative alla traccia, vedere [Creare una traccia &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre delle autorizzazioni ALTER TRACE.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fn_trace_geteventinfo &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [Guida di riferimento alle classi di evento SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [Traccia SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  

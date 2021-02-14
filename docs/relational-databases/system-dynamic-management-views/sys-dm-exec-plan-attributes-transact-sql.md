---
description: sys.dm_exec_plan_attributes (Transact-SQL)
title: sys.dm_exec_plan_attributes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_plan_attributes_TSQL
- dm_exec_plan_attributes_TSQL
- dm_exec_plan_attributes
- sys.dm_exec_plan_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_plan_attributes dynamic management function
ms.assetid: dacf3ab3-f214-482e-aab5-0dab9f0a3648
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0bbf64478b1ce114419df3e65c26ff453b3cfc5e
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342968"
---
# <a name="sysdm_exec_plan_attributes-transact-sql"></a>sys.dm_exec_plan_attributes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni attributo del piano specificato dall'handle di piano. È possibile utilizzare questa funzione con valori di tabella per recuperare informazioni dettagliate su un particolare piano, come i valori di chiave nella cache o il numero corrente di esecuzioni simultanee del piano.  
  
> [!NOTE]  
>  Alcune delle informazioni restituite tramite questa funzione sono mappate alla vista di compatibilità con le versioni precedenti di [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) .

## <a name="syntax"></a>Sintassi  
```  
sys.dm_exec_plan_attributes ( plan_handle )  
```  
  
## <a name="arguments"></a>Argomenti  
 *plan_handle*  
 Viene identificato in modo univoco un piano di query per un batch eseguito il cui piano risiede nella cache dei piani. *plan_handle* è di tipo **varbinary (64)**. È possibile ottenere l'handle del piano dalla vista a gestione dinamica [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) .  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|Attributo|**varchar(128)**|Nome dell'attributo associato al piano. La tabella immediatamente sotto questo elenco elenca i possibili attributi, i relativi tipi di dati e le relative descrizioni.|  
|Valore|**sql_variant**|Valore dell'attributo associato al piano.|  
|is_cache_key|**bit**|Indica se l'attributo viene utilizzato come parte della chiave di ricerca nella cache per il piano.|  

Nella tabella precedente, l' **attributo** può avere i valori seguenti:

|Attributo|Tipo di dati|Descrizione|  
|---------------|---------------|-----------------|  
|set_options|**int**|Indica i valori delle opzioni con cui è stato compilato il piano.|  
|objectid|**int**|Una delle chiavi principali utilizzate per la ricerca di un oggetto nella cache. Si tratta dell'ID oggetto archiviato in [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) per gli oggetti di database (procedure, viste, trigger e così via). Per i piani di tipo ad hoc o preparati, questo attributo corrisponde a un hash interno del testo del batch.|  
|dbid|**int**|ID del database contenente l'entità alla quale fa riferimento il piano.<br /><br /> Per i piani ad hoc o preparati, corrisponde all'ID del database da cui viene eseguito il batch.|  
|dbid_execute|**int**|Per gli oggetti di sistema archiviati nel database **Resource** , l'ID del database da cui viene eseguito il piano memorizzato nella cache. In tutti gli altri casi è 0.|  
|user_id|**int**|Il valore -2 indica che il batch inviato non dipende dalla risoluzione implicita del nome e può essere condiviso da diversi utenti. Questo è il metodo preferito. Qualsiasi altro valore rappresenta l'ID dell'utente che invia la query al database.| 
|language_id|**smallint**|ID della lingua della connessione in cui è stato creato l'oggetto della cache. Per ulteriori informazioni, vedere [ linguaggisys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|date_format|**smallint**|Formato della data della connessione in cui è stato creato l'oggetto della cache. Per altre informazioni, vedere [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|date_first|**tinyint**|Primo valore di data. Per altre informazioni, vedere [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|status|**int**|Bit di stato interni che fanno parte della chiave di ricerca nella cache.|  
|required_cursor_options|**int**|Opzioni di cursore specificate dall'utente, ad esempio il tipo di cursore.|  
|acceptable_cursor_options|**int**|Opzioni di cursore che potrebbero essere convertite in modo implicito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per supportare l'esecuzione dell'istruzione. Ad esempio, l'utente potrebbe specificare un cursore dinamico, ma Query Optimizer è autorizzato a convertire questo tipo di cursore in un cursore statico.|  
|inuse_exec_context|**int**|Numero di batch in esecuzione che utilizzano il piano di query.|  
|free_exec_context|**int**|Numero di contesti di esecuzione memorizzati nella cache per il piano di query, attualmente inutilizzati.|  
|hits_exec_context|**int**|Numero di riutilizzi del contesto di esecuzione recuperato dalla cache dei piani, con conseguente risparmio dell'overhead correlato alla ricompilazione dell'istruzione SQL. Il valore rappresenta un'aggregazione per tutti i batch eseguiti finora.|  
|misses_exec_context|**int**|Numero di volte in cui non è stato possibile trovare un contesto di esecuzione nella cache dei piani, con conseguente creazione di un nuovo contesto di esecuzione per l'esecuzione del batch.|  
|removed_exec_context|**int**|Numero di contesti di esecuzione rimossi a causa di richieste di memoria eccessive per il piano memorizzato nella cache.|  
|inuse_cursors|**int**|Numero di batch in esecuzione che contengono uno o più cursori che utilizzano il piano memorizzato nella cache.|  
|free_cursors|**int**|Numero di cursori inattivi o liberi per il piano memorizzato nella cache.|  
|hits_cursors|**int**|Numero di riutilizzi di un cursore inattivo ottenuto dal piano memorizzato nella cache. Il valore rappresenta un'aggregazione per tutti i batch eseguiti finora.|  
|misses_cursors|**int**|Numero di volte in cui non è stato possibile trovare un cursore inattivo nella cache.|  
|removed_cursors|**int**|Numero di cursori rimossi a causa di richieste di memoria eccessive per il piano memorizzato nella cache.|  
|sql_handle|**varbinary**(64)|Handle SQL per il batch.|  
|merge_action_type|**smallint**|Il tipo di piano di esecuzione del trigger utilizzato come risultato di un'istruzione MERGE.<br /><br /> 0 indicano un piano non-trigger, un piano del trigger che non viene eseguito come risultato di un'istruzione MERGE o un piano del trigger che viene eseguito come risultato di un'istruzione MERGE in cui viene specificata solo un'azione DELETE.<br /><br /> 1 indica un piano di trigger INSERT che è in esecuzione come risultato di un'istruzione MERGE.<br /><br /> 2 indica un piano di trigger UPDATE in esecuzione come risultato di un'istruzione MERGE.<br /><br /> 3 indica un piano di trigger DELETE che viene eseguito come risultato di un'istruzione MERGE che contiene un'azione INSERT o UPDATE corrispondente.<br /><br /> Per i trigger nidificati eseguiti da azioni a catena, questo valore è l'azione dell'istruzione MERGE che provoca la propagazione.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="remarks"></a>Commenti  
  
## <a name="set-options"></a>Opzioni SET  
 Le copie dello stesso piano compilato potrebbero differire solo per il valore nella colonna **set_options** . Ciò indica che connessioni diverse utilizzano set di opzioni SET diversi per la stessa query. L'utilizzo di set diversi di opzioni è in genere poco consigliabile perché può causare compilazioni aggiuntive, un minore riutilizzo del piano e un aumento delle dimensioni della cache dei piani in seguito alla presenza di più copie dei piani.  
  
### <a name="evaluating-set-options"></a>Valutazione delle opzioni SET  
 Per convertire il valore restituito in **set_options** alle opzioni con cui è stato compilato il piano, sottrarre i valori dal valore **set_options** , iniziando con il valore massimo possibile, fino a quando non si raggiunge 0. Ogni valore sottratto corrisponde a un'opzione utilizzata nel piano di query. Se, ad esempio, il valore in **set_options** è 251, le opzioni con cui è stato compilato il piano sono ANSI_NULL_DFLT_ON (128), QUOTED_IDENTIFIER (64), ANSI_NULLS (32), ANSI_WARNINGS (16), CONCAT_NULL_YIELDS_NULL (8), piano parallelo (2) e ANSI_PADDING (1).  
  
|Opzione|Valore|  
|------------|-----------|  
|ANSI_PADDING|1|  
|ParallelPlan<br /><br /> Indica che le opzioni di parallelismo del piano sono state modificate.|2|  
|FORCEPLAN|4|  
|CONCAT_NULL_YIELDS_NULL|8|  
|ANSI_WARNINGS|16|  
|ANSI_NULLS|32|  
|QUOTED_IDENTIFIER|64|  
|ANSI_NULL_DFLT_ON|128|  
|ANSI_NULL_DFLT_OFF|256|  
|NoBrowseTable<br /><br /> Indica che il piano non utilizza una tabella di lavoro per implementare un'operazione FOR BROWSE.|512|  
|TriggerOneRow<br /><br /> Indica che il piano contiene l'ottimizzazione della singola riga per le tabelle delta del trigger AFTER.|1024|  
|ResyncQuery<br /><br /> Indica che la query è stata inviata da stored procedure di sistema interne.|2048|  
|ARITH_ABORT|4096|  
|NUMERIC_ROUNDABORT|8192|  
|DATEFIRST|16384|  
|DATEFORMAT|32768|  
|LanguageID|65536|  
|UPON<br /><br /> Indica che l'opzione di database PARAMETERIZATION era impostata su FORCED al momento della compilazione del piano.|131072|  
|ROWCOUNT|**Si applica a:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] A [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /><br /> 262144|  
  
## <a name="cursors"></a>Cursori  
 I cursori inattivi vengono memorizzati nella cache in un piano compilato in modo che la memoria utilizzata per archiviare il cursore possa essere riutilizzata da utenti simultanei dei cursori. Si supponga, ad esempio, che un batch dichiari e utilizzi un cursore senza deallocarlo. Se due utenti eseguono lo stesso batch, saranno presenti due cursori attivi. Dopo la deallocazione dei cursori, potenzialmente in batch diversi, la memoria utilizzata per archiviare il cursore viene assegnata alla cache e non rilasciata. Questo elenco dei cursori inattivi viene mantenuto nel piano compilato. In occasione della successiva esecuzione del batch, la memoria per il cursore nella cache verrà riutilizzata e inizializzata in modo appropriato come cursore attivo.  
  
### <a name="evaluating-cursor-options"></a>Valutazione delle opzioni di cursore  
 Per convertire il valore restituito in **required_cursor_options** e **acceptable_cursor_options** alle opzioni con cui è stato compilato il piano, sottrarre i valori dal valore della colonna, iniziando con il valore massimo possibile, fino a quando non si raggiunge 0. Ogni valore sottratto corrisponde a un'opzione di cursore utilizzata nel piano di query.  
  
|Opzione|Valore|  
|------------|-----------|  
|nessuno|0|  
|INSENSITIVE|1|  
|SCROLL|2|  
|READ ONLY|4|  
|FOR UPDATE|8|  
|LOCAL|16|  
|GLOBAL|32|  
|FORWARD_ONLY|64|  
|KEYSET|128|  
|DYNAMIC|256|  
|SCROLL_LOCKS|512|  
|OPTIMISTIC|1024|  
|STATIC|2048|  
|FAST_FORWARD|4096|  
|IN PLACE|8192|  
|FOR *select_statement*|16384|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-attributes-for-a-specific-plan"></a>R. Restituzione degli attributi per un piano specifico  
 Nell'esempio seguente vengono restituiti tutti gli attributi per un piano specificato. Viene prima di tutto eseguita una query sulla vista a gestione dinamica `sys.dm_exec_cached_plans` per ottenere l'handle del piano specificato. Nella seconda query, sostituire `<plan_handle>` con il valore di handle di piani restituito dalla prima query.  
  
```sql  
SELECT plan_handle, refcounts, usecounts, size_in_bytes, cacheobjtype, objtype   
FROM sys.dm_exec_cached_plans;  
GO  
SELECT attribute, value, is_cache_key  
FROM sys.dm_exec_plan_attributes(<plan_handle>);  
GO  
```  
  
### <a name="b-returning-the-set-options-for-compiled-plans-and-the-sql-handle-for-cached-plans"></a>B. Restituzione delle opzioni SET per i piani compilati e dell'handle SQL per i piani memorizzati nella cache  
 Nell'esempio seguente viene restituito un valore che rappresenta le opzioni con cui è stato compilato ogni piano. Inoltre, viene restituito l'handle SQL per tutti i piani memorizzati nella cache.  
  
```sql  
SELECT plan_handle, pvt.set_options, pvt.sql_handle  
FROM (  
    SELECT plan_handle, epa.attribute, epa.value   
    FROM sys.dm_exec_cached_plans   
        OUTER APPLY sys.dm_exec_plan_attributes(plan_handle) AS epa  
    WHERE cacheobjtype = 'Compiled Plan') AS ecpa   
PIVOT (MAX(ecpa.value) FOR ecpa.attribute IN ("set_options", "sql_handle")) AS pvt;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
  


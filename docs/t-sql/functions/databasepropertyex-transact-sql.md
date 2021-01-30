---
description: DATABASEPROPERTYEX (Transact-SQL)
title: DATABASEPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASEPROPERTYEX
- DATABASEPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DATABASEPROPERTYEX function
- displaying database properties
- database properties [SQL Server]
ms.assetid: 8a9e0ffb-28b5-4640-95b2-a54e3e5ad941
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fa57771189978abed741d27bbeba153e974545ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183616"
---
# <a name="databasepropertyex-transact-sql"></a>DATABASEPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Per un database specificato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], questa funzione restituisce l'impostazione corrente dell'opzione o proprietà del database specificata.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATABASEPROPERTYEX ( database , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*database*  
Espressione che specifica il nome del database per il quale `DATABASEPROPERTYEX` restituisce le informazioni sulla proprietà denominata. *database* ha il tipo di dati **nvarchar(128)** .  

Per [!INCLUDE[ssSDS](../../includes/sssds-md.md)], `DATABASEPROPERTYEX` richiede il nome del database corrente. Se viene specificato un nome di database diverso, restituisce NULL per tutte le proprietà.
  
*property*  
Espressione che specifica il nome della proprietà del database da restituire. *property* ha il tipo di dati **varchar(128)** e supporta uno dei valori in questa tabella:
  
> [!NOTE]  
>  Se il database non è ancora avviato, le chiamate a `DATABASEPROPERTYEX` restituiscono NULL se `DATABASEPROPERTYEX` recupera tali valori mediante accesso diretto al database, anziché tramite i metadati. Un database con l'opzione AUTO_CLOSE impostata su ON o comunque non in linea è considerato come non avviato.  
  
|Proprietà|Descrizione|Valore restituito|  
|---|---|---|
|Regole di confronto|Nome delle regole di confronto predefinite per il database.|Nome delle regole di confronto.<br /><br /> NULL: database non avviato.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ComparisonStyle|Stile di confronto di Windows per le regole di confronto. Usare i seguenti valori di stile per creare una mappa di bit per il valore ComparisonStyle terminato:<br /><br /> Ignora maiuscole/minuscole: 1<br /><br /> Ignora accento: 2<br /><br /> Ignora Kana: 65536<br /><br /> Ignora larghezza: 131072<br /><br /> <br /><br /> Il valore predefinito 196609, ad esempio, è il risultato della combinazione delle opzioni Ignora maiuscole/minuscole, Ignora Kana e Ignora larghezza.|Restituisce lo stile di confronto.<br /><br /> Restituisce 0 per tutte le regole di confronto binarie.<br /><br /> Tipo di dati di base: **int**|  
|Edizione|Edizione o livello di servizio del database.|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br /> <br /><br /> Utilizzo generico<br /><br /> Business Critical<br /><br /> Basic<br /><br /> Standard<br /><br /> Premium<br /><br /> System (per il database master)<br /><br /> NULL: database non avviato.<br /><br /> Tipo di dati di base: **nvarchar**(64)|  
|IsAnsiNullDefault|Il database segue le regole ISO per il supporto dei valori Null.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAnsiNullsEnabled|Tutti i confronti con un valore Null restituiscono unknown.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAnsiPaddingEnabled|Le stringhe vengono riempite in modo che abbiano tutte la stessa lunghezza prima dell'esecuzione di confronti o inserimenti.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAnsiWarningsEnabled|SQL Server visualizza messaggi di errore o di avviso se si verificano condizioni di errore standard.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsArithmeticAbortEnabled|Le query vengono interrotte se durante l'esecuzione si verifica un errore di overflow o di divisione per zero.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAutoClose|Il database viene chiuso correttamente e le risorse corrispondenti vengono liberate dopo la disconnessione dell'ultimo utente.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAutoCreateStatistics|In Query Optimizer vengono create statistiche di colonna singola, se necessario, per migliorare le prestazioni di esecuzione delle query.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAutoCreateStatisticsIncremental|Se possibile, le statistiche a colonna singola create automaticamente sono incrementali.|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAutoShrink|I file di database sono sottoposti periodicamente a compattazione automatica.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsAutoUpdateStatistics|Quando una query usa statistiche esistenti potenzialmente non aggiornate, Query Optimizer aggiorna tali statistiche.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|
|IsClone|Il database è una copia di un database utente contenente solo schema e statistiche, creato con DBCC CLONEDATABASE. Vedere questo [articolo del supporto tecnico Microsoft](https://support.microsoft.com/help/3177838).|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 e versioni successive.<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**| 
|IsCloseCursorsOnCommitEnabled|Quando viene eseguito il commit di una transazione, tutti i cursori aperti vengono chiusi.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsFulltextEnabled|Il database è abilitato per l'indicizzazione full-text e semantica.|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**<br /><br /> **Nota:** ora il valore di questa proprietà non ha alcun effetto. I database utente sono sempre abilitati per la ricerca full-text. La proprietà verrà rimossa in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non usare questa proprietà in un nuovo progetto di sviluppo e modificare prima possibile le applicazioni in cui viene attualmente usata.|  
|IsInStandBy|Il database è online in sola lettura e consente il ripristino di log.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsLocalCursorsDefault|Le dichiarazioni del cursore hanno come impostazione predefinita LOCAL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsMemoryOptimizedElevateToSnapshotEnabled|Le tabelle con ottimizzazione per la memoria sono accessibili tramite l'isolamento SNAPSHOT quando l'impostazione della sessione HIGH TRANSACTION ISOLATION LEVEL è impostata su READ COMMITTED, READ UNCOMMITTED o su un livello di isolamento inferiore.|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> Tipo di dati di base: **int**|  
|IsMergePublished|Se la replica è installata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta la pubblicazione delle tabelle di un database per l'esecuzione di una replica di tipo merge.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsNullConcat|Un operando di concatenazione Null produce un valore NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsNumericRoundAbortEnabled|Se si verifica una perdita di precisione nelle espressioni, vengono generati errori.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsParameterizationForced|L'opzione SET dell'hint di database PARAMETERIZATION è impostata su FORCED.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido|  
|IsQuotedIdentifiersEnabled|L'uso delle virgolette doppie negli identificatori è consentito.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsPublished|Se la replica è installata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta la pubblicazione delle tabelle del database per l'esecuzione di una replica snapshot o transazionale.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsRecursiveTriggersEnabled|L'attivazione ricorsiva dei trigger è abilitata.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsSubscribed|Il database è sottoscritto a una pubblicazione.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsSyncWithBackup|Indica se il database è un database pubblicato o di distribuzione e se supporta un ripristino senza ripercussioni sulla replica transazionale.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**|  
|IsTornPageDetectionEnabled|In [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] vengono rilevate le operazioni di I/O non completate a causa di un'interruzione dell'alimentazione o di altri malfunzionamenti del sistema.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**| 
|IsVerifiedClone|Il database è una copia di un database utente contenente solo schema e statistiche, creato con l'opzione WITH VERIFY_CLONEDB di DBCC CLONEDATABASE. Per altre informazioni, vedere questo [articolo del supporto tecnico Microsoft](https://support.microsoft.com/help/3177838).|**Si applica a**: A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **int**| 
|IsXTPSupported|Indica se il database supporta OLTP in memoria, ovvero la creazione e l'uso di tabelle ottimizzate per la memoria e moduli compilati in modo nativo.<br /><br /> Specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:<br /><br /> IsXTPSupported è indipendente dall'esistenza di qualsiasi filegroup MEMORY_OPTIMIZED_DATA, necessario per la creazione di oggetti OLTP In memoria.|**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido, errore o non applicabile<br /><br /> Tipo di dati di base: **int**|  
|LastGoodCheckDbTime|Data e ora dell'ultima esecuzione con esito positivo di DBCC CHECKDB nel database specificato.<sup>1</sup> Se il comando DBCC CHECKDB non è stato eseguito in un database, viene restituito 1900-01-01 00:00:00.000.|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] a partire da SP2.</br>[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] a partire da CU9.</br>[!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] o versioni successive.</br>Database SQL di Azure.<br/><br/>Valore datetime<br /><br /> NULL: input non valido<br /><br /> Tipo di dati di base: **datetime**| 
|LCID|Identificatore delle impostazioni locali (LCID) di Windows per le regole di confronto.|Valore LCID, in formato decimale.<br /><br /> Tipo di dati di base: **int**|  
|MaxSizeInBytes|Dimensioni massime del database in byte.|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br />[Database SQL di Azure e Azure Synapse Analytics](/azure/sql-database/sql-database-single-database-scale#dtu-based-purchasing-model): il valore è basato su SLO, a meno che non sia stata acquistata una risorsa di archiviazione aggiuntiva.<br /><br />[vCore](/azure/sql-database/sql-database-single-database-scale#vcore-based-purchasing-model): il valore è espresso in incrementi di 1 GB fino alle dimensioni massime.<br /><br />NULL: database non avviato<br /><br /> Tipo di dati di base: **bigint**|  
|Ripristino|Modello di recupero del database|FULL: Modello di recupero con registrazione completa<br /><br /> BULK_LOGGED: modello con registrazione minima delle operazioni bulk<br /><br /> SIMPLE: Modello di recupero con registrazione minima<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|ServiceObjective|Descrive il livello di prestazioni del database nel [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] o in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].|I tipi validi sono:<br /><br /> Null = database non avviato<br /><br /> Shared (per le edizioni Web/Business)<br /><br /> Basic<br /><br /> S0<br /><br /> S1<br /><br /> S2<br /><br /> S3<br /><br /> P1<br /><br /> P2<br /><br /> P3<br /><br /> ElasticPool<br /><br /> System (per il database master)<br /><br /> Tipo di dati di base: **nvarchar(32)**|  
|ServiceObjectiveId|ID dell'obiettivo di servizio in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].|**uniqueidentifier** che identifica l'obiettivo di servizio.|  
|SQLSortOrder|ID del tipo di ordinamento supportato nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|0: il database usa le regole di confronto di Windows<br /><br /> >0: ID del tipo di ordinamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NULL: input non valido o database non avviato<br /><br /> Tipo di dati di base: **tinyint**|  
|Stato|Stato del database.|ONLINE: il database è disponibile per le query.<br /><br /> **Nota:** La funzione può restituire lo stato ONLINE durante l'apertura del database e mentre questo non è ancora stato recuperato. Per sapere se un database ONLINE è in grado di accettare connessioni, eseguire una query sulla proprietà Collation di **DATABASEPROPERTYEX**. Il database ONLINE può accettare le connessioni quando usando le regole di confronto del database viene restituito un valore non Null. Per i database Always On, eseguire una query sulle colonne database_state o database_state_desc di `sys.dm_hadr_database_replica_states`.<br /><br /> OFFLINE: il database è stato portato offline in modo esplicito.<br /><br /> RESTORING: è stato avviato il ripristino del database.<br /><br /> RECOVERING: è stato avviato il recupero del database e questo non è ancora pronto per le query.<br /><br /> SUSPECT: il recupero del database non è riuscito.<br /><br /> EMERGENCY: il database è in modalità di emergenza con accesso in sola lettura. L'accesso è limitato ai membri del ruolo sysadmin.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|Updateability|Indica se è possibile modificare i dati.|READ_ONLY: il database supporta la lettura ma non la modifica dei dati.<br /><br /> READ_WRITE: il database supporta la lettura e la modifica dei dati.<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|UserAccess|Specifica gli utenti autorizzati ad accedere al database.|SINGLE_USER: solo un utente db_owner, dbcreator o sysadmin alla volta<br /><br /> RESTRICTED_USER: solo i membri dei ruoli db_owner, dbcreator e sysadmin<br /><br /> MULTI_USER: tutti gli utenti<br /><br /> Tipo di dati di base: **nvarchar(128)**|  
|Versione|Numero di versione interno del codice [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con cui è stato creato il database. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|Numero di versione: database aperto.<br /><br /> NULL: database non avviato.<br /><br /> Tipo di dati di base: **int**| 

<br/>   

> [!NOTE]  
> <sup>1</sup> Per i database che fanno parte di un gruppo di disponibilità, `LastGoodCheckDbTime` restituisce la data e ora dell'ultima esecuzione con esito positivo di DBCC CHECKDB nella replica primaria, indipendentemente dalla replica da cui si esegue il comando. 

## <a name="return-types"></a>Tipi restituiti
**sql_variant**
  
## <a name="exceptions"></a>Eccezioni  
Restituisce NULL in caso di errore o se un chiamante non ha l'autorizzazione necessaria per visualizzare l'oggetto.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come `OBJECT_ID` possono restituire NULL se l'utente non ha le autorizzazioni necessarie per l'oggetto. Per altre informazioni, vedere [Configurazione della visibilità dei metadati](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Osservazioni  
`DATABASEPROPERTYEX` restituisce solo un'impostazione della proprietà alla volta. Per visualizzare più impostazioni della proprietà, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).
  
## <a name="examples"></a>Esempi  
  
### <a name="a-retrieving-the-status-of-the-auto_shrink-database-option"></a>R. Recupero dello stato dell'opzione AUTO_SHRINK del database  
Questo esempio restituisce lo stato dell'opzione di database AUTO_SHRINK per il database `AdventureWorks`.
  
```sql
SELECT DATABASEPROPERTYEX('AdventureWorks2014', 'IsAutoShrink');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] Indica che l'opzione AUTO_SHRINK è disattivata.
  
```
------------------  
0  
```  
  
### <a name="b-retrieving-the-default-collation-for-a-database"></a>B. Recupero delle regole di confronto predefinite per un database  
In questo esempio vengono restituiti vari attributi del database `AdventureWorks`.
  
```sql
SELECT   
    DATABASEPROPERTYEX('AdventureWorks2014', 'Collation') AS Collation,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'Edition') AS Edition,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'ServiceObjective') AS ServiceObjective,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'MaxSizeInBytes') AS MaxSizeInBytes  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Collation                     Edition        ServiceObjective  MaxSizeInBytes  
----------------------------  -------------  ----------------  --------------  
SQL_Latin1_General_CP1_CI_AS  DataWarehouse  DW1000            5368709120  
```  
  
## <a name="see-also"></a>Vedere anche
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[Stati del database](../../relational-databases/databases/database-states.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)
  
  

---
description: sp_spaceused (Transact-SQL)
title: sp_spaceused (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_spaceused_TSQL
- sp_spaceused
dev_langs:
- TSQL
helpviewer_keywords:
- sp_spaceused
ms.assetid: c6253b48-29f5-4371-bfcd-3ef404060621
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 480b61493dc38ea91679e590f3abe63bde3a48c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189309"
---
# <a name="sp_spaceused-transact-sql"></a>sp_spaceused (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Visualizza il numero di righe, lo spazio su disco riservato e lo spazio su disco utilizzato per una tabella, una vista indicizzata o una coda di [!INCLUDE[ssSB](../../includes/sssb-md.md)] nel database corrente oppure visualizza lo spazio su disco riservato e utilizzato dall'intero database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
sp_spaceused [[ @objname = ] 'objname' ]   
[, [ @updateusage = ] 'updateusage' ]  
[, [ @mode = ] 'mode' ]  
[, [ @oneresultset = ] oneresultset ]  
[, [ @include_total_xtp_storage = ] include_total_xtp_storage ]
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argomenti  

Per [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] e [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] , è `sp_spaceused` necessario specificare i parametri denominati, ad esempio `sp_spaceused (@objname= N'Table1');` invece di basarsi sulla posizione ordinale dei parametri. 

`[ @objname = ] 'objname'`
   
 Nome completo o non qualificato della tabella, della vista indicizzata o della coda per cui si desidera ottenere informazioni sull'utilizzo dello spazio. Le virgolette sono necessarie solo se viene specificato un nome di oggetto completo. Se viene specificato un nome di oggetto completo, ovvero contenente un nome di database, il nome del database deve essere quello del database corrente.  
Se *ObjName* viene omesso, vengono restituiti i risultati per l'intero database.  
*ObjName* è di **tipo nvarchar (776)** e il valore predefinito è null.  
> [!NOTE]  
> [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] e [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] supportano solo oggetti database e Table.
  
`[ @updateusage = ] 'updateusage'` Indica che è necessario eseguire DBCC UPDATEUSAGE per aggiornare le informazioni sull'utilizzo dello spazio. Quando *ObjName* viene omesso, l'istruzione viene eseguita sull'intero database. in caso contrario, l'istruzione viene eseguita in *ObjName*. I valori possono essere **true** o **false**. *UPDATEUSAGE* è di tipo **varchar (5)** e il valore predefinito è **false**.  
  
`[ @mode = ] 'mode'` Indica l'ambito dei risultati. Per una tabella o un database con estensione, il parametro *mode* consente di includere o escludere la parte remota dell'oggetto. Per ulteriori informazioni, vedere [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
 L'argomento *mode* può includere i valori seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|ALL|Restituisce le statistiche di archiviazione dell'oggetto o del database, inclusi sia la parte locale che la parte remota.|  
|LOCAL_ONLY|Restituisce le statistiche di archiviazione solo della parte locale dell'oggetto o del database. Se l'oggetto o il database non è abilitato per l'estensione, restituisce le stesse statistiche di quando @mode = all.|  
|REMOTE_ONLY|Restituisce le statistiche di archiviazione solo della parte remota dell'oggetto o del database. Questa opzione genera un errore quando si verifica una delle condizioni seguenti:<br /><br /> La tabella non è abilitata per l'estensione.<br /><br /> La tabella è abilitata per l'estensione, ma non è mai stata abilitata la migrazione dei dati. In questo caso, la tabella remota non dispone ancora di uno schema.<br /><br /> L'utente ha eliminato manualmente la tabella remota.<br /><br /> Il provisioning dell'archivio dati remoto ha restituito uno stato di esito positivo, ma in realtà non è riuscito.|  
  
 la *modalità* è **varchar (11)** e il valore predefinito è **n'all''**.  
  
`[ @oneresultset = ] oneresultset` Indica se restituire un singolo set di risultati. L'argomento *oneresultset* può avere i valori seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0|Quando *\@ ObjName* è null o non è specificato, vengono restituiti due set di risultati. Il comportamento predefinito è due set di risultati.|  
|1|Quando *\@ ObjName* = null o non è specificato, viene restituito un singolo set di risultati.|  
  
 *oneresultset* è di **bit** e il valore predefinito è **0**.  

`[ @include_total_xtp_storage] 'include_total_xtp_storage'`
**Si applica a:** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , [!INCLUDE[sssds-md](../../includes/sssds-md.md)] .  
  
 Quando @oneresultset = 1, il parametro @include_total_xtp_storage determina se il singolo ResultSet include colonne per l'archiviazione MEMORY_OPTIMIZED_DATA. Il valore predefinito è 0, ovvero, per impostazione predefinita, se il parametro viene omesso, le colonne XTP non vengono incluse nel set di risultati.  

## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Se *ObjName* viene omesso e il valore di *oneresultset* è 0, vengono restituiti i set di risultati seguenti per fornire informazioni sulle dimensioni correnti del database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nome del database corrente.|  
|**database_size**|**varchar (18)**|Dimensioni del database corrente, espresse in megabyte. **database_size** include i file di dati e di log.|  
|**spazio non allocato**|**varchar (18)**|Spazio nel database non riservato per i relativi oggetti.|  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**riservati**|**varchar (18)**|Quantità totale di spazio allocato per gli oggetti del database.|  
|**data**|**varchar (18)**|Quantità totale di spazio utilizzato per i dati.|  
|**index_size**|**varchar (18)**|Quantità totale di spazio utilizzato per gli indici.|  
|**inutilizzati**|**varchar (18)**|Quantità totale di spazio riservato per gli oggetti del database ma non ancora utilizzato.|  
  
 Se *ObjName* viene omesso e il valore di *oneresultset* è 1, viene restituito il seguente set di risultati singolo per fornire informazioni sulle dimensioni correnti del database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nome del database corrente.|  
|**database_size**|**varchar (18)**|Dimensioni del database corrente, espresse in megabyte. **database_size** include i file di dati e di log.|  
|**spazio non allocato**|**varchar (18)**|Spazio nel database non riservato per i relativi oggetti.|  
|**riservati**|**varchar (18)**|Quantità totale di spazio allocato per gli oggetti del database.|  
|**data**|**varchar (18)**|Quantità totale di spazio utilizzato per i dati.|  
|**index_size**|**varchar (18)**|Quantità totale di spazio utilizzato per gli indici.|  
|**inutilizzati**|**varchar (18)**|Quantità totale di spazio riservato per gli oggetti del database ma non ancora utilizzato.|  
  
 Se viene specificato *ObjName* , viene restituito il set di risultati seguente per l'oggetto specificato.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**nvarchar(128)**|Nome dell'oggetto per cui sono state richieste informazioni sull'utilizzo dello spazio.<br /><br /> Il nome dello schema dell'oggetto non viene restituito. Se il nome dello schema è obbligatorio, utilizzare il [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) o [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) viste a gestione dinamica per ottenere informazioni sulle dimensioni equivalenti.|  
|**rows**|**char (20)**|Numero di righe esistenti nella tabella. Se l'oggetto specificato è una coda di [!INCLUDE[ssSB](../../includes/sssb-md.md)], in questa colonna viene indicato il numero di messaggi presenti nella coda.|  
|**riservati**|**varchar (18)**|Quantità totale di spazio riservato per *ObjName*.|  
|**data**|**varchar (18)**|Quantità totale di spazio utilizzato dai dati in *ObjName*.|  
|**index_size**|**varchar (18)**|Quantità totale di spazio utilizzato dagli indici in *ObjName*.|  
|**inutilizzati**|**varchar (18)**|Quantità totale di spazio riservato per *ObjName* ma non ancora utilizzato.|  
 
Questa è la modalità predefinita, quando non viene specificato alcun parametro. Vengono restituiti i set di risultati seguenti che descrivono in dettaglio le informazioni sulle dimensioni del database su disco. 

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nome del database corrente.|  
|**database_size**|**varchar (18)**|Dimensioni del database corrente, espresse in megabyte. **database_size** include i file di dati e di log. Se nel database è presente un filegroup MEMORY_OPTIMIZED_DATA, sono incluse le dimensioni totali su disco di tutti i file del checkpoint nel filegroup.|  
|**spazio non allocato**|**varchar (18)**|Spazio nel database non riservato per i relativi oggetti. Se nel database è presente un filegroup MEMORY_OPTIMIZED_DATA, sono incluse le dimensioni totali su disco dei file del checkpoint con stato precreato nel filegroup.|  

Spazio utilizzato dalle tabelle nel database: (questo set di risultati non riflette le tabelle ottimizzate per la memoria, poiché non esiste un account per tabella per l'utilizzo del disco) 

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**riservati**|**varchar (18)**|Quantità totale di spazio allocato per gli oggetti del database.|  
|**data**|**varchar (18)**|Quantità totale di spazio utilizzato per i dati.|  
|**index_size**|**varchar (18)**|Quantità totale di spazio utilizzato per gli indici.|  
|**inutilizzati**|**varchar (18)**|Quantità totale di spazio riservato per gli oggetti del database ma non ancora utilizzato.|

Il set di risultati seguente viene restituito **solo se** il database dispone di un filegroup MEMORY_OPTIMIZED_DATA con almeno un contenitore: 

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**xtp_precreated**|**varchar (18)**|Dimensioni totali dei file del checkpoint con stato precreato, in KB. Viene conteggiato per lo spazio non allocato nel database nel suo complesso. [Ad esempio, se sono presenti 600.000 KB di file di checkpoint precreati, questa colonna contiene ' 600000 KB ']|  
|**xtp_used**|**varchar (18)**|Dimensioni totali dei file del checkpoint con stati in fase di costruzione, ACTIVE e destinazione di MERGE, in KB. Si tratta dello spazio su disco usato attivamente per i dati nelle tabelle ottimizzate per la memoria.|  
|**xtp_pending_truncation**|**varchar (18)**|Dimensioni totali dei file del checkpoint con WAITING_FOR_LOG_TRUNCATION di stato, in KB. Si tratta dello spazio su disco utilizzato per i file del checkpoint in attesa di pulizia, quando si verifica il troncamento del log.|

Se *ObjName* viene omesso, il valore di oneresultset è 1 e *include_total_xtp_storage* è 1, viene restituito il seguente set di risultati singolo per fornire informazioni sulle dimensioni correnti del database. Se `include_total_xtp_storage` è 0 (impostazione predefinita), le ultime tre colonne vengono omesse. 

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nome del database corrente.|  
|**database_size**|**varchar (18)**|Dimensioni del database corrente, espresse in megabyte. **database_size** include i file di dati e di log. Se nel database è presente un filegroup MEMORY_OPTIMIZED_DATA, sono incluse le dimensioni totali su disco di tutti i file del checkpoint nel filegroup.|
|**spazio non allocato**|**varchar (18)**|Spazio nel database non riservato per i relativi oggetti. Se nel database è presente un filegroup MEMORY_OPTIMIZED_DATA, sono incluse le dimensioni totali su disco dei file del checkpoint con stato precreato nel filegroup.|  
|**riservati**|**varchar (18)**|Quantità totale di spazio allocato per gli oggetti del database.|  
|**data**|**varchar (18)**|Quantità totale di spazio utilizzato per i dati.|  
|**index_size**|**varchar (18)**|Quantità totale di spazio utilizzato per gli indici.|  
|**inutilizzati**|**varchar (18)**|Quantità totale di spazio riservato per gli oggetti del database ma non ancora utilizzato.|
|**xtp_precreated**|**varchar (18)**|Dimensioni totali dei file del checkpoint con stato precreato, in KB. Questa operazione viene conteggiata per lo spazio non allocato nel database nel suo complesso. Restituisce NULL se il database non dispone di un filegroup memory_optimized_data con almeno un contenitore. *Questa colonna è inclusa solo se @include_total_xtp_storage = 1*.| 
|**xtp_used**|**varchar (18)**|Dimensioni totali dei file del checkpoint con stati in fase di costruzione, ACTIVE e destinazione di MERGE, in KB. Si tratta dello spazio su disco usato attivamente per i dati nelle tabelle ottimizzate per la memoria. Restituisce NULL se il database non dispone di un filegroup memory_optimized_data con almeno un contenitore. *Questa colonna è inclusa solo se @include_total_xtp_storage = 1*.| 
|**xtp_pending_truncation**|**varchar (18)**|Dimensioni totali dei file del checkpoint con WAITING_FOR_LOG_TRUNCATION di stato, in KB. Si tratta dello spazio su disco utilizzato per i file del checkpoint in attesa di pulizia, quando si verifica il troncamento del log. Restituisce NULL se il database non dispone di un filegroup memory_optimized_data con almeno un contenitore. Questa colonna è inclusa solo se `@include_total_xtp_storage=1` .|

## <a name="remarks"></a>Commenti  
 **database_size** è generalmente maggiore della somma dello spazio **riservato**  +  non **allocato** , perché include le dimensioni dei file di log, ma **riservato** e **unallocated_space** considera solo le pagine di dati. In alcuni casi con analisi sinapsi di Azure, questa istruzione potrebbe non essere vera. 
  
 Le pagine utilizzate da indici XML e indici full-text sono incluse in **index_size** per entrambi i set di risultati. Quando si specifica *ObjName* , le pagine per gli indici XML e gli indici full-text per l'oggetto vengono conteggiate anche nei risultati totali **riservati** e **index_size** .  
  
 Se l'utilizzo dello spazio viene calcolato per un database o un oggetto che dispone di un indice spaziale, le colonne di dimensioni dello spazio, ad esempio **database_size**, **riservato** e **index_size**, includono le dimensioni dell'indice spaziale.  
  
 Quando si specifica *UPDATEUSAGE* , l'oggetto [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] analizza le pagine di dati nel database e apporta le eventuali correzioni necessarie alle viste del catalogo **sys.allocation_units** e **sys.** Partitions riguardanti lo spazio di archiviazione utilizzato da ogni tabella. In alcune situazioni, ad esempio dopo l'eliminazione di un indice, le informazioni sullo spazio restituite per la tabella non sono aggiornate. *UPDATEUSAGE* può richiedere del tempo per l'esecuzione in tabelle o database di grandi dimensioni. Utilizzare *UPDATEUSAGE* solo quando si sospetta che vengano restituiti valori non corretti e quando il processo non avrà effetti negativi su altri utenti o processi nel database. Se lo si preferisce, è possibile eseguire l'istruzione DBCC UPDATEUSAGE separatamente.  
  
> [!NOTE]  
>  In caso di eliminazione o ricompilazione di indici di grandi dimensioni oppure di eliminazione o troncamento di tabelle di grandi dimensioni, in [!INCLUDE[ssDE](../../includes/ssde-md.md)] le deallocazioni di pagine effettive e i relativi blocchi associati vengono posticipati fino all'esecuzione del commit della transazione. Le operazioni di eliminazione posticipate non rendono immediatamente disponibile lo spazio allocato. Pertanto, i valori restituiti da **sp_spaceused** immediatamente dopo l'eliminazione o il troncamento di un oggetto di grandi dimensioni potrebbero non corrispondere allo spazio su disco effettivo disponibile.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per eseguire **sp_spaceused** è concessa al ruolo **public** . Solo i membri del ruolo predefinito del database **db_owner** possono specificare il parametro **\@updateusage**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-disk-space-information-about-a-table"></a>R. Visualizzazione di informazioni relative allo spazio su disco per una tabella  
 Nell'esempio seguente vengono visualizzate informazioni relative allo spazio su disco per la tabella `Vendor` e i relativi indici.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused N'Purchasing.Vendor';  
GO  
```  
  
### <a name="b-displaying-updated-space-information-about-a-database"></a>B. Visualizzazione di informazioni sullo spazio aggiornate per un database  
 Nell'esempio seguente viene riepilogato lo spazio utilizzato nel database corrente e viene utilizzato il parametro facoltativo `@updateusage` per garantire la restituzione di valori aggiornati.  
  
```sql  
USE AdventureWorks008R2;  
GO  
EXEC sp_spaceused @updateusage = N'TRUE';  
GO  
```  
  
### <a name="c-displaying-space-usage-information-about-the-remote-table-associated-with-a-stretch-enabled-table"></a>C. Visualizzazione delle informazioni sull'utilizzo dello spazio sulla tabella remota associata a una tabella abilitata per l'estensione  
 Nell'esempio seguente viene riepilogato lo spazio usato dalla tabella remota associata a una tabella abilitata per l'estensione usando l'argomento **\@ mode** per specificare la destinazione remota. Per ulteriori informazioni, vedere [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
```sql  
USE StretchedAdventureWorks2016  
GO  
EXEC sp_spaceused N'Purchasing.Vendor', @mode = 'REMOTE_ONLY'  
```  
  
### <a name="d-displaying-space-usage-information-for-a-database-in-a-single-result-set"></a>D. Visualizzazione delle informazioni sull'utilizzo dello spazio per un database in un singolo set di risultati  
 Nell'esempio seguente viene riepilogato l'utilizzo dello spazio per il database corrente in un unico set di risultati.  
  
```sql  
USE AdventureWorks2016  
GO  
EXEC sp_spaceused @oneresultset = 1  
```  

### <a name="e-displaying-space-usage-information-for-a-database-with-at-least-one-memory_optimized-file-group-in-a-single-result-set"></a>E. Visualizzazione delle informazioni sull'utilizzo dello spazio per un database con almeno un gruppo di file di MEMORY_OPTIMIZED in un unico set di risultati 
 Nell'esempio seguente viene riepilogato l'utilizzo dello spazio per il database corrente con almeno un gruppo di file di MEMORY_OPTIMIZED in un unico set di risultati.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused @updateusage = 'FALSE', @mode = 'ALL', @oneresultset = '1', @include_total_xtp_storage = '1';
GO
``` 

### <a name="f-displaying-space-usage-information-for-a-memory_optimized-table-object-in-a-database"></a>F. Visualizzazione delle informazioni sull'utilizzo dello spazio per un oggetto MEMORY_OPTIMIZED tabella in un database.
 Nell'esempio seguente viene riepilogato l'utilizzo dello spazio per un oggetto MEMORY_OPTIMIZED tabella nel database corrente con almeno un MEMORY_OPTIMIZED filegroup.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused
@objname = N'VehicleTemparatures',
@updateusage = 'FALSE',
@mode = 'ALL',
@oneresultset = '0',
@include_total_xtp_storage = '1';
GO
```  

## <a name="see-also"></a>Vedere anche  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC UPDATEUSAGE &#40;&#41;Transact-SQL ](../../t-sql/database-console-commands/dbcc-updateusage-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

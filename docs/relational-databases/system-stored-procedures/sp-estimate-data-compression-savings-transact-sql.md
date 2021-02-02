---
description: sp_estimate_data_compression_savings (Transact-SQL)
title: sp_estimate_data_compression_savings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_estimate_data_compression_savings_TSQL
- sp_estimate_data_compression_savings
dev_langs:
- TSQL
helpviewer_keywords:
- compression [SQL Server], estimating
- sp_estimate_data_compression_savings
ms.assetid: 6f6c7150-e788-45e0-9d08-d6c2f4a33729
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 93d169d364369e87f1c363297c2a5c73c58e442b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237844"
---
# <a name="sp_estimate_data_compression_savings-transact-sql"></a>sp_estimate_data_compression_savings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le dimensioni correnti degli oggetti richiesti e stima le dimensioni dell'oggetto per lo stato di compressione richiesto. La compressione può essere valutata per intere tabelle o parti di esse, Sono inclusi gli heap, gli indici cluster, gli indici non cluster, gli indici columnstore, le viste indicizzate e le partizioni delle tabelle e degli indici. Gli oggetti possono essere compressi usando la compressione di riga, pagina, columnstore o archivio columnstore. Se la tabella, la partizione o l'indice è già compresso, è possibile utilizzare questa procedura per stimare le dimensioni della tabella, della partizione o dell'indice se venisse ricompresso.  
  
> [!NOTE]
> La compressione e la **sp_estimate_data_compression_savings** non sono disponibili in ogni edizione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 Per stimare le dimensioni dell'oggetto in caso di applicazione dell'impostazione di compressione richiesta, questa stored procedure esegue il campionamento dell'oggetto di origine e carica i relativi dati in una tabella e in un indice equivalenti creati in tempdb. La tabella o l'indice creato in tempdb viene quindi compresso in base all'impostazione richiesta e viene calcolato il risparmio stimato in caso di utilizzo della compressione.  
  
 Per modificare lo stato di compressione di una tabella, di un indice o di una partizione, utilizzare le istruzioni [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [alter index](../../t-sql/statements/alter-index-transact-sql.md) . Per informazioni generali sulla compressione, vedere [compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
  
> [!NOTE]  
> Se i dati esistenti sono frammentati, potrebbe essere possibile ridurne le dimensioni senza utilizzare la compressione ricompilando l'indice. Per gli indici, il fattore di riempimento viene applicato durante la ricompilazione. Questo potrebbe comportare un aumento delle dimensioni dell'indice.  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_estimate_data_compression_savings   
     [ @schema_name = ] 'schema_name'    
   , [ @object_name = ] 'object_name'   
   , [ @index_id = ] index_id   
   , [ @partition_number = ] partition_number   
   , [ @data_compression = ] 'data_compression'   
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @schema_name =]'*schema_name*'  
 Nome dello schema del database che contiene la tabella o la vista indicizzata. *schema_name* è di **tipo sysname**. Se *schema_name* è null, viene usato lo schema predefinito dell'utente corrente.  
  
 [ @object_name =]'*object_name*'  
 Nome della tabella o della vista indicizzata su cui è basato l'indice. *object_name* è di tipo **sysname**.  
  
 [ @index_id =] *index_id*  
 ID dell'indice. *index_id* è di **tipo int**. i possibili valori sono i seguenti: il numero di ID di un indice, NULL o 0 se *object_id* è un heap. Per restituire informazioni per tutti gli indici per una tabella di base o una vista, specificare NULL. Se si specifica NULL, è necessario specificare NULL anche per *partition_number*.  
  
 [ @partition_number =] *partition_number*  
 Numero di partizione nell'oggetto. *partition_number* è di **tipo int**. i possibili valori sono i seguenti: il numero di partizione di un indice o heap, null o 1 per un indice o un heap non partizionato.  
  
 Per specificare la partizione, è anche possibile specificare la funzione [$Partition](../../t-sql/functions/partition-transact-sql.md) . Per restituire le informazioni per tutte le partizioni dell'oggetto, specificare NULL.  
  
 [ @data_compression =]'*DATA_COMPRESSION*'  
 Tipo di compressione da valutare. *DATA_COMPRESSION* può essere uno dei valori seguenti: None, Row, Page, columnstore o COLUMNSTORE_ARCHIVE.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Per offrire informazioni sulle dimensioni correnti e stimate della tabella, dell'indice o della partizione, viene restituito il set di risultati seguente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_name|**sysname**|Nome della tabella o della vista indicizzata.|  
|schema_name|**sysname**|Schema della tabella o della vista indicizzata.|  
|index_id|**int**|ID di un indice:<br /><br /> 0 = heap<br /><br /> 1 = indice cluster<br /><br /> > 1 = Indice non cluster|  
|partition_number|**int**|Numero di partizioni. Per una tabella o un indice non partizionato viene restituito 1.|  
|size_with_current_compression_setting (KB)|**bigint**|Dimensioni attuali della tabella, della partizione o dell'indice richiesto.|  
|size_with_requested_compression_setting (KB)|**bigint**|Dimensioni stimate della tabella, della partizione o dell'indice in caso di utilizzo dell'impostazione di compressione richiesta e, se applicabile, del fattore di riempimento esistente e presupponendo che non vi sia frammentazione.|  
|sample_size_with_current_compression_setting (KB)|**bigint**|Dimensioni del campione con l'impostazione di compressione corrente. È inclusa qualsiasi frammentazione.|  
|sample_size_with_requested_compression_setting (KB)|**bigint**|Dimensioni del campione creato utilizzando l'impostazione di compressione richiesta e, se applicabile, il fattore di riempimento esistente e senza frammentazione.|  
  
## <a name="remarks"></a>Commenti  
 Usare `sp_estimate_data_compression_savings` per stimare i risparmi che possono verificarsi quando si abilita una tabella o una partizione per la compressione di righe, pagine, columnstore o archivio columnstore. Se, ad esempio, le dimensioni medie della riga possono essere ridotte del 40%, è possibile ridurre del 40% le dimensioni dell'oggetto. Si potrebbe non ottenere un risparmio in termini di spazio a seconda del fattore di riempimento e delle dimensioni della riga. Se, ad esempio, si dispone di una riga di 8.000 byte e si riducono le dimensioni del 40%, è comunque possibile adattare una sola riga a una pagina di dati. e non si ottiene alcun risparmio.  
  
 Se i risultati dell'esecuzione di `sp_estimate_data_compression_savings` indicano un aumento delle dimensioni della tabella, significa che in molte righe della tabella viene utilizzata quasi la precisione completa dei tipi di dati e l'aggiunta del limitato overhead necessario per il formato compresso supera il risparmio derivante dalla compressione. In questi rari casi, non abilitare la compressione.  
  
 Se per una tabella è abilitata la compressione, utilizzare `sp_estimate_data_compression_savings` per stimare le dimensioni medie della riga nel caso in cui la tabella non fosse compressa.  
  
 Durante questa operazione viene acquisito un blocco (IS) nella tabella. Se non è possibile ottenere un blocco (IS), la procedura viene bloccata. La tabella viene analizzata con il livello di isolamento Read committed.  
  
 Se l'impostazione di compressione richiesta corrisponde all'impostazione di compressione corrente, la stored procedure restituirà le dimensioni stimate senza frammentazione dei dati e utilizzando il fattore di riempimento esistente.  
  
 Se l'ID della partizione o dell'indice non esiste, non viene restituito alcun risultato.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `SELECT` per la tabella.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Prima di SQL Server 2019, questa procedura non si applicava agli indici columnstore e pertanto non accettava il COLUMNStore dei parametri di compressione dei dati e la COLUMNSTORE_ARCHIVE.  A partire da SQL Server 2019, gli indici columnstore possono essere usati sia come oggetto di origine per la stima che come tipo di compressione richiesto.

 > [!IMPORTANT]
 > Quando i [metadati tempdb con ottimizzazione](../databases/tempdb-database.md#memory-optimized-tempdb-metadata) per la memoria sono abilitati in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , la creazione di indici columnstore in tabelle temporanee non è supportata. A causa di questa limitazione, sp_estimate_data_compression_savings non è supportata con i parametri COLUMNStore e COLUMNSTORE_ARCHIVE compressione dati quando Memory-Optimized metadati TempDB sono abilitati.

## <a name="considerations-for-columnstore-indexes"></a>Considerazioni sugli indici columnstore
 A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , `sp_estimate_compression_savings` supporta la stima della compressione dell'archivio columnstore e columnstore. A differenza della compressione di pagine e righe, per applicare la compressione columnstore a un oggetto è necessario creare un nuovo indice columnstore. Per questo motivo, quando si usano le opzioni COLUMNStore e COLUMNSTORE_ARCHIVE di questa procedura, il tipo dell'oggetto di origine fornito alla routine determina il tipo di indice columnstore usato per la stima delle dimensioni compresse. La tabella seguente illustra gli oggetti di riferimento usati per stimare i risparmi di compressione per ogni tipo di oggetto di origine quando il @data_compression parametro è impostato su columnstore o COLUMNSTORE_ARCHIVE.

 |Oggetto di origine|Oggetto di riferimento|
 |-----------------|---------------|
 |Heap|Indice columnstore cluster|
 |Indice cluster|Indice columnstore cluster|
 |Indice non cluster|Indice columnstore non cluster (incluse le colonne chiave e le colonne incluse dell'indice non cluster specificato, nonché la colonna di partizione della tabella, se presente)|
 |indice columnstore non cluster|Indice columnstore non cluster (incluse le stesse colonne dell'indice columnstore non cluster specificato)|
 |Indice columnstore cluster|Indice columnstore cluster|

> [!NOTE]  
> Quando si stima la compressione columnstore da un oggetto di origine rowstore (indice cluster, indice non cluster o heap), se nell'oggetto di origine sono presenti colonne che hanno un tipo di dati non supportato in un indice columnstore, sp_estimate_compression_savings avrà esito negativo con un errore.

 Analogamente, quando il `@data_compression` parametro è impostato su `NONE` , `ROW` o `PAGE` e l'oggetto di origine è un indice columnstore, nella tabella seguente vengono descritti gli oggetti di riferimento utilizzati.

 |Oggetto di origine|Oggetto di riferimento|
 |-----------------|---------------|
 |Indice columnstore cluster|Heap|
 |indice columnstore non cluster|Indice non cluster (incluse le colonne contenute nell'indice columnstore non cluster come colonne chiave e la colonna di partizione della tabella, se presente, come colonna inclusa)|

> [!NOTE]  
> Quando si stima la compressione rowstore (nessuno, riga o pagina) da un oggetto di origine columnstore, assicurarsi che l'indice di origine non contenga più di 32 colonne, perché questo è il limite supportato in un indice rowstore (non cluster).
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono stimate le dimensioni della tabella `Production.WorkOrderRouting` in caso di utilizzo della compressione `ROW`.  
  
```sql  
USE AdventureWorks2016;  
GO  
EXEC sp_estimate_data_compression_savings 'Production', 'WorkOrderRouting', NULL, NULL, 'ROW' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Implementazione della compressione Unicode](../../relational-databases/data-compression/unicode-compression-implementation.md)  
  
  

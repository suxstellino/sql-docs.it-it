---
title: sys.dm_exec_describe_first_result_set (Transact-SQL)
description: sys.dm_exec_describe_first_result_set (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_describe_first_result_set
- sys.dm_exec_describe_first_result_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set catalog view
ms.assetid: 6ea88346-0bdb-4f0e-9f1f-4d85e3487d23
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 06/10/2016
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2a9c52669618f25d685016db93de99f01bd095ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99106615"
---
# <a name="sysdm_exec_describe_first_result_set-transact-sql"></a>sys.dm_exec_describe_first_result_set (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione a gestione dinamica accetta un' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione come parametro e descrive i metadati del primo set di risultati per l'istruzione.  
  
 **sys.dm_exec_describe_first_result_set** ha la stessa definizione del set di risultati [sys.dm_exec_describe_first_result_set_for_object &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md) ed è simile a SP_DESCRIBE_FIRST_RESULT_SET &#40;[Transact-SQL](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)&#41;.  
  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_exec_describe_first_result_set(@tsql, @params, @include_browse_information)  
```  
  
## <a name="arguments"></a>Argomenti  
 *\@TSQL*  
 Una o più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. *Transact-SQL_batch* può essere di **tipo nvarchar (**_n_*_)_* o **nvarchar (max)**.  
  
 *\@params*  
 \@params fornisce una stringa di dichiarazione per i parametri del [!INCLUDE[tsql](../../includes/tsql-md.md)] batch, analogamente a sp_executesql. I parametri possono essere di **tipo nvarchar (n)** o **nvarchar (max)**.  
  
 È una stringa che contiene le definizioni di tutti i parametri incorporati nella [!INCLUDE[tsql](../../includes/tsql-md.md)] *_batch*. La stringa deve essere una costante o una variabile Unicode. Ogni definizione di parametro è costituita da un nome del parametro e da un tipo di dati. *n* è un segnaposto che indica definizioni di parametro aggiuntive. Ogni parametro specificato in stmt deve essere definito in \@ params. Se l' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione o il batch nell'istruzione non contiene parametri, \@ params non è necessario. Il valore predefinito per questo parametro è NULL.  
  
 *\@include_browse_information*  
 Se impostato su 1, ogni query viene analizzata come se per essa fosse stata specificata un'opzione FOR BROWSE. Vengono restituite informazioni aggiuntive sulla tabella di origine e sulle colonne chiave.  
  
## <a name="table-returned"></a>Tabella restituita  
 Questi metadati comuni vengono restituiti come un set di risultati. In una riga di ogni colonna nei metadati dei risultati viene fornita la descrizione del tipo e l'ammissione di valori Null della colonna nel formato illustrato nella tabella seguente. Se la prima istruzione non esiste per ogni percorso di controllo, viene restituito un set di risultati con zero righe.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|Specifica che la colonna è una colonna aggiuntiva inserita a scopo informativo e di esplorazione che non compare effettivamente nel set di risultati.|  
|**column_ordinal**|**int**|Contiene la posizione ordinale della colonna nel set di risultati. La posizione della prima colonna viene specificata come 1.|  
|**nome**|**sysname**|Contiene il nome della colonna se è possibile determinare un nome. In caso contrario, conterrà NULL.|  
|**is_nullable**|**bit**|Contiene i valori seguenti:<br /><br /> Valore 1 se la colonna ammette valori NULL.<br /><br /> Valore 0 se la colonna non ammette valori NULL.<br /><br /> Valore 1 se non è possibile determinare se la colonna ammette valori NULL.|  
|**system_type_id**|**int**|Contiene la system_type_id del tipo di dati della colonna come specificato in sys. Types. Per i tipi CLR, anche se la colonna system_type_name restituisce NULL, in questa colonna viene restituito il valore 240.|  
|**system_type_name**|**nvarchar(256)**|Contiene il nome e gli argomenti, ad esempio lunghezza, precisione e scala, specificati per il tipo di dati della colonna.<br /><br /> Se il tipo di dati è un tipo di alias definito dall'utente, il tipo di sistema sottostante viene specificato qui.<br /><br /> Se il tipo di dati è un tipo CLR definito dall'utente, in questa colonna viene restituito NULL.|  
|**max_length**|**smallint**|Lunghezza massima in byte della colonna.<br /><br /> -1 = il tipo di dati della colonna è **varchar (max)**, **nvarchar (max)**, **varbinary (max)** o **XML**.<br /><br /> Per le colonne di **testo** , il valore **max_length** sarà 16 o il valore impostato da **sp_tableoption ' text in row '**.|  
|**precisione**|**tinyint**|Precisione della colonna se basata su valori numerici. In caso contrario, restituisce 0.|  
|**scale**|**tinyint**|Scala della colonna se basata su valori numerici. In caso contrario, restituisce 0.|  
|**nome_regole_di_confronto**|**sysname**|Nome delle regole di confronto della colonna se basata su caratteri. In caso contrario, viene restituito NULL.|  
|**user_type_id**|**int**|Per i tipi di alias e CLR, contiene il valore user_type_id del tipo di dati della colonna come specificato in sys.types. In caso contrario, è NULL.|  
|**user_type_database**|**sysname**|Per i tipi di alias e CLR, contiene il nome del database in cui è definito il tipo. In caso contrario, è NULL.|  
|**user_type_schema**|**sysname**|Per i tipi di alias e CLR, contiene il nome dello schema in cui è definito il tipo. In caso contrario, è NULL.|  
|**user_type_name**|**sysname**|Per i tipi di alias e CLR, contiene il nome del tipo. In caso contrario, è NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Per i tipi CLR, restituisce il nome dell'assembly e la classe che definisce il tipo. In caso contrario, è NULL.|  
|**xml_collection_id**|**int**|Contiene il valore xml_collection_id del tipo di dati della colonna come specificato in sys.columns. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**xml_collection_database**|**sysname**|Contiene il database in cui viene definita la raccolta XML Schema associata a questo tipo. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**xml_collection_schema**|**sysname**|Contiene lo schema in cui viene definita la raccolta XML Schema associata a questo tipo. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**xml_collection_name**|**sysname**|Contiene il nome della raccolta XML Schema associata a questo tipo. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**is_xml_document**|**bit**|Restituisce 1 se il tipo di dati restituito è XML ed è garantito che questo tipo sia un documento XML completo (incluso un nodo radice), contrariamente a un frammento XML. In caso contrario, restituisce 0.|  
|**is_case_sensitive**|**bit**|Restituisce 1 se la colonna è di un tipo stringa con distinzione tra maiuscole e minuscole. In caso contrario, restituisce 0.|  
|**is_fixed_length_clr_type**|**bit**|Restituisce 1 se la colonna è di un tipo CLR a lunghezza fissa. In caso contrario, restituisce 0.|  
|**source_server**|**sysname**|Nome del server di origine (se ha origine in un server remoto). Il nome viene fornito come visualizzato in sys. Servers. Restituisce NULL se la colonna ha origine nel server locale o se non è possibile determinare in quale server ha origine. Viene popolata solo se sono richieste informazioni di esplorazione.|  
|**source_database**|**sysname**|Nome del database di origine restituito dalla colonna in questo risultato. Restituisce NULL se non è possibile determinare il database. Viene popolata solo se sono richieste informazioni di esplorazione.|  
|**source_schema**|**sysname**|Nome dello schema di origine restituito dalla colonna in questo risultato. Restituisce NULL se non è possibile determinare lo schema. Viene popolata solo se sono richieste informazioni di esplorazione.|  
|**source_table**|**sysname**|Nome della tabella di origine restituita dalla colonna in questo risultato. Restituisce NULL se non è possibile determinare la tabella. Viene popolata solo se sono richieste informazioni di esplorazione.|  
|**source_column**|**sysname**|Nome della colonna di origine restituita dalla colonna del risultato. Restituisce NULL se non è possibile determinare la colonna. Viene popolata solo se sono richieste informazioni di esplorazione.|  
|**is_identity_column**|**bit**|Restituisce 1 se la colonna è una colonna Identity e 0 in caso contrario. Restituisce NULL se non è possibile determinare se la colonna è una colonna Identity.|  
|**is_part_of_unique_key**|**bit**|Restituisce 1 se la colonna fa parte di un indice univoco, inclusi vincoli univoci e primari, e 0 in caso contrario. Restituisce NULL se non è possibile determinare se la colonna fa parte di un indice univoco. Viene popolata solo se sono richieste informazioni di esplorazione.|  
|**is_updateable**|**bit**|Restituisce 1 se la colonna può essere aggiornata e 0 in caso contrario. Restituisce NULL se non è possibile determinare se la colonna può essere aggiornata.|  
|**is_computed_column**|**bit**|Restituisce 1 se la colonna è una colonna calcolata e 0 in caso contrario. Restituisce NULL se non è possibile determinare se la colonna è una colonna calcolata.|  
|**is_sparse_column_set**|**bit**|Restituisce 1 se la colonna è una colonna di tipo sparse e 0 in caso contrario. Restituisce NULL se non è possibile determinare se la colonna fa parte di un set di colonne di tipo sparse.|  
|**ordinal_in_order_by_list**|**smallint**|La posizione della colonna è nell'elenco ORDER BY. Restituisce NULL se la colonna non compare nell'elenco ORDER BY o se l'elenco ORDER BY non può essere determinato in modo univoco.|  
|**order_by_list_length**|**smallint**|Lunghezza dell'elenco ORDER BY. Restituisce NULL se non è presente alcun elenco ORDER BY o se l'elenco ORDER BY non può essere determinato in modo univoco. Si noti che questo valore sarà lo stesso per tutte le righe restituite da sp_describe_first_result_set.|  
|**order_by_is_descending**|**smallint NULL**|Se ordinal_in_order_by_list non è NULL, la colonna **order_by_is_descending** indica la direzione della clausola ORDER BY per questa colonna. In caso contrario, viene restituito NULL.|  
|**error_number**|**int**|Contiene il numero dell'errore restituito dalla funzione. La colonna contiene NULL se non si è verificato alcun errore.|  
|**error_severity**|**int**|Contiene la gravità restituita dalla funzione. La colonna contiene NULL se non si è verificato alcun errore.|  
|**error_state**|**int**|Contiene il messaggio di stato restituito dalla funzione. La colonna contiene NULL se non si è verificato alcun errore.|  
|**error_message**|**nvarchar (4096)**|Contiene il messaggio restituito dalla funzione. La colonna contiene NULL se non si è verificato alcun errore.|  
|**error_type**|**int**|Contiene un numero intero che rappresenta l'errore restituito. Viene eseguito il mapping a error_type_desc. Vedere l'elenco nelle osservazioni.|  
|**error_type_desc**|**nvarchar(60)**|Contiene una breve stringa in caratteri maiuscoli che rappresenta l'errore restituito. Viene eseguito il mapping a error_type. Vedere l'elenco nelle osservazioni.|  
  
## <a name="remarks"></a>Commenti  
 Questa funzione utilizza lo stesso algoritmo di **sp_describe_first_result_set**. Per ulteriori informazioni, vedere [sp_describe_first_result_set &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 Nella tabella seguente vengono elencati i tipi di errore con le relative descrizioni  
  
|error_type|error_type|Descrizione|  
|-----------------|-----------------|-----------------|  
|1|MISC|Tutti gli errori che non sono stati descritti.|  
|2|SYNTAX|Errore di sintassi nel batch.|  
|3|CONFLICTING_RESULTS|Impossibile determinare il risultato a causa di un conflitto tra due possibili prime istruzioni.|  
|4|DYNAMIC_SQL|Impossibile determinare il risultato perché codice SQL dinamico potrebbe potenzialmente restituire il primo risultato.|  
|5|CLR_PROCEDURE|Impossibile determinare il risultato perché una stored procedure CLR potrebbe potenzialmente restituire il primo risultato.|  
|6|CLR_TRIGGER|Impossibile determinare il risultato perché un trigger CLR potrebbe potenzialmente restituire il primo risultato.|  
|7|EXTENDED_PROCEDURE|Impossibile determinare il risultato perché una stored procedure estesa potrebbe potenzialmente restituire il primo risultato.|  
|8|UNDECLARED_PARAMETER|Impossibile determinare il risultato perché il tipo di dati di una o più colonne del set di risultati dipende potenzialmente da un parametro non dichiarato.|  
|9|RECURSION|Non è stato possibile determinare il risultato perché il batch contiene un'istruzione ricorsiva.|  
|10|TEMPORARY_TABLE|Non è stato possibile determinare il risultato perché il batch contiene una tabella temporanea e non è supportato da **sp_describe_first_result_set**.|  
|11|UNSUPPORTED_STATEMENT|Non è stato possibile determinare il risultato perché il batch contiene un'istruzione che non è supportata da **sp_describe_first_result_set**, ad esempio FETCH, REVERT e così via.|  
|12|OBJECT_TYPE_NOT_SUPPORTED|Il \@ object_id passato alla funzione non è supportato (ovvero non è un stored procedure)|  
|13|OBJECT_DOES_NOT_EXIST|\@Impossibile trovare il object_id passato alla funzione nel catalogo di sistema.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione per eseguire l' \@ argomento TSQL.  
  
## <a name="examples"></a>Esempio  
 Altri esempi nell'argomento [sp_describe_first_result_set &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) possono essere adattati per l'uso **sys.dm_exec_describe_first_result_set**.  
  
### <a name="a-returning-information-about-a-single-transact-sql-statement"></a>R. Restituzione di informazioni su una sola istruzione Transact-SQL  
 Nel codice seguente vengono restituite informazioni sui risultati di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_exec_describe_first_result_set  
(N'SELECT object_id, name, type_desc FROM sys.indexes', null, 0) ;  
```  
  
### <a name="b-returning-information-about-a-procedure"></a>B. Restituzione di informazioni su una procedura  
 Nell'esempio seguente viene creato un stored procedure denominato pr_TestProc che restituisce due set di risultati. Nell'esempio viene dimostrato che **sys.dm_exec_describe_first_result_set** restituisce informazioni sul primo set di risultati nella procedura.  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE PROC Production.TestProc  
AS  
SELECT Name, ProductID, Color FROM Production.Product ;  
SELECT Name, SafetyStockLevel, SellStartDate FROM Production.Product ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set  
('Production.TestProc', NULL, 0) ;  
```  
  
### <a name="c-returning-metadata-from-a-batch-that-contains-multiple-statements"></a>C. Restituzione di metadati da un batch che contiene più istruzioni  
 Nell'esempio seguente viene valutato un batch che contiene due istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il set di risultati descrive il primo set di risultati restituito.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set(  
N'SELECT CustomerID, TerritoryID, AccountNumber FROM Sales.Customer WHERE CustomerID = @CustomerID;  
SELECT * FROM Sales.SalesOrderHeader;',  
N'@CustomerID int', 0) AS a;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_describe_first_result_set &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
  
  

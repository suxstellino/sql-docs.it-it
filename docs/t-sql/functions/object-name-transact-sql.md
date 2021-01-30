---
description: OBJECT_NAME (Transact-SQL)
title: OBJECT_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_NAME
- OBJECT_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OBJECT_NAME function
- viewing database object names
- objects [SQL Server], names
- database objects [SQL Server], names
- displaying database object names
- database objects [SQL Server]
- names [SQL Server], database objects
ms.assetid: 7d5b923f-0c3e-4af9-b39b-132807a6d5b3
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 86bdd14a335cae7accc810ba14285fa75952e978
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189902"
---
# <a name="object_name-transact-sql"></a>OBJECT_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il nome dell'oggetto di database per gli oggetti definiti a livello di ambito di schema. Per un elenco degli oggetti con ambito schema, vedere [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
OBJECT_NAME ( object_id [, database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *object_id*  
 ID dell'oggetto da utilizzare. *object_id* è di tipo **int** e si presume che sia un oggetto con ambito schema nel database specificato o nel contesto del database corrente.  
  
 *database_id*  
 ID del database in cui l'oggetto deve essere cercato. *database_id* è di tipo **int**.  
  
## <a name="return-types"></a>Tipi restituiti  
 **sysname**  
  
## <a name="exceptions"></a>Eccezioni  
 Restituisce NULL in caso di errore o se un chiamante non dispone dell'autorizzazione necessaria per visualizzare l'oggetto. Se l'opzione AUTO_CLOSE del database di destinazione è impostata su ON, la funzione aprirà il database.  
  
 Un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come OBJECT_NAME possono restituire NULL se l'utente non dispone di alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ANY per l'oggetto. Per specificare l'ID di un database, è inoltre necessaria l'autorizzazione CONNECT per il database o l'abilitazione dell'account guest.  
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare funzioni di sistema nell'elenco di selezione, nella clausola WHERE e in tutti i casi in cui è consentita un'espressione. Per altre informazioni, vedere [Espressioni](../../t-sql/language-elements/expressions-transact-sql.md) e [WHERE](../../t-sql/queries/where-transact-sql.md).  
  
 Il valore restituito da questa funzione di sistema utilizza le regole di confronto del database corrente.  
  
 Per impostazione predefinita, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] presuppone che *object_id* sia nel contesto del database corrente. Una query che fa riferimento a un valore di *object_id* in un altro database restituisce NULL oppure risultati errati. Ad esempio, nella query seguente il contesto del database corrente è [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. [!INCLUDE[ssDE](../../includes/ssde-md.md)] cerca di restituire un nome di oggetto per l'ID di oggetto specificato in tale database anziché il database specificato nella clausola FROM della query. Verranno pertanto restituite informazioni errate.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT OBJECT_NAME(object_id)  
FROM master.sys.objects;  
GO  
```  
  
 È possibile risolvere i nomi di oggetto nel contesto di un altro database specificando un ID database. Nell'esempio seguente viene specificato l'ID del database `master` nella funzione `OBJECT_SCHEMA_NAME` e vengono restituiti i risultati corretti.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT OBJECT_SCHEMA_NAME(object_id, 1) AS schema_name  
FROM master.sys.objects;  
GO  
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-object_name-in-a-where-clause"></a>R. Utilizzo di OBJECT_NAME in una clausola WHERE  
 Nell'esempio seguente vengono restituite le colonne dalla vista del catalogo `sys.objects` per l'oggetto specificato da `OBJECT_NAME` nella clausola `WHERE` dell'istruzione `SELECT`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyID INT;  
SET @MyID = (SELECT OBJECT_ID('AdventureWorks2012.Production.Product',  
    'U'));  
SELECT name, object_id, type_desc  
FROM sys.objects  
WHERE name = OBJECT_NAME(@MyID);  
GO  
```  
  
### <a name="b-returning-the-object-schema-name-and-object-name"></a>B. Restituzione del nome dello schema dell'oggetto e del nome dell'oggetto  
 Nell'esempio seguente vengono restituiti il nome dello schema dell'oggetto, il nome dell'oggetto e il testo SQL per tutti i piani di query memorizzati nella cache che non sono istruzioni ad hoc o preparate.  
  
```sql  
SELECT DB_NAME(st.dbid) AS database_name,   
    OBJECT_SCHEMA_NAME(st.objectid, st.dbid) AS schema_name,  
    OBJECT_NAME(st.objectid, st.dbid) AS object_name,   
    st.text AS query_text  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
WHERE st.objectid IS NOT NULL;  
GO  
```  
  
### <a name="c-returning-three-part-object-names"></a>C. Restituzione dei nomi degli oggetti in tre parti  
 Nell'esempio seguente vengono restituiti il nome del database, dello schema e dell'oggetto, nonché tutte le altre colonne della vista a gestione dinamica `sys.dm_db_index_operational_stats` per tutti gli oggetti di tutti i database.  
  
```sql  
SELECT QUOTENAME(DB_NAME(database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_SCHEMA_NAME(object_id, database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_NAME(object_id, database_id))  
    , *   
FROM sys.dm_db_index_operational_stats(NULL, NULL, NULL, NULL);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-object_name-in-a-where-clause"></a>D. Utilizzo di OBJECT_NAME in una clausola WHERE  
 Nell'esempio seguente vengono restituite le colonne dalla vista del catalogo `sys.objects` per l'oggetto specificato da `OBJECT_NAME` nella clausola `WHERE` dell'istruzione `SELECT`. Il numero dell'oggetto (274100017 nell'esempio seguente) sarà diverso.  Per testare l'esempio, cercare un numero di oggetto valido eseguendo `SELECT name, object_id FROM sys.objects;` nel database.  
  
```sql  
SELECT name, object_id, type_desc  
FROM sys.objects  
WHERE name = OBJECT_NAME(274100017);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)  
  
  


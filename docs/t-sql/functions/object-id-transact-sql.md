---
description: OBJECT_ID (Transact-SQL)
title: OBJECT_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_ID
- OBJECT_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], IDs
- identification numbers [SQL Server], database objects
- checking object exists
- IDs [SQL Server], database objects
- OBJECT_ID function
- database objects [SQL Server], IDs
- displaying object IDs
- viewing object IDs
- verifying object exists
ms.assetid: f89286db-440f-4218-a828-30881ce3077a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6039bcbf036093af3fcb3c22e7a3845afb485574
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735105"
---
# <a name="object_id-transact-sql"></a>OBJECT_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il numero di identificazione di oggetto di database di un oggetto con ambito schema.  
  
> [!IMPORTANT]  
>  Gli oggetti senza ambito schema, ad esempio i trigger DDL, non possono essere soggetti a query tramite OBJECT_ID. Per gli oggetti che non si trovano nella visualizzazione del catalogo [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md), ottenere i numeri di identificazione dell'oggetto eseguendo una query sulla vista del catalogo appropriata. Ad esempio, per restituire il numero di identificazione oggetto di un trigger DDL, usare `SELECT OBJECT_ID FROM sys.triggers WHERE name = 'DatabaseTriggerLog``'`.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
OBJECT_ID ( '[ database_name . [ schema_name ] . | schema_name . ]   
  object_name' [ ,'object_type' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *object_name* **'**  
 Oggetto da utilizzare. *object_name* è **varchar** o **nvarchar**. Se *object_name* è **varchar** viene convertito in modo implicito in **nvarchar**. L'indicazione dei nomi dello schema e del database è facoltativa.  
  
 **'** *object_type* **'**  
 Tipo di oggetto a livello di ambito dello schema. *object_type* è **varchar** o **nvarchar**. Se *object_type* è **varchar** viene convertito in modo implicito in **nvarchar**. Per un elenco dei tipi di oggetti, vedere la colonna **type** in [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="exceptions"></a>Eccezioni  
 Per un indice spaziale, OBJECT_ID restituisce NULL.  
  
 Restituisce Null in caso di errore.  
  
 Un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come OBJECT_ID possono restituire NULL se l'utente non dispone di alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Osservazioni  
 Se il parametro per una funzione di sistema è facoltativo, vengono utilizzati il database, il computer host, l'utente del server o l'utente del database correnti. Le funzioni predefinite devono essere sempre seguite da parentesi.  
  
 Quando si specifica il nome di una tabella temporanea, questo deve essere preceduto dal nome del database, a meno che il database corrente sia **tempdb**. Ad esempio: `SELECT OBJECT_ID('tempdb..#mytemptable')`.  
  
 È possibile utilizzare funzioni di sistema nell'elenco di selezione, nella clausola WHERE e in tutti i casi in cui è consentita un'espressione. Per altre informazioni, vedere [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md) e [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-object-id-for-a-specified-object"></a>R. Restituzione dell'ID oggetto per un oggetto specificato  
 Nell'esempio seguente viene restituito l'ID oggetto per la tabella `Production.WorkOrder` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE master;  
GO  
SELECT OBJECT_ID(N'AdventureWorks2012.Production.WorkOrder') AS 'Object ID';  
GO  
```  
  
### <a name="b-verifying-that-an-object-exists"></a>B. Verifica dell'esistenza di un oggetto  
 Nell'esempio seguente viene controllata l'esistenza di una tabella specificata mediante la verifica della presenza dell'ID oggetto. Se la tabella esiste, verrà eliminata. Se la tabella non esiste, l'istruzione `DROP TABLE` non verrà eseguita.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'dbo.AWBuildVersion', N'U') IS NOT NULL  
DROP TABLE dbo.AWBuildVersion;  
GO  
```  
  
### <a name="c-using-object_id-to-specify-the-value-of-a-system-function-parameter"></a>C. Utilizzo di OBJECT_ID per specificare il valore di un parametro di una funzione di sistema  
 Nell'esempio seguente vengono restituite informazioni per tutti gli indici e le partizioni della tabella `Person.Address` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] usando la funzione [sys.dm_db_index_operational_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md).  
 
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
> [!IMPORTANT]  
>  Quando si utilizzano le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] DB_ID e OBJECT_ID per restituire un valore di parametro, verificare sempre che venga restituito un ID valido. Se risulta impossibile trovare il nome del database o dell'oggetto, ad esempio quando tali nomi non esistono o sono stati immessi con un'ortografia errata, entrambe le funzioni restituiranno NULL. La funzione **sys.dm_db_index_operational_stats** interpreta NULL come un valore di carattere jolly che specifica tutti i database o tutti gli oggetti. Poiché può trattarsi di un'operazione accidentale, gli esempi riportati in questa sezione dimostrano la procedura sicura per determinare gli ID di database e oggetti.
  
```sql  
DECLARE @db_id INT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
IF @db_id IS NULL   
  BEGIN;  
    PRINT N'Invalid database';  
  END;  
ELSE IF @object_id IS NULL  
  BEGIN;  
    PRINT N'Invalid object';  
  END;  
ELSE  
  BEGIN;  
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);  
  END;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-returning-the-object-id-for-a-specified-object"></a>D. Restituzione dell'ID oggetto per un oggetto specificato  
 Nell'esempio seguente viene restituito l'ID oggetto per la tabella `FactFinance` nel database [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].  
  
```sql  
SELECT OBJECT_ID('AdventureWorksPDW2012.dbo.FactFinance') AS 'Object ID';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)  
  
  


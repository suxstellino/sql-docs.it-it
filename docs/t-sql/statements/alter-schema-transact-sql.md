---
description: ALTER SCHEMA (Transact-SQL)
title: ALTER SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SCHEMA
- ALTER_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], transferring
- transferring objects between schemas
- ALTER SCHEMA statement
- schemas [SQL Server], modifying
- modifying schemas
ms.assetid: 0a760138-460e-410a-a3c1-d60af03bf2ed
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49d0136858544c10c73c06060fca9155b2634d18
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742991"
---
# <a name="alter-schema-transact-sql"></a>ALTER SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Trasferisce un'entità a protezione diretta da uno schema a un altro.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER SCHEMA schema_name   
   TRANSFER [ <entity_type> :: ] securable_name   
[;]  
  
<entity_type> ::=  
    {  
    Object | Type | XML Schema Collection  
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER SCHEMA schema_name   
   TRANSFER [ OBJECT :: ] securable_name   
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome di uno schema nel database corrente in cui verrà spostata l'entità a protezione diretta. Non può essere SYS o INFORMATION_SCHEMA.  
  
 \<entity_type>  
 Classe dell'entità per la quale viene modificato il proprietario. L'oggetto rappresenta l'impostazione predefinita.  
  
 *securable_name*  
 Nome in una o due parti di un'entità a protezione diretta con ambito schema da spostare nello schema.  
  
## <a name="remarks"></a>Osservazioni  
 Utenti e schemi sono completamente distinti.  
  
 È possibile utilizzare ALTER SCHEMA solo per spostare le entità a protezione diretta tra schemi presenti nello stesso database. Per modificare o eliminare un'entità a sicurezza diretta all'interno di uno schema, utilizzare l'istruzione ALTER o DROP specifica dell'entità a sicurezza diretta desiderata.  
  
 Se per *securable_name* viene usato un nome in una parte, verranno usate le regole di risoluzione dei nomi attualmente valide per individuare l'entità a protezione diretta.  
  
 Tutte le autorizzazioni associate all'entità a sicurezza diretta vengono eliminate quando l'entità viene spostata nel nuovo schema. Se è stato impostato in modo esplicito, il proprietario dell'entità a sicurezza diretta rimarrà invariato. Se impostato su SCHEMA OWNER, il proprietario dell'entità a protezione diretta rimarrà SCHEMA OWNER. Tuttavia, in seguito allo spostamento SCHEMA OWNER verrà risolto nel nome del proprietario del nuovo schema. Il valore di principal_id del nuovo proprietario sarà NULL.  
  
 Lo spostamento di una stored procedure, una funzione, una vista o un trigger non consente di modificare il nome dello schema, ove presente, dell'oggetto corrispondente nella colonna definition della vista del catalogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) oppure ottenuto usando la funzione predefinita [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md). È pertanto consigliabile evitare di usare ALTER SCHEMA per spostare questi tipi di oggetto. In alternativa, eliminare e ricreare l'oggetto nel nuovo schema.  
  
 Lo spostamento di un oggetto, ad esempio una tabella o un sinonimo, non aggiorna automaticamente i riferimenti a tale oggetto. ed è necessario modificare manualmente tutti gli oggetti che fanno riferimento all'oggetto trasferito. Se, ad esempio, si sposta una tabella a cui viene fatto riferimento all'interno di un trigger, è necessario modificare il trigger in base al nuovo nome dello schema. Usare [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) per elencare le dipendenze dall'oggetto prima di spostarlo.  

 Per modificare lo schema di una tabella tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], in Esplora oggetti fare clic con il pulsante destro del mouse sulla tabella e scegliere **Progetta**. Premere **F4** per aprire la finestra Proprietà. Nella casella **Schema** selezionare un nuovo schema.  
 
 ALTER SCHEMA usa un blocco a livello di schema.
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Per trasferire un'entità a sicurezza diretta da un altro schema, l'utente deve disporre dell'autorizzazione CONTROL per l'entità (non per lo schema) e dell'autorizzazione ALTER per lo schema di destinazione.  
  
 Se l'entità a protezione diretta è associata a una specifica dell'istruzione EXECUTE AS OWNER e il proprietario è impostato su SCHEMA OWNER, l'utente deve avere anche l'autorizzazione IMPERSONATE per il proprietario dello schema di destinazione.  
  
 Tutte le autorizzazioni associate all'entità a protezione diretta in fase di trasferimento vengono eliminate al momento dello spostamento.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-transferring-ownership-of-a-table"></a>R. Trasferimento della proprietà di una tabella  
 Nell'esempio seguente viene modificato lo schema `HumanResources` mediante il trasferimento nello schema 'HumanResources' della tabella `Address` dello schema `Person`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER SCHEMA HumanResources TRANSFER Person.Address;  
GO  
```  
  
### <a name="b-transferring-ownership-of-a-type"></a>B. Trasferimento della proprietà di un tipo  
 Nell'esempio seguente viene creato un tipo nello schema `Production`, quindi il tipo viene trasferito nello schema `Person`.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TYPE Production.TestType FROM [VARCHAR](10) NOT NULL ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
  
-- Change the type to the Person schema.  
ALTER SCHEMA Person TRANSFER type::Production.TestType ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-transferring-ownership-of-a-table"></a>C. Trasferimento della proprietà di una tabella  
 Nell'esempio seguente viene creata una tabella `Region` nello schema `dbo`, viene creato uno schema `Sales` e viene spostata la tabella `Region` dallo schema `dbo` allo schema `Sales`.  
  
```sql  
CREATE TABLE dbo.Region   
    (Region_id INT NOT NULL,  
    Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
  
CREATE SCHEMA Sales;  
GO  
  
ALTER SCHEMA Sales TRANSFER OBJECT::dbo.Region;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  


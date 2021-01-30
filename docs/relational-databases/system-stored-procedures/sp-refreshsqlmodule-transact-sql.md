---
description: sp_refreshsqlmodule (Transact-SQL)
title: sp_refreshsqlmodule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refreshsqlmodule_TSQL
- sp_refreshsqlmodule
dev_langs:
- TSQL
helpviewer_keywords:
- metadata [SQL Server], stored procedures
- metadata [SQL Server], triggers
- metadata [SQL Server], views
- triggers [SQL Server], refreshing metadata
- views [SQL Server], refreshing metadata
- sp_refreshsqlmodule
- metadata [SQL Server], functions
- stored procedures [SQL Server], refreshing metadata
- user-defined functions [SQL Server], refreshing metadata
ms.assetid: f0022a05-50dd-4620-961d-361b1681d375
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: aee058df3a328c6e580f2a90f4bca5cc243e9a67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210489"
---
# <a name="sp_refreshsqlmodule-transact-sql"></a>sp_refreshsqlmodule (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Aggiorna i metadati per stored procedure non associata a schema specificato, funzione definita dall'utente, vista, trigger DML, trigger DDL a livello di database, o trigger DDL a livello di server nel database corrente. I metadati persistenti per questi oggetti, come i tipi di dati dei parametri, possono diventare obsoleti in seguito a modifiche degli oggetti sottostanti.
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_refreshsqlmodule [ @name = ] 'module_name'   
    [ , [ @namespace = ] ' <class> ' ]  
  
<class> ::=  
{  
  | DATABASE_DDL_TRIGGER  
  | SERVER_DDL_TRIGGER  
}  
  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'module\_name'` Nome del stored procedure, funzione definita dall'utente, vista, trigger DML, trigger DDL a livello di database o trigger DDL a livello di server. *module_name* non può essere un stored procedure di Common Language Runtime (CLR) o una funzione CLR. *module_name* non possono essere associati a schema. *module_name* è di **tipo nvarchar** e non prevede alcun valore predefinito. *module_name* può essere un identificatore in più parti, ma può fare riferimento solo agli oggetti nel database corrente.  
  
`[ , @namespace = ] ' \<class> '` È la classe del modulo specificato. Quando *module_name* è un trigger DDL, \<class> è obbligatorio. *\<class>* è di **tipo nvarchar**(20). Gli input validi sono:  

* DATABASE_DDL_TRIGGER

* SERVER_DDL_TRIGGER: **si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_refreshsqlmodule** deve essere eseguito quando vengono apportate modifiche agli oggetti sottostanti il modulo che influiscono sulla definizione. In caso contrario, le query o le chiamate su tale modulo potrebbero generare risultati imprevisti. Per aggiornare una vista, è possibile usare **sp_refreshsqlmodule** o **sp_refreshview** con gli stessi risultati.  
  
 **sp_refreshsqlmodule** non influisce sulle autorizzazioni, le proprietà estese o le opzioni set associate all'oggetto.  
  
 Per aggiornare un trigger DDL a livello di server, eseguire questa stored procedure dal contesto di un qualsiasi database.  
  
> [!NOTE]  
>  Tutte le firme associate all'oggetto vengono eliminate quando si esegue **sp_refreshsqlmodule**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono necessarie l'autorizzazione ALTER per il modulo e l'autorizzazione REFERENCES per i tipi CLR definiti dall'utente e le raccolte di XML Schema a cui fa riferimento l'oggetto. È necessario disporre dell'autorizzazione ALTER ANY DATABASE DDL TRIGGER per il database corrente quando il modulo specificato è un trigger DDL a livello di database. Richiede l'autorizzazione CONTROL SERVER quando il modulo specificato è un trigger DDL a livello di server.  
  
 Per i moduli definiti nella clausola EXECUTE AS è inoltre richiesta l'autorizzazione IMPERSONATE per l'entità specificata. In genere, l'aggiornamento di un oggetto non comporta modifiche per l'entità EXECUTE AS corrispondente, a meno che il modulo non venga definito con EXECUTE AS USER e il nome utente dell'entità non venga risolto in seguito in un utente diverso da quello utilizzato al momento della creazione del modulo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-refreshing-a-user-defined-function"></a>R. Aggiornamento di una funzione definita dall'utente  
 Nell'esempio seguente viene aggiornata una funzione definita dall'utente. Nell'esempio vengono creati il tipo di dati alias `mytype` e la funzione definita dall'utente `to_upper` che utilizza `mytype`. Il tipo di dati `mytype` viene quindi rinominato in `myoldtype` e viene creato un nuovo `mytype` con una diversa definizione. La funzione `dbo.to_upper` viene aggiornata in modo da fare riferimento alla nuova implementazione di `mytype`, anziché alla precedente.  
  
```  
-- Create an alias type.  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT 'mytype' FROM sys.types WHERE name = 'mytype')  
DROP TYPE mytype;  
GO  
  
CREATE TYPE mytype FROM nvarchar(5);  
GO  
  
IF OBJECT_ID ('dbo.to_upper', 'FN') IS NOT NULL  
DROP FUNCTION dbo.to_upper;  
GO  
  
CREATE FUNCTION dbo.to_upper (@a mytype)  
RETURNS mytype  
WITH ENCRYPTION  
AS  
BEGIN  
RETURN upper(@a)  
END;  
GO  
  
SELECT dbo.to_upper('abcde');  
GO  
  
-- Increase the length of the alias type.  
sp_rename 'mytype', 'myoldtype', 'userdatatype';  
GO  
  
CREATE TYPE mytype FROM nvarchar(10);  
GO  
  
-- The function parameter still uses the old type.  
SELECT name, type_name(user_type_id)   
FROM sys.parameters   
WHERE object_id = OBJECT_ID('dbo.to_upper');  
GO  
  
SELECT dbo.to_upper('abcdefgh'); -- Fails because of truncation  
GO  
  
-- Refresh the function to bind to the renamed type.  
EXEC sys.sp_refreshsqlmodule 'dbo.to_upper';  
  
-- The function parameters are now bound to the correct type and the statement works correctly.  
SELECT name, type_name(user_type_id) FROM sys.parameters  
WHERE object_id = OBJECT_ID('dbo.to_upper');  
GO  
  
SELECT dbo.to_upper('abcdefgh');  
GO  
```  
  
### <a name="b-refreshing-a-database-level-ddl-trigger"></a>B. Aggiornamento di un trigger DDL a livello di database  
 Nell'esempio seguente viene aggiornato un trigger DDL a livello di database.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_refreshsqlmodule @name = 'ddlDatabaseTriggerLog' , @namespace = 'DATABASE_DDL_TRIGGER';  
GO  
```  
  
### <a name="c-refreshing-a-server-level-ddl-trigger"></a>C. Aggiornamento di un trigger DDL a livello di server  
 Nell'esempio seguente viene aggiornato un trigger DDL a livello di server.  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|  
  
```  
USE master;  
GO  
EXEC sys.sp_refreshsqlmodule @name = 'ddl_trig_database' , @namespace = 'SERVER_DDL_TRIGGER';  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_refreshview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  

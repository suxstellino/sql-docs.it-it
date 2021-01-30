---
description: OBJECT_DEFINITION (Transact-SQL)
title: OBJECT_DEFINITION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_DEFINITION_TSQL
- OBJECT_DEFINITION
dev_langs:
- TSQL
helpviewer_keywords:
- viewing source text
- source text [SQL Server]
- displaying source text
- OBJECT_DEFINITION function
ms.assetid: 2ac837c7-eca9-4d29-b06e-72e30450c68d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 134f2a0ad313fe48e6d500014b8429a9164397ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189904"
---
# <a name="object_definition-transact-sql"></a>OBJECT_DEFINITION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il testo di origine [!INCLUDE[tsql](../../includes/tsql-md.md)] della definizione di un oggetto specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
OBJECT_DEFINITION ( object_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *object_id*  
 ID dell'oggetto da utilizzare. *object_id* è di tipo **int** e rappresenta un oggetto nel contesto del database corrente.  
  
## <a name="return-types"></a>Tipi restituiti  
 **nvarchar(max)**  
  
## <a name="exceptions"></a>Eccezioni  
 Restituisce NULL in caso di errore o se un chiamante non dispone dell'autorizzazione necessaria per visualizzare l'oggetto.  
  
 Un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come OBJECT_DEFINITION possono restituire NULL se l'utente non dispone di alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Osservazioni  
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] presuppone che *object_id* si trovi nel contesto di database corrente. Le regole di confronto della definizione dell'oggetto corrispondono sempre alle regole di confronto del contesto del database chiamante.  
  
 OBJECT_DEFINITION è applicabile ai tipi di oggetti seguenti:  
  
-   C = vincolo CHECK  
  
-   D = DEFAULT (vincolo o valore autonomo)  
  
-   P = stored procedure SQL  
  
-   FN = funzione scalare SQL  
  
-   R = regola  
  
-   RF = procedura di filtro della replica  
  
-   TR = trigger SQL (trigger DML con ambito schema, o trigger DDL con ambito database o server)  
  
-   IF = funzione SQL inline valutata a livello di tabella  
  
-   TF = funzione SQL con valori di tabella  
  
-   V = vista  
  
## <a name="permissions"></a>Autorizzazioni  
 Le definizioni degli oggetti di sistema sono visibili pubblicamente. La definizione degli oggetti utente è visibile al proprietario degli oggetti o agli utenti autorizzati che hanno una delle autorizzazioni seguenti: ALTER, CONTROL, TAKE OWNERSHIP o VIEW DEFINITION. Queste autorizzazioni sono assegnate implicitamente ai membri dei ruoli predefiniti del database **db_owner**, **db_ddladmin** e **db_securityadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-source-text-of-a-user-defined-object"></a>R. Restituzione del testo di origine di un oggetto definito dall'utente  
 Nell'esempio seguente viene restituita la definizione di un trigger definito dall'utente, `uAddress`, nello schema `Person`. La funzione predefinita `OBJECT_ID` viene utilizzata per restituire l'ID dell'oggetto del trigger all'istruzione `OBJECT_DEFINITION`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID(N'Person.uAddress')) AS [Trigger Definition];   
GO  
```  
  
### <a name="b-returning-the-source-text-of-a-system-object"></a>B. Restituzione del testo di origine di un oggetto di sistema  
 Nell'esempio seguente viene restituita la definizione della stored procedure di sistema `sys.sp_columns`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID(N'sys.sp_columns')) AS [Object Definition];  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)  
  
  

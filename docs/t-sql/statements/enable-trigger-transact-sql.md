---
description: ENABLE TRIGGER (Transact-SQL)
title: ENABLE TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENABLE TRIGGER
- ENABLE_TSQL
- ENABLE_TRIGGER_TSQL
- ENABLE
dev_langs:
- TSQL
helpviewer_keywords:
- DDL triggers, enabling
- triggers [SQL Server], enabling
- DML triggers, enabling
- ENABLE TRIGGER statement
ms.assetid: 6e21f0ad-68d0-432f-9c7c-a119dd2d3fc9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3ed2081a912291eed4682ba0e853df66f36e1801
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091929"
---
# <a name="enable-trigger-transact-sql"></a>ENABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Abilita un trigger DML, DDL o LOGON.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ENABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*schema_name*  
Nome dello schema a cui appartiene il trigger. *schema_name* non può essere specificato per i trigger DDL o LOGON.  
  
*trigger_name*  
Nome del trigger da abilitare.  
  
ALL  
Indica che vengono abilitati tutti i trigger definiti nell'ambito della clausola ON.  
  
*object_name*  
Nome della tabella o della vista su cui deve essere eseguito il trigger DML *trigger_name*.  
  
DATABASE  
Per un trigger DDL, indica che *trigger_name* è stato creato o modificato per essere eseguito con ambito database.  
  
ALL SERVER  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
Per un trigger DDL, indica che *trigger_name* è stato creato o modificato per essere eseguito con ambito server. ALL SERVER si applica anche ai trigger LOGON.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
## <a name="remarks"></a>Osservazioni  
L'abilitazione di un trigger non comporta la sua creazione ex-novo. Un trigger disabilitato continua a esistere come oggetto nel database corrente, ma non viene attivato. Se si abilita un trigger, questo verrà attivato ogni volta che vengono eseguite istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per le quali è stato programmato in origine. I trigger vengono disabilitati tramite [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md). I trigger DML definiti su tabelle possono essere disabilitati o abilitati anche tramite [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
Per abilitare un trigger DML, un utente deve avere almeno l'autorizzazione ALTER per la tabella o vista in base alla quale il trigger è stato creato.  
  
Per abilitare un trigger DDL con ambito server (ON ALL SERVER) o un trigger LOGON, un utente deve avere l'autorizzazione CONTROL SERVER per il server. Per abilitare un trigger DDL nell'ambito del database (ON DATABASE), un utente deve avere almeno l'autorizzazione ALTER ANY DATABASE DDL TRIGGER nel database corrente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-enabling-a-dml-trigger-on-a-table"></a>R. Abilitazione di un trigger DML in una tabella  
Nell'esempio seguente il trigger `uAddress` creato nella tabella `Address` del database AdventureWorks viene disabilitato e quindi abilitato.  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
ENABLE Trigger Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-enabling-a-ddl-trigger"></a>B. Abilitazione di un trigger DDL  
Nell'esempio seguente viene creato un trigger DDL `safety` con ambito database, che viene quindi disabilitato e poi abilitato.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "safety" to drop or alter tables!'   
   ROLLBACK;  
GO  
DISABLE TRIGGER safety ON DATABASE;  
GO  
ENABLE TRIGGER safety ON DATABASE;  
GO  
```  
  
### <a name="c-enabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. Abilitazione di tutti i trigger definiti nello stesso ambito  
Nell'esempio seguente vengono abilitati tutti i trigger DDL creati nell'ambito del server.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql  
ENABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  

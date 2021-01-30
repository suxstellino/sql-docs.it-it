---
description: DISABLE TRIGGER (Transact-SQL)
title: DISABLE TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DISABLE_TSQL
- DISABLE
- DISABLE TRIGGER
- DISABLE_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DML triggers, disabling
- DDL triggers, disabling
- DISABLE TRIGGER statement
- triggers [SQL Server], disabling
- disabling triggers
ms.assetid: e6529f06-e442-437e-a7bf-41790bc092c5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 01302c2d9acdf1ce89246be18fd54c3cae72ebff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159345"
---
# <a name="disable-trigger-transact-sql"></a>DISABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Consente di disabilitare un trigger.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DISABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome dello schema a cui appartiene il trigger. *schema_name* non può essere specificato per i trigger DDL o LOGON.  
  
 *trigger_name*  
 Nome del trigger che si desidera disabilitare.  
  
 ALL  
 Disabilita tutti i trigger con l'ambito specificato nella clausola ON.  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea trigger nei database pubblicati per la replica di tipo merge. Se si specifica ALL nei database pubblicati, questi trigger vengono disabilitati e la replica viene ostacolata. Prima di utilizzare l'argomento ALL verificare che il database corrente non sia pubblicato per la replica di tipo merge.  
  
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
 Per impostazione predefinita, i trigger vengono abilitati in fase di creazione. La disabilitazione di un trigger non ne comporta l'eliminazione. Il trigger continua a esistere come oggetto nel database corrente ma non viene attivato quando viene eseguita una qualsiasi istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in cui è stato programmato. I trigger possono essere riabilitati tramite [ENABLE TRIGGER](../../t-sql/statements/enable-trigger-transact-sql.md). I trigger DML definiti su tabelle possono essere disabilitati o abilitati anche tramite [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 La modifica del trigger tramite l'istruzione **ALTER TRIGGER** comporta l'abilitazione del trigger.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per disabilitare un trigger DML, è necessario disporre almeno dell'autorizzazione ALTER per la tabella o la vista per cui il trigger è stato creato.  
  
 Per disabilitare un trigger DDL con ambito server (ON ALL SERVER) o un trigger LOGON, è necessario disporre dell'autorizzazione CONTROL SERVER per il server. Per disabilitare un trigger DDL con ambito database (ON DATABASE), è necessario disporre almeno dell'autorizzazione ALTER ANY DATABASE DDL TRIGGER nel database corrente.  
  
## <a name="examples"></a>Esempio  
Gli esempi seguenti sono descritti nel database AdventureWorks2012.
  
### <a name="a-disabling-a-dml-trigger-on-a-table"></a>R. Disabilitazione di un trigger DML in una tabella  
 Nell'esempio seguente viene disabilitato il trigger `uAddress` creato per la tabella `Person`.  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-disabling-a-ddl-trigger"></a>B. Disabilitazione di un trigger DDL  
 Nell'esempio seguente viene creato e quindi disabilitato un trigger DDL `safety` con ambito database.  
  
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
```  
  
### <a name="c-disabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. Disabilitazione di tutti i trigger definiti con lo stesso ambito  
 Nell'esempio seguente vengono disabilitati tutti i trigger DDL creati nell'ambito del server.  
  
```sql  
DISABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  

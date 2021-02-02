---
description: DROP TRIGGER (Transact-SQL)
title: DROP TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP TRIGGER
- DROP_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- renaming triggers
- triggers [SQL Server], removing
- DDL triggers, removing
- DROP TRIGGER statement
- deleting triggers
- dropping triggers
- removing triggers
- DML triggers, removing
ms.assetid: 092d0d71-9f1e-4e38-a1c4-2487adfa5b4e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0be25328507c6afa210cd4d51817551395f94770
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237756"
---
# <a name="drop-trigger-transact-sql"></a>DROP TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Rimuove uno o più trigger DML o DDL dal database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
DROP TRIGGER [ IF EXISTS ] [schema_name.]trigger_name [ ,...n ] [ ; ]  
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE or UPDATE statement (DDL Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON { DATABASE | ALL SERVER }   
[ ; ]  
  
-- Trigger on a LOGON event (Logon Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON ALL SERVER  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *IF EXISTS*  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level), [!INCLUDE[sssds](../../includes/sssds-md.md)]).  
  
 Rimuove in modo condizionale il trigger solo se esiste già.  
  
 *schema_name*  
 Nome dello schema a cui appartiene un trigger DML. L'ambito dei trigger DML è definito nello schema della tabella o della vista in cui sono i trigger stessi creati. *schema_name* non può essere specificato per i trigger DDL o LOGON.  
  
 *trigger_name*  
 Nome del trigger da rimuovere. Per vedere un elenco dei trigger attualmente creati, usare [sys.server_assembly_modules](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) o [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md).  
  
 DATABASE  
 Indica che l'ambito del trigger DDL corrisponde al database corrente. È necessario specificare DATABASE se tale argomento è stato specificato anche al momento della creazione o della modifica del trigger.  
  
 ALL SERVER  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Indica che l'ambito del trigger DDL corrisponde al server corrente. È necessario specificare ALL SERVER se tale valore è stato specificato anche al momento della creazione o della modifica del trigger. ALL SERVER si applica anche ai trigger LOGON.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile rimuovere un trigger DML eliminando il trigger stesso o la tabella di trigger corrispondente. Quando si elimina una tabella, vengono eliminati anche tutti i trigger associati.  
  
 Se si elimina un trigger, le informazioni relative al trigger vengono rimosse dalle viste del catalogo **sys.objects**, **sys.triggers** e **sys.sql_modules**.  
  
 È possibile eliminare più trigger DDL con una singola istruzione DROP TRIGGER solo se tutti i trigger sono stati creati con clausole ON identiche.  
  
 Per rinominare un trigger, utilizzare le istruzioni DROP TRIGGER e CREATE TRIGGER. Per modificare la definizione di un trigger, utilizzare ALTER TRIGGER.  
  
 Per altre informazioni su come determinare le dipendenze per un trigger specifico, vedere [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md), [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) e [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).  
  
 Per altre informazioni su come visualizzare il testo del trigger, vedere [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md) e [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
 Per altre informazioni su come visualizzare un elenco dei trigger esistenti, vedere [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) e [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Per l'eliminazione di un trigger DML è richiesta l'autorizzazione ALTER per la tabella o la vista in cui è definito il trigger.  
  
 Per eliminare un trigger DDL definito con ambito server (ON ALL SERVER) o un trigger LOGON è necessaria l'autorizzazione CONTROL SERVER nel server. Per l'eliminazione di un trigger DDL con ambito database (ON DATABASE) è richiesta l'autorizzazione ALTER ANY DATABASE DDL TRIGGER nel database corrente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-a-dml-trigger"></a>R. Eliminazione di un trigger DML  
 Nell'esempio seguente viene eliminato il trigger `employee_insupd` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile usare la sintassi DROP TRIGGER IF EXISTS.  
  
```sql  
IF OBJECT_ID ('employee_insupd', 'TR') IS NOT NULL  
   DROP TRIGGER employee_insupd;  
```  
  
### <a name="b-dropping-a-ddl-trigger"></a>B. Eliminazione di un trigger DDL  
 Nell'esempio seguente viene eliminato il trigger DDL `safety`.  
  
> [!IMPORTANT]  
>  Poiché i trigger DDL non hanno ambito schema e pertanto non sono inclusi nella vista del catalogo **sys.objects**, non è possibile usare la funzione OBJECT_ID per eseguire una query e scoprire se esistono nel database. Per gli oggetti che non hanno ambito schema è necessario eseguire query nella vista del catalogo appropriata. Per i trigger DDL usare **sys.triggers**.  
  
```sql  
DROP TRIGGER safety  
ON DATABASE;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Ottieni informazioni sui trigger DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  

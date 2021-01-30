---
description: ALTER DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)
title: ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_DATABASE_AUDIT_SPECIFICATION_TSQL
- ALTER DATABASE AUDIT SPECIFICATION
- ALTER_DATABASE_AUDIT_TSQL
- ALTER DATABASE AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE AUDIT SPECIFICATION statement
ms.assetid: 85f4e7e6-a330-4de0-9048-64f386ccc314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 088c1503fd306386ca85f36be97b78dcf480e7ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195931"
---
# <a name="alter-database-audit-specification-transact-sql"></a>ALTER DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica un oggetto specifica controllo database utilizzando la funzionalità [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit. Per altre informazioni, vedere [SQL Server Audit &#40;Motore di database&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    [ FOR SERVER AUDIT audit_name ]  
    [ { { ADD | DROP } (   
           { <audit_action_specification> | audit_action_group_name }   
                )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      <action_specification>[ ,...n ] ON [ class :: ] securable   
     BY principal [ ,...n ]   
}  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *audit_specification_name*  
 Nome della specifica del controllo.  
  
 *audit_name*  
 Nome del controllo al quale viene applicata questa specifica.  
  
 *audit_action_specification*  
 Nome di una o più azioni controllabili a livello di database. Per un elenco dei gruppi di azioni di controllo, vedere [Azioni e gruppi di azioni di SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Nome di uno o più gruppi di azioni controllabili a livello di database. Per un elenco dei gruppi di azioni di controllo, vedere [Azioni e gruppi di azioni di SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Nome della classe nell'entità a sicurezza diretta, se applicabile.  
  
 *securable*  
 Tabella, vista oppure altro oggetto a sicurezza diretta nel database cui applicare l'azione di controllo oppure il gruppo di azioni di controllo. Per altre informazioni, vedere [Entità a protezione diretta](../../relational-databases/security/securables.md).  
  
 *column*  
 Nome della colonna nell'entità a sicurezza diretta, se applicabile.  
  
 *principal*  
 Nome di entità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cui applicare l'azione di controllo oppure il gruppo di azioni di controllo. Per altre informazioni, vedere [Principals &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH **(** STATE **=** { ON | OFF } **)**  
 Abilita o disabilita la raccolta di record mediante il controllo per questa specifica del controllo. Le modifiche relative allo stato della specifica di controllo devono essere apportate all'esterno di una transazione utente e non possono contenere altre modifiche nella stessa istruzione in presenza di una transizione da ON a OFF.  
  
## <a name="remarks"></a>Commenti  
 Le specifiche del controllo del database sono oggetti non a sicurezza diretta che risiedono in un database specifico. È necessario impostare lo stato di una specifica di controllo sull'opzione OFF per apportare modifiche a una specifica del controllo del database. Se ALTER DATABASE AUDIT SPECIFICATION viene eseguita quando un controllo è abilitato con qualsiasi altra opzione diversa da STATE=OFF, verrà visualizzato un messaggio di errore. Per altre informazioni, vedere [tempdb Database](../../relational-databases/databases/tempdb-database.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Gli utenti che dispongono dell'autorizzazione ALTER ANY DATABASE AUDIT possono modificare specifiche del controllo del database e associarle a qualsiasi controllo.  
  
 Dopo essere stata creata, la specifica del controllo del database può essere visualizzata dalle entità che dispongono dell'autorizzazione CONTROL SERVER o ALTER ANY DATABASE AUDIT o dell'account sysadmin oppure dalle entità che possono accedere esplicitamente al controllo.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene modificata una specifica del controllo del database denominata `HIPAA_Audit_DB_Specification` che controlla le istruzioni `SELECT` mediante l'utente `dbo`, per un oggetto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit denominato `HIPAA_Audit`.  
  
```sql  
ALTER DATABASE AUDIT SPECIFICATION HIPAA_Audit_DB_Specification  
FOR SERVER AUDIT HIPAA_Audit  
    ADD (SELECT  
         ON OBJECT::dbo.Table1  
         BY dbo)  
    WITH (STATE = ON);  
GO  
```  
  
 Per un esempio completo delle modalità di creazione di un controllo, vedere [SQL Server Audit &#40;motore di database&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [Creazione di un controllo del server e di una specifica del controllo del server](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

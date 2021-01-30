---
description: DROP SERVER ROLE (Transact-SQL)
title: DROP SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SERVER ROLE
- DROP_SERVER_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, DROP
- DROP SERVER ROLE statement
ms.assetid: a2a1e6e6-e40c-4d6a-81be-d197b80bf226
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9993ef678f2c8526e0ce9aa9daa1b7ac1d1e82e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160205"
---
# <a name="drop-server-role-transact-sql"></a>DROP SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

  Rimuove un ruolo del server definito dall'utente.  
  
 I ruoli del server definiti dall'utente sono una novità di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
DROP SERVER ROLE role_name  
```  
  
## <a name="arguments"></a>Argomenti  
 *role_name*  
 Specifica il ruolo del server definito dall'utente da rimuovere dal server.  
  
## <a name="remarks"></a>Commenti  
 I ruoli del server definiti dall'utente proprietari di entità a protezione diretta non possono essere rimossi dal server. Per rimuovere un ruolo del server definito dall'utente proprietario di entità a protezione diretta, è innanzitutto necessario trasferire la proprietà di tali entità oppure eliminarle.  
  
 I ruoli del server definiti dall'utente che includono membri non possono essere rimossi. Per rimuovere un ruolo del server definito dall'utente che include membri, è prima necessario rimuovere i membri del ruolo usando [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 Impossibile rimuovere i ruoli predefiniti del server.  
  
 È possibile visualizzare le informazioni sulle appartenenze dei ruoli eseguendo una query sulla vista del catalogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il ruolo del server o l'autorizzazione ALTER ANY SERVER ROLE.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-to-drop-a-server-role"></a>R. Per eliminare un ruolo del server  
 Nell'esempio seguente viene eliminato il ruolo del server `purchasing`.  
  
```sql  
DROP SERVER ROLE purchasing;  
GO  
```  
  
### <a name="b-to-view-role-membership"></a>B. Per visualizzare l'appartenenza ai ruoli  
 Per visualizzare l'appartenenza ai ruoli, usare la pagina relativa al **ruolo del server (membri)** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o eseguire la query seguente:  
  
```sql  
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
### <a name="c-to-view-role-membership"></a>C. Per visualizzare l'appartenenza ai ruoli  
 Per determinare se un ruolo del server possiede un altro ruolo del server, eseguire la query seguente:  
  
```sql  
SELECT SP1.name AS RoleOwner, SP2.name AS Server_Role  
FROM sys.server_principals AS SP1  
JOIN sys.server_principals AS SP2  
    ON SP1.principal_id = SP2.owning_principal_id   
ORDER BY SP1.name ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  

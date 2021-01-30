---
description: CREATE SERVER ROLE (Transact-SQL)
title: CREATE SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SERVER_ROLE_TSQL
- CREATE SERVER ROLE
- SERVER ROLE
- CREATE_SERVER_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE
- SERVER ROLE, CREATE
- CREATE SERVER ROLE statement
- ROLE
- user-defined server roles [SQL Server]
- roles, server
ms.assetid: 30c92f80-f7f6-4a84-ae89-16e69add0de6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cfaf0335c2ba61e093764352b16847f0f96b295c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192650"
---
# <a name="create-server-role-transact-sql"></a>CREATE SERVER ROLE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un nuovo ruolo del server definito dall'utente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
CREATE SERVER ROLE role_name [ AUTHORIZATION server_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *role_name*  
 Nome del ruolo del server che si desidera creare.  
  
 AUTHORIZATION *server_principal*  
 Account di accesso che diventerà proprietario del nuovo ruolo del server. Se non viene specificato alcun account di accesso, il ruolo del server sarà di proprietà dell'account di accesso che esegue l'istruzione CREATE SERVER ROLE.  
  
## <a name="remarks"></a>Osservazioni  
 I ruoli del server sono entità a protezione diretta a livello di server. Dopo aver creato un ruolo del server, configurare le autorizzazioni a livello di server per il ruolo tramite GRANT, DENY e REVOKE. Per aggiungere o rimuovere account di accesso a o da un ruolo del server, usare [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md). Per eliminare un ruolo del server, usare [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md). Per altre informazioni, vedere [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 È possibile visualizzare i ruoli del server eseguendo una query sulle viste del catalogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) e [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Non è possibile concedere ai ruoli del server l'autorizzazione sulle entità a protezione diretta a livello di database. Per creare ruoli del database, vedere [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md).  
  
 Per informazioni sulla progettazione di un sistema di autorizzazioni, vedere [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE SERVER ROLE o l'appartenenza al ruolo predefinito del server sysadmin.  
  
 È anche richiesta l'autorizzazione IMPERSONATE in *server_principal* per gli account di accesso, l'autorizzazione ALTER per i ruoli del server usati come *server_principal* o l'appartenenza a un gruppo di Windows usato come server_principal.  
  
 Verrà generato l'evento Audit Server Principal Management con il tipo di oggetto impostato sul ruolo del server e il tipo di evento da aggiungere.  
  
 Se si utilizza l'opzione AUTHORIZATION per assegnare la proprietà del ruolo del server, sono necessarie anche le autorizzazioni seguenti:  
  
-   Per assegnare la proprietà di un ruolo del server a un altro account di accesso, è richiesta l'autorizzazione IMPERSONATE per tale account di accesso.  
  
-   Per assegnare la proprietà di un ruolo del server a un altro ruolo del server, è richiesta l'appartenenza al ruolo del server destinatario oppure l'autorizzazione ALTER per tale ruolo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-server-role-that-is-owned-by-a-login"></a>R. Creazione di un ruolo del server di proprietà di un account di accesso  
 Nell'esempio seguente viene creato il ruolo del server `buyers` di proprietà dell'account di accesso `BenMiller`.  
  
```sql  
USE master;  
CREATE SERVER ROLE buyers AUTHORIZATION BenMiller;  
GO  
```  
  
### <a name="b-creating-a-server-role-that-is-owned-by-a-fixed-server-role"></a>B. Creazione di un ruolo del server di proprietà di un ruolo predefinito del server  
 Nell'esempio seguente viene creato il ruolo del server `auditors` di proprietà del ruolo predefinito del server `securityadmin`.  
  
```sql  
USE master;  
CREATE SERVER ROLE auditors AUTHORIZATION securityadmin;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  

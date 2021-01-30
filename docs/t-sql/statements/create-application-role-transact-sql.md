---
description: CREATE APPLICATION ROLE (Transact-SQL)
title: CREATE APPLICATION ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPLICATION_ROLE_TSQL
- CREATE APPLICATION ROLE
- sql13.swb.applicationrole.permissions.f1
- APPLICATION
- APPLICATION ROLE
- CREATE_APPLICATION_ROLE_TSQL
- APPLICATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE APPLICATION ROLE statement
- application roles [SQL Server], creating
ms.assetid: 647386da-ee80-41cf-86c9-dd590f9d66b6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 61ea2134ab91109434aab73072f8befb3d6bd29f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205812"
---
# <a name="create-application-role-transact-sql"></a>CREATE APPLICATION ROLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Aggiunge un ruolo applicazione al database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
CREATE APPLICATION ROLE application_role_name   
    WITH PASSWORD = 'password' [ , DEFAULT_SCHEMA = schema_name ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *application_role_name*  
 Specifica il nome del ruolo applicazione. Il nome non deve già essere usato per fare riferimento a un'altra entità nel database.  
  
 PASSWORD **='** _password_ **'**  
 Specifica la password che verrà usata dagli utenti di database per attivare il ruolo applicazione. È necessario usare sempre password complesse. *password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 DEFAULT_SCHEMA **=** _schema\_name_  
 Specifica il primo schema in cui il server eseguirà la ricerca per la risoluzione dei nomi degli oggetti per il ruolo. Se DEFAULT_SCHEMA non viene definito, il ruolo applicazione utilizzerà DBO come schema predefinito. *schema_name* può essere uno schema che non esiste nel database.  
  
## <a name="remarks"></a>Commenti  
  
> [!IMPORTANT]  
>  In fase di impostazione delle password per i ruoli applicazione viene eseguito il controllo dei requisiti di complessità delle password. Le applicazioni che richiamano i ruoli applicazione devono archiviare le relative password. Le password dei ruoli applicazione devono essere sempre archiviate in forma crittografata.  
  
 I ruoli applicazione sono visibili nella vista del catalogo [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).  
  
 Per altre informazioni sull'uso dei ruoli applicazione, vedere [Ruoli applicazione](../../relational-databases/security/authentication-access/application-roles.md).  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY APPLICATION ROLE nel database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creato un ruolo applicazione denominato `weekly_receipts` con la password `987Gbv876sPYY5m23` e lo schema predefinito `Sales`.  
  
```sql  
CREATE APPLICATION ROLE weekly_receipts   
    WITH PASSWORD = '987G^bv876sPY)Y5m23'   
    , DEFAULT_SCHEMA = Sales;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Ruoli applicazione](../../relational-databases/security/authentication-access/application-roles.md)   
 [sp_setapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [ALTER APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [Criteri password](../../relational-databases/security/password-policy.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

---
description: DENY (autorizzazioni per schemi) (Transact-SQL)
title: DENY (autorizzazioni per schemi) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [SQL Server], schemas
- schemas [SQL Server], permissions
- permissions [SQL Server], schemas
- DENY statement, schemas
ms.assetid: 300a67c4-d226-4653-9e9f-7ae4d53fcf33
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f27ddae862eb4cfc3b84db6d945818ca706e1034
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177719"
---
# <a name="deny-schema-permissions-transact-sql"></a>DENY (autorizzazioni per schemi) (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Nega autorizzazioni per uno schema.  
  

![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DENY permission  [ ,...n ] } ON SCHEMA :: schema_name  
    TO database_principal [ ,...n ]   
    [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*permission*  
Specifica un'autorizzazione che può essere negata per uno schema. Per un elenco di queste autorizzazioni, vedere la sezione Osservazioni di seguito in questo articolo.  
  
ON SCHEMA **::** schema *_name*  
Specifica lo schema per cui viene negata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
*database_principal*  
Specifica l'entità a cui viene negata l'autorizzazione. *database_principal* può essere una delle seguenti entità di sicurezza:  
  
-   Utente del database  
-   Ruolo del database  
-   Ruolo applicazione  
-   Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows  
-   Utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
-   Utente del database di cui è stato eseguito il mapping a un certificato  
-   Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
-   Utente del database sul quale non viene eseguito il mapping ad alcuna entità server  
  
CASCADE  
Nega l'autorizzazione a eventuali altre entità per cui l'oggetto specificato *database_principal* ha concesso l'autorizzazione.
  
*denying_principal*  
Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione. *denying_principal* può essere una delle seguenti entità di sicurezza:  
  
-   Utente del database  
-   Ruolo del database  
-   Ruolo applicazione  
-   Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows  
-   Utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
-   Utente del database di cui è stato eseguito il mapping a un certificato  
-   Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
-   Utente del database sul quale non viene eseguito il mapping ad alcuna entità server  
  
## <a name="remarks"></a>Commenti  
Uno schema è un'entità a protezione diretta a livello di database, contenuta nel database padre nella gerarchia di autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile negare per uno schema. La tabella contiene le autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione dello schema|Autorizzazione dello schema in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|-----------------------|----------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SCHEMA|  
|CONTROL|CONTROL|CONTROL|  
|CREATE SEQUENCE|ALTER|ALTER ANY SCHEMA|  
|DELETE|CONTROL|DELETE|  
|EXECUTE|CONTROL|EXECUTE|  
|INSERT|CONTROL|INSERT|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|CONTROL|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'autorizzazione CONTROL per lo schema. Se si usa l'opzione AS, l'entità specificata deve essere proprietaria dello schema.  
  
## <a name="see-also"></a>Vedere anche  
[CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md)   
[DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
[Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
[Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
[sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
[HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  

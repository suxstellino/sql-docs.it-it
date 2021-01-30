---
description: DENY - autorizzazioni per tipi (Transact-SQL)
title: DENY - autorizzazioni per tipi (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, types
- permissions [SQL Server], types
- type permissions [SQL Server]
- denying permissions [SQL Server], types
ms.assetid: 564e3500-c567-43dc-993b-9ab50e99cf3f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 18aa1ff5bbfdf44d17a5b107f07c623f60f2a4bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211679"
---
# <a name="deny-type-permissions-transact-sql"></a>DENY - autorizzazioni per tipi (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Nega le autorizzazioni per un tipo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DENY permission  [ ,...n ] ON TYPE :: [ schema_name . ] type_name  
        TO <database_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS <database_principal> ]  
  
<database_principal> ::=   
        Database_user   
    | Database_role   
    | Application_role   
    | Database_user_mapped_to_Windows_User   
    | Database_user_mapped_to_Windows_Group   
    | Database_user_mapped_to_certificate   
    | Database_user_mapped_to_asymmetric_key   
    | Database_user_with_no_login  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere negata per un tipo. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 ON TYPE **::** [ _schema_name_ **.** ] *type_name*  
 Specifica il tipo per cui viene negata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio. Se si omette *schema_name*, viene usato lo schema predefinito. Se si specifica *schema_name*, il qualificatore di ambito dello schema ( **.** ) è obbligatorio.  
  
 TO \<database_principal>  
 Specifica l'entità a cui viene negata l'autorizzazione.  
  
 CASCADE  
 Indica che l'autorizzazione negata viene negata anche ad altre entità alle quali è stata concessa da questa entità.  
  
 AS \<database_principal>  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione.  
  
 *Database_user*  
 Specifica un utente di database.  
  
 *Database_role*  
 Specifica un ruolo del database.  
  
 *Application_role*  
   
 Specifica un ruolo applicazione.  
  
 *Database_user_mapped_to_Windows_User*  
 
 Specifica un utente del database sul quale viene eseguito il mapping a un utente di Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
 
 Specifica un utente del database sul quale viene eseguito il mapping a un gruppo di Windows.  
  
 *Database_user_mapped_to_certificate*  
 
 Specifica un utente del database sul quale viene eseguito il mapping a un certificato.  
  
 *Database_user_mapped_to_asymmetric_key*  
  
 Specifica un utente del database sul quale viene eseguito il mapping a una chiave asimmetrica.  
  
 *Database_user_with_no_login*  
 Specifica un utente del database per cui non esiste un'entità corrispondente a livello del server.  
  
## <a name="remarks"></a>Osservazioni  
 Un tipo è un'entità a sicurezza diretta a livello di schema contenuta nello schema padre nella gerarchia delle autorizzazioni.  
  
> [!IMPORTANT]  
>  Le autorizzazioni **GRANT**, **DENY,** e **REVOKE** non si applicano ai tipi di sistema. Ai tipi definiti dall'utente è possibile concedere autorizzazioni. Per altre informazioni sui tipi definiti dall'utente, vedere [Uso di tipi definiti dall'utente in SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md).  
  
 Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile negare per un tipo, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del tipo|Autorizzazione del tipo in cui è inclusa|Autorizzazione dello schema in cui è inclusa|  
|---------------------|--------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|EXECUTE|CONTROL|EXECUTE|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il tipo. Se si utilizza la clausola AS, l'entità specificata deve essere proprietaria del tipo per cui vengono negate le autorizzazioni.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene negata l'autorizzazione `VIEW DEFINITION` con `CASCADE` per il tipo definito dall'utente `PhoneNumber` a `KhalidR`. `PhoneNumber` è incluso nello schema `Telemarketing`.  
  
```sql 
DENY VIEW DEFINITION ON TYPE::Telemarketing.PhoneNumber   
    TO KhalidR CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [GRANT - Autorizzazioni per tipi &#40;Transact-SQL&#41;](../../t-sql/statements/grant-type-permissions-transact-sql.md)   
 [REVOKE - Autorizzazioni per tipi &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-type-permissions-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità a protezione diretta](../../relational-databases/security/securables.md)  
  
  

---
title: REVOKE - Autorizzazioni per entità di database
description: Revocare le autorizzazioni per un utente di database, un ruolo del database o un ruolo applicazione.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], permissions
- REVOKE statement, roles
- database user permissions [SQL Server]
- REVOKE statement, users
- application roles [SQL Server], permissions
ms.assetid: c45e1086-c25b-48bb-a764-4a893e983db2
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 312c90b77326bf153cc73f805ebed0e8a1b020f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209895"
---
# <a name="revoke-database-principal-permissions-transact-sql"></a>REVOKE - autorizzazioni per entità di database (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Revoca le autorizzazioni concesse o negate per un utente di database, un ruolo del database o un ruolo applicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ]    
    ON   
    {  [ USER :: database_user ]  
       | [ ROLE :: database_role ]  
       | [ APPLICATION ROLE :: application_role ]  
    }  
    { FROM | TO } <database_principal> [ ,...n ]  
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
 Specifica un'autorizzazione che può essere revocata per un'entità di database. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 USER ::*database_user*  
 Specifica la classe e il nome dell'utente per cui viene revocata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 ROLE ::*database_role*  
 Specifica la classe e il nome del ruolo per cui viene revocata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 APPLICATION ROLE ::*application_role*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 Specifica la classe e il nome del ruolo applicazione per cui viene revocata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 GRANT OPTION  
 Indica che verrà revocato il diritto di concedere l'autorizzazione specificata ad altre entità. L'autorizzazione stessa non verrà revocata.  
  
> [!IMPORTANT]  
>  Se l'autorizzazione specificata è stata concessa all'entità senza l'opzione GRANT, l'autorizzazione stessa verrà revocata.  
  
 CASCADE  
 Indica che l'autorizzazione che viene revocata anche ad altre entità a cui è stata concessa o negata da questa entità.  
  
> [!CAUTION]  
>  La revoca propagata di un'autorizzazione concessa con WITH GRANT OPTION comporterà la revoca sia delle autorizzazioni GRANT che delle autorizzazioni DENY per tale autorizzazione.  
  
 AS \<database_principal> Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di revocare l'autorizzazione.  
  
 *Database_user*  
 Specifica un utente di database.  
  
 *Database_role*  
 Specifica un ruolo del database.  
  
 *Application_role*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 Specifica un ruolo applicazione.  
  
 *Database_user_mapped_to_Windows_User*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Specifica un utente del database sul quale viene eseguito il mapping a un utente di Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Specifica un utente del database sul quale viene eseguito il mapping a un gruppo di Windows.  
  
 *Database_user_mapped_to_certificate*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Specifica un utente del database sul quale viene eseguito il mapping a un certificato.  
  
 *Database_user_mapped_to_asymmetric_key*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Specifica un utente del database sul quale viene eseguito il mapping a una chiave asimmetrica.  
  
 *Database_user_with_no_login*  
 Specifica un utente del database per cui non esiste un'entità corrispondente a livello del server.  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="database-user-permissions"></a>Autorizzazioni per utenti di database  
 Un utente di database è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile revocare per un utente di database, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione dell'utente di database|Autorizzazione dell'utente di database in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|IMPERSONATE|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY USER|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="database-role-permissions"></a>Autorizzazioni per ruoli del database  
 Un ruolo del database è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile revocare per un ruolo del database, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del ruolo del database|Autorizzazione del ruolo del database in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="application-role-permissions"></a>Autorizzazioni per i ruoli applicazione  
 Un ruolo applicazione è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile revocare per un ruolo applicazione, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del ruolo applicazione|Autorizzazione del ruolo applicazione in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|---------------------------------|--------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY APPLICATION ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per l'entità specificata o un'autorizzazione di livello superiore che include l'autorizzazione CONTROL.  
  
 Gli utenti che dispongono dell'autorizzazione CONTROL per un database, ad esempio i membri del ruolo predefinito del database **db_owner**, possono concedere qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-revoking-control-permission-on-a-user-from-another-user"></a>R. Revoca dell'autorizzazione CONTROL per un utente a un altro utente  
 Nell'esempio seguente viene revocata l'autorizzazione `CONTROL` per l'utente [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] del database `Wanida` all'utente `RolandX`.  
  
```sql  
USE AdventureWorks2012;  
REVOKE CONTROL ON USER::Wanida FROM RolandX;  
GO  
```  
  
### <a name="b-revoking-view-definition-permission-on-a-role-from-a-user-to-which-it-was-granted-with-grant-option"></a>B. Revoca dell'autorizzazione VIEW DEFINITION per un ruolo a un utente a cui è stata concessa con WITH GRANT OPTION  
 Nell'esempio seguente viene revocata l'autorizzazione `VIEW DEFINITION` per il ruolo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] del database `SammamishParking` all'utente di database `JinghaoLiu`. Viene specificata l'opzione `CASCADE` perché all'utente `JinghaoLiu` l'autorizzazione `VIEW DEFINITION` è stata concessa con `WITH GRANT OPTION`.  
  
```sql 
USE AdventureWorks2012;  
REVOKE VIEW DEFINITION ON ROLE::SammamishParking   
    FROM JinghaoLiu CASCADE;  
GO  
```  
  
### <a name="c-revoking-impersonate-permission-on-a-user-from-an-application-role"></a>C. Revoca dell'autorizzazione IMPERSONATE per un utente a un ruolo applicazione  
 Nell'esempio seguente viene revocata l'autorizzazione `IMPERSONATE` per l'utente `HamithaL` al ruolo applicazione `AccountsPayable17` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
```sql  
USE AdventureWorks2012;  
REVOKE IMPERSONATE ON USER::HamithaL FROM AccountsPayable17;  
GO    
```  
  
## <a name="see-also"></a>Vedere anche  
 [GRANT - autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)   
 [DENY - autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  


---
title: REVOKE - Autorizzazioni per entità server
description: Revocare le autorizzazioni per un account di accesso di SQL Server.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, impersonation
- permissions [SQL Server], impersonation
- permissions [SQL Server], logins
- impersonate [SQL Server], revoking
- logins [SQL Server], revoking
- REVOKE statement, logins
ms.assetid: 75409024-f150-4326-af16-9d60e900df18
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 062096e77c7e907594f7c2b9a372e3eb1899324a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185938"
---
# <a name="revoke-server-principal-permissions-transact-sql"></a>Autorizzazioni per entità server REVOKE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Revoca le autorizzazioni concesse o negate per un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    { FROM | TO } <server_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
    SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey     
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere revocata per un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 LOGIN **::** *SQL_Server_login*  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per cui viene revocata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 SERVER ROLE **::** *server_role*  
 Specifica il ruolo del server a cui viene revocata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 { FROM | TO } \<server_principal> Specifica l'account di accesso o il ruolo del server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da cui viene revocata l'autorizzazione.  
  
 *SQL_Server_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato da un account di accesso di Windows.  
  
 *SQL_Server_login_from_certificate*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a un certificato.  
  
 *SQL_Server_login_from_AsymKey*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a una chiave asimmetrica.  
  
 *server_role*  
 Specifica il nome di un ruolo del server definito dall'utente.  
  
 GRANT OPTION  
 Indica che verrà revocato il diritto di concedere l'autorizzazione specificata ad altre entità. L'autorizzazione stessa non verrà revocata.  
  
> [!IMPORTANT]  
>  Se l'autorizzazione specificata è stata concessa all'entità senza l'opzione GRANT, l'autorizzazione stessa verrà revocata.  
  
 CASCADE  
 Indica che l'autorizzazione che viene revocata anche ad altre entità a cui è stata concessa o negata da questa entità.  
  
> [!CAUTION]  
>  La revoca propagata di un'autorizzazione concessa con WITH GRANT OPTION comporterà la revoca sia delle autorizzazioni GRANT che delle autorizzazioni DENY per tale autorizzazione.  
  
 AS *SQL_Server_login*  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal quale l'entità che esegue la query ottiene il diritto di revocare l'autorizzazione.  
  
## <a name="remarks"></a>Osservazioni  
 I ruoli del server e gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono entità a protezione diretta a livello di server. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile revocare per un ruolo del server o account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del ruolo del server o dell'account di accesso di SQL Server|Autorizzazione del ruolo del server o dell'account di accesso di SQL Server in cui è inclusa|Autorizzazione del server in cui è inclusa|  
|------------------------------------------------|-----------------------------------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL SERVER|  
|IMPERSONATE|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|ALTER|CONTROL|ALTER ANY LOGIN<br /><br /> ALTER ANY SERVER ROLE|  
  
## <a name="permissions"></a>Autorizzazioni  
 Per gli account di accesso, è richiesta l'autorizzazione CONTROL per l'account di accesso o l'autorizzazione ALTER ANY LOGIN per il server.  
  
 Per i ruoli del server, è richiesta l'autorizzazione CONTROL per il ruolo del server o l'autorizzazione ALTER ANY SERVER ROLE per il server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-revoking-impersonate-permission-on-a-login"></a>R. Revoca dell'autorizzazione IMPERSONATE per un account di accesso  
 Nell'esempio seguente viene revocata l'autorizzazione `IMPERSONATE` per l'account di accesso `WanidaBenshoof` di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato dall'utente di Windows `AdvWorks\YoonM`.  
  
```sql  
USE master;  
REVOKE IMPERSONATE ON LOGIN::WanidaBenshoof FROM [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-revoking-view-definition-permission-with-cascade"></a>B. Revoca dell'autorizzazione VIEW DEFINITION con CASCADE  
 Nell'esempio seguente viene revocata l'autorizzazione `VIEW DEFINITION` per l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`EricKurjan` all'account accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`RMeyyappan`. L'opzione `CASCADE` indica che l'autorizzazione `VIEW DEFINITION` per `EricKurjan` verrà revocata anche alle entità a cui `RMeyyappan` ha concesso tale autorizzazione.  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON LOGIN::EricKurjan FROM RMeyyappan   
    CASCADE;  
GO   
```  
  
### <a name="c-revoking-view-definition-permission-on-a-server-role"></a>C. Revoca dell'autorizzazione VIEW DEFINITION per un ruolo del server  
 Nell'esempio seguente viene revocata l'autorizzazione `VIEW DEFINITION` nel ruolo del server `Sales` per il ruolo del server `Auditors`.  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [Autorizzazioni per entità server GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [Autorizzazioni per entità server DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  


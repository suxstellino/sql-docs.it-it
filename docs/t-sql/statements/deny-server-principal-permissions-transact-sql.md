---
description: Autorizzazioni per entità server DENY (Transact-SQL)
title: Autorizzazioni per entità server DENY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, impersonate
- permissions [SQL Server], impersonate
- impersonate [SQL Server], denying
- DENY statement, logins
- permissions [SQL Server], logins
- denying permissions [SQL Server], logins
- servers [SQL Server], permissions
- logins [SQL Server], denying access
ms.assetid: 859affa7-0567-47d1-9490-57c1abbd619b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8e3ea7c43e664883324f87f031ba47f985ef78bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177692"
---
# <a name="deny-server-principal-permissions-transact-sql"></a>Autorizzazioni per entità server DENY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Nega le autorizzazioni concesse per un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DENY permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    TO <server_principal> [ ,...n ]  
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
 Specifica un'autorizzazione che può essere negata per un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 LOGIN **::** *SQL_Server_login*  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per cui viene negata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 SERVER ROLE **::** *server_role*  
 Specifica il ruolo del server a cui viene negata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 TO \<server_principal>  
 Specifica il ruolo del server o l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui viene concessa l'autorizzazione.  
  
 TO *SQL_Server_login*  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui viene negata l'autorizzazione.  
  
 *SQL_Server_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato da un account di accesso di Windows.  
  
 *SQL_Server_login_from_certificate*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a un certificato.  
  
 *SQL_Server_login_from_AsymKey*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a una chiave asimmetrica.  
  
 *server_role*  
 Specifica il nome di un ruolo del server.  
  
 CASCADE  
 Indica che l'autorizzazione negata viene negata anche ad altre entità alle quali è stata concessa da questa entità.  
  
 AS *SQL_Server_login*  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile negare autorizzazioni nell'ambito del server solo se il database corrente è il database master.  
  
 Le informazioni sulle autorizzazioni del server sono disponibili nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md). Le informazioni sulle entità server sono disponibili nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 L'istruzione DENY non riesce se non si specifica CASCADE per la negazione di un'autorizzazione a un'entità a cui l'autorizzazione è stata concessa con GRANT OPTION.  
  
 I ruoli del server e gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono entità a protezione diretta a livello di server. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile negare per un ruolo del server o account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
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
  
### <a name="a-denying-impersonate-permission-on-a-login"></a>R. Negazione dell'autorizzazione IMPERSONATE per un account di accesso  
 Nell'esempio seguente viene negata l'autorizzazione `IMPERSONATE` per l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`WanidaBenshoof` a un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato dall'utente di Windows `AdvWorks\YoonM`.  
  
```  
USE master;  
DENY IMPERSONATE ON LOGIN::WanidaBenshoof TO [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-denying-view-definition-permission-with-cascade"></a>B. Negazione dell'autorizzazione VIEW DEFINITION con CASCADE  
 Nell'esempio seguente viene negata l'autorizzazione `VIEW DEFINITION` per l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`EricKurjan` all'account accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`RMeyyappan`. L'opzione `CASCADE` indica che l'autorizzazione `VIEW DEFINITION` per `EricKurjan` verrà negata anche alle entità a cui `RMeyyappan` ha concesso tale autorizzazione.  
  
```sql  
USE master;  
DENY VIEW DEFINITION ON LOGIN::EricKurjan TO RMeyyappan   
    CASCADE;  
GO   
```  
  
### <a name="c-denying-view-definition-permission-on-a-server-role"></a>C. Negazione dell'autorizzazione VIEW DEFINITION per un ruolo del server  
 Nell'esempio seguente viene negata l'autorizzazione `VIEW DEFINITION` nel ruolo del server `Sales` per il ruolo del server `Auditors`.  
  
```sql 
USE master;  
DENY VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [Autorizzazioni per entità server GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [Autorizzazioni per entità server REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  

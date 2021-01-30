---
title: GRANT - Autorizzazioni per un gruppo di disponibilità
description: Concedere le autorizzazioni per un gruppo di disponibilità Always On.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], permissions
- GRANT statement, availability groups
- granting permissions, [SQL Server], availability groups
- permissions [SQL Server], availability group
ms.assetid: 060eb839-666a-4046-9e1d-5edc9ea75a11
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d1047980f83de32c5631c7d45673fe743607ebbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208170"
---
# <a name="grant-availability-group-permissions-transact-sql"></a>GRANT, autorizzazioni del gruppo di disponibilità (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede le autorizzazioni su un gruppo di disponibilità AlwaysOn.  
  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GRANT permission  [ ,...n ] ON AVAILABILITY GROUP :: availability_group_name  
        TO < server_principal >  [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
        SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere concessa su un gruppo di disponibilità. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 ON AVAILABILITY GROUP **::** _availability_group_name_  
 Specifica il gruppo di disponibilità per cui viene concessa l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 TO \<server_principal>  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui viene concessa l'autorizzazione.  
  
 *SQL_Server_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato da un account di accesso di Windows.  
  
 *SQL_Server_login_from_certificate*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a un certificato.  
  
 *SQL_Server_login_from_AsymKey*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a una chiave asimmetrica.  
  
 WITH GRANT OPTION  
 Indica che l'entità potrà inoltre concedere l'autorizzazione specificata ad altre entità.  
  
 AS *SQL_Server_login*  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal quale l'entità che esegue la query ottiene il diritto di concedere l'autorizzazione.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile concedere autorizzazioni nell'ambito del server solo se il database corrente è il database **master**.  
  
 Le informazioni sui gruppi di disponibilità sono visibili nella vista del catalogo [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md). Le informazioni sulle autorizzazioni del server sono visibili nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) e le informazioni sulle entità server sono visibili nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Un gruppo di disponibilità è un'entità a protezione diretta a livello server. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere su un gruppo di disponibilità, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del gruppo di disponibilità|Autorizzazione del gruppo di disponibilità in cui è inclusa|Autorizzazione del server in cui è inclusa|  
|-----------------------------------|----------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY AVAILABILITY GROUP|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
 Per un grafico di tutte le autorizzazioni del [!INCLUDE[ssDE](../../includes/ssde-md.md)], vedere il documento relativo alle [autorizzazioni del motore di database](https://aka.ms/sql-permissions-poster).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il gruppo di disponibilità o l'autorizzazione ALTER ANY AVAILABILITY GROUP per il server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-granting-view-definition-permission-on-an-availability-group"></a>R. Concessione dell'autorizzazione VIEW DEFINITION per un gruppo di disponibilità  
 Nell'esempio seguente viene concessa l'autorizzazione `VIEW DEFINITION` sul gruppo di disponibilità `MyAg` per l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`ZArifin`.  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON AVAILABILITY GROUP::MyAg TO ZArifin;  
GO  
```  
  
### <a name="b-granting-take-ownership-permission-with-the-grant-option"></a>B. Concessione dell'autorizzazione TAKE OWNERSHIP con GRANT OPTION  
 Nell'esempio seguente viene concessa l'autorizzazione `TAKE OWNERSHIP` per il gruppo di disponibilità `MyAg` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski` con `GRANT OPTION`.  
  
```sql  
USE master;  
GRANT TAKE OWNERSHIP ON AVAILABILITY GROUP::MyAg TO PKomosinski   
    WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-control-permission-on-an-availability-group"></a>C. Concessione dell'autorizzazione CONTROL per un gruppo di disponibilità  
 Nell'esempio seguente viene concessa l'autorizzazione `CONTROL` sul gruppo di disponibilità `MyAg` per l'utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski`. CONTROL concede all'account di accesso il controllo completo del gruppo di disponibilità, anche se non è il proprietario del gruppo di disponibilità. Per cambiare la proprietà, vedere [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
```sql  
USE master;  
GRANT CONTROL ON AVAILABILITY GROUP::MyAg TO PKomosinski;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [REVOKE - autorizzazioni del gruppo di disponibilità &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)   
 [DENY - autorizzazioni del gruppo di disponibilità &#40;Transact-SQL&#41;](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Viste del catalogo dei gruppi di disponibilità Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md) [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

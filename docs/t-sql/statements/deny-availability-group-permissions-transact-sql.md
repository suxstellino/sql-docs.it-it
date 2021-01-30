---
title: DENY - Autorizzazioni per un gruppo di disponibilità
description: Negare le autorizzazioni per un gruppo di disponibilità Always On.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], permissions
- permissions [SQL Server], availability group
- DENY statement, availability groups
- denying permissions, [SQL Server], availability groups
ms.assetid: bda60b36-a0b9-4c20-80c1-6a5cb1d638a5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: aeb7315bead31911bdd9f62b9ef86a8ee1de9c97
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177787"
---
# <a name="deny-availability-group-permissions-transact-sql"></a>DENY, autorizzazioni del gruppo di disponibilità (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Nega le autorizzazioni per un gruppo di disponibilità AlwaysOn in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DENY permission  [ ,...n ] ON AVAILABILITY GROUP :: availability_group_name  
        TO < server_principal >  [ ,...n ]  
    [ CASCADE ]  
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
 Specifica un'autorizzazione che può essere negata su un gruppo di disponibilità. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 ON AVAILABILITY GROUP **::** _availability_group_name_  
 Specifica il gruppo di disponibilità per cui viene negata l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
 TO \<server_principal>  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui viene negata l'autorizzazione.  
  
 *SQL_Server_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creato da un account di accesso di Windows.  
  
 *SQL_Server_login_from_certificate*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a un certificato.  
  
 *SQL_Server_login_from_AsymKey*  
 Specifica il nome di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a una chiave asimmetrica.  
  
 CASCADE  
 Indica che l'autorizzazione negata viene negata anche ad altre entità alle quali è stata concessa da questa entità.  
  
 AS *SQL_Server_login*  
 Specifica l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile negare autorizzazioni nell'ambito del server solo se il database corrente è il database **master**.  
  
 Le informazioni sui gruppi di disponibilità sono visibili nella vista del catalogo [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md). Le informazioni sulle autorizzazioni del server sono visibili nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) e le informazioni sulle entità server sono visibili nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Un gruppo di disponibilità è un'entità a protezione diretta a livello server. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile negare su un gruppo di disponibilità, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del gruppo di disponibilità|Autorizzazione del gruppo di disponibilità in cui è inclusa|Autorizzazione del server in cui è inclusa|  
|-----------------------------------|----------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY AVAILABILITY GROUP|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il gruppo di disponibilità o l'autorizzazione ALTER ANY AVAILABILITY GROUP per il server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-denying-view-definition-permission-on-an-availability-group"></a>R. Negazione dell'autorizzazione VIEW DEFINITION per un gruppo di disponibilità  
 Nell'esempio seguente viene negata l'autorizzazione `VIEW DEFINITION` sul gruppo di disponibilità `MyAg` per l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`ZArifin`.  
  
```sql  
USE master;  
DENY VIEW DEFINITION ON AVAILABILITY GROUP::MyAg TO ZArifin;  
GO  
```  
  
### <a name="b-denying-take-ownership-permission-with-the-cascade-option"></a>B. Negazione dell'autorizzazione TAKE OWNERSHIP con l'opzione CASCADE  
 Nell'esempio seguente viene negata l'autorizzazione `TAKE OWNERSHIP` per il gruppo di disponibilità `MyAg` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski` con l'opzione `CASCADE`.  
  
```sql  
USE master;  
DENY TAKE OWNERSHIP ON AVAILABILITY GROUP::MyAg TO PKomosinski   
    CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [REVOKE - autorizzazioni del gruppo di disponibilità &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)   
 [GRANT - autorizzazioni del gruppo di disponibilità &#40;Transact-SQL&#41;](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Viste del catalogo dei gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

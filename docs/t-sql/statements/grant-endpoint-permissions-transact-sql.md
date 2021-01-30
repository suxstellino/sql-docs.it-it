---
description: GRANT - autorizzazioni per endpoint (Transact-SQL)
title: GRANT - autorizzazioni per endpoint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- endpoints [SQL Server], permissions
- granting permissions [SQL Server], endpoints
- GRANT statement, endpoints
- permissions [SQL Server], endpoints
ms.assetid: 9eda885c-fc3a-4c9d-8de6-ce07fb35a934
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7c2a366e55f27475b2bda9e0189ed079f8b796d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205775"
---
# <a name="grant-endpoint-permissions-transact-sql"></a>GRANT - autorizzazioni per endpoint (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede le autorizzazioni per un endpoint.  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GRANT permission  [ ,...n ] ON ENDPOINT :: endpoint_name  
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
 Specifica un'autorizzazione che può essere concessa per un endpoint. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 ON ENDPOINT **::** _endpoint_name_  
 Specifica l'endpoint per cui viene concessa l'autorizzazione. Il qualificatore di ambito ( **::** ) è obbligatorio.  
  
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
  
 Le informazioni sugli endpoint sono visibili nella vista del catalogo [sys.endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md). Le informazioni sulle autorizzazioni del server sono visibili nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) e le informazioni sulle entità server sono visibili nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Un endpoint è un'entità a protezione diretta a livello del server. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un endpoint, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione dell'endpoint|Autorizzazione dell'endpoint in cui è inclusa|Autorizzazione del server in cui è inclusa|  
|-------------------------|------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY ENDPOINT|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per l'endpoint o l'autorizzazione ALTER ANY ENDPOINT per il server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-granting-view-definition-permission-on-an-endpoint"></a>R. Concessione dell'autorizzazione VIEW DEFINITION per un endpoint  
 Nell'esempio seguente viene concessa l'autorizzazione `VIEW DEFINITION` per l'endpoint `Mirror7` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`ZArifin`.  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON ENDPOINT::Mirror7 TO ZArifin;  
GO  
```  
  
### <a name="b-granting-take-ownership-permission-with-the-grant-option"></a>B. Concessione dell'autorizzazione TAKE OWNERSHIP con GRANT OPTION  
 Nell'esempio seguente viene concessa l'autorizzazione `TAKE OWNERSHIP` per l'endpoint `Shipping83` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski` con `GRANT OPTION`.  
  
```sql  
USE master;  
GRANT TAKE OWNERSHIP ON ENDPOINT::Shipping83 TO PKomosinski   
    WITH GRANT OPTION;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DENY - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)   
 [REVOKE - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [Viste del catalogo degli endpoint &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [sys.endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

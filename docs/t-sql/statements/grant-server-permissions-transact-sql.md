---
description: GRANT - autorizzazioni per server (Transact-SQL)
title: GRANT - autorizzazioni per server (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- GRANT statement, servers
- permissions [SQL Server], servers
- servers [SQL Server], permissions
- granting permissions [SQL Server], servers
ms.assetid: 7e880a5a-3bdc-491f-a167-7a9ed338be7f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4101c95ccb33a42310bfed6498239c25d3433379
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813016"
---
# <a name="grant-server-permissions-transact-sql"></a>GRANT - autorizzazioni per server (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede le autorizzazioni per un server. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
GRANT permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ] [ WITH GRANT OPTION ]  
    [ AS <grantor_principal> ]  
  
<grantee_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
  
<grantor_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere concessa per un server. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 TO \<grantee_principal> Specifica l'entità a cui viene concessa l'autorizzazione.  
  
 AS \<grantor_principal>Specifica l'entità dalla quale l'entità che esegue la query ottiene il diritto di concedere l'autorizzazione.  
  
 WITH GRANT OPTION  
 Indica che l'entità potrà inoltre concedere l'autorizzazione specificata ad altre entità.  
  
 *SQL_Server_login*  
 Specifica un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_mapped_to_Windows_login*  
 Specifica un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a un utente di Windows.  
  
 *SQL_Server_login_mapped_to_Windows_group*  
 Specifica un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping a un gruppo di Windows.  
  
 *SQL_Server_login_mapped_to_certificate*  
 Specifica un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di cui è stato eseguito il mapping a un certificato.  
  
 *SQL_Server_login_mapped_to_asymmetric_key*  
 Specifica un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di cui è stato eseguito il mapping a una chiave asimmetrica.  
  
 *server_role*  
 Specifica un ruolo del server definito dall'utente.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile concedere autorizzazioni nell'ambito del server solo se il database corrente è il database master.  
  
 Le informazioni sulle autorizzazioni del server sono visibili nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) e le informazioni sulle entità server nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Le informazioni sulle appartenenze dei ruoli del server sono visibili nella vista del catalogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Un server rappresenta il livello più alto nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un server.  
  
|Autorizzazione del server|Autorizzazione del server in cui è inclusa|  
|-----------------------|----------------------------------|  
|ADMINISTER BULK OPERATIONS|CONTROL SERVER|  
|ALTER ANY AVAILABILITY GROUP<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|ALTER ANY CONNECTION|CONTROL SERVER|  
|ALTER ANY CREDENTIAL|CONTROL SERVER|  
|ALTER ANY DATABASE|CONTROL SERVER|  
|ALTER ANY ENDPOINT|CONTROL SERVER|  
|ALTER ANY EVENT NOTIFICATION|CONTROL SERVER|  
|ALTER ANY EVENT SESSION|CONTROL SERVER|  
|ALTER ANY LINKED SERVER|CONTROL SERVER|  
|ALTER ANY LOGIN|CONTROL SERVER|  
|ALTER ANY SERVER AUDIT|CONTROL SERVER|  
|ALTER ANY SERVER ROLE<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|ALTER RESOURCES|CONTROL SERVER|  
|ALTER SERVER STATE|CONTROL SERVER|  
|ALTER SETTINGS|CONTROL SERVER|  
|ALTER TRACE|CONTROL SERVER|  
|AUTHENTICATE SERVER|CONTROL SERVER|  
|CONNECT ANY DATABASE<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|CONNECT SQL|CONTROL SERVER|  
|CONTROL SERVER|CONTROL SERVER|  
|CREATE ANY DATABASE|ALTER ANY DATABASE|  
|Creare un gruppo di disponibilità<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|ALTER ANY AVAILABILITY GROUP|  
|CREATE DDL EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|CREATE ENDPOINT|ALTER ANY ENDPOINT|  
|CREATE SERVER ROLE<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|ALTER ANY SERVER ROLE|  
|CREATE TRACE EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|EXTERNAL ACCESS ASSEMBLY|CONTROL SERVER|  
|IMPERSONATE ANY LOGIN<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|SELECT ALL USER SECURABLES<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|SHUTDOWN|CONTROL SERVER|  
|UNSAFE ASSEMBLY|CONTROL SERVER|  
|VIEW ANY DATABASE|VIEW ANY DEFINITION|  
|VIEW ANY DEFINITION|CONTROL SERVER|  
|VIEW SERVER STATE|ALTER SERVER STATE|  
  
 In [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] sono state aggiunte le tre autorizzazioni server riportate di seguito.  
  
 Autorizzazione **CONNECT ANY DATABASE**  
 Concedere l'autorizzazione **CONNECT ANY DATABASE** a un account di accesso che deve connettersi a tutti i database attualmente esistenti e ai nuovi database che potrebbero essere creati in futuro. Non concede alcuna autorizzazione nei database oltre la connessione. Combinare con **SELECT ALL USER SECURABLES** o **VIEW SERVER STATE** per consentire a un processo di controllo di visualizzare tutti i dati o tutti gli stati del database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Autorizzazione **IMPERSONATE ANY LOGIN**  
 Quando viene concessa, consente a un processo di livello intermedio di rappresentare l'account dei client a cui ci si connette, quando si connette ai database. Quando viene negata, è possibile che a un account di accesso con privilegi elevati venga impedito di rappresentare altri account di accesso. Ad esempio, è possibile che a un account di accesso con autorizzazione **CONTROL SERVER** venga impedito di rappresentare altri account di accesso.  
  
 Autorizzazione **SELECT ALL USER SECURABLES**  
 Quando viene concessa, un account di accesso, ad esempio un revisore, può visualizzare i dati in tutti i database a cui l'utente può connettersi. Quando negata, impedisce l'accesso agli oggetti a meno che non siano nello schema **sys**.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa. I membri del ruolo predefinito del server sysadmin possono concedere qualsiasi autorizzazione.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-granting-a-permission-to-a-login"></a>R. Concessione di un'autorizzazione a un account di accesso  
 Nell'esempio seguente viene concessa l'autorizzazione `CONTROL SERVER` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`TerryEminhizer`.  
  
```sql  
USE master;  
GRANT CONTROL SERVER TO TerryEminhizer;  
GO  
```  
  
### <a name="b-granting-a-permission-that-has-grant-permission"></a>B. Concessione di un'autorizzazione che include l'autorizzazione GRANT  
 Nell'esempio seguente viene concessa l'autorizzazione `ALTER ANY EVENT NOTIFICATION` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`JanethEsteves` con il diritto di concedere tale autorizzazione a un altro account di accesso.  
  
```sql  
USE master;  
GRANT ALTER ANY EVENT NOTIFICATION TO JanethEsteves WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-a-permission-to-a-server-role"></a>C. Concessione di un'autorizzazione a un ruolo del server  
 Nell'esempio seguente vengono creati due ruoli del server denominati `ITDevAdmin` e `ITDevelopers`. Concede l'autorizzazione `ALTER ANY DATABASE` al ruolo del server definito dall'utente `ITDevAdmin` inclusa l'opzione `WITH GRANT` in modo che il ruolo del server `ITDevAdmin` possa riassegnare l'autorizzazione `ALTER ANY DATABASE`. Nell'esempio viene quindi concessa a `ITDevelopers` l'autorizzazione per l'utilizzo dell'autorizzazione `ALTER ANY DATABASE` del ruolo del server `ITDevAdmin`.  
  
```sql  
USE master;  
CREATE SERVER ROLE ITDevAdmin ;  
CREATE SERVER ROLE ITDevelopers ;  
GRANT ALTER ANY DATABASE TO ITDevAdmin WITH GRANT OPTION ;  
GRANT ALTER ANY DATABASE TO ITDevelopers AS ITDevAdmin ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY - Autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [Gerarchia delle autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  

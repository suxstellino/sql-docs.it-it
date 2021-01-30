---
description: REVOKE - autorizzazioni per server (Transact-SQL)
title: REVOKE - autorizzazioni per server (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], servers
- REVOKE statement, server permissions
- servers [SQL Server], permissions
ms.assetid: 7b9a56b3-face-452e-a655-147dac306ba1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d0302007b004d1208104928aa5ce3b87243e4c2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210941"
---
# <a name="revoke-server-permissions-transact-sql"></a>REVOKE - autorizzazioni per server (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove le autorizzazioni GRANT e DENY a livello del server.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission  [ ,...n ]   
    { TO | FROM } <grantee_principal> [ ,...n ]  
        [ CASCADE ]  
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
  
 { TO | FROM } \<grantee_principal>Specifica l'entità da cui viene revocata l'autorizzazione.  
  
 AS \<grantor_principal> Specifica l'entità dalla quale l'entità che esegue la query ottiene il diritto di revocare l'autorizzazione.  
  
 GRANT OPTION FOR  
 Indica che verrà revocato il diritto di concedere l'autorizzazione specificata ad altre entità. L'autorizzazione stessa non verrà revocata.  
  
> [!IMPORTANT]  
>  Se l'autorizzazione specificata è stata concessa all'entità senza l'opzione GRANT, l'autorizzazione stessa verrà revocata.  
  
 CASCADE  
 Indica che l'autorizzazione che viene revocata anche ad altre entità a cui è stata concessa o negata da questa entità.  
  
> [!CAUTION]  
>  La revoca propagata di un'autorizzazione concessa con WITH GRANT OPTION comporterà la revoca sia delle autorizzazioni GRANT che delle autorizzazioni DENY per tale autorizzazione.  
  
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
 È possibile revocare autorizzazioni nell'ambito del server solo se il database corrente è il database master.  
  
 Con REVOKE vengono rimosse sia le autorizzazioni GRANT che le autorizzazioni DENY.  
  
 Utilizzare REVOKE GRANT OPTION FOR per revocare il diritto di riconcedere l'autorizzazione specificata. Se l'entità dispone dell'autorizzazione con il diritto di concessione, verrà revocato il diritto di concedere tale autorizzazione, ma l'autorizzazione stessa non verrà revocata. Se invece l'entità dispone dell'autorizzazione specificata senza opzione GRANT, verrà revocata l'autorizzazione stessa.  
  
 Le informazioni sulle autorizzazioni del server sono visibili nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) e le informazioni sulle entità server nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Le informazioni sulle appartenenze dei ruoli del server sono visibili nella vista del catalogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Un server rappresenta il livello più alto nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile revocare per un server.  
  
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
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL SERVER o l'appartenenza al ruolo predefinito del server sysadmin.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-revoking-a-permission-from-a-login"></a>R. Revoca di un'autorizzazione a un account di accesso  
 Nell'esempio seguente viene revocata l'autorizzazione `VIEW SERVER STATE` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`WanidaBenshoof`.  
  
```sql  
USE master;  
REVOKE VIEW SERVER STATE FROM WanidaBenshoof;  
GO  
```  
  
### <a name="b-revoking-the-with-grant-option"></a>B. Revoca dell'opzione WITH GRANT  
 Nell'esempio seguente viene revocato il diritto di concedere l'autorizzazione `CONNECT SQL` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`JanethEsteves`.  
  
```sql  
USE master;  
REVOKE GRANT OPTION FOR CONNECT SQL FROM JanethEsteves;  
GO  
```  
  
 L'account di accesso dispone ancora dell'autorizzazione CONNECT SQL, ma non potrà più concedere tale autorizzazione ad altre entità.  
  
## <a name="see-also"></a>Vedere anche  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY - Autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE - autorizzazioni per server (Transact-SQL)](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [Gerarchia delle autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  

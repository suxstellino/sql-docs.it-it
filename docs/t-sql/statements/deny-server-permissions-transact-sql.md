---
description: DENY - autorizzazioni per server (Transact-SQL)
title: DENY - autorizzazioni per server (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], servers
- denying permissions [SQL Server], servers
- servers [SQL Server], permissions
- DENY statement, servers
ms.assetid: 68d6b2a9-c36f-465a-9cd2-01d43a667e99
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 290b7e30d198f18fc1d07049b80a0cea78673724
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688903"
---
# <a name="deny-server-permissions-transact-sql"></a>DENY - autorizzazioni per server (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Nega le autorizzazioni per un server.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DENY permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ]  
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
 Specifica un'autorizzazione che può essere negata per un server. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 CASCADE  
 Indica che l'autorizzazione viene negata all'entità specificata e a tutte le entità alle quali l'entità ha concesso l'autorizzazione. Obbligatorio quando l'entità dispone dell'autorizzazione con GRANT OPTION. 
  
 TO \<server_principal>  
 Specifica l'entità a cui viene negata l'autorizzazione.  
  
 AS \<grantor_principal>  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione.
Usare la clausola AS principal per indicare che l'entità registrata come l'utente che nega l'autorizzazione deve essere un'entità diversa dalla persona che esegue l'istruzione. Si supponga ad esempio che l'utente Mary sia principal_id 12 e l'utente Raul sia principal 15. Mary esegue `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. Ora la tabella sys.database_permissions indicherà l'utente 15 (Raul) come grantor_principal_id dell'istruzione di negazione anche se l'istruzione è stata effettivamente eseguita dall'utente 13 (Mary).
  
L'uso di AS in questa istruzione non implica la possibilità di rappresentare un altro utente.    
  
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
 Specifica un ruolo del server.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile negare autorizzazioni nell'ambito del server solo se il database corrente è il database master.  
  
 Le informazioni sulle autorizzazioni del server sono visibili nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) e le informazioni sulle entità server nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Le informazioni sulle appartenenze dei ruoli del server sono visibili nella vista del catalogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Un server rappresenta il livello più alto nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile negare per un server.  
  
|Autorizzazione del server|Autorizzazione del server in cui è inclusa|  
|-----------------------|----------------------------------|  
|ADMINISTER BULK OPERATIONS|CONTROL SERVER|  
|ALTER ANY AVAILABILITY GROUP<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|ALTER ANY CONNECTION|CONTROL SERVER|  
|ALTER ANY CREDENTIAL|CONTROL SERVER|  
|ALTER ANY DATABASE|CONTROL SERVER|  
|ALTER ANY ENDPOINT|CONTROL SERVER|  
|ALTER ANY EVENT NOTIFICATION|CONTROL SERVER|  
|ALTER ANY EVENT SESSION|CONTROL SERVER|  
|ALTER ANY LINKED SERVER|CONTROL SERVER|  
|ALTER ANY LOGIN|CONTROL SERVER|  
|ALTER ANY SERVER AUDIT|CONTROL SERVER|  
|ALTER ANY SERVER ROLE<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|ALTER RESOURCES|CONTROL SERVER|  
|ALTER SERVER STATE|CONTROL SERVER|  
|ALTER SETTINGS|CONTROL SERVER|  
|ALTER TRACE|CONTROL SERVER|  
|AUTHENTICATE SERVER|CONTROL SERVER|  
|CONNECT ANY DATABASE<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|CONNECT SQL|CONTROL SERVER|  
|CONTROL SERVER|CONTROL SERVER|  
|CREATE ANY DATABASE|ALTER ANY DATABASE|  
|Creare un gruppo di disponibilità<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).|ALTER ANY AVAILABILITY GROUP|  
|CREATE DDL EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|CREATE ENDPOINT|ALTER ANY ENDPOINT|  
|CREATE SERVER ROLE<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).|ALTER ANY SERVER ROLE|  
|CREATE TRACE EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|EXTERNAL ACCESS ASSEMBLY|CONTROL SERVER|  
|IMPERSONATE ANY LOGIN<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|SELECT ALL USER SECURABLES<br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
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
 È richiesta l'autorizzazione CONTROL SERVER o la proprietà dell'entità a sicurezza diretta. Se si utilizza la clausola AS, l'entità specificata deve essere proprietaria dell'entità a sicurezza diretta per cui vengono negate le autorizzazioni.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-denying-connect-sql-permission-to-a-sql-server-login-and-principals-to-which-the-login-has-regranted-it"></a>R. Negazione dell'autorizzazione CONNECT SQL a un account di accesso di SQL Server e alle entità a cui l'account di accesso ha concesso tale autorizzazione  
 Nell'esempio seguente viene negata l'autorizzazione `CONNECT SQL` all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Annika` e a tutte le entità a cui tale account di accesso ha concesso l'autorizzazione.  
  
```sql  
USE master;  
DENY CONNECT SQL TO Annika CASCADE;  
GO  
```  
  
### <a name="b-denying-create-endpoint-permission-to-a-sql-server-login-using-the-as-option"></a>B. Negazione dell'autorizzazione CREATE ENDPOINT a un account di accesso di SQL Server con l'opzione AS  
 Nell'esempio seguente viene negata l'autorizzazione `CREATE ENDPOINT` all'utente `ArifS`. Nell'esempio viene utilizzata l'opzione `AS` per specificare `MandarP` come entità da cui l'entità che esegue l'istruzione deriva il diritto di effettuare l'operazione.  
  
```sql  
USE master;  
DENY CREATE ENDPOINT TO ArifS AS MandarP;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY - autorizzazioni per server (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [Gerarchia delle autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  

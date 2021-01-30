---
description: CREATE USER (Transact-SQL)
title: CREATE USER (Transact-SQL)
ms.custom: ''
ms.date: 11/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITHOUT_LOGIN_TSQL
- CREATE_USER_TSQL
- SQL13.SWB.DATABASEUSER.OWNEDSCHEMAS.F1
- WITHOUT LOGIN
- CREATE USER
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- adding users
- WITHOUT LOGIN [SQL Server]
- CREATE USER statement
- database user additions [SQL Server]
- USER WITHOUT LOGIN [SQL Server]
- users [SQL Server], adding
- users [SQL Server]
ms.assetid: 01de7476-4b25-4d58-85b7-1118fe64aa80
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a1f9092cad18bb2df5fe024403dc9e80401de71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177892"
---
# <a name="create-user-transact-sql"></a>CREATE USER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Aggiunge un utente al database corrente. Di seguito sono elencati i 12 tipi di utenti con un esempio della sintassi di base:  
  
**Utenti basati su account di accesso nel database master**: tipo di utente più comune.  
  
-   Utente basato su un account di accesso basato su un account Active Directory. `CREATE USER [Contoso\Fritz];`     
-   Utente basato su un account di accesso basato su un gruppo di Windows. `CREATE USER [Contoso\Sales];`   
-   Utente basato su un account di accesso che usa l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `CREATE USER Mary;`  
  
**Utenti che effettuano l'autenticazione a livello di database**: consigliato per rendere più portatile il database.  
 Sempre consentito in [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Consentito solo in un database indipendente in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].  
  
-   Utente basato su un utente di Windows che non dispone di account di accesso. `CREATE USER [Contoso\Fritz];`    
-   Utente basato su un gruppo di Windows che non dispone di account di accesso. `CREATE USER [Contoso\Sales];`  
-   Utente [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] basato su un utente Azure Active Directory. `CREATE USER [Fritz@contoso.com] FROM EXTERNAL PROVIDER;`     

-   Utente del database indipendente con password. Non è disponibile in [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]. `CREATE USER Mary WITH PASSWORD = '********';`   
  
**Utenti basati sulle entità di sicurezza di Windows che si connettono tramite account di accesso del gruppo di Windows**  
  
-   Utente basato su un utente di Windows che non dispone di account di accesso, ma che può connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] tramite l'appartenenza a un gruppo di Windows. `CREATE USER [Contoso\Fritz];`  
  
-   Utente basato su un gruppo di Windows che non dispone di account di accesso, ma che può connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] tramite l'appartenenza a un gruppo di Windows diverso. `CREATE USER [Contoso\Fritz];`  
  
**Utenti che non possono eseguire l'autenticazione**: questi utenti non possono accedere a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
-   Utente che non dispone di account di accesso. Non può effettuare l'accesso ma è possibile concedergli autorizzazioni. `CREATE USER CustomApp WITHOUT LOGIN;`    
-   Utente basato su un certificato. Non può effettuare l'accesso, ma può firmare moduli ed è possibile concedergli autorizzazioni. `CREATE USER TestProcess FOR CERTIFICATE CarnationProduction50;`  
-   Utente basato su una chiave asimmetrica. Non può effettuare l'accesso, ma può firmare moduli ed è possibile concedergli autorizzazioni. `CREATE User TestProcess FROM ASYMMETRIC KEY PacificSales09;`   
 
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure SQL Managed Instance
  
-- Syntax Users based on logins in master  
CREATE USER user_name   
    [   
        { FOR | FROM } LOGIN login_name   
    ]  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that authenticate at the database  
CREATE USER   
    {  
      windows_principal [ WITH <options_list> [ ,... ] ]  
  
    | user_name WITH PASSWORD = 'password' [ , <options_list> [ ,... ]   
    | Azure_Active_Directory_principal FROM EXTERNAL PROVIDER   
    }  
  
 [ ; ]  
  
-- Users based on Windows principals that connect through Windows group logins  
CREATE USER   
    {   
          windows_principal [ { FOR | FROM } LOGIN windows_principal ]  
        | user_name { FOR | FROM } LOGIN windows_principal  
}  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that cannot authenticate   
CREATE USER user_name   
    {  
         WITHOUT LOGIN [ WITH <limited_options_list> [ ,... ] ]  
       | { FOR | FROM } CERTIFICATE cert_name   
       | { FOR | FROM } ASYMMETRIC KEY asym_key_name   
    }  
 [ ; ]  
  
<options_list> ::=  
      DEFAULT_SCHEMA = schema_name  
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }  
    | SID = sid   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name ]   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
-- SQL Database syntax when connected to a federation member  
CREATE USER user_name  
[;]

-- Syntax for users based on Azure AD logins for Azure SQL Managed Instance
CREATE USER user_name   
    [   { FOR | FROM } LOGIN login_name  ]  
    | FROM EXTERNAL PROVIDER
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  

<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name 
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ] 
```

> [!NOTE]
> L'amministratore di Azure AD per la funzionalità Istanza gestita di SQL di Azure dopo la creazione è stato modificato. Per altre informazioni, vedere [Nuove funzionalità di amministrazione di Azure AD per l'istanza gestita](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

```syntaxsql
-- Syntax for Azure Synapse Analytics  
  
CREATE USER user_name   
    [ { { FOR | FROM } { LOGIN login_name }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]

CREATE USER Azure_Active_Directory_principal FROM EXTERNAL PROVIDER  
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]
``` 
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CREATE USER user_name   
    [ { { FOR | FROM }  
      {   
        LOGIN login_name   
      }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *user_name*  
 Specifica il nome con cui viene identificato l'utente all'interno del database. *user_name* è un **sysname**. Non può superare la lunghezza di 128 caratteri. Quando si crea un utente basato su un'entità di Windows, il nome dell'entità di Windows diventa il nome utente, a meno che non ne sia stato specificato un altro.  
  
 LOGIN *login_name*  
 Specifica l'account di accesso per cui viene creato l'utente del database. *login_name* deve essere un account di accesso valido nel server. Può essere un account di accesso basato su un'entità di Windows (utente o gruppo) o un account di accesso che usa l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando si accede al database tramite questo account di accesso di SQL Server, si acquisisce il nome e l'ID dell'utente del database che si sta creando. Quando si crea un account di accesso con mapping da un'entità di Windows, usare il formato **[** _\<domainName\>_ **\\** _\<loginName\>_ **]** . Per alcuni esempi, vedere [Riepilogo della sintassi](#SyntaxSummary).  
  
 Se l'istruzione CREATE USER è l'unica istruzione in un batch SQL, il database SQL di Azure supporta la clausola WITH LOGIN. Se l'istruzione CREATE USER non è l'unica istruzione in un batch SQL o viene eseguita in SQL dinamico, la clausola WITH LOGIN non è supportata.  
  
 WITH DEFAULT_SCHEMA = *schema_name*  
 Specifica il primo schema in cui verrà eseguita una ricerca nel server durante la risoluzione dei nomi di oggetti per l'utente del database.  
  
 '*windows_principal*'  
 Specifica l'entità di Windows per la quale viene creato l'utente del database. The *windows_principal* può essere un utente o un gruppo di Windows. L'utente verrà creato anche se *windows_principal* non ha un account di accesso. Quando si effettua la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se *windows_principal* non ha un account di accesso, l'entità di sicurezza di Windows deve eseguire l'autenticazione a livello del [!INCLUDE[ssDE](../../includes/ssde-md.md)] tramite l'appartenenza a un gruppo di Windows che abbia un account di accesso. In alternativa, la stringa di connessione deve specificare il database indipendente come catalogo iniziale. Quando si crea un utente da un'entità di Windows, usare il formato **[** _\<domainName\>_ **\\** _\<loginName\>_ **]** . Per alcuni esempi, vedere [Riepilogo della sintassi](#SyntaxSummary). I nomi degli utenti basati su utenti di Active Directory devono avere un numero di caratteri inferiore a 21.
  
 '*Azure_Active_Directory_principal*'  
 **Si applica a**: [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].  
  
 Specifica l'entità di sicurezza di Azure Active Directory per la quale viene creato l'utente del database. *Azure_Active_Directory_principal* può essere un utente, un gruppo o un'applicazione di Azure Active Directory. Gli utenti di Azure Active Directory non possono avere un account di accesso di autenticazione di Windows in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Possono averlo solo gli utenti del database. La stringa di connessione deve specificare il database indipendente come catalogo iniziale.

 Per le entità di sicurezza di Azure AD, la sintassi CREATE USER richiede:

- UserPrincipalName dell'oggetto di Azure AD per utenti di Azure AD.

  - `CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;`  
  - `CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;`

- DisplayName dell'oggetto di Azure AD per i gruppi di Azure AD e le applicazioni di Azure AD. Nel caso del gruppo di sicurezza *Nurses* si userebbe:  
  
  - `CREATE USER [Nurses] FROM EXTERNAL PROVIDER;`  
  
 Per altre informazioni, vedere [Connessione al database SQL tramite l'autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).  
  
WITH PASSWORD = '*password*'  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Può essere usato solo in un database indipendente. Specifica la password per l'utente che viene creato. A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le informazioni relative alle password archiviate vengono calcolate tramite SHA-512 della password salt.  
  
WITHOUT LOGIN  
 Specifica che sull'utente non deve essere eseguito il mapping a un account di accesso esistente.  
  
CERTIFICATE *cert_name*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Specifica il certificato per cui viene creato l'utente del database.  
  
ASYMMETRIC KEY *asym_key_name*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Specifica la chiave asimmetrica per cui viene creato l'utente del database.  
  
DEFAULT_LANGUAGE = *{ NONE \| \<lcid> \| \<language name> \| \<language salias> }*  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Specifica la lingua predefinita per il nuovo utente. Se per l'utente viene specificata una lingua predefinita e la lingua predefinita del database viene modificata in seguito, la lingua predefinita dell'utente rimane quella specificata. Se non viene specificata alcuna lingua predefinita, la lingua predefinita dell'utente sarà la lingua predefinita del database. Se per l'utente non viene specificata la lingua predefinita e la lingua predefinita del database viene modificata in seguito, la lingua predefinita dell'utente sarà la nuova lingua predefinita del database.  
  
> [!IMPORTANT]  
>  *DEFAULT_LANGUAGE* viene usato unicamente per gli utenti di database indipendenti.  
  
SID = *sid*  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Si applica solo agli utenti con password (autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) in un database indipendente. Specifica il SID del nuovo utente del database. Se questa opzione è inclusa, tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene assegnato un SID automaticamente. Usare il parametro SID per creare utenti in più database con la stessa identità (SID). Ciò risulta utile per la creazione di utenti in più database per preparare il failover Always On. Per determinare il SID di un utente, eseguire una query su sys.database_principals.  
  
ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  
 **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Elimina i controlli sui metadati di crittografia nel server nelle operazioni di copia bulk. Ciò consente all'utente di eseguire la copia bulk dei dati crittografati fra tabelle o database senza decrittografare i dati. Il valore predefinito è OFF.  
  
> [!WARNING]  
>  L'uso improprio di questa opzione può causare il danneggiamento dei dati. Per altre informazioni, vedere [Migrare dati sensibili protetti da Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
## <a name="remarks"></a>Osservazioni  
 Se si omette FOR LOGIN, per il nuovo utente del database verrà eseguito il mapping all'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con lo stesso nome.  
  
 Lo schema predefinito sarà il primo schema in cui verrà eseguita la ricerca nel server durante la risoluzione dei nomi di oggetti per l'utente del database. Se non specificato diversamente, lo schema predefinito sarà il proprietario degli oggetti creati dall'utente del database.  
  
 Se l'utente dispone di uno schema predefinito, verrà utilizzato tale schema. Se l'utente non dispone di uno schema predefinito, ma è un membro di un gruppo che dispone di uno schema predefinito, verrà utilizzato lo schema predefinito del gruppo. Se l'utente non dispone di uno schema predefinito ed è un membro di più gruppi, lo schema predefinito per l'utente sarà quello del gruppo di Windows con principal_id inferiore e uno schema predefinito impostato in modo esplicito. Non è possibile selezionare in modo esplicito uno degli schemi predefiniti disponibili come schema preferito. Se non possono essere determinati schemi predefiniti per un utente, viene usato lo schema **dbo**.  
  
 È possibile impostare DEFAULT_SCHEMA prima della creazione dello schema a cui punta.  
  
 Non è possibile specificare DEFAULT_SCHEMA quando si crea un utente con mapping a un certificato o a una chiave asimmetrica.  
  
 Il valore di DEFAULT_SCHEMA viene ignorato se l'utente è un membro del ruolo predefinito del server sysadmin. Tutti i membri del ruolo predefinito del server sysadmin dispongono di uno schema predefinito di `dbo`.  
  
 La clausola WITHOUT LOGIN crea un utente su cui non è stato eseguito il mapping a un account di accesso di SQL Server. Tale utente può connettersi ad altri database come guest. È possibile concedere autorizzazioni a questo utente senza account di accesso. Quando il contesto di sicurezza viene modificato in un utente senza account di accesso, gli utenti originali ricevono le autorizzazioni dell'utente senza account di accesso. Vedere l'esempio [D. Creazione e uso di un utente senza un account di accesso](#withoutLogin).  
  
 Solo gli utenti di cui è stato eseguito il mapping a entità di sicurezza di Windows possono contenere un carattere barra rovesciata ( **\\** ).
  
 Non è possibile utilizzare CREATE USER per creare un utente guest, in quanto l'utente guest è già presente in ogni database. Per abilitare l'utente guest, concedere a tale utente l'autorizzazione CONNECT nel modo seguente:  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
 Le informazioni sugli utenti del database sono visibili nella vista del catalogo [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).

Una nuova estensione della sintassi, **FROM EXTERNAL PROVIDER** è disponibile per la creazione di account di accesso di Azure AD a livello di server in Istanza gestita di SQL di Azure. Gli account di accesso Azure AD consentono di eseguire il mapping delle entità di Azure AD a livello di database sugli account di accesso Azure AD a livello di server. Per creare un utente di Azure AD da un account di accesso di Azure AD, usare la sintassi seguente:

`CREATE USER [AAD_principal] FROM LOGIN [Azure AD login]`

Quando si crea l'utente nel database di Istanza gestita di SQL di Azure, login_name deve corrispondere a un account di accesso di Azure AD esistente, altrimenti, con la clausola **FROM EXTERNAL PROVIDER** si crea unicamente un utente di Azure AD senza account di accesso nel database master. Ad esempio, questo comando crea un utente indipendente:

`CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER`
  
##  <a name="syntax-summary"></a><a name="SyntaxSummary"></a> Riepilogo della sintassi  
 **Utenti basati su account di accesso nel database master**  
  
 Nell'elenco seguente viene illustrata la sintassi possibile per gli utenti basati su account di accesso. Le opzioni dello schema predefinito non sono elencate.  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FOR LOGIN SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FROM LOGIN SQLAUTHLOGIN`  
  
**Utenti che eseguono l'autenticazione a livello del database**  
  
 Nell'elenco seguente viene illustrata la sintassi possibile per gli utenti che possono essere usati solo in un database indipendente. Gli utenti creati non saranno correlati ad alcun account di accesso nel database **master**. Le opzioni della lingua e dello schema predefinito non sono elencate.  
  
> [!IMPORTANT]  
>  Questa sintassi concede agli utenti l'accesso al database e il nuovo accesso al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER Barry WITH PASSWORD = 'sdjklalie8rew8337!$d'`  
  
**Utenti basati su entità di sicurezza di Windows senza account di accesso nel database master**  
  
 L'elenco seguente illustra la sintassi possibile per gli utenti che hanno accesso al [!INCLUDE[ssDE](../../includes/ssde-md.md)] attraverso un gruppo di Windows, ma che non hanno un account di accesso nel database **master**. Questa sintassi può essere usata in tutti i tipi di database. Le opzioni della lingua e dello schema predefinito non sono elencate.  
  
 Questa sintassi è simile agli utenti basati su account di accesso nel database master, tuttavia questa categoria di utente non dispone di un account di accesso nel database master. L'utente deve disporre dell'accesso al [!INCLUDE[ssDE](../../includes/ssde-md.md)] attraverso un account di accesso basato su un gruppo di Windows.  
  
 Questa sintassi è simile a utenti del database indipendente basati su entità di Windows, tuttavia questa categoria di utente non ottiene il nuovo accesso al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
  
**Utenti che non possono eseguire l'autenticazione**  
  
 Nell'elenco seguente viene illustrata la sintassi possibile per gli utenti che non possono effettuare l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   `CREATE USER RIGHTSHOLDER WITHOUT LOGIN`  
-   `CREATE USER CERTUSER FOR CERTIFICATE SpecialCert`  
-   `CREATE USER CERTUSER FROM CERTIFICATE SpecialCert`  
-   `CREATE USER KEYUSER FOR ASYMMETRIC KEY SecureKey`  
-   `CREATE USER KEYUSER FROM ASYMMETRIC KEY SecureKey`  
  
## <a name="security"></a>Sicurezza  
 Quando si crea un utente si concede l'accesso a un database, ma non si concede automaticamente alcun accesso agli oggetti contenuti in un database. Dopo avere creato un utente, le azioni comuni consistono nell'aggiungere utenti a ruoli del database con autorizzazione ad accedere a oggetti del database o nel concedere autorizzazioni all'utente per gli oggetti. Per informazioni sulla progettazione di un sistema di autorizzazioni, vedere [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
### <a name="special-considerations-for-contained-databases"></a>Considerazioni speciali relative ai database indipendenti  
 Quando si effettua la connessione a un database indipendente, se l'utente non ha un account di accesso nel database **master**, la stringa di connessione deve includere il nome del database indipendente come catalogo iniziale. Il parametro del catalogo iniziale è sempre obbligatorio per un utente del database indipendente con password.  
  
 In un database indipendente, la creazione di utenti agevola la separazione del database dall'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. In questo modo, risulterà più facile spostare il database in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Database indipendenti](../../relational-databases/databases/contained-databases.md) e [Utenti di database indipendente: rendere portabile un database](../../relational-databases/security/contained-database-users-making-your-database-portable.md). Per modificare un utente del database basato su un account di accesso dell'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un utente del database indipendente con password, vedere [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).  
  
 In un database indipendente non è necessario che gli utenti abbiano un account di accesso nel database **master**. Gli amministratori del [!INCLUDE[ssDE](../../includes/ssde-md.md)] devono essere consapevoli che l'accesso a un database indipendente può essere concesso al livello del database, anziché al livello del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per altre informazioni, vedere [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
 Quando si usano utenti di database indipendenti in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], configurare l'accesso usando una regola del firewall a livello di database, invece di una regola del firewall a livello di server. Per altre informazioni, vedere[sp_set_database_firewall_rule &#40;Database di SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md).
 
Per gli utenti di database indipendenti [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] e [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], SQL Server Management Studio supporta Multi-Factor Authentication. Per altre informazioni, vedere [Supporto di SQL Server Management Studio (SSMS) per l'autenticazione MFA di Azure AD con database SQL e [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/).  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY USER per il database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-database-user-based-on-a-sql-server-login"></a>R. Creazione di un utente del database basato su un account di accesso di SQL server  
 Nell'esempio seguente viene innanzitutto creato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominato `AbolrousHazem`, quindi viene creato un utente del database corrispondente `AbolrousHazem` in `AdventureWorks2012`.  
  
```sql  
CREATE LOGIN AbolrousHazem   
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
```   
Passare a un database utente. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio, usare l'istruzione `USE AdventureWorks2012`. In [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], è necessario creare una nuova connessione al database utente.

```sql   
CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
GO   
```  
  
### <a name="b-creating-a-database-user-with-a-default-schema"></a>B. Creazione di un utente del database con uno schema predefinito  
 Nell'esempio seguente viene innanzitutto creato un account di accesso al server denominato `WanidaBenshoof` con una password, quindi viene creato un corrispondente utente del database `Wanida` con lo schema predefinito `Marketing`.  
  
```sql  
CREATE LOGIN WanidaBenshoof   
    WITH PASSWORD = '8fdKJl3$nlNv3049jsKK';  
USE AdventureWorks2012;  
CREATE USER Wanida FOR LOGIN WanidaBenshoof   
    WITH DEFAULT_SCHEMA = Marketing;  
GO  
```  
  
### <a name="c-creating-a-database-user-from-a-certificate"></a>C. Creazione di un utente del database da un certificato  
 Nell'esempio seguente viene creato un utente del database `JinghaoLiu` dal certificato `CarnationProduction50`.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql  
USE AdventureWorks2012;  
CREATE CERTIFICATE CarnationProduction50  
    WITH SUBJECT = 'Carnation Production Facility Supervisors',  
    EXPIRY_DATE = '11/11/2011';  
GO  
CREATE USER JinghaoLiu FOR CERTIFICATE CarnationProduction50;  
GO   
```  
  
###  <a name="d-creating-and-using-a-user-without-a-login"></a><a name="withoutLogin"></a> D. Creazione e uso di un utente senza un account di accesso  
 Nell'esempio seguente viene creato un utente del database `CustomApp` che non esegue il mapping a un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nell'esempio viene concessa l'autorizzazione`adventure-works\tengiz0` di un utente per rappresentare l'utente `CustomApp`.  
  
```sql  
USE AdventureWorks2012 ;  
CREATE USER CustomApp WITHOUT LOGIN ;  
GRANT IMPERSONATE ON USER::CustomApp TO [adventure-works\tengiz0] ;  
GO   
```  
  
 Per usare le credenziali di `CustomApp`, l'utente `adventure-works\tengiz0` esegue la seguente istruzione.  
  
```sql  
EXECUTE AS USER = 'CustomApp' ;  
GO  
```  
  
 Per ripristinare le credenziali `adventure-works\tengiz0`, l'utente esegue la seguente istruzione.  
  
```sql  
REVERT ;  
GO  
```  
  
### <a name="e-creating-a-contained-database-user-with-password"></a>E. Creazione di un utente del database indipendente con password  
 Nell'esempio seguente viene creato un utente del database indipendente con password. Questo esempio può essere eseguito solo in un database indipendente.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. Questo esempio funziona correttamente in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] se si rimuove DEFAULT_LANGUAGE.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER Carlo  
WITH PASSWORD='RN92piTCh%$!~3K9844 Bl*'  
    , DEFAULT_LANGUAGE=[Brazilian]  
    , DEFAULT_SCHEMA=[dbo]  
GO   
```  
  
### <a name="f-creating-a-contained-database-user-for-a-domain-login"></a>F. Creazione di un utente del database indipendente per un account di accesso al dominio  
 Nell'esempio seguente viene creato un utente del database indipendente per un account di accesso denominato Fritz in un dominio denominato Contoso. Questo esempio può essere eseguito solo in un database indipendente.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER [Contoso\Fritz] ;  
GO   
```  
  
### <a name="g-creating-a-contained-database-user-with-a-specific-sid"></a>G. Creazione di un utente del database indipendente con un SID specifico  
 Nell'esempio seguente viene creato un utente del database indipendente con autenticazione di SQL Server denominato CarmenW. Questo esempio può essere eseguito solo in un database indipendente.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER CarmenW WITH PASSWORD = 'a8ea v*(Rd##+'  
, SID = 0x01050000000000090300000063FF0451A9E7664BA705B10E37DDC4B7;
```  
  
### <a name="h-creating-a-user-to-copy-encrypted-data"></a>H. Creazione di un utente per copiare dati crittografati  
 L'esempio seguente crea un utente che può copiare dati protetti dalla funzionalità Always Encrypted da un set di tabelle contenenti colonne crittografate a un altro set di tabelle con colonne crittografate (nello stesso database o in un altro).  Per altre informazioni, vedere [Migrare dati sensibili protetti da Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql  
CREATE USER [Chin]   
WITH   
      DEFAULT_SCHEMA = dbo  
    , ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = ON ;  
```

### <a name="i-create-an-azure-ad-user-from-an-azure-ad-login-in-sql-managed-instance"></a>I. Creare un utente di Azure AD da un account di accesso di Azure AD in Istanza gestita di SQL

 Per creare un utente di Azure AD da un account di accesso di Azure AD, usare la sintassi seguente.

 Accedere all'istanza gestita con un account di accesso di Azure AD con il ruolo `sysadmin`. La sintassi seguente crea un utente di Azure AD bob@contoso.com dall'account di accesso bob@contoso.com. Questo account di accesso è stato creato nell'esempio [CREATE LOGIN](./create-login-transact-sql.md#examples).

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [bob@contoso.com];
GO
```

> [!IMPORTANT]
> Durante la creazione di un **USER** da un account di accesso di Azure AD, specificare come *user_name* lo stesso *login_name* di **LOGIN**.

La creazione di un utente di Azure AD come gruppo da un account di accesso di Azure AD che rappresenta un gruppo è supportata.

```sql
CREATE USER [AAD group] FROM LOGIN [AAD group];
GO
```

È anche possibile creare un utente di Azure AD da un account di accesso di Azure AD che rappresenta un gruppo.

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [AAD group];
GO
```

### <a name="j-create-an-azure-ad-user-without-an-aad-login-for-the-database"></a>J. Creazione di un utente di Azure AD senza un account di accesso AAD per il database

La sintassi seguente viene usata per creare un utente di Azure AD bob@contoso.com nel database di Istanza gestita di SQL (utente indipendente):

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
GO
```

## <a name="next-steps"></a>Passaggi successivi  
Dopo aver creato l'utente, è consigliabile aggiungerlo a un ruolo del database tramite l'istruzione [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md).  
È anche consigliabile [concedere autorizzazioni per oggetti](../../t-sql/statements/grant-object-permissions-transact-sql.md) al ruolo perché possa accedere alle tabelle. Per informazioni generali sul modello di sicurezza di SQL Server, vedere [Autorizzazioni](../../relational-databases/security/permissions-database-engine.md).   
  
## <a name="see-also"></a>Vedere anche  
 [Creare un utente di database](../../relational-databases/security/authentication-access/create-a-database-user.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Database indipendenti](../../relational-databases/databases/contained-databases.md)   
 [Connessione al database SQL con l'autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)   
 [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
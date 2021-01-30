---
description: CREATE LOGIN (Transact-SQL)
title: CREATE LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_LOGIN_TSQL
- CREATE LOGIN
- LOGIN_TSQL
- LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- passwords [SQL Server], logins
- mapping logins [SQL Server]
- logins [SQL Server], creating
- CREATE LOGIN statement
- permissions [SQL Server], logins
- Windows domain accounts [SQL Server]
- re-hashing passwords
- certificates [SQL Server], logins
ms.assetid: eb737149-7c92-4552-946b-91085d8b1b01
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a6a695e5bc2fe0c02a097f66317ea2f3f1161cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192633"
---
# <a name="create-login-transact-sql"></a>CREATE LOGIN (Transact-SQL)

Crea un account di accesso per SQL Server, il database SQL, Azure Synapse Analytics o i database della piattaforma di strumenti analitici. Fare clic su una delle seguenti schede per la sintassi, gli argomenti, i commenti, le autorizzazioni e gli esempi per una versione particolare.

CREATE LOGIN partecipa nelle transazioni. Se si esegue CREATE LOGIN all'interno di una transazione di cui viene eseguito il rollback, la creazione dell'account di accesso viene sottoposta a rollback. Se si esegue all'interno di una transazione, l'account di accesso creato può essere usato solo dopo il commit della transazione.

Per altre informazioni sulle convenzioni di sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Database SQL di Azure](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita<br />di SQL di Azure](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server
CREATE LOGIN login_name { WITH <option_list1> | FROM <sources> }

<option_list1> ::=
    PASSWORD = { 'password' | hashed_password HASHED } [ MUST_CHANGE ]
    [ , <option_list2> [ ,... ] ]

<option_list2> ::=
    SID = sid
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | CHECK_EXPIRATION = { ON | OFF}
    | CHECK_POLICY = { ON | OFF}
    | CREDENTIAL = credential_name

<sources> ::=
    WINDOWS [ WITH <windows_options>[ ,... ] ]
    | CERTIFICATE certname
    | ASYMMETRIC KEY asym_key_name
  
<windows_options> ::=
    DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>Argomenti

*login_name* specifica il nome dell'account di accesso creato. Esistono quattro tipi di account di accesso: account di accesso di SQL Server, account di accesso di Windows, account di accesso con mapping a un certificato e account di accesso con mapping a una chiave asimmetrica. Quando si creano account di accesso che vengono mappati da un account di dominio di Windows, è necessario usare il nome di accesso utente precedente a Windows 2000 nel formato [\<domainName>\\<nome_account_accesso>]. Non è possibile usare un nome UPN nel formato login_name@DomainName. Vedere l'esempio D più avanti in questo articolo. Gli account di accesso di autenticazione sono di tipo **sysname** e devono essere conformi alle regole per gli [Identificatori](../../relational-databases/databases/database-identifiers.md) e non possono contenere un simbolo " **\\** ". Gli account di accesso di Windows possono contenere un simbolo " **\\** ". I nomi degli account di accesso basati su utenti di Active Directory devono avere un numero di caratteri inferiore a 21.

PASSWORD **=** '*password*' Si applica solo agli account di accesso di SQL Server. Specifica la password per l'account di accesso che viene creato. Usare una password complessa. Per altre informazioni, vedere [Password complesse](../../relational-databases/security/strong-passwords.md) e [Criteri password](../../relational-databases/security/password-policy.md). A partire da SQL Server 2012 (11.x), le informazioni relative alle password archiviate vengono calcolate usando l'algoritmo SHA-512 della password con salting.

Per le password viene fatta distinzione tra maiuscole e minuscole. Le password devono essere sempre composte da almeno otto caratteri e non possono superare i 128 caratteri. Le password possono includere i caratteri a-z, A-Z, 0-9 e la maggior parte dei caratteri non alfanumerici. Le password non possono contenere virgolette singole o il valore di *login_name*.

PASSWORD **=** *hashed\_password* si applica solo alla parola chiave HASHED. Specifica il valore hash della password per l'account di accesso in fase di creazione.

HASHED Si applica solo agli account di accesso SQL Server. Specifica che è già stato eseguito l'hashing per la password immessa dopo l'argomento PASSWORD. Se questa opzione non è selezionata, viene generato l'hash della stringa immessa come password prima che questa venga archiviata nel database. Questa opzione deve essere utilizzata solo per la migrazione dei database da un server a un altro. Non utilizzare l'opzione HASHED per creare nuovi account di accesso. L'opzione HASHED non può essere usata con hash creati da SQL 7 o versioni precedenti.

MUST_CHANGE Si applica solo agli account di accesso SQL Server. Se questa opzione è inclusa, in SQL Server viene richiesto all'utente di immettere una nuova password al primo uso del nuovo account di accesso.

CREDENTIAL **=** _credential\_name_ è il nome della credenziale di cui eseguire il mapping al nuovo account di accesso di SQL Server. La credenziale deve già esistere nel server. Attualmente questa opzione consente solo di connettere la credenziale a un account di accesso. Non è possibile eseguire il mapping di una credenziale all'account di accesso dell'amministratore di sistema.

SID = *sid* si usa per ricreare un account di accesso. Si applica solo agli account di accesso con autenticazione di SQL Server, non agli account di accesso con autenticazione di Windows. Specifica il SID del nuovo account di accesso con autenticazione di SQL Server. Se questa opzione non viene usata, SQL Server assegna un SID automaticamente. La struttura del SID dipende dalla versione di SQL Server. SID di account di accesso di SQL Server: valore letterale (**binary(16)** ) a 16 byte basato su un GUID. Ad esempio: `SID = 0x14585E90117152449347750164BA00A7`.

DEFAULT_DATABASE **=** _database_ specifica il database predefinito da assegnare all'account di accesso. Se questa opzione non è inclusa, il database predefinito viene impostato su master.

DEFAULT_DATABASE **=** _language_ specifica la lingua predefinita da assegnare all'account di accesso. Se questa opzione non è inclusa, la lingua predefinita viene impostata sulla lingua predefinita corrente del server. Se la lingua predefinita del server viene modificata in seguito, la lingua predefinita dell'account di accesso rimane inalterata.

CHECK_EXPIRATION **=** { ON | **OFF** } si applica solo agli account di accesso di SQL Server. Specifica se i criteri di scadenza delle password devono essere applicati a questo account di accesso. Il valore predefinito è OFF.

CHECK_POLICY **=** { ON | **OFF** } si applica solo agli account di accesso di SQL Server. Specifica che i criteri password di Windows del computer in cui è in esecuzione SQL Server devono essere applicati a questo account di accesso. Il valore predefinito è ON.

Se i criteri di Windows richiedono password complesse, queste devono contenere almeno tre dei quattro caratteri seguenti:

- Un carattere maiuscolo (A-Z).
- Un carattere minuscolo (a-z)
- Una cifra (0-9).
- Un carattere non alfanumerico, ad esempio uno spazio, _, @, *, ^, %, !, $, # o &.

WINDOWS specifica che l'account di accesso deve associato a un account di accesso di Windows.

CERTIFICATE *certname* specifica il nome di un certificato da associare a questo account di accesso. Questo certificato deve essere già presente nel database master.

ASYMMETRIC KEY *asym_key_name* specifica il nome di una chiave asimmetrica da associare a questo account di accesso. Questa chiave deve essere già presente nel database master.

## <a name="remarks"></a>Osservazioni

- Per le password viene fatta distinzione tra maiuscole e minuscole.
- L'uso di password con hashing già eseguito è supportato solo quando si creano gli account di accesso di SQL Server.
- Se si specifica MUST_CHANGE, è necessario impostare CHECK_EXPIRATION e CHECK_POLICY su ON. In caso contrario, l'istruzione non verrà eseguita correttamente.
- Una combinazione di CHECK_POLICY = OFF e CHECK_EXPIRATION = ON non è supportata.
- Quando l'opzione CHECK_POLICY è impostata su OFF, il valore di *lockout_time* viene reimpostato e l'opzione CHECK_EXPIRATION viene impostata su OFF.

> [!IMPORTANT]
> CHECK_EXPIRATION e CHECK_POLICY sono imposte solo in Windows Server 2003 e versioni successive. Per ulteriori informazioni, vedere [Password Policy](../../relational-databases/security/password-policy.md).

- Gli account di accesso creati da certificati o chiavi asimmetriche vengono utilizzati solo per la firma del codice e non possono essere usati per la connessione a SQL Server. È possibile creare un account di accesso da un certificato o una chiave asimmetrica solo quando il certificato o la chiave asimmetrica esistono già nel database master.
- Per uno script con cui trasferire gli account di accesso, vedere [Trasferimento di accessi e password tra istanze di SQL Server](https://support.microsoft.com/kb/918992).
- La creazione automatica di un account di accesso abilita il nuovo account a cui viene concessa l'autorizzazione **CONNECT SQL** a livello di server.
- La [modalità di autenticazione](../../relational-databases/security/choose-an-authentication-mode.md) del server deve corrispondere al tipo di account di accesso per consentire l'accesso.
- Per informazioni sulla progettazione di un sistema di autorizzazioni, vedere [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).

## <a name="permissions"></a>Autorizzazioni

- Solo gli utenti con autorizzazione **ALTER ANY LOGIN** per il server o appartenenza al ruolo predefinito del server **securityadmin** possono creare account di accesso. Per altre informazioni, vedere [Ruoli a livello di server](/azure/sql-database/sql-database-manage-logins#groups-and-roles) e [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).
- Se viene usata l'opzione **CREDENTIAL**, è richiesta anche l'autorizzazione **ALTER ANY CREDENTIAL** nel server.

## <a name="after-creating-a-login"></a>Dopo la creazione di un account di accesso

Una volta creato, un account di accesso può connettersi a SQL Server ma ha solo le autorizzazioni concesse al ruolo **public**. Provare a eseguire alcune delle attività seguenti.

- Per connettersi a un database, creare un utente del database per l'account di accesso. Per altre informazioni, vedere [CREATE USER](../../t-sql/statements/create-user-transact-sql.md).
- Creare un ruolo del server definito dall'utente tramite [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md). Usare **ALTER SERVER ROLE** ... **ADD MEMBER** per aggiungere il nuovo account di accesso al ruolo del server definito dall'utente. Per altre informazioni, vedere [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) e [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).
- Usare **sp_addsrvrolemember** per aggiungere l'account di accesso a un ruolo predefinito del server. Per altre informazioni, vedere [Ruoli a livello di Server](../../relational-databases/security/authentication-access/server-level-roles.md) e [sp_addsrvrolemember](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).
- Usare l'istruzione **GRANT** per concedere le autorizzazioni a livello di server al nuovo account di accesso o a un ruolo in cui esso è contenuto. Per altre informazioni, vedere [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="examples"></a>Esempi

### <a name="a-creating-a-login-with-a-password"></a>R. Creazione di un account di accesso con una password

Nell'esempio seguente viene creato un account di accesso per un utente specifico e viene assegnata una password.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-with-a-password-that-must-be-changed"></a>B. Creazione di un account di accesso con una password da modificare

Nell'esempio seguente viene creato un account di accesso per un utente specifico e viene assegnata una password. L'opzione `MUST_CHANGE` richiede all'utente di modificare questa password alla prima connessione al server.

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'
    MUST_CHANGE, CHECK_EXPIRATION = ON;
GO
```

> [!NOTE]
> Impossibile utilizzare l'opzione MUST_CHANGE se CHECK_EXPIRATION è impostato su OFF.

### <a name="c-creating-a-login-mapped-to-a-credential"></a>C. Creazione di un account di accesso sul quale viene eseguito il mapping a una credenziale

Nell'esempio seguente viene creato l'account di accesso per utente specifico, utilizzando il relativo nome. Su questo account di accesso viene eseguito il mapping alla credenziale.

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>',
    CREDENTIAL = <credentialName>;
GO
```

### <a name="d-creating-a-login-from-a-certificate"></a>D. Creazione di un account di accesso da un certificato

Nell'esempio seguente viene creato un account di accesso per un utente specifico da un certificato nel database master.

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

```sql
USE MASTER;
CREATE CERTIFICATE <certificateName>
    WITH SUBJECT = '<login_name> certificate in master database',
    EXPIRY_DATE = '12/05/2025';
GO
CREATE LOGIN <login_name> FROM CERTIFICATE <certificateName>;
GO
```

### <a name="e-creating-a-login-from-a-windows-domain-account"></a>E. Creazione di un account di accesso da un account di dominio di Windows

Nell'esempio seguente viene creato un account di accesso da un account di dominio di Windows.

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

```sql
CREATE LOGIN [<domainName>\<login_name>] FROM WINDOWS;
GO
```

### <a name="f-creating-a-login-from-a-sid"></a>F. Creazione di un account di accesso da un SID

L'esempio seguente crea prima un account di accesso con autenticazione di SQL Server e determina il SID dell'account di accesso.

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';
SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

La query restituisce 0x241C11948AEEB749B0D22646DB1A19F2 come SID. La query dell'utente restituirà un valore diverso. Le istruzioni seguenti eliminano l'account di accesso e quindi ricreano l'account di accesso. Utilizzare il SID della query precedente.

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

### <a name="g-creating-a-login-with-multiple-arguments"></a>G. Creazione di un account di accesso con più argomenti

Nell'esempio seguente viene illustrato come collegare più argomenti insieme usando le virgole tra ogni argomento.

```sql
CREATE LOGIN [MyUser]
WITH PASSWORD = 'MyPassword',
DEFAULT_DATABASE = MyDatabase,
CHECK_POLICY = OFF,
CHECK_EXPIRATION = OFF ;
```

## <a name="see-also"></a>Vedere anche

- [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Entità](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Criteri password](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Creare un account di accesso](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Database SQL di Azure\*_**
    :::column-end:::
    :::column:::
        [Istanza gestita<br />di SQL di Azure](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>Database SQL

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for Azure SQL Database
CREATE LOGIN login_name
 { WITH <option_list> }

<option_list> ::=
    PASSWORD = { 'password' }
    [ , SID = sid ]
```

## <a name="arguments"></a>Argomenti

*login_name* specifica il nome dell'account di accesso creato. I database singoli e in pool nel database SQL di Azure e nei database in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] supportano solo gli account di accesso SQL. Per creare account per utenti di Azure Active Directory o per creare account utente non associati a un account di accesso, usare l'istruzione [CREATE USER](create-user-transact-sql.md). Per altre informazioni, vedere [Gestire gli account di accesso nel database SQL di Azure](/azure/sql-database/sql-database-manage-logins).

PASSWORD **='** password* *'* specifica la password per l'account di accesso SQL da creare. Usare una password complessa. Per altre informazioni, vedere [Password complesse](../../relational-databases/security/strong-passwords.md) e [Criteri password](../../relational-databases/security/password-policy.md). A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le informazioni relative alle password archiviate vengono calcolate tramite SHA-512 della password salt.

Per le password viene fatta distinzione tra maiuscole e minuscole. Le password devono essere sempre composte da almeno otto caratteri e non possono superare i 128 caratteri. Le password possono includere i caratteri a-z, A-Z, 0-9 e la maggior parte dei caratteri non alfanumerici. Le password non possono contenere virgolette singole o il valore di *login_name*.

SID = *sid* si usa per ricreare un account di accesso. Si applica solo agli account di accesso con autenticazione di SQL Server, non agli account di accesso con autenticazione di Windows. Specifica il SID del nuovo account di accesso con autenticazione di SQL Server. Se questa opzione non viene usata, SQL Server assegna un SID automaticamente. La struttura del SID dipende dalla versione di SQL Server. Per il database SQL, si tratta in genere di un valore letterale (**binary(32)** ) a 32 byte costituito da `0x01060000000000640000000000000000` e 16 byte che rappresentano un GUID. Ad esempio: `SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`.

## <a name="remarks"></a>Osservazioni

- Per le password viene fatta distinzione tra maiuscole e minuscole.
- La creazione automatica di un account di accesso abilita il nuovo account a cui viene concessa l'autorizzazione **CONNECT SQL** a livello di server.

> [!IMPORTANT]
> Per informazioni sull'uso di account di accesso e utenti nel database SQL di Azure, vedere [Gestire gli account di accesso nel database SQL di Azure](/azure/sql-database/sql-database-manage-logins).

## <a name="login"></a>Login

### <a name="sql-database-logins"></a>Account di accesso al database SQL

L'istruzione **CREATE LOGIN** deve essere l'unica istruzione in un batch.

In alcuni metodi di connessione al database SQL, come ad esempio **sqlcmd**, è necessario accodare il nome del server del database SQL al nome dell'account di accesso nella stringa di connessione usando la notazione *\<login>* @ *\<server>* . Ad esempio, se l'account di accesso è `login1` e il nome completo del server del database SQL è `servername.database.windows.net`, il parametro *username* della stringa di connessione deve essere `login1@servername`. Poiché la lunghezza totale del parametro *username* è di 128 caratteri, *login_name* è limitato a 127 caratteri meno la lunghezza del nome del server. Nell'esempio la lunghezza di `login_name` può essere solo di 117 caratteri poiché `servername` è costituito da 10 caratteri.

Nel database SQL è necessario essere connessi al database master con le autorizzazioni appropriate per creare un account di accesso. Per altre informazioni, vedere [Creare ulteriori account di accesso e utenti con autorizzazioni amministrative](/azure/sql-database/sql-database-manage-logins#create-additional-logins-and-users-having-administrative-permissions).

Le regole di SQL Server consentono di creare un account di accesso con autenticazione di SQL Server nel formato \<loginname>@\<servername>. Se il server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] in uso è **myazureserver** e l'account di accesso è **myemail@live.com** , l'account di accesso deve essere specificato nel formato **myemail@live.com@myazureserver** .

Nel database SQL i dati dell'account di accesso necessari per autenticare una connessione e le regole del firewall a livello di server vengono memorizzati temporaneamente nella cache in ogni database. Questa cache viene aggiornata periodicamente. Per forzare un aggiornamento della cache di autenticazione e assicurarsi che un database abbia la versione più recente della tabella di account di accesso, eseguire [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Autorizzazioni

Solo l'account di accesso dell'entità di livello server (creato dal processo di provisioning) o i membri del ruolo del database `loginmanager` nel database master possono creare nuovi account di accesso. Per altre informazioni, vedere [Creare ulteriori account di accesso e utenti con autorizzazioni amministrative](/azure/sql-database/sql-database-manage-logins#create-additional-logins-and-users-having-administrative-permissions).

## <a name="examples"></a>Esempi

### <a name="a-creating-a-login-with-a-password"></a>R. Creazione di un account di accesso con una password

Nell'esempio seguente viene creato un account di accesso per un utente specifico e viene assegnata una password.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-from-a-sid"></a>B. Creazione di un account di accesso da un SID

L'esempio seguente crea prima un account di accesso con autenticazione di SQL Server e determina il SID dell'account di accesso.

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

La query restituisce 0x241C11948AEEB749B0D22646DB1A19F2 come SID. La query dell'utente restituirà un valore diverso. Le istruzioni seguenti eliminano l'account di accesso e quindi ricreano l'account di accesso. Utilizzare il SID della query precedente.

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

## <a name="see-also"></a>Vedere anche

- [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Entità](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Criteri password](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Creare un account di accesso](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL di Azure](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Istanza gestita<br />di SQL di Azure \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Istanza gestita di SQL di Azure

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for Azure SQL Managed Instance
CREATE LOGIN login_name [FROM EXTERNAL PROVIDER] { WITH <option_list> [,..]}

<option_list> ::=
    PASSWORD = {'password'}
    | SID = sid
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>Argomenti

*login_name*: se usato con la clausola **FROM EXTERNAL PROVIDER**, l'account di accesso specifica l'entità di Azure Active Directory (AD), ovvero un utente, un gruppo o un'applicazione di Azure Active Directory. In caso contrario, l'account di accesso rappresenta il nome dell'account di accesso SQL creato.

FROM EXTERNAL PROVIDER </br>
Specifica che l'account di accesso è per il servizio Autenticazione di Azure AD.

PASSWORD **=** '*password*' specifica la password per l'account di accesso SQL da creare. Usare una password complessa. Per altre informazioni, vedere [Password complesse](../../relational-databases/security/strong-passwords.md) e [Criteri password](../../relational-databases/security/password-policy.md). A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le informazioni relative alle password archiviate vengono calcolate tramite SHA-512 della password salt.

Per le password viene fatta distinzione tra maiuscole e minuscole. Le password devono essere sempre composte da almeno dieci caratteri e non possono superare i 128 caratteri. Le password possono includere i caratteri a-z, A-Z, 0-9 e la maggior parte dei caratteri non alfanumerici. Le password non possono contenere virgolette singole o il valore di *login_name*.

SID **=** *sid* si usa per ricreare l'account di accesso. Si applica solo agli account di accesso per l'autenticazione di SQL Server. Specifica il SID del nuovo account di accesso con autenticazione di SQL Server. Se questa opzione non viene usata, SQL Server assegna un SID automaticamente. La struttura del SID dipende dalla versione di SQL Server. Per il database SQL, si tratta in genere di un valore letterale (**binary(32)** ) a 32 byte costituito da `0x01060000000000640000000000000000` e 16 byte che rappresentano un GUID. Ad esempio: `SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`.

## <a name="remarks"></a>Osservazioni

- Per le password viene fatta distinzione tra maiuscole e minuscole.
- È stata introdotta una nuova sintassi per la creazione di entità di livello server mappate agli account Azure AD (**FROM EXTERNAL PROVIDER**)
- Quando si specifica **FROM EXTERNAL PROVIDER**:

  - login_name deve rappresentare un account Azure AD esistente (utente, gruppo o applicazione) accessibile in Azure AD dall'Istanza gestita SQL di Azure corrente. Per le entità di sicurezza di Azure AD, la sintassi CREATE LOGIN richiede:
    - UserPrincipalName dell'oggetto di Azure AD per utenti di Azure AD.
    - DisplayName dell'oggetto di Azure AD per i gruppi di Azure AD e le applicazioni di Azure AD.
  - Non è possibile usare l'opzione **PASSWORD**.
- Per impostazione predefinita, quando la clausola **FROM EXTERNAL PROVIDER** viene omessa, verrà creato un normale account di accesso SQL.
- Gli account di accesso di Azure AD sono visibili in sys.server_principals con il valore type column impostato su **E** e type_desc impostato su **EXTERNAL_LOGIN** per gli account di accesso mappati ad utenti di Azure AD oppure con il valore type column impostato su **X** e il valore type_desc impostato su **EXTERNAL_GROUP** per gli account di accesso mappati a gruppi di Azure AD.
- Per uno script con cui trasferire gli account di accesso, vedere [Trasferimento di accessi e password tra istanze di SQL Server](https://support.microsoft.com/kb/918992).
- La creazione automatica di un account di accesso abilita il nuovo account a cui viene concessa l'autorizzazione **CONNECT SQL** a livello di server.

> [!IMPORTANT]
> Per informazioni sull'uso di account di accesso e utenti nel database SQL di Azure, vedere [Gestire gli account di accesso nel database SQL di Azure](/azure/sql-database/sql-database-manage-logins).

## <a name="logins-and-permissions"></a>Account di accesso e autorizzazioni

Solo l'account di accesso dell'entità di livello server (creato dal processo di provisioning) o i membri del ruolo del database `securityadmin` o `sysadmin` nel database master possono creare nuovi account di accesso. Per altre informazioni, vedere [Ruoli a livello di server](/azure/sql-database/sql-database-manage-logins#groups-and-roles) e [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).

Per impostazione predefinita, l'autorizzazione standard concessa a un nuovo account di accesso di Azure AD nel database master è: **CONNECT SQL** e **VIEW ANY DATABASE**.

### <a name="sql-managed-instance-logins"></a>Account di accesso per Istanza gestita di SQL

- È richiesta l'autorizzazione **ALTER ANY LOGIN** nel server o l'appartenenza a uno dei ruoli predefiniti del server `securityadmin` o `sysadmin`. Il comando Create può essere eseguito solo da un account Azure Active Directory (Azure AD) con l'autorizzazione **ALTER ANY LOGIN** nel server o l'appartenenza a uno di questi ruoli.
- Se l'account di accesso è un'entità di sicurezza SQL, solo gli account di accesso che fanno parte del ruolo `sysadmin` possono usare il comando per creare gli account di accesso per un account Azure AD.
- Deve essere un membro di Azure AD all'interno della stessa directory usata per l'Istanza gestita SQL di Azure.

## <a name="after-creating-a-login"></a>Dopo la creazione di un account di accesso

> [!NOTE]
> L'amministratore di Azure AD per la funzionalità Istanza gestita di SQL di Azure dopo la creazione è stato modificato. Per altre informazioni, vedere [Nuove funzionalità di amministrazione di Azure AD per l'istanza gestita](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

Una volta creato, un account di accesso può connettersi a un'istanza gestita, ma ha solo le autorizzazioni concesse al ruolo **public**. Provare a eseguire alcune delle attività seguenti.

- Per creare un utente di Azure AD da un account di accesso di Azure AD, vedere [CREATE USER](../../t-sql/statements/create-user-transact-sql.md).
- Per concedere le autorizzazioni a un utente in un database, usare l'istruzione **ALTER SERVER ROLE** ... **ADD MEMBER** per aggiungere l'utente a uno dei ruoli predefiniti del database o a un ruolo personalizzato o concedere le autorizzazioni all'utente direttamente tramite l'istruzione [GRANT](../../t-sql/statements/grant-transact-sql.md). Per altre informazioni, vedere [Utenti non amministratori](/azure/sql-database/sql-database-manage-logins#non-administrator-users), [Ruoli amministrativi aggiuntivi a livello di server](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) e l'istruzione [GRANT](grant-transact-sql.md).
- Per concedere autorizzazioni a livello di server, creare un utente del database nel database master e usare l'istruzione **ALTER SERVER ROLE** ... **ADD MEMBER** per aggiungere l'utente a uno dei ruoli di amministrazione del server. Per altre informazioni, vedere [Ruoli a livello di server](/azure/sql-database/sql-database-manage-logins#groups-and-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) e [Ruoli del server](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles).
  - Usare il comando seguente per aggiungere il ruolo `sysadmin` a un account di accesso di Azure AD: `ALTER SERVER ROLE sysadmin ADD MEMBER [AzureAD_Login_name]`
- Usare l'istruzione **GRANT** per concedere le autorizzazioni a livello di server al nuovo account di accesso o a un ruolo in cui esso è contenuto. Per altre informazioni, vedere [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="limitations"></a>Limitazioni

- L'impostazione di un account di accesso di Azure AD mappato a un gruppo di Azure AD come proprietario del database non è supportata.
- Le rappresentazione delle entità di livello server di Azure AD con altre entità di Azure AD è supportata, ad esempio la clausola [EXECUTE AS](execute-as-transact-sql.md).
- Solo le entità di livello server SQL (account di accesso) incluse nel ruolo `sysadmin` possono eseguire le operazioni seguenti destinate a entità di Azure AD:
  - EXECUTE AS USER
  - EXECUTE AS LOGIN
- Gli utenti esterni (guest) importati da un'altra directory di Azure AD non possono essere configurati direttamente come amministratori di Azure AD per Istanza gestita di SQL tramite il portale di Azure. È possibile invece aggiungere un utente esterno a un gruppo con sicurezza abilitata di Azure AD e configurare il gruppo come amministratore dell'istanza. È possibile usare PowerShell o l'interfaccia della riga di comando di Azure per impostare singoli utenti guest come amministratore dell'istanza.
- In un gruppo di failover, l'account di accesso non viene replicato nell'istanza secondaria, ma viene salvato nel database master, un database di sistema e, pertanto, non viene replicato geograficamente. Per risolvere questo problema, è necessario che l'utente crei un account di accesso con lo stesso SID nell'istanza secondaria.

```SQL
-- Code to create login on the secondary instance
CREATE LOGIN foo WITH PASSWORD = '<enterStrongPasswordHere>', SID = <login_sid>;
```

## <a name="examples"></a>Esempi

### <a name="a-creating-a-login-with-a-password"></a>R. Creazione di un account di accesso con una password

Nell'esempio seguente viene creato un account di accesso per un utente specifico e viene assegnata una password.

 ```sql
 CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
 GO
 ```

### <a name="b-creating-a-login-from-a-sid"></a>B. Creazione di un account di accesso da un SID

 L'esempio seguente crea prima un account di accesso con autenticazione di SQL Server e determina il SID dell'account di accesso.

 ```sql
 CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

 SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
 GO
 ```

La query restituisce 0x241C11948AEEB749B0D22646DB1A19F2 come SID. La query dell'utente restituirà un valore diverso. Le istruzioni seguenti eliminano l'account di accesso e quindi ricreano l'account di accesso. Utilizzare il SID della query precedente.

 ```sql
 DROP LOGIN TestLogin;
 GO

 CREATE LOGIN TestLogin
 WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

 SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
 GO
 ```

### <a name="c-creating-a-login-for-a-local-azure-ad-account"></a>C. Creazione di un account di accesso per un account Azure AD locale

 Nell'esempio seguente viene creato un account di accesso per l'account Azure AD joe@myaad.onmicrosoft.com presente nell'istanza di Azure AD *myaad*.

```sql
CREATE LOGIN [joe@myaad.onmicrosoft.com] FROM EXTERNAL PROVIDER
GO
```

### <a name="d-creating-a-login-for-a-federated-azure-ad-account"></a>D. Creazione di un account di accesso per un account Azure AD federato

 Nell'esempio seguente viene creato un account di accesso per l'account Azure AD federato bob@contoso.com presente nell'istanza di Azure AD denominata *contoso*. L'utente bob può anche essere un utente guest.

```sql
CREATE LOGIN [bob@contoso.com] FROM EXTERNAL PROVIDER
GO
```

### <a name="e-creating-a-login-for-an-azure-ad-group"></a>E. Creazione di un account di accesso per un gruppo di Azure AD

 Nell'esempio seguente viene creato un account di accesso per il gruppo di Azure AD *mygroup* presente nell'istanza di Azure AD *myaad*

```sql
CREATE LOGIN [mygroup] FROM EXTERNAL PROVIDER
GO
```

### <a name="f-creating-a-login-for-an-azure-ad-application"></a>F. Creazione di un account di accesso per un'applicazione Azure AD

Nell'esempio seguente viene creato un account di accesso per l'applicazione Azure AD *myapp* presente nell'istanza di Azure AD *myaad*

```sql
CREATE LOGIN [myapp] FROM EXTERNAL PROVIDER
```

### <a name="g-check-newly-added-logins"></a>G. Controllare i nuovi account di accesso aggiunti

Per controllare i nuovi account di accesso aggiunti, eseguire il comando T-SQL seguente:

```sql
SELECT *
FROM sys.server_principals;
GO
```

## <a name="see-also"></a>Vedere anche

- [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Entità](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Criteri password](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Creare un account di accesso](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL di Azure](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita<br />di SQL di Azure](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for Azure Synapse Analytics
CREATE LOGIN login_name
 { WITH <option_list> }

<option_list> ::=
    PASSWORD = { 'password' }
    [ , SID = sid ]
```

## <a name="arguments"></a>Argomenti

*login_name* specifica il nome dell'account di accesso creato. SQL Analytics in Azure Synapse supporta solo gli account di accesso SQL. Per creare account per gli utenti di Azure Active Directory, usare l'istruzione [CREATE USER](create-user-transact-sql.md).

PASSWORD **='** password* *'* specifica la password per l'account di accesso SQL da creare. Usare una password complessa. Per altre informazioni, vedere [Password complesse](../../relational-databases/security/strong-passwords.md) e [Criteri password](../../relational-databases/security/password-policy.md). A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le informazioni relative alle password archiviate vengono calcolate tramite SHA-512 della password salt.

Per le password viene fatta distinzione tra maiuscole e minuscole. Le password devono essere sempre composte da almeno otto caratteri e non possono superare i 128 caratteri. Le password possono includere i caratteri a-z, A-Z, 0-9 e la maggior parte dei caratteri non alfanumerici. Le password non possono contenere virgolette singole o il valore di *login_name*.

 SID = *sid* si usa per ricreare un account di accesso. Si applica solo agli account di accesso con autenticazione di SQL Server, non agli account di accesso con autenticazione di Windows. Specifica il SID del nuovo account di accesso con autenticazione di SQL Server. Se questa opzione non viene usata, SQL Server assegna un SID automaticamente. La struttura del SID dipende dalla versione di SQL Server. Per SQL Analytics si tratta di un valore letterale (**binary(32)** ) a 32 byte costituito da `0x01060000000000640000000000000000` e 16 byte che rappresentano un GUID. Ad esempio: `SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`.

## <a name="remarks"></a>Osservazioni

- Per le password viene fatta distinzione tra maiuscole e minuscole.
- Per uno script con cui trasferire gli account di accesso, vedere [Trasferimento di accessi e password tra istanze di SQL Server](https://support.microsoft.com/kb/918992).
- La creazione automatica di un account di accesso abilita il nuovo account a cui viene concessa l'autorizzazione **CONNECT SQL** a livello di server.
- La [modalità di autenticazione](../../relational-databases/security/choose-an-authentication-mode.md) del server deve corrispondere al tipo di account di accesso per consentire l'accesso.
- Per informazioni sulla progettazione di un sistema di autorizzazioni, vedere [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).

## <a name="logins"></a>Logins

L'istruzione **CREATE LOGIN** deve essere l'unica istruzione in un batch.

Quando si usa uno strumento, ad esempio **sqlcmd**, per la connessione ad Azure Synapse è necessario accodare il nome del server di SQL Analytics al nome dell'account di accesso nella stringa di connessione usando la notazione *\<login>* @ *\<server>* . Ad esempio, se l'account di accesso è `login1` e il nome completo del server di SQL Analytics è `servername.database.windows.net`, il parametro *username* della stringa di connessione deve essere `login1@servername`. Poiché la lunghezza totale del parametro *username* è di 128 caratteri, *login_name* è limitato a 127 caratteri meno la lunghezza del nome del server. Nell'esempio la lunghezza di `login_name` può essere solo di 117 caratteri poiché `servername` è costituito da 10 caratteri.

Per creare un account di accesso, è necessario essere connessi al database master.

Le regole di SQL Server consentono di creare un account di accesso con autenticazione di SQL Server nel formato \<loginname>@\<servername>. Se il server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] in uso è **myazureserver** e l'account di accesso è **myemail@live.com** , l'account di accesso deve essere specificato nel formato **myemail@live.com@myazureserver** .

I dati dell'account di accesso necessari per autenticare una connessione e le regole del firewall a livello di server sono memorizzati temporaneamente nella cache in ogni database. Questa cache viene aggiornata periodicamente. Per forzare un aggiornamento della cache di autenticazione e assicurarsi che un database abbia la versione più recente della tabella di account di accesso, eseguire [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

Per altre informazioni sugli account di accesso, vedere [Gestione di database e account di accesso](/azure/sql-database/sql-database-manage-logins).

## <a name="permissions"></a>Autorizzazioni

Solo l'account di accesso dell'entità di livello server (creato dal processo di provisioning) o i membri del ruolo del database `loginmanager` nel database master possono creare nuovi account di accesso. Per altre informazioni, vedere [Ruoli a livello di server](/azure/sql-database/sql-database-manage-logins#groups-and-roles) e [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).

## <a name="after-creating-a-login"></a>Dopo la creazione di un account di accesso

Non appena viene creato, l'account di accesso può connettersi ad Azure Synapse ma ha solo le autorizzazioni concesse al ruolo **public**. Provare a eseguire alcune delle attività seguenti.

- Per connettersi a un database, creare un utente del database per l'account di accesso. Per altre informazioni, vedere [CREATE USER](../../t-sql/statements/create-user-transact-sql.md).
- Per concedere le autorizzazioni a un utente in un database, usare l'istruzione **ALTER SERVER ROLE** ... **ADD MEMBER** per aggiungere l'utente a uno dei ruoli predefiniti del database o a un ruolo personalizzato o concedere le autorizzazioni all'utente direttamente tramite l'istruzione [GRANT](grant-transact-sql.md). Per altre informazioni, vedere [Utenti non amministratori](/azure/sql-database/sql-database-manage-logins#non-administrator-users), [Ruoli amministrativi aggiuntivi a livello di server](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) e l'istruzione [GRANT](grant-transact-sql.md).
- Per concedere autorizzazioni a livello di server, creare un utente del database nel database master e usare l'istruzione **ALTER SERVER ROLE** ... **ADD MEMBER** per aggiungere l'utente a uno dei ruoli di amministrazione del server. Per altre informazioni, vedere [Ruoli a livello di server](/azure/sql-database/sql-database-manage-logins#groups-and-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) e [Ruoli del server](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles).

- Usare l'istruzione **GRANT** per concedere le autorizzazioni a livello di server al nuovo account di accesso o a un ruolo in cui esso è contenuto. Per altre informazioni, vedere [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="examples"></a>Esempi

### <a name="a-creating-a-login-with-a-password"></a>R. Creazione di un account di accesso con una password

Nell'esempio seguente viene creato un account di accesso per un utente specifico e viene assegnata una password.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-from-a-sid"></a>B. Creazione di un account di accesso da un SID

 L'esempio seguente crea prima un account di accesso con autenticazione di SQL Server e determina il SID dell'account di accesso.

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

La query restituisce 0x241C11948AEEB749B0D22646DB1A19F2 come SID. La query dell'utente restituirà un valore diverso. Le istruzioni seguenti eliminano l'account di accesso e quindi ricreano l'account di accesso. Utilizzare il SID della query precedente.

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

## <a name="see-also"></a>Vedere anche

- [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Entità](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Criteri password](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Creare un account di accesso](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL di Azure](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita<br />di SQL di Azure](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Piattaforma di strumenti<br />analitici (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Sistema della piattaforma di analisi

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for Analytics Platform System
CREATE LOGIN loginName { WITH <option_list1> | FROM WINDOWS }

<option_list1> ::=
    PASSWORD = { 'password' } [ MUST_CHANGE ]
    [ , <option_list> [ ,... ] ]
  
<option_list> ::=
      CHECK_EXPIRATION = { ON | OFF}
    | CHECK_POLICY = { ON | OFF}
```

## <a name="arguments"></a>Argomenti

*login_name* specifica il nome dell'account di accesso creato. Esistono quattro tipi di account di accesso: account di accesso di SQL Server, account di accesso di Windows, account di accesso con mapping a un certificato e account di accesso con mapping a una chiave asimmetrica. Quando si creano account di accesso che vengono mappati da un account di dominio di Windows, è necessario usare il nome di accesso utente precedente a Windows 2000 nel formato [\<domainName>\\<nome_account_accesso>]. Non è possibile usare un nome UPN nel formato login_name@DomainName. Vedere l'esempio D più avanti in questo articolo. Gli account di accesso di autenticazione sono di tipo **sysname** e devono essere conformi alle regole per gli [Identificatori](../../relational-databases/databases/database-identifiers.md) e non possono contenere un simbolo " **\\** ". Gli account di accesso di Windows possono contenere un simbolo " **\\** ". I nomi degli account di accesso basati su utenti di Active Directory devono avere un numero di caratteri inferiore a 21.

PASSWORD **='** _password_' Si applica solo agli account di accesso SQL Server. Specifica la password per l'account di accesso che viene creato. Usare una password complessa. Per altre informazioni, vedere [Password complesse](../../relational-databases/security/strong-passwords.md) e [Criteri password](../../relational-databases/security/password-policy.md). A partire da SQL Server 2012 (11.x), le informazioni relative alle password archiviate vengono calcolate usando l'algoritmo SHA-512 della password con salting.

Per le password viene fatta distinzione tra maiuscole e minuscole. Le password devono essere sempre composte da almeno otto caratteri e non possono superare i 128 caratteri. Le password possono includere i caratteri a-z, A-Z, 0-9 e la maggior parte dei caratteri non alfanumerici. Le password non possono contenere virgolette singole o il valore di *login_name*.

MUST_CHANGE Si applica solo agli account di accesso SQL Server. Se questa opzione è inclusa, in SQL Server viene richiesto all'utente di immettere una nuova password al primo uso del nuovo account di accesso.

CHECK_EXPIRATION **=** { ON | **OFF** } si applica solo agli account di accesso di SQL Server. Specifica se i criteri di scadenza delle password devono essere applicati a questo account di accesso. Il valore predefinito è OFF.

CHECK_POLICY **=** { ON | **OFF** } si applica solo agli account di accesso di SQL Server. Specifica che i criteri password di Windows del computer in cui è in esecuzione SQL Server devono essere applicati a questo account di accesso. Il valore predefinito è ON.

Se i criteri di Windows richiedono password complesse, queste devono contenere almeno tre dei quattro caratteri seguenti:

- Un carattere maiuscolo (A-Z).
- Un carattere minuscolo (a-z)
- Una cifra (0-9).
- Un carattere non alfanumerico, ad esempio uno spazio, _, @, *, ^, %, !, $, # o &.

WINDOWS specifica che l'account di accesso deve associato a un account di accesso di Windows.

## <a name="remarks"></a>Osservazioni

- Per le password viene fatta distinzione tra maiuscole e minuscole.
- Se si specifica MUST_CHANGE, è necessario impostare CHECK_EXPIRATION e CHECK_POLICY su ON. In caso contrario, l'istruzione non verrà eseguita correttamente.
- Una combinazione di CHECK_POLICY = OFF e CHECK_EXPIRATION = ON non è supportata.
- Quando l'opzione CHECK_POLICY è impostata su OFF, il valore di *lockout_time* viene reimpostato e l'opzione CHECK_EXPIRATION viene impostata su OFF.

> [!IMPORTANT]
> CHECK_EXPIRATION e CHECK_POLICY sono imposte solo in Windows Server 2003 e versioni successive. Per ulteriori informazioni, vedere [Password Policy](../../relational-databases/security/password-policy.md).

- Per uno script con cui trasferire gli account di accesso, vedere [Trasferimento di accessi e password tra istanze di SQL Server](https://support.microsoft.com/kb/918992).
- La creazione automatica di un account di accesso abilita il nuovo account a cui viene concessa l'autorizzazione **CONNECT SQL** a livello di server.
- Per informazioni sulla progettazione di un sistema di autorizzazioni, vedere [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).

## <a name="permissions"></a>Autorizzazioni

Solo gli utenti con autorizzazione **ALTER ANY LOGIN** per il server o appartenenza al ruolo predefinito del server **securityadmin** possono creare account di accesso. Per altre informazioni, vedere [Ruoli a livello di server](/azure/sql-database/sql-database-manage-logins#groups-and-roles) e [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).

## <a name="after-creating-a-login"></a>Dopo la creazione di un account di accesso

Non appena viene creato, l'account di accesso può connettersi ad Azure Synapse Analytics ma ha solo le autorizzazioni concesse al ruolo **public**. Provare a eseguire alcune delle attività seguenti.

- Per connettersi a un database, creare un utente del database per l'account di accesso. Per altre informazioni, vedere [CREATE USER](../../t-sql/statements/create-user-transact-sql.md).
- Creare un ruolo del server definito dall'utente tramite [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md). Usare **ALTER SERVER ROLE** ... **ADD MEMBER** per aggiungere il nuovo account di accesso al ruolo del server definito dall'utente. Per altre informazioni, vedere [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) e [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).
- Usare **sp_addsrvrolemember** per aggiungere l'account di accesso a un ruolo predefinito del server. Per altre informazioni, vedere [Ruoli a livello di Server](../../relational-databases/security/authentication-access/server-level-roles.md) e [sp_addsrvrolemember](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).
- Usare l'istruzione **GRANT** per concedere le autorizzazioni a livello di server al nuovo account di accesso o a un ruolo in cui esso è contenuto. Per altre informazioni, vedere [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="examples"></a>Esempi

### <a name="g-creating-a-sql-server-authentication-login-with-a-password"></a>G. Creazione di un account di accesso di autenticazione di SQL Server con una password

Nell'esempio seguente viene creato l'account di accesso `Mary7` con la password `A2c3456`.

```sql
CREATE LOGIN Mary7 WITH PASSWORD = 'A2c3456$#' ;
```

### <a name="h-using-options"></a>H. Uso delle opzioni

Nell'esempio seguente viene creato l'account di accesso `Mary8` con password e alcuni degli argomenti facoltativi.

```sql
CREATE LOGIN Mary8 WITH PASSWORD = 'A2c3456$#' MUST_CHANGE,
CHECK_EXPIRATION = ON,
CHECK_POLICY = ON;
```

### <a name="i-creating-a-login-from-a-windows-domain-account"></a>I. Creazione di un account di accesso da un account di dominio di Windows

Nell'esempio seguente viene creato un account di accesso da un account di dominio di Windows denominato `Mary` nel dominio `Contoso`.

```sql
CREATE LOGIN [Contoso\Mary] FROM WINDOWS;
GO
```

## <a name="see-also"></a>Vedere anche

- [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Entità](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Criteri password](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Creare un account di accesso](../../relational-databases/security/authentication-access/create-a-login.md)

---

::: moniker-end

---
description: ALTER USER (Transact-SQL)
title: ALTER USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_USER_TSQL
- ALTER USER
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database users
- ALTER USER statement
- renaming databases
- schemas [SQL Server], default
- database user names [SQL Server]
- names [SQL Server], database users
- default schemas
- modifying default schemas
ms.assetid: 344fc6ce-a008-47c8-a02e-47fae66cc590
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c884d4b07afc92f6a95a5688d3f085d1d7e4f696
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199425"
---
# <a name="alter-user-transact-sql"></a>ALTER USER (Transact-SQL)

Rinomina un utente del database oppure ne modifica lo schema predefinito.

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Database SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server

ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
```

## <a name="arguments"></a>Argomenti

 *userName* specifica il nome con cui viene identificato l'utente all'interno del database.

 LOGIN **=** _loginName_ modifica il mapping di un utente associandolo a un altro account di accesso, modificando l'ID di sicurezza (SID) dell'utente in modo che corrisponda all'ID di sicurezza dell'account di accesso.

 NAME **=** _newUserName_ specifica il nuovo nome dell'utente. *newUserName* non deve essere già presente nel database corrente.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } specifica il primo schema nel quale il server eseguirà una ricerca durante la risoluzione dei nomi di oggetti per l'utente. L'impostazione dello schema predefinito su NULL comporta la rimozione di uno schema predefinito da un gruppo di Windows. Non è possibile usare NULL con un utente di Windows.

 PASSWORD **=** '*password*'  **si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Specifica la password per l'utente che viene modificato. Per le password viene fatta distinzione tra maiuscole e minuscole.

> [!NOTE]
> Questa opzione è disponibile solo per gli utenti contenuti. Per altre informazioni, vedere [Database indipendenti](../../relational-databases/databases/contained-databases.md) e [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Password dell'utente corrente che verrà sostituita da '*password*'. Per le password viene fatta distinzione tra maiuscole e minuscole. *OLD_PASSWORD* è necessaria per modificare una password, a meno che non si disponga dell'autorizzazione **ALTER ANY USER**. Si richiede *OLD_PASSWORD* per impedire agli utenti con autorizzazione **IMPERSONATION** di modificare la password.

> [!NOTE]
> Questa opzione è disponibile solo per gli utenti contenuti.

 DEFAULT_LANGUAGE **=** _{ NONE \| \<lcid> \| \<language name> \| \<language alias> }_ **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.

 Specifica una lingua predefinita da assegnare all'utente. Se questa opzione è impostata su NONE, la lingua predefinita viene impostata sulla lingua predefinita corrente del database. Se la lingua predefinita del database viene modificata in seguito, la lingua predefinita dell'utente rimarrà invariata. *DEFAULT_LANGUAGE* può essere l'ID locale (lcid), il nome della lingua o l'alias della lingua.

> [!NOTE]
> È possibile specificare questa opzione solo in un database indipendente e solo per utenti contenuti.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Elimina i controlli sui metadati di crittografia nel server nelle operazioni di copia bulk. Ciò consente all'utente di eseguire la copia bulk dei dati crittografati tra tabelle o database senza decrittografare i dati. Il valore predefinito è OFF.

> [!WARNING]
> L'uso improprio di questa opzione può causare il danneggiamento dei dati. Per altre informazioni, vedere [Migrare dati sensibili protetti da Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Osservazioni

 Lo schema predefinito sarà il primo schema in cui verrà eseguita la ricerca nel server durante la risoluzione dei nomi di oggetti per l'utente del database. Se non specificato diversamente, lo schema predefinito sarà il proprietario degli oggetti creati dall'utente del database.

 Se l'utente ha uno schema predefinito, verrà usato lo schema predefinito. Se l'utente non ha uno schema predefinito ma è membro di un gruppo che ha uno schema predefinito, verrà usato lo schema predefinito del gruppo. Se l'utente non ha uno schema predefinito ed è membro di più di un gruppo, lo schema predefinito per l'utente sarà quello del gruppo di Windows con il valore principal_id più basso e uno schema predefinito impostato in modo esplicito. Se non possono essere determinati schemi predefiniti per un utente, viene usato lo schema **dbo**.

 DEFAULT_SCHEMA può essere impostato su uno schema che attualmente non è presente nel database. È pertanto possibile assegnare uno schema predefinito tramite DEFAULT_SCHEMA a un utente prima della creazione dello schema.

 Non è possibile specificare DEFAULT_SCHEMA per un utente di cui è stato eseguito il mapping a un certificato o a una chiave asimmetrica.

> [!IMPORTANT]
> Il valore di DEFAULT_SCHEMA viene ignorato se l'utente è un membro del ruolo predefinito del server **sysadmin**. Tutti i membri del ruolo predefinito del server **sysadmin** dispongono di uno schema predefinito di `dbo`.

 È possibile modificare il nome di un utente sul quale viene eseguito il mapping a un account di accesso o a un gruppo di Windows solo se il SID del nuovo nome utente corrisponde al SID registrato nel database. Questa verifica consente di impedire lo spoofing degli account di accesso di Windows nel database.

 La clausola WITH LOGIN consente di modificare il mapping di un utente associandolo a un account di accesso diverso. Non è possibile modificare con questa clausola il mapping degli utenti che non hanno un account di accesso o degli utenti con mapping a un certificato o a una chiave asimmetrica. È possibile modificare il mapping solo di utenti di SQL e di utenti (o gruppi) di Windows. Non è possibile usare la clausola WITH LOGIN per modificare il tipo di utente, ad esempio per modificare un account di Windows in un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Se vengono soddisfatte le seguenti condizioni, il nome dell'utente viene automaticamente modificando nel nome dell'account di accesso.

- L'utente è un utente di Windows.

- Il nome è un nome di Windows (contiene una barra rovesciata).

- Non è stato specificato alcun nome.

- Il nome corrente è diverso dal nome dell'account di accesso.

 In caso contrario, l'utente non verrà rinominato, a meno che il chiamante non richiami anche la clausola NAME.

Il nome di un utente di cui è stato eseguito il mapping a un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificato o una chiave asimmetrica non può contenere il carattere barra rovesciata (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Security

> [!NOTE]
> Un utente con autorizzazione **ALTER ANY USER** può modificare lo schema predefinito di qualsiasi utente. È possibile che un utente con uno schema modificato selezioni involontariamente i dati dalla tabella errata o esegua codice dallo schema errato.

### <a name="permissions"></a>Autorizzazioni

 Per modificare il nome di un utente, è necessaria l'autorizzazione **ALTER ANY USER**.

 Per modificare l'account di accesso di destinazione di un utente, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare il nome di un utente con autorizzazione **CONTROL** per il database, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare la lingua o lo schema predefinito, è necessaria l'autorizzazione **ALTER** per l'utente. Gli utenti possono modificare il proprio schema predefinito o la lingua.

## <a name="examples"></a>Esempi

Tutti gli esempi vengono eseguiti in un database utente.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modifica del nome di un utente del database

 Nell'esempio seguente il nome dell'utente del database `Mary5` viene modificato in `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modifica dello schema predefinito di un utente

 Nell'esempio seguente lo schema predefinito dell'utente `Mary51` viene modificato in `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Modifica contemporanea di diverse opzioni

 Nell'esempio seguente vengono modificate diverse opzioni per un utente del database indipendente in un'istruzione.

**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

## <a name="see-also"></a>Vedere anche

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Database indipendenti](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Database SQL \*_**
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>Database SQL

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for Azure SQL Database

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = schemaName
| LOGIN = loginName
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
[;]

-- Azure SQL Database Update Syntax
ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- SQL Database syntax when connected to a federation member
ALTER USER userName
 WITH <set_item> [ ,... n ]
[;]

<set_item> ::=
 NAME = newUserName
```

## <a name="arguments"></a>Argomenti

 *userName* specifica il nome con cui viene identificato l'utente all'interno del database.

 LOGIN **=** _loginName_ modifica il mapping di un utente associandolo a un altro account di accesso, modificando l'ID di sicurezza (SID) dell'utente in modo che corrisponda all'ID di sicurezza dell'account di accesso.

 Se l'istruzione ALTER USER è l'unica istruzione in un batch SQL, il database SQL di Azure supporta la clausola WITH LOGIN. Se l'istruzione ALTER USER non è l'unica istruzione in un batch SQL o viene eseguita in SQL dinamico, la clausola WITH LOGIN non è supportata.

 NAME **=** _newUserName_ specifica il nuovo nome dell'utente. *newUserName* non deve essere già presente nel database corrente.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } specifica il primo schema nel quale il server eseguirà una ricerca durante la risoluzione dei nomi di oggetti per l'utente. L'impostazione dello schema predefinito su NULL comporta la rimozione di uno schema predefinito da un gruppo di Windows. L'opzione NULL non può essere usata con un utente di Windows.

 PASSWORD **=** '*password*'  **si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Specifica la password per l'utente che viene modificato. Per le password viene fatta distinzione tra maiuscole e minuscole.

> [!NOTE]
> Questa opzione è disponibile solo per gli utenti contenuti. Per altre informazioni, vedere [Database indipendenti](../../relational-databases/databases/contained-databases.md) e [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Password dell'utente corrente che verrà sostituita da '*password*'. Per le password viene fatta distinzione tra maiuscole e minuscole. *OLD_PASSWORD* è necessaria per modificare una password, a meno che non si disponga dell'autorizzazione **ALTER ANY USER**. Si richiede *OLD_PASSWORD* per impedire agli utenti con autorizzazione **IMPERSONATION** di modificare la password.

> [!NOTE]
> Questa opzione è disponibile solo per gli utenti contenuti.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Elimina i controlli sui metadati di crittografia nel server nelle operazioni di copia bulk. Ciò consente all'utente di eseguire la copia bulk dei dati crittografati tra tabelle o database senza decrittografare i dati. Il valore predefinito è OFF.

> [!WARNING]
> L'uso improprio di questa opzione può causare il danneggiamento dei dati. Per altre informazioni, vedere [Migrare dati sensibili protetti da Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Osservazioni

 Lo schema predefinito sarà il primo schema in cui verrà eseguita la ricerca nel server durante la risoluzione dei nomi di oggetti per l'utente del database. Se non specificato diversamente, lo schema predefinito sarà il proprietario degli oggetti creati dall'utente del database.

 Se l'utente dispone di uno schema predefinito, verrà utilizzato tale schema. Se l'utente non ha uno schema predefinito ma è membro di un gruppo che ha uno schema predefinito, verrà usato lo schema predefinito del gruppo. Se l'utente non ha uno schema predefinito ed è membro di più di un gruppo, lo schema predefinito per l'utente sarà quello del gruppo di Windows con il valore principal_id più basso e uno schema predefinito impostato in modo esplicito. Se non possono essere determinati schemi predefiniti per un utente, viene usato lo schema **dbo**.

 DEFAULT_SCHEMA può essere impostato su uno schema che attualmente non è presente nel database. È pertanto possibile assegnare uno schema predefinito tramite DEFAULT_SCHEMA a un utente prima della creazione dello schema.

 Non è possibile specificare DEFAULT_SCHEMA per un utente di cui è stato eseguito il mapping a un certificato o a una chiave asimmetrica.

> [!IMPORTANT]
> Il valore di DEFAULT_SCHEMA viene ignorato se l'utente è un membro del ruolo predefinito del server **sysadmin**. Tutti i membri del ruolo predefinito del server **sysadmin** dispongono di uno schema predefinito di `dbo`.

 È possibile modificare il nome di un utente sul quale viene eseguito il mapping a un account di accesso o a un gruppo di Windows solo se il SID del nuovo nome utente corrisponde al SID registrato nel database. Questa verifica consente di impedire lo spoofing degli account di accesso di Windows nel database.

 La clausola WITH LOGIN consente di modificare il mapping di un utente associandolo a un account di accesso diverso. Non è possibile modificare con questa clausola il mapping degli utenti che non hanno un account di accesso o degli utenti con mapping a un certificato o a una chiave asimmetrica. È possibile modificare il mapping solo di utenti di SQL e di utenti (o gruppi) di Windows. Non è possibile usare la clausola WITH LOGIN per modificare il tipo di utente, ad esempio per modificare un account di Windows in un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Se vengono soddisfatte le seguenti condizioni, il nome dell'utente viene automaticamente modificando nel nome dell'account di accesso.

- L'utente è un utente di Windows.

- Il nome è un nome di Windows (contiene una barra rovesciata).

- Non è stato specificato alcun nome.

- Il nome corrente è diverso dal nome dell'account di accesso.

 In caso contrario, l'utente non verrà rinominato, a meno che il chiamante non richiami anche la clausola NAME.

Il nome di un utente di cui è stato eseguito il mapping a un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificato o una chiave asimmetrica non può contenere il carattere barra rovesciata (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Security

> [!NOTE]
> Un utente con autorizzazione **ALTER ANY USER** può modificare lo schema predefinito di qualsiasi utente. È possibile che un utente con uno schema modificato selezioni involontariamente i dati dalla tabella errata o esegua codice dallo schema errato.

### <a name="permissions"></a>Autorizzazioni

 Per modificare il nome di un utente, è necessaria l'autorizzazione **ALTER ANY USER**.

 Per modificare l'account di accesso di destinazione di un utente, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare il nome di un utente con autorizzazione **CONTROL** per il database, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare la lingua o lo schema predefinito, è necessaria l'autorizzazione **ALTER** per l'utente. Gli utenti possono modificare il proprio schema predefinito o la lingua.

## <a name="examples"></a>Esempi

Tutti gli esempi vengono eseguiti in un database utente.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modifica del nome di un utente del database

 Nell'esempio seguente il nome dell'utente del database `Mary5` viene modificato in `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modifica dello schema predefinito di un utente

 Nell'esempio seguente lo schema predefinito dell'utente `Mary51` viene modificato in `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Modifica contemporanea di diverse opzioni

 Nell'esempio seguente vengono modificate diverse opzioni per un utente del database indipendente in un'istruzione.

```sql
ALTER USER Philip
WITHNAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per';
GO
```

## <a name="see-also"></a>Vedere anche

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Database indipendenti](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Istanza gestita di SQL \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Istanza gestita di SQL di Azure

## <a name="syntax"></a>Sintassi

> [!IMPORTANT]
> Se applicate a utenti con account di accesso di Azure AD, per Istanza gestita di SQL di Azure sono supportate solo le opzioni seguenti: `DEFAULT_SCHEMA = { schemaName | NULL }` e `DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }`
> </br> </br> È disponibile una nuova estensione della sintassi aggiunta per semplificare l'esecuzione di un nuovo mapping degli utenti in un database di cui è stata eseguita la migrazione a Istanza gestita di SQL di Azure. La sintassi ALTER USER consente di eseguire il mapping degli utenti di database in un dominio federato e sincronizzato con Azure AD ad account di accesso Azure AD.

```syntaxsql
-- Syntax for SQL Managed Instance
ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- Users or groups that are migrated as federated and synchronized with Azure AD have the following syntax:

/** Applies to Windows users that were migrated and have the following user names:
- Windows user <domain\user>
- Windows group <domain\MyWindowsGroup>
- Windows alias <MyWindowsAlias>
**/

ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
 NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
```

## <a name="arguments"></a>Argomenti

 *userName* specifica il nome con cui viene identificato l'utente all'interno del database.

 LOGIN **=** _loginName_ modifica il mapping di un utente associandolo a un altro account di accesso, modificando l'ID di sicurezza (SID) dell'utente in modo che corrisponda all'ID di sicurezza dell'account di accesso.

 Se l'istruzione ALTER USER è l'unica istruzione in un batch SQL, il database SQL di Azure supporta la clausola WITH LOGIN. Se l'istruzione ALTER USER non è l'unica istruzione in un batch SQL o viene eseguita in SQL dinamico, la clausola WITH LOGIN non è supportata.

 NAME **=** _newUserName_ specifica il nuovo nome dell'utente. *newUserName* non deve essere già presente nel database corrente.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } specifica il primo schema nel quale il server eseguirà una ricerca durante la risoluzione dei nomi di oggetti per l'utente. L'impostazione dello schema predefinito su NULL comporta la rimozione di uno schema predefinito da un gruppo di Windows. L'opzione NULL non può essere usata con un utente di Windows.

 PASSWORD **=** '*password*'

 Specifica la password per l'utente che viene modificato. Per le password viene fatta distinzione tra maiuscole e minuscole.

> [!NOTE]
> Questa opzione è disponibile solo per gli utenti contenuti. Per altre informazioni, vedere [Database indipendenti](../../relational-databases/databases/contained-databases.md) e [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_

 Password dell'utente corrente che verrà sostituita da '*password*'. Per le password viene fatta distinzione tra maiuscole e minuscole. *OLD_PASSWORD* è necessaria per modificare una password, a meno che non si disponga dell'autorizzazione **ALTER ANY USER**. Si richiede *OLD_PASSWORD* per impedire agli utenti con autorizzazione **IMPERSONATION** di modificare la password.

> [!NOTE]
> Questa opzione è disponibile solo per gli utenti contenuti.

 DEFAULT_LANGUAGE **=** _{ NONE | \<lcid> | \<language name> | \<language alias> }_

 Specifica una lingua predefinita da assegnare all'utente. Se questa opzione è impostata su NONE, la lingua predefinita viene impostata sulla lingua predefinita corrente del database. Se la lingua predefinita del database viene modificata in seguito, la lingua predefinita dell'utente rimarrà invariata. *DEFAULT_LANGUAGE* può essere l'ID locale (lcid), il nome della lingua o l'alias della lingua.

> [!NOTE]
> È possibile specificare questa opzione solo in un database indipendente e solo per utenti contenuti.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]

 Elimina i controlli sui metadati di crittografia nel server nelle operazioni di copia bulk. Ciò consente all'utente di eseguire la copia bulk dei dati crittografati tra tabelle o database senza decrittografare i dati. Il valore predefinito è OFF.

> [!WARNING]
> L'uso improprio di questa opzione può causare il danneggiamento dei dati. Per altre informazioni, vedere [Migrare dati sensibili protetti da Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Osservazioni

 Lo schema predefinito sarà il primo schema in cui verrà eseguita la ricerca nel server durante la risoluzione dei nomi di oggetti per l'utente del database. Se non specificato diversamente, lo schema predefinito sarà il proprietario degli oggetti creati dall'utente del database.

 Se l'utente dispone di uno schema predefinito, verrà utilizzato tale schema. Se l'utente non ha uno schema predefinito ma è membro di un gruppo che ha uno schema predefinito, verrà usato lo schema predefinito del gruppo. Se l'utente non ha uno schema predefinito ed è membro di più di un gruppo, lo schema predefinito per l'utente sarà quello del gruppo di Windows con il valore principal_id più basso e uno schema predefinito impostato in modo esplicito. Se non possono essere determinati schemi predefiniti per un utente, viene usato lo schema **dbo**.

 DEFAULT_SCHEMA può essere impostato su uno schema che attualmente non è presente nel database. È pertanto possibile assegnare uno schema predefinito tramite DEFAULT_SCHEMA a un utente prima della creazione dello schema.

 Non è possibile specificare DEFAULT_SCHEMA per un utente di cui è stato eseguito il mapping a un certificato o a una chiave asimmetrica.

> [!IMPORTANT]
> Il valore di DEFAULT_SCHEMA viene ignorato se l'utente è un membro del ruolo predefinito del server **sysadmin**. Tutti i membri del ruolo predefinito del server **sysadmin** dispongono di uno schema predefinito di `dbo`.

 È possibile modificare il nome di un utente sul quale viene eseguito il mapping a un account di accesso o a un gruppo di Windows solo se il SID del nuovo nome utente corrisponde al SID registrato nel database. Questa verifica consente di impedire lo spoofing degli account di accesso di Windows nel database.

 La clausola WITH LOGIN consente di modificare il mapping di un utente associandolo a un account di accesso diverso. Non è possibile modificare con questa clausola il mapping degli utenti che non hanno un account di accesso o degli utenti con mapping a un certificato o a una chiave asimmetrica. È possibile modificare il mapping solo di utenti di SQL e di utenti (o gruppi) di Windows. Non è possibile usare la clausola WITH LOGIN per modificare il tipo di utente, ad esempio per modificare un account di Windows in un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'unica eccezione si verifica quando si modifica un utente di Windows in un utente di Azure AD.

> [!NOTE]
> Le regole seguenti non si applicano agli utenti di Windows in Istanza gestita di SQL di Azure perché non è supportata la creazione di account di accesso di Windows in Istanza gestita di SQL di Azure. È possibile usare l'opzione WITH LOGIN solo se sono presenti account di accesso Azure AD.

 Se vengono soddisfatte le seguenti condizioni, il nome dell'utente viene automaticamente modificando nel nome dell'account di accesso.

- L'utente è un utente di Windows.

- Il nome è un nome di Windows (contiene una barra rovesciata).

- Non è stato specificato alcun nome.

- Il nome corrente è diverso dal nome dell'account di accesso.

 In caso contrario, l'utente non verrà rinominato, a meno che il chiamante non richiami anche la clausola NAME.

Il nome di un utente di cui è stato eseguito il mapping a un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificato o una chiave asimmetrica non può contenere il carattere barra rovesciata (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

### <a name="remarks-for-windows-users-in-sql-on-premises-migrated-to-azure-sql-managed-instance"></a>Osservazioni per gli utenti di Windows in SQL locale di cui è stata eseguita la migrazione a Istanza gestita di SQL di Azure

Queste note si applicano all'autenticazione come utenti di Windows che sono stati federati e sincronizzati con Azure AD.

> [!NOTE]
> L'amministratore di Azure AD per la funzionalità Istanza gestita di SQL di Azure dopo la creazione è stato modificato. Per altre informazioni, vedere [Nuove funzionalità di amministrazione di Azure AD per l'istanza gestita](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

- La convalida degli utenti o dei gruppi di Windows di cui è stato eseguito il mapping ad Azure AD viene eseguita per impostazione predefinita tramite API Graph in tutte le versioni della sintassi ALTER USER usata per la migrazione.
- Gli utenti locali con alias (usare un nome diverso dall'account di Windows originale) manterranno il nome con alias.
- Per l'autenticazione di Azure AD, il parametro LOGIN si applica solo a Istanza gestita di SQL di Azure e non può essere usato con il database SQL.
- Per visualizzare gli account di accesso per le entità di sicurezza di Azure AD, usare il comando seguente: `select * from sys.server_principals`.
- Controllare che il tipo indicato dell'account di accesso sia `E` o `X`.
- Non è possibile usare l'opzione PASSWORD per gli utenti di Azure AD.
- In tutti i casi di migrazione, i ruoli e le autorizzazioni degli utenti o dei gruppi di Windows verranno trasferiti automaticamente nei nuovi utenti o gruppi di Azure AD.
- È disponibile una nuova estensione della sintassi, **FROM EXTERNAL PROVIDER**, per la modifica di utenti e gruppi di Windows da SQL locale a utenti e gruppi di Azure AD. Il dominio Windows deve essere federato con Azure AD e tutti i membri del dominio Windows devono esistere in Azure AD quando si usa questa estensione. La sintassi **FROM EXTERNAL PROVIDER** si applica a Istanza gestita di SQL di Azure e deve essere usata nel caso in cui gli utenti di Windows non abbiano account di accesso nell'istanza di SQL originale e sia necessario eseguirne il mapping a utenti di database Azure AD autonomi.
- In questo caso il valore userName consentito può essere:
- Un utente di Windows (_domain\user_).
- Un gruppo di Windows (_MyWindowsGroup_).
- Un alias di Windows (_MyWindowsAlias_).
- Il risultato del comando ALTER sostituisce il valore userName precedente con il nome corrispondente trovato in Azure AD in base all'ID di sicurezza (SID) originale del valore userName precedente. Il nome modificato viene sostituito e archiviato nei metadati del database:
- (_domain\user_) verrà sostituito con user@domain.com di Azure AD.
- (_domain\\MyWidnowsGroup_) verrà sostituito con il gruppo di Azure AD.
- (_MyWindowsAlias_) rimarrà invariato, ma l'ID di sicurezza (SID) di questo utente verrà verificato in Azure AD.

> [!NOTE]
> Se l'ID di sicurezza (SID) dell'utente originale convertito in objectID non viene trovato in Azure AD, il comando ALTER USER avrà esito negativo.

- Per visualizzare gli utenti modificati, usare il comando seguente: `select * from sys.database_principals`
- Controllare che il tipo indicato dell'utente sia `E` o `X`.
- Quando NAME viene usato per eseguire la migrazione degli utenti di Windows a utenti di Azure AD, si applicano le restrizioni seguenti:
- È necessario specificare un valore LOGIN valido.
- NAME viene verificato in Azure AD e può essere solo:
- Il nome di LOGIN.
- Un alias (il nome non può esistere in Azure AD).
- In tutti gli altri casi la sintassi causerà un errore.

## <a name="security"></a>Security

> [!NOTE]
> Un utente con autorizzazione **ALTER ANY USER** può modificare lo schema predefinito di qualsiasi utente. È possibile che un utente con uno schema modificato selezioni involontariamente i dati dalla tabella errata o esegua codice dallo schema errato.

### <a name="permissions"></a>Autorizzazioni

 Per modificare il nome di un utente, è necessaria l'autorizzazione **ALTER ANY USER**.

 Per modificare l'account di accesso di destinazione di un utente, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare il nome di un utente con autorizzazione **CONTROL** per il database, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare la lingua o lo schema predefinito, è necessaria l'autorizzazione **ALTER** per l'utente. Gli utenti possono modificare il proprio schema predefinito o la lingua.

## <a name="examples"></a>Esempi

Tutti gli esempi vengono eseguiti in un database utente.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modifica del nome di un utente del database

 Nell'esempio seguente il nome dell'utente del database `Mary5` viene modificato in `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modifica dello schema predefinito di un utente

 Nell'esempio seguente lo schema predefinito dell'utente `Mary51` viene modificato in `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Modifica contemporanea di diverse opzioni

 Nell'esempio seguente vengono modificate diverse opzioni per un utente del database indipendente in un'istruzione.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

### <a name="d-map-the-user-in-the-database-to-an-azure-ad-login-after-migration"></a>D. Eseguire il mapping dell'utente nel database a un account di accesso Azure AD dopo la migrazione

L'esempio seguente riesegue il mapping dell'utente `westus/joe` a un utente di Azure AD `joe@westus.com`. Questo esempio riguarda gli account di accesso che esistono già nell'istanza gestita. Questa operazione deve essere eseguita dopo aver completato la migrazione di un database a Istanza gestita di SQL di Azure e quando si vuole usare l'account di accesso di Azure AD per l'autenticazione.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com
```

### <a name="e-map-an-old-windows-user-in-the-database-without-a-login-in-azure-sql-managed-instance-to-an-azure-ad-user"></a>E. Eseguire il mapping di un utente di Windows precedente nel database senza un account di accesso in Istanza gestita di SQL di Azure a un utente di Azure AD

L'esempio seguente riesegue il mapping dell'utente `westus/joe` senza un account di accesso a un utente di Azure AD `joe@westus.com`. L'utente federato deve esistere in Azure AD.

```sql
ALTER USER [westus/joe] FROM EXTERNAL PROVIDER
```

### <a name="f-map-the-user-alias-to-an-existing-azure-ad-login"></a>F. Eseguire il mapping dell'alias utente a un account di accesso Azure AD esistente

L'esempio seguente riesegue il mapping del nome utente `westus\joe` a `joe_alias`. In questo caso l'account di accesso Azure AD corrispondente è `joe@westus.com`.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com, name= joe_alias
```

### <a name="g-map-a-windows-group-that-was-migrated-in-azure-sql-managed-instance-to-an-azure-ad-group"></a>G. Eseguire il mapping di un gruppo di Windows di cui è stata eseguita la migrazione in Istanza gestita di SQL di Azure a un gruppo di Azure AD

L'esempio seguente riesegue il mapping del gruppo locale precedente `westus\mygroup` a un gruppo di Azure AD `mygroup` nell'istanza gestita. Il gruppo deve esistere in Azure AD.

```sql
ALTER USER [westus\mygroup] WITH LOGIN = mygroup
```

## <a name="see-also"></a>Vedere anche

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Database indipendenti](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)
- [Esercitazione: Migrazione di utenti e gruppi di Windows locali di SQL Server in Istanza gestita di SQL tramite la sintassi DDL T-SQL](/azure/sql-database/tutorial-managed-instance-azure-active-directory-migration)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for Azure Synapse

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Argomenti

 *userName* specifica il nome con cui viene identificato l'utente all'interno del database.

 LOGIN **=** _loginName_ modifica il mapping di un utente associandolo a un altro account di accesso, modificando l'ID di sicurezza (SID) dell'utente in modo che corrisponda all'ID di sicurezza dell'account di accesso.

 Se l'istruzione ALTER USER è l'unica istruzione in un batch SQL, il database SQL di Azure supporta la clausola WITH LOGIN. Se l'istruzione ALTER USER non è l'unica istruzione in un batch SQL o viene eseguita in SQL dinamico, la clausola WITH LOGIN non è supportata.

 NAME **=** _newUserName_ specifica il nuovo nome dell'utente. *newUserName* non deve essere già presente nel database corrente.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } specifica il primo schema nel quale il server eseguirà una ricerca durante la risoluzione dei nomi di oggetti per l'utente. L'impostazione dello schema predefinito su NULL comporta la rimozione di uno schema predefinito da un gruppo di Windows. L'opzione NULL non può essere usata con un utente di Windows.

## <a name="remarks"></a>Osservazioni

 Lo schema predefinito sarà il primo schema in cui verrà eseguita la ricerca nel server durante la risoluzione dei nomi di oggetti per l'utente del database. Se non specificato diversamente, lo schema predefinito sarà il proprietario degli oggetti creati dall'utente del database.

 Se l'utente dispone di uno schema predefinito, verrà utilizzato tale schema. Se l'utente non ha uno schema predefinito ma è membro di un gruppo che ha uno schema predefinito, verrà usato lo schema predefinito del gruppo. Se l'utente non ha uno schema predefinito ed è membro di più di un gruppo, lo schema predefinito per l'utente sarà quello del gruppo di Windows con il valore principal_id più basso e uno schema predefinito impostato in modo esplicito. Se non possono essere determinati schemi predefiniti per un utente, viene usato lo schema **dbo**.

 DEFAULT_SCHEMA può essere impostato su uno schema che attualmente non è presente nel database. È pertanto possibile assegnare uno schema predefinito tramite DEFAULT_SCHEMA a un utente prima della creazione dello schema.

 Non è possibile specificare DEFAULT_SCHEMA per un utente di cui è stato eseguito il mapping a un certificato o a una chiave asimmetrica.

> [!IMPORTANT]
> Il valore di DEFAULT_SCHEMA viene ignorato se l'utente è un membro del ruolo predefinito del server **sysadmin**. Tutti i membri del ruolo predefinito del server **sysadmin** dispongono di uno schema predefinito di `dbo`.

 La clausola WITH LOGIN consente di modificare il mapping di un utente associandolo a un account di accesso diverso. Non è possibile modificare con questa clausola il mapping degli utenti che non hanno un account di accesso o degli utenti con mapping a un certificato o a una chiave asimmetrica. È possibile modificare il mapping solo di utenti di SQL e di utenti (o gruppi) di Windows. Non è possibile usare la clausola WITH LOGIN per modificare il tipo di utente, ad esempio per modificare un account di Windows in un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Se vengono soddisfatte le seguenti condizioni, il nome dell'utente viene automaticamente modificando nel nome dell'account di accesso.

- Non è stato specificato alcun nome.

- Il nome corrente è diverso dal nome dell'account di accesso.

 In caso contrario, l'utente non verrà rinominato, a meno che il chiamante non richiami anche la clausola NAME.

Il nome di un utente di cui è stato eseguito il mapping a un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificato o una chiave asimmetrica non può contenere il carattere barra rovesciata (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Security

> [!NOTE]
> Un utente con autorizzazione **ALTER ANY USER** può modificare lo schema predefinito di qualsiasi utente. È possibile che un utente con uno schema modificato selezioni involontariamente i dati dalla tabella errata o esegua codice dallo schema errato.

### <a name="permissions"></a>Autorizzazioni

 Per modificare il nome di un utente, è necessaria l'autorizzazione **ALTER ANY USER**.

 Per modificare l'account di accesso di destinazione di un utente, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare il nome di un utente con autorizzazione **CONTROL** per il database, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare la lingua o lo schema predefinito, è necessaria l'autorizzazione **ALTER** per l'utente. Gli utenti possono modificare il proprio schema predefinito o la lingua.

## <a name="examples"></a>Esempi

Tutti gli esempi vengono eseguiti in un database utente.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modifica del nome di un utente del database

 Nell'esempio seguente il nome dell'utente del database `Mary5` viene modificato in `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modifica dello schema predefinito di un utente

Nell'esempio seguente lo schema predefinito dell'utente `Mary51` viene modificato in `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Vedere anche

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Database indipendenti](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
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

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Argomenti

 *userName* specifica il nome con cui viene identificato l'utente all'interno del database.

 LOGIN **=** _loginName_ modifica il mapping di un utente associandolo a un altro account di accesso, modificando l'ID di sicurezza (SID) dell'utente in modo che corrisponda all'ID di sicurezza dell'account di accesso.

 Se l'istruzione ALTER USER è l'unica istruzione in un batch SQL, il database SQL di Azure supporta la clausola WITH LOGIN. Se l'istruzione ALTER USER non è l'unica istruzione in un batch SQL o viene eseguita in SQL dinamico, la clausola WITH LOGIN non è supportata.

 NAME **=** _newUserName_ specifica il nuovo nome dell'utente. *newUserName* non deve essere già presente nel database corrente.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } specifica il primo schema nel quale il server eseguirà una ricerca durante la risoluzione dei nomi di oggetti per l'utente. L'impostazione dello schema predefinito su NULL comporta la rimozione di uno schema predefinito da un gruppo di Windows. L'opzione NULL non può essere usata con un utente di Windows.

## <a name="remarks"></a>Osservazioni

 Lo schema predefinito sarà il primo schema in cui verrà eseguita la ricerca nel server durante la risoluzione dei nomi di oggetti per l'utente del database. Se non specificato diversamente, lo schema predefinito sarà il proprietario degli oggetti creati dall'utente del database.

 Se l'utente dispone di uno schema predefinito, verrà utilizzato tale schema. Se l'utente non ha uno schema predefinito ma è membro di un gruppo che ha uno schema predefinito, verrà usato lo schema predefinito del gruppo. Se l'utente non ha uno schema predefinito ed è membro di più di un gruppo, lo schema predefinito per l'utente sarà quello del gruppo di Windows con il valore principal_id più basso e uno schema predefinito impostato in modo esplicito. Se non possono essere determinati schemi predefiniti per un utente, viene usato lo schema **dbo**.

 DEFAULT_SCHEMA può essere impostato su uno schema che attualmente non è presente nel database. È pertanto possibile assegnare uno schema predefinito tramite DEFAULT_SCHEMA a un utente prima della creazione dello schema.

 Non è possibile specificare DEFAULT_SCHEMA per un utente di cui è stato eseguito il mapping a un certificato o a una chiave asimmetrica.

> [!IMPORTANT]
> Il valore di DEFAULT_SCHEMA viene ignorato se l'utente è un membro del ruolo predefinito del server **sysadmin**. Tutti i membri del ruolo predefinito del server **sysadmin** dispongono di uno schema predefinito di `dbo`.

 La clausola WITH LOGIN consente di modificare il mapping di un utente associandolo a un account di accesso diverso. Non è possibile modificare con questa clausola il mapping degli utenti che non hanno un account di accesso o degli utenti con mapping a un certificato o a una chiave asimmetrica. È possibile modificare il mapping solo di utenti di SQL e di utenti (o gruppi) di Windows. Non è possibile usare la clausola WITH LOGIN per modificare il tipo di utente, ad esempio per modificare un account di Windows in un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Se vengono soddisfatte le seguenti condizioni, il nome dell'utente viene automaticamente modificando nel nome dell'account di accesso.

- Non è stato specificato alcun nome.

- Il nome corrente è diverso dal nome dell'account di accesso.

 In caso contrario, l'utente non verrà rinominato, a meno che il chiamante non richiami anche la clausola NAME.

Il nome di un utente di cui è stato eseguito il mapping a un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificato o una chiave asimmetrica non può contenere il carattere barra rovesciata (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Security

> [!NOTE]
> Un utente con autorizzazione **ALTER ANY USER** può modificare lo schema predefinito di qualsiasi utente. È possibile che un utente con uno schema modificato selezioni involontariamente i dati dalla tabella errata o esegua codice dallo schema errato.

### <a name="permissions"></a>Autorizzazioni

 Per modificare il nome di un utente, è necessaria l'autorizzazione **ALTER ANY USER**.

 Per modificare l'account di accesso di destinazione di un utente, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare il nome di un utente con autorizzazione **CONTROL** per il database, è necessaria l'autorizzazione **CONTROL** per il database.

 Per modificare la lingua o lo schema predefinito, è necessaria l'autorizzazione **ALTER** per l'utente. Gli utenti possono modificare il proprio schema predefinito o la lingua.

## <a name="examples"></a>Esempi

Tutti gli esempi vengono eseguiti in un database utente.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modifica del nome di un utente del database

 Nell'esempio seguente il nome dell'utente del database `Mary5` viene modificato in `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modifica dello schema predefinito di un utente
 Nell'esempio seguente lo schema predefinito dell'utente `Mary51` viene modificato in `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Vedere anche

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Database indipendenti](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end

---
description: ALTER AUTHORIZATION (Transact-SQL)
title: ALTER AUTHORIZATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/01/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AUTHORIZATION_TSQL
- ALTER AUTHORIZATION
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], transferring
- modifying entity ownership
- full-text search [SQL Server], permissions
- owners [SQL Server], entities
- ALTER AUTHORIZATION statement
- entity ownership [SQL Server]
- transferring ownership
- search property lists [SQL Server], permissions
- TAKE OWNERSHIP
ms.assetid: 8c805ae2-91ed-4133-96f6-9835c908f373
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95a1787412fa84c30ac0b14d670f94c72af10d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270508"
---
# <a name="alter-authorization-transact-sql"></a>ALTER AUTHORIZATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica la proprietà di un'entità a protezione diretta.

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server
ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
      OBJECT | ASSEMBLY | ASYMMETRIC KEY | AVAILABILITY GROUP | CERTIFICATE
    | CONTRACT | TYPE | DATABASE | ENDPOINT | FULLTEXT CATALOG
    | FULLTEXT STOPLIST | MESSAGE TYPE | REMOTE SERVICE BINDING
    | ROLE | ROUTE | SCHEMA | SEARCH PROPERTY LIST | SERVER ROLE
    | SERVICE | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for SQL Database

ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
    OBJECT | ASSEMBLY | ASYMMETRIC KEY | CERTIFICATE
     | TYPE | DATABASE | FULLTEXT CATALOG
     | FULLTEXT STOPLIST
     | ROLE | SCHEMA | SEARCH PROPERTY LIST
     | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

    <class_type> ::= {
    SCHEMA
     | OBJECT
    }

    <entity_name> ::=
    {
    schema_name
     | [ schema_name. ] object_name
    }
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::= {
    DATABASE
     | SCHEMA
     | OBJECT
    }

<entity_name> ::=
    {
    database_name
     | schema_name
     | [ schema_name. ] object_name
    }
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

\<class_type> è la classe dell'entità a protezione diretta per la quale viene modificato il proprietario. OBJECT è l'impostazione predefinita.

|Classe|Prodotto|
|-|-|
|OBJECT|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|ASSEMBLY|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|ASYMMETRIC KEY|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|AVAILABILITY GROUP |**SI APPLICA A**: SQL Server 2012 e versioni successive.|
|CERTIFICATE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|CONTRACT|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|
|DATABASE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Per ulteriori informazioni, vedere [ALTER AUTHORIZATION for Databases](#alter-authorization-for-databases).|
|ENDPOINT|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|
|FULLTEXT CATALOG|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|FULLTEXT STOPLIST|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|MESSAGE TYPE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|
|REMOTE SERVICE BINDING|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|
|ROLE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|ROUTE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|
|SCHEMA|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|SEARCH PROPERTY LIST|**SI APPLICA A**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|SERVER ROLE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|
|SERVICE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.|
|SYMMETRIC KEY|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|TYPE|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|XML SCHEMA COLLECTION|**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|

*entity_name* Nome dell'entità.

*principal_name* | Nome del proprietario dello SCHEMA dell'entità di sicurezza che diviene proprietaria dell'entità. Gli oggetti di database devono essere di proprietà di un'entità di database oppure di un utente o ruolo del database. Gli oggetti server, ad esempio i database, devono essere di proprietà di un'entità server (un account di accesso). Specificare il **proprietario dello schema** come * principal_name-per indicare che l'oggetto deve essere di proprietà dell'entità che possiede lo schema dell'oggetto.

## <a name="remarks"></a>Osservazioni

È possibile utilizzare l'istruzione ALTER AUTHORIZATION per modificare la proprietà di qualsiasi entità associata a un proprietario. La proprietà di entità incluse in un database può essere trasferita a qualsiasi entità a livello di database. La proprietà di entità a livello di server può essere trasferita solo a entità a livello di server.

> [!IMPORTANT]
> A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], un utente può essere proprietario di un elemento OBJECT o TYPE contenuto in uno schema di proprietà di un altro utente di database, diversamente da quanto consentito nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md) e [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md).

È possibile trasferire la proprietà delle seguenti entità incluse nello schema di tipo "object": tabelle, viste, funzioni, procedure, code e sinonimi.

Non è possibile trasferire la proprietà delle entità seguenti: server collegati, statistiche, vincoli, regole, valori predefiniti, trigger, code di [!INCLUDE[ssSB](../../includes/sssb-md.md)], credenziali, funzioni di partizione, schemi di partizione, chiavi master del database, chiavi master del servizio e notifiche di eventi.

Non è possibile trasferire la proprietà di membri delle classi a protezione diretta seguenti: server, account di accesso, utenti, ruoli applicazione e colonne.

L'opzione SCHEMA OWNER è valida solo in caso di trasferimento della proprietà di un'entità inclusa nello schema. L'opzione SCHEMA OWNER trasferirà la proprietà dell'entità al proprietario dello schema in cui si trova. Solo le entità della classe OBJECT, TYPE o XML SCHEMA COLLECTION sono incluse nello schema.

Se l'entità di destinazione non è un database e l'entità sta per essere trasferita a un nuovo proprietario, verranno eliminate tutte le autorizzazioni per la destinazione.

> [!CAUTION]
> Il comportamento degli schemi in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è diverso rispetto alle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile che il codice in cui gli schemi sono equivalenti agli utenti del database non restituisca risultati corretti. Le viste del catalogo precedenti, inclusa sysobjects, non devono essere usate in un database in cui è stata usata una delle istruzioni DDL seguenti: CREATE SCHEMA, ALTER SCHEMA, DROP SCHEMA, CREATE USER, ALTER USER, DROP USER, CREATE ROLE, ALTER ROLE, DROP ROLE, CREATE APPROLE, ALTER APPROLE, DROP APPROLE, ALTER AUTHORIZATION. In questi database è necessario usare le nuove viste del catalogo, in cui si tiene conto della separazione tra entità e schemi introdotta in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Per altre informazioni sulle viste del catalogo, vedere [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).

 Si noti inoltre quanto segue:

> [!IMPORTANT]
> L'unico modo affidabile per individuare il proprietario di un oggetto consiste nell'eseguire una query sulla vista del catalogo **sys.objects**. L'unico modo affidabile per cercare il proprietario di un tipo consiste nell'utilizzare la funzione TYPEPROPERTY.

## <a name="special-cases-and-conditions"></a>Casi e condizioni speciali

Nella tabella seguente sono elencati casi, eccezioni e condizioni speciali validi per la modifica delle autorizzazioni.

|Classe|Condizione|
|-----------|---------------|
|OBJECT|Non è possibile modificare la proprietà di trigger, vincoli, regole, valori predefiniti, statistiche, oggetti di sistema, code, viste indicizzate o tabelle con viste indicizzate.|
|SCHEMA|In caso di trasferimento della proprietà, verranno eliminate le autorizzazioni per gli oggetti inclusi nello schema che non dispongono di proprietari espliciti. Non è possibile modificare il proprietario di sys, dbo o information_schema.|
|TYPE|Non è possibile modificare la proprietà di una classe TYPE appartenente a sys o information_schema.|
|CONTRACT, MESSAGE TYPE o SERVICE|Non è possibile modificare la proprietà delle entità di sistema.|
|SYMMETRIC KEY|Non è possibile modificare la proprietà delle chiavi temporanee globali.|
|CERTIFICATE o ASYMMETRIC KEY|Non è possibile trasferire la proprietà di queste entità a un ruolo o gruppo.|
|ENDPOINT|L'entità deve essere un account di accesso.|

## <a name="alter-authorization-for-databases"></a> ALTER AUTHORIZATION per i database

### <a name="for-sql-server"></a>Per SQL Server

**Requisiti per il nuovo proprietario:** Il nuovo oggetto Principal proprietario deve essere uno dei seguenti:

- Un account di accesso con autenticazione di SQL Server.
- Un account di accesso con autenticazione di Windows che rappresenta un utente di Windows (non un gruppo).
- Un utente di Windows che esegue l'autenticazione usando un account di accesso con autenticazione di Windows che rappresenta un gruppo di Windows.

**Requisiti per la persona che esegue l'istruzione ALTER AUTHORIZATION:** Se non si è membri del ruolo predefinito del server **sysadmin** , è necessario disporre almeno dell'autorizzazione TAKE OWNERSHIP per il database e disporre dell'autorizzazione IMPERSONATE per il nuovo account di accesso proprietario.

### <a name="for-azure-sql-database"></a>Per il database SQL di Azure

**Requisiti per il nuovo proprietario:** Il nuovo oggetto Principal proprietario deve essere uno dei seguenti:

- Un account di accesso con autenticazione di SQL Server.
- Un utente federato (non un gruppo) presente in Azure AD.
- Un utente gestito (non un gruppo) o un'applicazione presente in Azure AD.

Se il nuovo proprietario è un utente di Azure Active Directory, non può esistere come utente nel database in cui il nuovo proprietario diventerà il nuovo proprietario del database. È necessario rimuovere l'utente di Azure AD dal database prima di eseguire l'istruzione ALTER AUTHORIZATION che modifica la proprietà del database impostandola sul nuovo utente. Per altre informazioni sulla configurazione di utenti di Azure Active Directory con il database SQL, vedere [Connessione al database SQL oppure a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] con l'autenticazione di Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/).

**Requisiti per la persona che esegue l'istruzione ALTER AUTHORIZATION:** Per modificare il proprietario del database, è necessario connettersi al database di destinazione.

Il proprietario di un database può essere modificato dai tipi seguenti di account.

- L'account di accesso dell'entità di livello servizio (l'amministratore di SQL Azure indicato durante la creazione del server di database SQL).
- L'amministratore di Azure Active Directory per il server SQL di Azure.
- Il proprietario corrente del database.

Nella tabella seguente vengono riepilogati i requisiti:

Esecutore  |Destinazione  |Risultato
---------|---------|---------
Account di accesso con autenticazione di SQL Server  |Account di accesso con autenticazione di SQL Server|Operazione completata
Account di accesso con autenticazione di SQL Server  |Utente di Azure AD|Esito negativo
Utente di Azure AD  |Account di accesso con autenticazione di SQL Server|Operazione completata
Utente di Azure AD  |Utente di Azure AD|Operazione completata

Per verificare un proprietario del database di Azure AD, eseguire il comando Transact-SQL seguente in un database utente (in questo esempio `testdb`).

```sql
SELECT CAST(owner_sid as uniqueidentifier) AS Owner_SID
FROM sys.databases
WHERE name = 'testdb';
```

L'output sarà un identificatore, ad esempio 6D8B81F6-7C79-444C-8858-4AF896C03C67, che corrisponde a Azure AD ObjectID assegnato a `richel@cqclinic.onmicrosoft.com` quando un utente di accesso con autenticazione SQL Server è il proprietario del database, eseguire l'istruzione seguente nel database master per verificare il proprietario del database:

```sql
SELECT d.name, d.owner_sid, sl.name
FROM sys.databases AS d
JOIN sys.sql_logins AS sl
ON d.owner_sid = sl.sid;

```

### <a name="best-practice"></a>Procedura consigliata

Anziché usare gli utenti di Azure AD come singoli proprietari del database, usare un gruppo di Azure AD come membro del ruolo predefinito del database **db_owner**. La procedura seguente illustra come configurare un account di accesso disabilitato come proprietario del database e trasformare un gruppo di Azure Active Directory (`mydbogroup`) in un membro del ruolo **db_owner**.

1. Accedere a SQL Server come amministratore di Azure AD e modificare il proprietario del database in un account di accesso con autenticazione di SQL Server disabilitato. Ad esempio, eseguire il comando seguente dal database utente:

   ```sql
   ALTER AUTHORIZATION ON database::testdb TO DisabledLogin;
   ```

1. Creare un gruppo di Azure AD che diventerà proprietario del database e aggiungerlo come utente al database utente. Ad esempio:

   ```sql
   CREATE USER [mydbogroup] FROM EXTERNAL PROVIDER;
   ```

1. Nel database utente aggiungere l'utente che rappresenta il gruppo di Azure AD al ruolo predefinito del database **db_owner**. Ad esempio:

   ```sql
   ALTER ROLE db_owner ADD MEMBER mydbogroup;
   ```

A questo punto i membri `mydbogroup` possono gestire centralmente il database come membri del ruolo **db_owner**.

- Quando i membri di questo gruppo vengono rimossi dal gruppo di Azure AD, perdono automaticamente le autorizzazioni dbo per il database.
- Analogamente, i nuovi membri aggiunti al gruppo di Azure Active Directory `mydbogroup`, ottengono l'accesso dbo per il database.

Per verificare se un utente specifico dispone dell'autorizzazione dbo valida, l'utente deve eseguire l'istruzione seguente:

```sql
SELECT IS_MEMBER ('db_owner');
```

Se il valore restituito è 1, significa che l'utente è un membro del ruolo.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione TAKE OWNERSHIP per l'entità. Se il nuovo proprietario non è l'utente che sta eseguendo l'istruzione 1) è richiesta l'autorizzazione IMPERSONATE per il nuovo proprietario se si tratta di un utente o un account di accesso, oppure 2) se il nuovo proprietario è un ruolo, è richiesta l'appartenenza al ruolo o l'autorizzazione ALTER per tale ruolo, oppure 3) se il nuovo proprietario è un ruolo applicazione, è richiesta l'autorizzazione ALTER per il ruolo applicazione.

## <a name="examples"></a>Esempi

### <a name="a-transfer-ownership-of-a-table"></a>R. Trasferire la proprietà di una tabella

Nell'esempio seguente la proprietà della tabella `Sprockets` viene trasferita all'utente `MichikoOsada`. La tabella è inclusa nello schema `Parts`.

```sql
ALTER AUTHORIZATION ON OBJECT::Parts.Sprockets TO MichikoOsada;
GO
```

La query potrebbe essere simile alla seguente:

```sql
ALTER AUTHORIZATION ON Parts.Sprockets TO MichikoOsada;
GO
```

Se lo schema degli oggetti non è incluso come parte dell'istruzione, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] cercherà l'oggetto nello schema predefinito degli utenti. Ad esempio:

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
```

### <a name="b-transfer-ownership-of-a-view-to-the-schema-owner"></a>B. Trasferire la proprietà di una vista al proprietario dello schema

Nell'esempio seguente la proprietà della vista `ProductionView06` viene trasferita al proprietario dello schema che la contiene. La vista è inclusa nello schema `Production`.

```sql
ALTER AUTHORIZATION ON OBJECT::Production.ProductionView06 TO SCHEMA OWNER;
GO
```

### <a name="c-transfer-ownership-of-a-schema-to-a-user"></a>C. Trasferire la proprietà di uno schema a un utente

Nell'esempio seguente la proprietà dello schema `SeattleProduction11` viene trasferita all'utente `SandraAlayo`.

```sql
ALTER AUTHORIZATION ON SCHEMA::SeattleProduction11 TO SandraAlayo;
GO
```

### <a name="d-transfer-ownership-of-an-endpoint-to-a-sql-server-login"></a>D. Trasferire la proprietà di un endpoint a un account di accesso di SQL Server

Nell'esempio seguente la proprietà dell'endpoint `CantabSalesServer1` viene trasferita a `JaePak`. Poiché è un'entità a protezione diretta a livello di server, l'endpoint può essere trasferito solo a un'entità a livello di server.

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

```sql
ALTER AUTHORIZATION ON ENDPOINT::CantabSalesServer1 TO JaePak;
GO
```

### <a name="e-changing-the-owner-of-a-table"></a>E. Modifica del proprietario di una tabella

In tutti gli esempi seguenti il proprietario della tabella `Sprockets` nel database `Parts` viene modificato nell'utente database `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON dbo.Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::dbo.Sprockets TO MichikoOsada;
```

### <a name="f-changing-the-owner-of-a-database"></a>F. Modifica del proprietario di un database

 **SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].

 Nell'esempio seguente il proprietario del database `Parts` viene modificato nell'account di accesso `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON DATABASE::Parts TO MichikoOsada;
```

### <a name="g-changing-the-owner-of-a-sql-database-to-an-azure-ad-user"></a>G. Modifica del proprietario di un database SQL in un utente di AD Azure

Nell'esempio seguente un amministratore di Azure Active Directory per SQL Server in un'organizzazione con un account di Active Directory denominato `cqclinic.onmicrosoft.com` può modificare la proprietà corrente di un database `targetDB` e impostare un utente AAD `richel@cqclinic.onmicorsoft.com` come nuovo proprietario del database usando il comando seguente:

```sql
ALTER AUTHORIZATION ON database::targetDB TO [rachel@cqclinic.onmicrosoft.com];
```

Azure AD richiede le parentesi quadre `[]` intorno al nome utente.

## <a name="see-also"></a>Vedere anche
 [OBJECTPROPERTY &#40;Transact-sql&#41;](../../t-sql/functions/objectproperty-transact-sql.md) [TypeProperty &#40;transact-sql&#41;](../../t-sql/functions/typeproperty-transact-sql.md) [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)


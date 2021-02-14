---
description: GRANT - autorizzazioni per database (Transact-SQL)
title: GRANT - autorizzazioni per database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server]
- database permissions [SQL Server]
- granting permissions [SQL Server], databases
- permissions [SQL Server], databases
- database permissions [SQL Server], granting
- GRANT statement, databases
ms.assetid: 499e5ed6-945c-4791-ab45-68dec0b9c289
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 185e4f53346d61ec3d53462fec3797b50f20d4c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338761"
---
# <a name="grant-database-permissions-transact-sql"></a>GRANT - autorizzazioni per database (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Concede le autorizzazioni per un database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql

GRANT <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ WITH GRANT OPTION ]
    [ AS <database_principal> ]

<permission>::=
permission | ALL [ PRIVILEGES ]

<database_principal> ::=
    Database_user
  | Database_role
  | Application_role
  | Database_user_mapped_to_Windows_User
  | Database_user_mapped_to_Windows_Group
  | Database_user_mapped_to_certificate
  | Database_user_mapped_to_asymmetric_key
  | Database_user_with_no_login
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*permission* specifica un'autorizzazione che può essere concessa per un database. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.

L'opzione ALL non concede tutte le autorizzazioni possibili. L'impostazione di ALL equivale a concedere le autorizzazioni seguenti: BACKUP DATABASE, BACKUP LOG, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE, e CREATE VIEW.

PRIVILEGES viene incluso per la conformità allo standard ANSI-92. Non modifica il funzionamento di ALL.

WITH GRANT OPTION indica che l'entità di sicurezza potrà anche concedere l'autorizzazione specificata ad altre entità di sicurezza.

AS \<database_principal>Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di concedere l'autorizzazione.

*Database_user* specifica un utente del database.

*Database_role* specifica un ruolo del database.

*Application_role*
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

Specifica un ruolo applicazione.

*Database_user_mapped_to_Windows_User*
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive

Specifica un utente del database sul quale viene eseguito il mapping a un utente di Windows.

*Database_user_mapped_to_Windows_Group*
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive

Specifica un utente del database sul quale viene eseguito il mapping a un gruppo di Windows.

*Database_user_mapped_to_certificate*
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive

Specifica un utente del database sul quale viene eseguito il mapping a un certificato.

*Database_user_mapped_to_asymmetric_key*
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive

Specifica un utente del database sul quale viene eseguito il mapping a una chiave asimmetrica.

*Database_user_with_no_login* specifica un utente del database senza entità di livello server corrispondente.

## <a name="remarks"></a>Osservazioni

> [!IMPORTANT]
> Una combinazione di autorizzazioni ALTER e REFERENCE potrebbe consentire in alcuni casi al beneficiario di visualizzare dati o eseguire funzioni non autorizzate. Ad esempio: un utente con autorizzazione ALTER per una tabella e autorizzazione REFERENCE per una funzione può creare una colonna calcolata su una funzione e determinarne l'esecuzione. In questo caso, è inoltre necessario disporre dell'autorizzazione SELECT per la colonna calcolata.

Un database è un'entità a protezione diretta contenuta nel server padre nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un database, insieme alle autorizzazioni più generali che le includono in modo implicito.

|Autorizzazione del database|Autorizzazione del database in cui è inclusa|Autorizzazione del server in cui è inclusa|
|-------------------------|------------------------------------|----------------------------------|
|ADMINISTER DATABASE BULK OPERATIONS<br/>**Si applica a:** [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER|CONTROL|ALTER ANY DATABASE|
|ALTER ANY APPLICATION ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ASSEMBLY|ALTER|CONTROL SERVER|
|ALTER ANY ASYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY CERTIFICATE|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN ENCRYPTION KEY|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN MASTER KEY DEFINITION|ALTER|CONTROL SERVER|
|ALTER ANY CONTRACT|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE AUDIT|ALTER|ALTER ANY SERVER AUDIT|
|ALTER ANY DATABASE DDL TRIGGER|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE EVENT NOTIFICATION|ALTER|ALTER ANY EVENT NOTIFICATION|
|ALTER ANY DATABASE EVENT SESSION<br />**Si applica a**: [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|ALTER|ALTER ANY EVENT SESSION|
|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY DATASPACE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL DATA SOURCE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL FILE FORMAT|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL LIBRARY <br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)].|CONTROL|CONTROL SERVER |
|ALTER ANY FULLTEXT CATALOG|ALTER|CONTROL SERVER|
|ALTER ANY MASK|CONTROL|CONTROL SERVER|
|ALTER ANY MESSAGE TYPE|ALTER|CONTROL SERVER|
|ALTER ANY REMOTE SERVICE BINDING|ALTER|CONTROL SERVER|
|ALTER ANY ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ROUTE|ALTER|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
|ALTER ANY SECURITY POLICY<br /> **Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY SERVICE|ALTER|CONTROL SERVER|
|ALTER ANY SYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY USER|ALTER|CONTROL SERVER|
|AUTHENTICATE|CONTROL|AUTHENTICATE SERVER|
|BACKUP DATABASE|CONTROL|CONTROL SERVER|
|BACKUP LOG|CONTROL|CONTROL SERVER|
|CHECKPOINT|CONTROL|CONTROL SERVER|
|CONNECT|CONNECT REPLICATION|CONTROL SERVER|
|CONNECT REPLICATION|CONTROL|CONTROL SERVER|
|CONTROL|CONTROL|CONTROL SERVER|
|CREATE AGGREGATE|ALTER|CONTROL SERVER|
|CREATE ANY EXTERNAL LIBRARY <br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)].|CONTROL|CONTROL SERVER |
|CREATE ASSEMBLY|ALTER ANY ASSEMBLY|CONTROL SERVER|
|CREATE ASYMMETRIC KEY|ALTER ANY ASYMMETRIC KEY|CONTROL SERVER|
|CREATE CERTIFICATE|ALTER ANY CERTIFICATE|CONTROL SERVER|
|CREATE CONTRACT|ALTER ANY CONTRACT|CONTROL SERVER|
|CREATE DATABASE|CONTROL|CREATE ANY DATABASE|
|CREATE DATABASE DDL EVENT NOTIFICATION|ALTER ANY DATABASE EVENT NOTIFICATION|CREATE DDL EVENT NOTIFICATION|
|CREATE DEFAULT|ALTER|CONTROL SERVER|
|CREATE FULLTEXT CATALOG|ALTER ANY FULLTEXT CATALOG|CONTROL SERVER|
|CREATE FUNCTION|ALTER|CONTROL SERVER|
|CREATE MESSAGE TYPE|ALTER ANY MESSAGE TYPE|CONTROL SERVER|
|CREATE PROCEDURE|ALTER|CONTROL SERVER|
|CREATE QUEUE|ALTER|CONTROL SERVER|
|CREATE REMOTE SERVICE BINDING|ALTER ANY REMOTE SERVICE BINDING|CONTROL SERVER|
|CREATE ROLE|ALTER ANY ROLE|CONTROL SERVER|
|CREATE ROUTE|ALTER ANY ROUTE|CONTROL SERVER|
|CREATE RULE|ALTER|CONTROL SERVER|
|CREATE SCHEMA|ALTER ANY SCHEMA|CONTROL SERVER|
|CREATE SERVICE|ALTER ANY SERVICE|CONTROL SERVER|
|CREATE SYMMETRIC KEY|ALTER ANY SYMMETRIC KEY|CONTROL SERVER|
|CREATE SYNONYM|ALTER|CONTROL SERVER|
|CREA TABELLA|ALTER|CONTROL SERVER|
|CREATE TYPE|ALTER|CONTROL SERVER|
|CREATE VIEW|ALTER|CONTROL SERVER|
|CREATE XML SCHEMA COLLECTION|ALTER|CONTROL SERVER|
|DELETE|CONTROL|CONTROL SERVER|
|EXECUTE|CONTROL|CONTROL SERVER|
|EXECUTE ANY EXTERNAL SCRIPT <br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)].|CONTROL|CONTROL SERVER|
|EXECUTE EXTERNAL SCRIPT <br /> **Si applica a**: [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].|EXECUTE ANY EXTERNAL SCRIPT|CONTROL SERVER|
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br />**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY COLUMN MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>Autorizzazioni

L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa.

Se si utilizza l'opzione AS, sono previsti i requisiti aggiuntivi seguenti.

|AS *granting_principal*|Autorizzazione aggiuntiva necessaria|
|------------------------------|------------------------------------|
|Utente del database|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|
|Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|
|Utente del database di cui è stato eseguito il mapping a un gruppo di Windows|Appartenenza al gruppo di Windows, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|
|Utente del database di cui è stato eseguito il mapping a un certificato|Appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|
|Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica|Appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|
|Utente del database di cui non è stato eseguito il mapping ad alcuna entità server|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|
|Ruolo del database|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|
|Ruolo applicazione|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|

I proprietari degli oggetti possono concedere autorizzazioni per gli oggetti di cui sono proprietari. Le entità con l'autorizzazione CONTROL per un'entità a protezione diretta possono concedere l'autorizzazione per quella entità.

Gli utenti che dispongono dell'autorizzazione CONTROL SERVER, ad esempio i membri del ruolo predefinito del server sysadmin, possono concedere qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel server.

## <a name="examples"></a>Esempi

### <a name="a-granting-permission-to-create-tables"></a>R. Concessione dell'autorizzazione per la creazione di tabelle

Nell'esempio seguente viene concessa l'autorizzazione `CREATE TABLE` per il database `AdventureWorks` all'utente `MelanieK`.

```sql
USE AdventureWorks;
GRANT CREATE TABLE TO MelanieK;
GO
```

### <a name="b-granting-showplan-permission-to-an-application-role"></a>B. Concessione dell'autorizzazione SHOWPLAN a un ruolo applicazione

 Nell'esempio seguente viene concessa l'autorizzazione `SHOWPLAN` per il database `AdventureWorks2012` al ruolo applicazione `AuditMonitor`.

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

```sql
USE AdventureWorks2012;
GRANT SHOWPLAN TO AuditMonitor;
GO
```

### <a name="c-granting-create-view-with-grant-option"></a>C. Concessione dell'autorizzazione CREATE VIEW con GRANT OPTION

 Nell'esempio seguente viene concessa l'autorizzazione `CREATE VIEW` per il database `AdventureWorks2012` all'utente `CarmineEs` con il diritto di concedere l'autorizzazione `CREATE VIEW` ad altre entità.

```sql
USE AdventureWorks2012;
GRANT CREATE VIEW TO CarmineEs WITH GRANT OPTION;
GO
```

### <a name="d-granting-control-permission-to-a-database-user"></a>D. Concessione dell'autorizzazione CONTROL a un utente del database

 Nell'esempio seguente viene concessa l'autorizzazione `CONTROL` per il database `AdventureWorks2012` all'utente del database `Sarah`. L'utente deve esistere nel database e il contesto deve essere impostato sul database.

```sql
USE AdventureWorks2012;
GRANT CONTROL ON DATABASE::AdventureWorks2012 TO Sarah;
GO
```

## <a name="see-also"></a>Vedere anche

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [Autorizzazioni](../../relational-databases/security/permissions-database-engine.md)
- [Entità](../../relational-databases/security/authentication-access/principals-database-engine.md)

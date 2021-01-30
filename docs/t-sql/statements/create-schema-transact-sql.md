---
description: CREATE SCHEMA (Transact-SQL)
title: CREATE SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SCHEMA_TSQL
- SCHEMA
- CREATE SCHEMA
- SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], schemas
- table creation [SQL Server], CREATE SCHEMA statement
- permissions [SQL Server], schemas
- schemas [SQL Server], creating
- CREATE SCHEMA statement
ms.assetid: df74fc36-20da-4efa-b412-c4e191786695
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 838a9c242b729d0fe0433aa9f155d5074f248c7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188525"
---
# <a name="create-schema-transact-sql"></a>CREATE SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crea uno schema nel database corrente. La transazione CREATE SCHEMA può anche creare tabelle e viste all'interno del nuovo schema e impostare le autorizzazioni GRANT, DENY o REVOKE per tali oggetti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE SCHEMA schema_name_clause [ <schema_element> [ ...n ] ]  
  
<schema_name_clause> ::=  
    {  
    schema_name  
    | AUTHORIZATION owner_name  
    | schema_name AUTHORIZATION owner_name  
    }  
  
<schema_element> ::=   
    {   
        table_definition | view_definition | grant_statement |   
        revoke_statement | deny_statement   
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE SCHEMA schema_name [ AUTHORIZATION owner_name ] [;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome con il quale viene identificato lo schema all'interno del database.  
  
 AUTHORIZATION *owner_name*  
 Specifica il nome dell'entità a livello di database che sarà proprietaria dello schema. L'entità può essere proprietaria di altri schemi e non può utilizzare lo schema corrente come schema predefinito.  
  
 *table_definition*  
 Specifica un'istruzione CREATE TABLE che crea una tabella all'interno dello schema. L'entità che esegue l'istruzione deve disporre dell'autorizzazione CREATE TABLE per il database corrente.  
  
 *view_definition*  
 Specifica un'istruzione CREATE VIEW che crea una vista all'interno dello schema. L'entità che esegue l'istruzione deve disporre dell'autorizzazione CREATE VIEW per il database corrente.  
  
 *grant_statement*  
 Specifica un'istruzione GRANT che concede le autorizzazioni per ogni entità a protezione diretta ad eccezione del nuovo schema.  
  
 *revoke_statement*  
 Specifica un'istruzione REVOKE che revoca le autorizzazioni per ogni entità a protezione diretta ad eccezione del nuovo schema.  
  
 *deny_statement*  
 Specifica un'istruzione DENY che nega le autorizzazioni per ogni entità a protezione diretta ad eccezione del nuovo schema.  
  
## <a name="remarks"></a>Osservazioni  
  
> [!NOTE]  
>  Le istruzioni che contengono CREATE SCHEMA AUTHORIZATION ma non specificano un nome sono supportate unicamente per compatibilità con le versioni precedenti. L'istruzione non causa un errore ma non crea uno schema.  
  
 CREATE SCHEMA consente di creare uno schema, le tabelle e le viste in esso contenute e concedere, revocare o negare le autorizzazioni per ogni entità a protezione diretta in una singola istruzione. Questa istruzione deve essere eseguita come batch separato. Gli oggetti creati tramite l'istruzione CREATE SCHEMA vengono creati all'interno dello schema di cui è in corso la creazione.  
  
 Le transazioni CREATE SCHEMA sono atomiche. In caso di errori durante l'esecuzione di un'istruzione CREATE SCHEMA, non verrà creata alcuna entità a protezione diretta e non verranno concesse autorizzazioni.  
  
 Le entità a protezione diretta da creare tramite CREATE SCHEMA possono essere elencate in qualsiasi ordine, ad eccezione delle viste che fanno riferimento ad altre viste. In questo caso, la vista a cui viene fatto riferimento deve essere creata prima della vista che vi fa riferimento.  
  
 Un'istruzione GRANT può pertanto concedere l'autorizzazione per un oggetto prima che l'oggetto stesso venga creato, mentre un'istruzione CREATE VIEW può essere visualizzata prima delle istruzioni CREATE TABLE che creano le tabelle a cui fa riferimento la vista. Le istruzioni CREATE TABLE, inoltre, possono dichiarare chiavi esterne per le tabelle definite successivamente nell'istruzione CREATE SCHEMA.  
  
> [!NOTE]  
>  Le clausole DENY e REVOKE sono supportate nelle istruzioni CREATE SCHEMA. Le clausole DENY e REVOKE vengono eseguite nell'ordine in cui compaiono nell'istruzione CREATE SCHEMA.  
  
 L'entità che esegue CREATE SCHEMA può specificare un'altra entità di database come proprietario dello schema di cui è in corso la creazione. A tale scopo, sono richieste ulteriori autorizzazioni, come illustrato nella sezione "Autorizzazioni" più avanti in questo argomento.  
  
 Il nuovo schema è di proprietà di una delle seguenti entità a livello di database: utente di database, ruolo di database o ruolo applicazione. Gli oggetti creati all'interno di uno schema appartengono al proprietario dello schema e hanno un valore NULL per **principal_id** in **sys.objects**. La proprietà degli oggetti contenuti in uno schema può essere trasferita a qualsiasi entità a livello di database, ma il proprietario dello schema mantiene sempre l'autorizzazione CONTROL per gli oggetti all'interno dello schema.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
 **Schema implicito e creazione di utenti**  
  
 In alcuni casi un utente può usare un database senza disporre di un account utente di database, ovvero un'entità database nel database. L'errore può verificarsi nelle situazioni seguenti:  
  
-   Un account di accesso dispone di privilegi **CONTROL SERVER**.  
  
-   Un utente di Windows non dispone di un account utente di database individuale (un'entità database nel database), ma accede a un database come membro di un gruppo di Windows che dispone di un account utente di database (un'entità database per il gruppo di Windows).  
  
 Quando un utente privo di account utente di database crea un oggetto senza specificare uno schema esistente, nel database saranno creati automaticamente un'entità database e uno schema predefinito per tale utente. L'entità database e lo schema creati avranno lo stesso nome usato dall'utente per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (il nome di accesso per l'autenticazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il nome utente di Windows).  
  
 Questo comportamento è necessario per permettere agli utenti che usano gruppi di Windows di creare e possedere oggetti. Può tuttavia comportare la creazione accidentale di schemi e utenti. Per evitare di creare implicitamente utenti e schema, creare sempre, quando possibile, in modo esplicito le entità database e assegnare uno schema predefinito. In alternativa, dichiarare in modo esplicito uno schema esistente quando si creano oggetti in un database, usando nomi oggetto costituiti da due o tre parti.  

> [!NOTE]
>  La creazione implicita di un utente di Azure Active Directory non è possibile in [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Poiché per la creazione di un utente Azure AD da un provider esterno è necessario verificare lo stato dell'utente in AAD, la creazione dell'utente non riesce con errore 2760: **Il nome di schema specificato "\<user_name@domain>" non esiste oppure non si ha l'autorizzazione per usarlo.** E quindi l'errore 2759: **Istruzione CREATE SCHEMA non riuscita a causa di errori precedenti.** Per evitare questi errori, in primo luogo creare l'utente di Azure AD dal provider esterno, quindi eseguire nuovamente l'istruzione che crea l'oggetto.
 
  
## <a name="deprecation-notice"></a>Informativa sulle funzionalità deprecate  
 Le istruzioni CREATE SCHEMA che non specificano un nome di schema sono attualmente supportate per compatibilità con le versioni precedenti. Tali istruzioni in realtà non creano uno schema all'interno del database, ma creano tabelle e viste e concedono autorizzazioni. Le entità non necessitano dell'autorizzazione CREATE SCHEMA per eseguire questa versione meno recente di CREATE SCHEMA, perché non viene creato alcuno schema. Questa funzionalità verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE SCHEMA per il database.  
  
 Per creare un oggetto specificato all'interno dell'istruzione CREATE SCHEMA, l'utente deve disporre dell'autorizzazione CREATE corrispondente.  
  
 Per specificare un altro utente come proprietario dello schema che viene creato, l'utente deve disporre dell'autorizzazione IMPERSONATE per quell'utente. Se si specifica un ruolo di database come proprietario, il chiamante deve disporre dell'appartenenza al ruolo oppure dell'autorizzazione ALTER per il ruolo.  
  
> [!NOTE]  
>  Per motivi di compatibilità con la sintassi precedente, non vengono controllate le autorizzazioni per CREATE SCHEMA perché non viene creato alcuno schema.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-schema-and-granting-permissions"></a>R. Creazione di uno schema e concessione di autorizzazioni  
 Nell'esempio seguente viene creato lo schema `Sprockets`, di proprietà di `Annik`, contenente la tabella `NineProngs`. L'istruzione concede `SELECT` a `Mandar` e nega `SELECT` a `Prasanna`. Si noti che `Sprockets` e `NineProngs` vengono creati in una singola istruzione.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SCHEMA Sprockets AUTHORIZATION Annik  
    CREATE TABLE NineProngs (source int, cost int, partnumber int)  
    GRANT SELECT ON SCHEMA::Sprockets TO Mandar  
    DENY SELECT ON SCHEMA::Sprockets TO Prasanna;  
GO   
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-creating-a-schema-and-a-table-in-the-schema"></a>B. Creazione di uno schema e di una tabella nello schema  
 Nell'esempio seguente viene creato lo schema `Sales` e poi la tabella `Sales.Region` nello schema.  
  
```sql  
CREATE SCHEMA Sales;  
GO
  
CREATE TABLE Sales.Region   
(Region_id INT NOT NULL,  
Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
```  
  
### <a name="c-setting-the-owner-of-a-schema"></a>C. Impostazione del proprietario di uno schema  
 Nell'esempio seguente viene creato lo schema `Production`, di proprietà di `Mary`.  
  
```sql  
CREATE SCHEMA Production AUTHORIZATION [Contoso\Mary];  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [DROP SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/drop-schema-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [Creazione di uno schema di database](../../relational-databases/security/authentication-access/create-a-database-schema.md)  
  
  



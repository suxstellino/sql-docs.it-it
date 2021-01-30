---
description: HAS_PERMS_BY_NAME (Transact-SQL)
title: HAS_PERMS_BY_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HAS_PERMS_BY_NAME
- HAS_PERMS_BY_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- current permission status
- checking permission status
- verifying permission status
- testing permissions
- HAS_PERMS_BY_NAME function
ms.assetid: eaf8cc82-1047-4144-9e77-0e1095df6143
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c80d6a873f0004344e7407dbf25adee5a02c2cd4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184008"
---
# <a name="has_perms_by_name-transact-sql"></a>HAS_PERMS_BY_NAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Valuta l'autorizzazione valida dell'utente corrente per un'entità a protezione diretta. Una funzione correlata è [fn_my_permissions](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
HAS_PERMS_BY_NAME ( securable , securable_class , permission    
    [ , sub-securable ] [ , sub-securable_class ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *securable*  
 Nome dell'entità a protezione diretta. Se l'entità a protezione diretta è il server stesso, questo valore deve essere impostato su NULL. *securable* è un'espressione scalare di tipo **sysname**. Non prevede alcun valore predefinito.  
  
 *securable_class*  
 Nome della classe dell'entità a protezione diretta in cui viene testata l'autorizzazione. *securable_class* è un'espressione scalare di tipo **nvarchar(60)**.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] l'argomento securable_class deve essere impostato su uno dei seguenti valori **DATABASE**, **OBJECT**, **ROLE**, **SCHEMA** o **USER**.  
  
 *permission*  
 Espressione scalare non Null di tipo **sysname** che rappresenta il nome dell'autorizzazione da controllare. Non prevede alcun valore predefinito. Il nome di autorizzazione ANY è un carattere jolly.  
  
 *sub-securable*  
 Espressione scalare facoltativa di tipo **sysname** che rappresenta il nome della sottoentità a protezione diretta in cui viene testata l'autorizzazione. Il valore predefinito è NULL.  
  
> [!NOTE]  
>  Le sottoentità a protezione diretta non possono usare le parentesi quadre nella forma **'[** _nome secondario_ **]'** . È necessario usare invece **'** _nome secondario_ **'** .  
  
 *sub-securable_class*  
 Espressione scalare facoltativa di tipo **nvarchar(60)** che rappresenta la classe della sottoentità a protezione diretta in cui viene testata l'autorizzazione. Il valore predefinito è NULL.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] l'argomento sub-securable_class è valido solo se l'argomento securable_class è impostato su **OBJECT**. Se l'argomento securable_class è impostato su **OBJECT**, l'argomento sub-securable_class deve essere impostato su **COLUMN**.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
 Restituisce NULL se la query non viene eseguita correttamente.  
  
## <a name="remarks"></a>Osservazioni  
 Questa funzione predefinita testa se l'entità principale dispone di una determinata autorizzazione valida per un'entità a protezione diretta specificata. HAS_PERMS_BY_NAME restituisce 1 quando l'utente dispone dell'autorizzazione valida sull'entità a protezione diretta, 0 in caso contrario oppure NULL quando la classe di entità a protezione diretta o l'autorizzazione non è valida. Di seguito vengono riportate le autorizzazioni valide:  
  
-   Autorizzazione concessa direttamente all'entità di protezione e non negata.  
  
-   Un'autorizzazione inclusa in un'autorizzazione di livello superiore assegnata all'entità di protezione e non negata.  
  
-   Un'autorizzazione concessa a un ruolo o a un gruppo cui appartiene l'entità di protezione e non negata.  
  
-   Un'autorizzazione di cui dispone un ruolo o un gruppo a cui appartiene l'entità di protezione e non negata.  
  
 La valutazione dell'autorizzazione viene sempre eseguita nel contesto di sicurezza del chiamante. Per determinare se altri utenti dispongono di un'autorizzazione valida, il chiamante deve disporre dell'autorizzazione IMPERSONATE per l'utente.  
  
 Per le entità a livello di schema, sono accettati i nomi non Null composti da una, due o tre parti. Per le entità a livello di database, è accettato un nome composto da una parte, con un valore Null ad indicare il database corrente. Per il server stesso, è richiesto un valore Null, ad indicare il server corrente. Questa funzione non può controllare le autorizzazioni per un server collegato o per un utente di Windows per cui non è stata creata un'entità a livello di server.  
  
 La query seguente restituirà un elenco di classi di entità a protezione diretta predefinite:  
  
```  
SELECT class_desc FROM sys.fn_builtin_permissions(default);  
```  
  
 Vengono utilizzate le regole di confronto seguenti:  
  
-   Regole di confronto del database corrente: entità a protezione diretta che includono le entità a protezione diretta non contenute in uno schema, entità a protezione diretta con ambito schema in una o due parti, database di destinazione quando si utilizza un nome in tre parti.  
  
-   Regole di confronto del database master: entità a protezione diretta del database.  
  
-   ANY non è supportato per i controlli a livello di colonna. È necessario specificare l'autorizzazione appropriata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-do-i-have-the-server-level-view-server-state-permission"></a>R. Verifica dell'autorizzazione utente VIEW SERVER STATE a livello di server  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
```sql  
SELECT HAS_PERMS_BY_NAME(null, null, 'VIEW SERVER STATE');  
```  
  
### <a name="b-am-i-able-to-impersonate-server-principal-ps"></a>B. Verifica della capacità dell'utente di rappresentare (IMPERSONATE) l'entità server Ps  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
```sql  
SELECT HAS_PERMS_BY_NAME('Ps', 'LOGIN', 'IMPERSONATE');  
```  
  
### <a name="c-do-i-have-any-permissions-in-the-current-database"></a>C. Verifica delle autorizzazioni dell'utente per il database corrente  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
```  
  
### <a name="d-does-database-principal-pd-have-any-permission-in-the-current-database"></a>D. Verifica delle autorizzazioni dell'entità di database Pd per il database corrente  
 Nell'esempio si presuppone che il chiamante disponga dell'autorizzazione IMPERSONATE per l'entità `Pd`.  
  
```sql  
EXECUTE AS user = 'Pd'  
GO  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
GO  
REVERT;  
GO  
```  
  
### <a name="e-can-i-create-procedures-and-tables-in-schema-s"></a>E. Verifica delle autorizzazioni dell'utente per creare procedure e tabelle nello schema S  
 Nell'esempio seguente è richiesta l'autorizzazione `ALTER` in `S` e l'autorizzazione `CREATE PROCEDURE` nel database e le autorizzazioni corrispondenti per le tabelle.  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE PROCEDURE')  
    & HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_procs,  
    HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE TABLE') &  
    HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_tables;  
```  
  
### <a name="f-which-tables-do-i-have-select-permission-on"></a>F. Verifica delle tabelle per cui l'utente dispone dell'autorizzazione SELECT  
  
```sql  
SELECT HAS_PERMS_BY_NAME  
(QUOTENAME(SCHEMA_NAME(schema_id)) + '.' + QUOTENAME(name),   
    'OBJECT', 'SELECT') AS have_select, * FROM sys.tables  
```  
  
### <a name="g-do-i-have-insert-permission-on-the-salesperson-table-in-adventureworks2012"></a>G. Verifica dell'autorizzazione INSERT nella tabella SalesPerson in AdventureWorks2012  
 Nell'esempio seguente si presuppone che `AdventureWorks2012` sia il contesto del database corrente dell'utente e viene utilizzato un nome composto da due parti.  
  
```sql  
SELECT HAS_PERMS_BY_NAME('Sales.SalesPerson', 'OBJECT', 'INSERT');  
```  
  
 Nell'esempio corrente non vi sono presupposizioni sul contesto del database corrente e viene utilizzato un nome composto da tre parti.  
  
```sql  
SELECT HAS_PERMS_BY_NAME('AdventureWorks2012.Sales.SalesPerson',   
    'OBJECT', 'INSERT');  
```  
  
### <a name="h-which-columns-of-table-t-do-i-have-select-permission-on"></a>H. Verifica delle colonne della tabella T per cui l'utente dispone dell'autorizzazione SELECT  
  
```sql  
SELECT name AS column_name,   
    HAS_PERMS_BY_NAME('T', 'OBJECT', 'SELECT', name, 'COLUMN')   
    AS can_select   
    FROM sys.columns AS c   
    WHERE c.object_id=object_id('T');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Gerarchia delle autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
  

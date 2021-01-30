---
description: Clausola EXECUTE AS (Transact-SQL)
title: Clausola EXECUTE AS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AS
- AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permission sets [SQL Server]
- queues [SQL Server]
- stored procedures [SQL Server], executing
- user-defined functions [SQL Server], execution context
- EXECUTE AS
- triggers [SQL Server], execution context
- execution context [SQL Server]
- switching execution context
- functions [SQL Server], execution context
ms.assetid: bd517aa3-f06e-4356-87d8-70de5df4494a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f36b2dd484e1308e8b7ee71d19d8f1f6315c3f59
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159145"
---
# <a name="execute-as-clause-transact-sql"></a>Clausola EXECUTE AS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile definire il contesto di esecuzione dei seguenti moduli definiti dall'utente: funzioni (ad eccezione delle funzioni inline con valori di tabella), procedure, code e trigger.  
  
 Specificando il contesto in cui viene eseguito il modulo, è possibile controllare quale account utente viene utilizzato da [!INCLUDE[ssDE](../../includes/ssde-md.md)] per convalidare le autorizzazioni per gli oggetti a cui viene fatto riferimento dal modulo. Ciò consente maggiore flessibilità e controllo nella gestione delle autorizzazioni all'interno della catena di oggetti esistente tra i moduli definiti dall'utente e gli oggetti cui viene fatto riferimento da tali moduli. È necessario concedere agli utenti le autorizzazioni solo nel modulo stesso, senza dover concedere loro le autorizzazioni esplicite per gli oggetti a cui viene fatto riferimento. Solo l'account utente con il quale viene eseguito il modulo deve disporre delle autorizzazioni per gli oggetti a cui ha accesso il modulo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- SQL Server Syntax  
Functions (except inline table-valued functions), Stored Procedures, and DML Triggers  
{ EXEC | EXECUTE } AS { CALLER | SELF | OWNER | 'user_name' }   
  
DDL Triggers with Database Scope  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'user_name' }   
  
DDL Triggers with Server Scope and logon triggers  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'login_name' }   
  
Queues  
{ EXEC | EXECUTE } AS { SELF | OWNER | 'user_name' }   
```  
  
```syntaxsql
-- Azure SQL Database Syntax  
Functions (except inline table-valued functions), Stored Procedures, and DML Triggers  
  
{ EXEC | EXECUTE } AS { CALLER | SELF | OWNER | 'user_name' }   
  
DDL Triggers with Database Scope  
  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'user_name' }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **CALLER**  
 Specifica che le istruzioni all'interno del modulo vengono eseguite nel contesto del chiamante del modulo. L'utente che esegue il modulo deve disporre delle autorizzazioni appropriate non solo per il modulo stesso, ma anche per tutti gli oggetti di database a cui fa riferimento il modulo.  
  
 CALLER è il valore predefinito per tutti i moduli ad eccezione delle code e mantiene lo stesso funzionamento di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 CALLER non può essere specificato in un'istruzione CREATE QUEUE o ALTER QUEUE.  
  
 **SELF**  
 EXECUTE AS SELF equivale a EXECUTE AS *user_name*, dove l'utente specificato è la persona che crea o modifica il modulo. L'ID utente effettivo della persona che crea o modifica i moduli viene archiviato nella colonna **execute_as_principal_id** della vista del catalogo **sys.sql_modules** o **sys.service_queues**.  
  
 SELF è il valore predefinito per le code.  
  
> [!NOTE]  
>  Per modificare l'ID utente della colonna **execute_as_principal_id** nella vista del catalogo **sys.service_queues**, è necessario specificare in modo esplicito l'impostazione EXECUTE AS nell'istruzione ALTER QUEUE.  
  
 OWNER  
 Specifica che le istruzioni all'interno del modulo vengono eseguite nel contesto del proprietario corrente del modulo. Se per modulo non esiste un proprietario specificato, viene utilizzato il proprietario dello schema del modulo. Non è possibile specificare OWNER per i trigger DDL o LOGON.  
  
> [!IMPORTANT]  
>  Su OWNER deve essere eseguito il mapping a un account singleton e non può essere un ruolo o un gruppo.  
  
 **'** *user_name* **'**  
 Specifica che le istruzioni all'interno del modulo vengono eseguite nel contesto dell'utente specificato in *user_name*. Le autorizzazioni per gli oggetti del modulo vengono verificate in base a *user_name*. Non è possibile specificare *user_name* per i trigger DDL con ambito server o i trigger LOGON. Usare invece *login_name*.  
  
 *user_name* deve esistere nel database corrente e deve essere un account singleton. *user_name* non può essere un gruppo, un ruolo, un certificato, una chiave o un account predefinito, ad esempio NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
 L'ID utente del contesto di esecuzione viene archiviato nei metadati e può essere visualizzato nella colonna **execute_as_principal_id** della vista del catalogo **sys.sql_modules** o **sys.assembly_modules**.  
  
 **'** *login_name* **'**  
 Specifica che le istruzioni all'interno del modulo vengono eseguite nel contesto dell'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificato in *login_name*. Le autorizzazioni per gli oggetti del modulo vengono verificate in base a *login_name*. È possibile specificare *login_name* solo per i trigger DDL con ambito server o i trigger LOGON.  
  
 *login_name* non può essere un gruppo, un ruolo, un certificato, una chiave o un account predefinito, ad esempio NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
## <a name="remarks"></a>Commenti  
 La modalità in cui [!INCLUDE[ssDE](../../includes/ssde-md.md)] valuta le autorizzazioni per gli oggetti cui viene fatto riferimento nel modulo dipende dalla catena di proprietà esistente tra gli oggetti chiamanti e gli oggetti cui viene fatto riferimento. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la concatenazione della proprietà rappresenta l'unico metodo disponibile per evitare di dover concedere all'utente chiamante l'accesso a tutti gli oggetti a cui viene fatto riferimento.  
  
 La concatenazione della proprietà presenta le limitazioni seguenti:  
  
-   Si applica solo a queste istruzioni DML: SELECT, INSERT, UPDATE e DELETE.  
  
-   I proprietari degli oggetti chiamanti devono corrispondere a quelli degli oggetti chiamati.  
  
-   Non si applica alle query dinamiche all'interno del modulo.  
  
 A prescindere dal contesto di esecuzione specificato nel modulo, vengono sempre applicate le azioni seguenti:  
  
-   Se il modulo viene eseguito, [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica innanzitutto che l'utente che esegue il modulo disponga dell'autorizzazione EXECUTE per il modulo.  
  
-   Le regole di concatenazione della proprietà continuano ad essere applicate. Ciò vuol dire che se i proprietari degli oggetti chiamanti corrispondono a quelli degli oggetti chiamati, le autorizzazioni per gli oggetti sottostanti non vengono controllate.  
  
 Quando un utente esegue un modulo che è stato specificato per essere eseguito in un contesto diverso da CALLER, viene verificata l'autorizzazione utente per l'esecuzione del modulo, ma i controlli delle autorizzazioni aggiuntive per gli oggetti a cui ha accesso il modulo vengono eseguiti in base all'account utente specificato nella clausola EXECUTE AS. In sostanza, l'utente che esegue il modulo rappresenta l'utente specificato.  
  
 Il contesto specificato nella clausola EXECUTE AS del modulo è valido solo per la durata dell'esecuzione del modulo. Il contesto ritorna al chiamante al termine dell'esecuzione del modulo.  
  
## <a name="specifying-a-user-or-login-name"></a>Specifica di un nome utente o un nome account di accesso  
 Non è possibile rimuovere un account di accesso a un server o un nome utente di database specificato nella clausola EXECUTE AS di un modulo fino a quando il modulo non è stato modificato per consentire l'esecuzione in un altro contesto.  
  
 Il nome utente o l'account di accesso specificato nella clausola EXECUTE AS deve esistere come entità in **sys.database_principals** o **sys.server_principals**, rispettivamente. In caso contrario, l'operazione di creazione o modifica del modulo non riesce. Inoltre, l'utente che crea o modifica il modulo deve disporre delle autorizzazioni IMPERSONATE per l'entità.  
  
 Se l'utente ha accesso implicito al database o all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'appartenenza a un gruppo di Windows, l'utente specificato nella clausola EXECUTE AS viene creato implicitamente al momento della creazione del modulo quando sussiste uno dei requisiti seguenti:  
  
-   L'utente o l'account di accesso specificato è un membro del ruolo predefinito del server **sysadmin**.  
  
-   L'utente che crea il modulo dispone dell'autorizzazione per creare entità.  
  
 Se non viene soddisfatto nessuno di questi requisiti, l'operazione di creazione del modulo non può essere completata.  
  
> [!IMPORTANT]  
>  Se il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER) viene eseguito come account locale (servizio locale o account utente locale), non disporrà dei privilegi per ottenere l'appartenenza ai gruppi di un account di dominio di Windows specificato nella clausola EXECUTE AS. Di conseguenza, l'esecuzione del modulo non potrà essere completata.  
  
 Si suppongano ad esempio le condizioni seguenti:  
  
-   Il gruppo **CompanyDomain\SQLUsers** ha accesso al database **Sales**.  
  
-   **CompanyDomain\SqlUser1** è un membro di **SQLUsers** e ha pertanto accesso al database **Sales**.  
  
-   L'utente che crea o modifica il modulo possiede le autorizzazioni per creare entità.  
  
 Quando viene eseguita l'istruzione `CREATE PROCEDURE` seguente, `CompanyDomain\SqlUser1` viene creato implicitamente come entità di database nel database `Sales`.  
  
```sql  
USE Sales;  
GO  
CREATE PROCEDURE dbo.usp_Demo  
WITH EXECUTE AS 'CompanyDomain\SqlUser1'  
AS  
SELECT user_name();  
GO  
```  
  
## <a name="using-execute-as-caller-stand-alone-statement"></a>Utilizzo dell'istruzione autonoma EXECUTE AS CALLER  
 Utilizzare l'istruzione autonoma EXECUTE AS CALLER all'interno di un modulo per impostare il contesto di esecuzione sul chiamante del modulo.  
  
 Presupporre che la stored procedure seguente venga chiamata da `SqlUser2`.  
  
```sql  
CREATE PROCEDURE dbo.usp_Demo  
WITH EXECUTE AS 'SqlUser1'  
AS  
SELECT user_name(); -- Shows execution context is set to SqlUser1.  
EXECUTE AS CALLER;  
SELECT user_name(); -- Shows execution context is set to SqlUser2, the caller of the module.  
REVERT;  
SELECT user_name(); -- Shows execution context is set to SqlUser1.  
GO  
```  
  
## <a name="using-execute-as-to-define-custom-permission-sets"></a>Utilizzo di EXECUTE AS per definire i set di autorizzazioni personalizzati  
 La specifica di un contesto di esecuzione per un modulo può risultare molto utile quando si desidera definire set di autorizzazioni personalizzati. Per alcune azioni ad esempio, come TRUNCATE TABLE, non è possibile concedere le autorizzazioni. Incorporando l'istruzione TRUNCATE TABLE all'interno di un modulo e specificando che il modulo viene eseguito come utente che dispone delle autorizzazioni per modificare la tabella, è possibile estendere le autorizzazioni necessarie per troncare la tabella all'utente al quale vengono concesse le autorizzazioni EXECUTE per il modulo.  
  
 Per visualizzare la definizione del modulo con il contesto di esecuzione specificato, usare la vista del catalogo [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
## <a name="best-practice"></a>Procedura consigliata  
 Specificare un account di accesso o un utente che dispone delle autorizzazioni minime necessarie per eseguire le operazioni definite nel modulo. Ad esempio, non specificare un account di un proprietario di database a meno che non siano necessarie tali autorizzazioni.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire un modulo specificato con EXECUTE AS, il chiamante deve disporre delle autorizzazioni EXECUTE per il modulo.  
  
 Per eseguire un modulo CLR specificato con EXECUTE AS che ha accesso alle risorse in un altro database o server, il database o server di destinazione deve considerare attendibile l'autenticatore del database nel quale ha origine il modulo (il database di origine).  
  
 Per specificare la clausola EXECUTE AS quando si crea o si modifica un modulo, è necessario disporre delle autorizzazioni IMPERSONATE per l'entità specificata, nonché delle autorizzazioni per creare il modulo. È sempre possibile rappresentare se stessi. Quando non è specificato alcun contesto di esecuzione o è specificato EXECUTE AS CALLER, le autorizzazioni IMPERSONATE non sono necessarie.  
  
 Per specificare un *login_name* o *user_name* con accesso implicito al database tramite l'appartenenza a un gruppo di Windows, è necessario avere le autorizzazioni CONTROL per il database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata una stored procedure nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] e viene assegnato il contesto di esecuzione a `OWNER`.  
  
```sql  
CREATE PROCEDURE HumanResources.uspEmployeesInDepartment   
@DeptValue int  
WITH EXECUTE AS OWNER  
AS  
    SET NOCOUNT ON;  
    SELECT e.BusinessEntityID, c.LastName, c.FirstName, e.JobTitle  
    FROM Person.Person AS c   
    INNER JOIN HumanResources.Employee AS e  
        ON c.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeeDepartmentHistory AS edh  
        ON e.BusinessEntityID = edh.BusinessEntityID  
    WHERE edh.DepartmentID = @DeptValue  
    ORDER BY c.LastName, c.FirstName;  
GO  
  
-- Execute the stored procedure by specifying department 5.  
EXECUTE HumanResources.uspEmployeesInDepartment 5;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.service_queues &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-service-queues-transact-sql.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  

---
description: EXECUTE AS (Transact-SQL)
title: EXECUTE AS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXECUTE AS
- EXECUTE_AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- WITH NO REVERT clause
- sessions [SQL Server], execution context
- EXECUTE AS
- execution context [SQL Server]
- switching execution context
ms.assetid: 613b8271-7f7d-4378-b7a2-5a7698551dbd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: eaeafb5eb8e64686429cb0c7b8ccac9c1b7785c2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740161"
---
# <a name="execute-as-transact-sql"></a>EXECUTE AS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Imposta il contesto di esecuzione di una sessione.  
  
 Per impostazione predefinita, una sessione inizia quando un utente si connette e termina quando l'utente si disconnette. Tutte le operazioni eseguite durante una sessione sono soggette alle verifiche delle autorizzazioni dell'utente connesso. Quando viene eseguita un'istruzione **EXECUTE AS**, il contesto di esecuzione della sessione viene impostato sull'account di accesso o sul nome utente specificato. In seguito all'impostazione del contesto specifico, le autorizzazioni vengono verificate in base ai token di sicurezza dell'account di accesso e dell'account utente per l'account specifico anziché in base alla persona che ha chiamato l'istruzione **EXECUTE AS**. In pratica, l'account utente o l'account di accesso viene rappresentato per l'intera durata dell'esecuzione della sessione o del modulo oppure il passaggio di contesto viene ripristinato in modo esplicito.  
  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
{ EXEC | EXECUTE } AS <context_specification>  
[;]  
  
<context_specification>::=  
{ LOGIN | USER } = 'name'  
    [ WITH { NO REVERT | COOKIE INTO @varbinary_variable } ]   
| CALLER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 LOGIN  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica che il contesto di esecuzione da rappresentare è un account di accesso. L'ambito di rappresentazione è a livello di server.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente, nel database SQL o in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
 USER  
 Specifica che il contesto da rappresentare è un utente nel database corrente. L'ambito di rappresentazione è limitato al database corrente. Un cambio di contesto a un utente del database non eredita le autorizzazioni a livello di server di tale utente.  
  
> [!IMPORTANT]  
>  Mentre il cambio di contesto all'utente del database è attivo, qualsiasi tentativo di accesso alle risorse esterne al database comporterà l'esito negativo dell'esecuzione dell'istruzione. Ciò è valido per le istruzioni USE *database*, le query distribuite e le query che fanno riferimento a un altro database che usa identificatori in tre o quattro parti.  
  
 '*name*' Nome utente o nome account di accesso valido. *name* deve essere membro del ruolo predefinito del server **sysadmin** oppure esistere come entità di sicurezza rispettivamente in [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) o [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 *name* può essere specificato come variabile locale.  
  
 *name* deve essere un account singleton e non può essere un gruppo, un ruolo, un certificato, una chiave oppure un account predefinito, ad esempio NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
 Per altre informazioni, vedere [Specifica di un nome utente o un nome account di accesso](#_user) di seguito in questo argomento.  
  
 NO REVERT  
 Specifica che non è possibile ripristinare il contesto precedente in seguito a un cambio di contesto. L'opzione **NO REVERT** può essere usata solo a livello ad hoc.  
  
 Per altre informazioni sul ripristino del contesto precedente, vedere [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md).  
  
 COOKIE INTO @*varbinary_variable*  
 Specifica che è possibile ripristinare il contesto di esecuzione precedente solo se l'istruzione REVERT WITH COOKIE chiamante contiene il valore corretto per @*varbinary_variable*. [!INCLUDE[ssDE](../../includes/ssde-md.md)] passa il cookie a @*varbinary_variable*. L'opzione **COOKIE INTO** può essere usata solo a livello ad hoc.  
  
 @*varbinary_variable* è **varbinary(8000)** .  
  
> [!NOTE]  
>  Il parametro **OUTPUT** del cookie è attualmente documentato come **varbinary(8000)** che rappresenta la lunghezza massima corretta. Tuttavia, l'implementazione corrente restituisce **varbinary(100)** . Le applicazioni devono riservare **varbinary(8000)** in modo che siano in grado di funzionare correttamente se le dimensioni restituite del cookie aumentano in una versione successiva.  
  
 CALLER  
 Se utilizzato all'interno di un modulo, specifica che le istruzioni all'interno del modulo vengono eseguite nel contesto del chiamante del modulo.
Se utilizzato all'esterno di un modulo, l'istruzione non esegue alcuna azione.
 > [!NOTE]  
>  Questa opzione non è disponibile in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
## <a name="remarks"></a>Osservazioni  
 Il cambio di contesto di esecuzione rimane valido finché non si verifica una delle situazioni seguenti:  
  
-   Viene eseguita un'altra istruzione EXECUTE AS.  
  
-   Viene eseguita un'istruzione REVERT.  
  
-   La sessione viene rimossa.  
  
-   La stored procedure o il trigger in cui è stato eseguito il comando è esistente.  
  
È possibile creare uno stack di contesti di esecuzione eseguendo più volte una chiamata all'istruzione EXECUTE AS in più entità. Quando viene chiamata, l'istruzione REVERT imposta il contesto sull'account di accesso o sull'utente nel successivo livello superiore nello stack di contesti. Per una dimostrazione di questo comportamento, vedere l'[esempio A](#_exampleA).  
  
##  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Indicazione di un nome utente o di un ID di accesso  
 Il nome utente o l'ID di accesso specificato in EXECUTE AS \<context_specification> deve esistere come entità rispettivamente in **sys.database_principals** o **sys.server_principals**. In caso contrario, l'istruzione EXECUTE AS ha esito negativo. È inoltre necessario concedere le autorizzazioni IMPERSONATE per l'entità. A meno che il chiamante non sia il proprietario del database o membro del ruolo predefinito del server **sysadmin**, l'entità deve esistere anche quando l'utente effettua l'accesso al database o all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'appartenenza a un gruppo di Windows. Si suppongano ad esempio le condizioni seguenti: 
  
-   Il gruppo **CompanyDomain\SQLUsers** ha accesso al database **Sales**.  
  
-   **CompanyDomain\SqlUser1** è membro del gruppo **SQLUsers** e pertanto può implicitamente accedere al database **Sales**.  
  
 Anche se **CompanyDomain\SqlUser1** può accedere al database in virtù dell'appartenenza al gruppo **SQLUsers**, l'istruzione `EXECUTE AS USER = 'CompanyDomain\SqlUser1'` ha esito negativo in quanto `CompanyDomain\SqlUser1` non esiste come entità nel database.  
  
Se l'utente è reso orfano, ovvero se l'accesso associato non esiste più, e non è stato creato con **WITHOUT LOGIN**, **EXECUTE AS** avrà esito negativo per tale utente.  
  
## <a name="best-practice"></a>Procedura consigliata  
 Specificare un account di accesso o un utente che disponga almeno dei privilegi necessari per eseguire operazioni nella sessione. Ad esempio, non specificare un nome account di accesso con autorizzazioni a livello di server se sono richieste solo autorizzazioni a livello di database oppure non specificare l'account di un proprietario di database a meno che siano richieste le autorizzazioni corrispondenti.  
  
> [!CAUTION]  
>  L'istruzione EXECUTE AS può avere esito positivo, a condizione che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] sia in grado di risolvere il nome. Se è presente un utente di dominio, è possibile che Windows sia in grado di risolvere l'utente per [!INCLUDE[ssDE](../../includes/ssde-md.md)], anche se l'utente di Windows non dispone dell'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ciò potrebbe creare una condizione in cui un account di accesso privo di autorizzazione di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risulti apparentemente connesso, sebbene l'account di accesso rappresentato disponga solo delle autorizzazioni concesse a public o guest.  
  
## <a name="using-with-no-revert"></a>Utilizzo di WITH NO REVERT  
 Se l'istruzione EXECUTE AS include la clausola facoltativa WITH NO REVERT, il contesto di esecuzione di una sessione non può essere ripristinato tramite REVERT oppure tramite l'esecuzione di un'altra istruzione EXECUTE AS. Il contesto impostato dall'istruzione rimane valido fino all'eliminazione della sessione.  
  
 Quando la clausola WITH NO REVERT COOKIE = @*varbinary_variabl* e è specificata, il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] passa il valore del cookie a @*varbinary_variabl* e. Il contesto di esecuzione impostato da tale istruzione può essere riportato al contesto precedente solo se l'istruzione REVERT WITH COOKIE = @*varbinary_variable* contiene lo stesso valore di *\@varbinary_variable*.  
  
 Questa opzione risulta utile in un ambiente in cui vengono utilizzati i pool di connessioni. Tramite i pool di connessioni vengono gestiti i gruppi di connessioni al database in modo che tali connessioni possano essere riutilizzate dalle applicazioni in un server applicazioni. Poiché il valore passato a *\@varbinary_variable* è noto solo al chiamante dell'istruzione EXECUTE AS, il chiamante è in grado di garantire che il contesto di esecuzione stabilito non venga modificato da altri.  
  
## <a name="determining-the-original-login"></a>Determinazione dell'account di accesso originale  
 Usare la funzione [ORIGINAL_LOGIN](../../t-sql/functions/original-login-transact-sql.md) per restituire il nome dell'account di accesso connesso all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile utilizzare questa funzione per restituire l'identità dell'account di accesso originale in sessioni in cui si verificano numerosi cambi di contesto espliciti o impliciti.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per specificare l'istruzione **EXECUTE AS** per un account di accesso, il chiamante deve disporre dell'autorizzazione **IMPERSONATE** per il nome dell'account di accesso specificato e non gli deve essere negata l'autorizzazione **IMPERSONATE ANY LOGIN**. Per specificare l'istruzione **EXECUTE AS** per un utente del database, il chiamante deve disporre delle autorizzazioni **IMPERSONATE** per il nome utente specificato. Se si specifica **EXECUTE AS CALLER**, le autorizzazioni **IMPERSONATE** non sono obbligatorie.  
  
## <a name="examples"></a>Esempi  
  
###  <a name="a-using-execute-as-and-revert-to-switch-context"></a><a name="_exampleA"></a> A. Utilizzo di EXECUTE AS e REVERT per cambiare contesto  
 Nell'esempio seguente viene creato uno stack di contesti di esecuzione utilizzando più entità. Viene quindi utilizzata l'istruzione `REVERT` per ripristinare il contesto di esecuzione al chiamante precedente. L'istruzione `REVERT` viene eseguita più volte per innalzare di livello lo stack finché il contesto di esecuzione viene impostato sul chiamante originale.  
  

```sql
USE AdventureWorks2012;  
GO  
--Create two temporary principals  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
--Give IMPERSONATE permissions on user2 to user1  
--so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
--Verify the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
--Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1 and login2.  
--The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
--Remove temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. Utilizzo della clausola WITH COOKIE  
 Nell'esempio seguente il contesto di esecuzione di una sessione viene impostato su un utente specificato e il COOKIE viene specificato nella clausola @*varbinary_variable* . Nell'istruzione `REVERT` è necessario specificare il valore passato alla variabile `@cookie` nell'istruzione `EXECUTE AS` per ripristinare correttamente il contesto al chiamante originale. Per eseguire questo esempio, l'account di accesso `login1` e l'utente `user1` creato nell'esempio A devono esistere.  
  

```sql 
DECLARE @cookie VARBINARY(8000);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie in a safe location in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(8000);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [EXECUTE AS Clause &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)  
  
  


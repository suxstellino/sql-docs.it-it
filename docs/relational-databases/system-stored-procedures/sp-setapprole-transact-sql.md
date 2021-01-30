---
description: sp_setapprole (Transact-SQL)
title: sp_setapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/12/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_setapprole
- sp_setapprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setapprole
ms.assetid: cf0901c0-5f90-42d4-9d5b-8772c904062d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f34be78bc4d3f47ff2a11d0a6ed09c22c3670dca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209280"
---
# <a name="sp_setapprole-transact-sql"></a>sp_setapprole (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Attiva le autorizzazioni associate a un ruolo applicazione nel database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  

```sql
sp_setapprole [ @rolename = ] 'role',  
    [ @password = ] { encrypt N'password' }
      |  
        'password' [ , [ @encrypt = ] { 'none' | 'odbc' } ]  
        [ , [ @fCreateCookie = ] true | false ]  
    [ , [ @cookie = ] @cookie OUTPUT ]  
```

## <a name="arguments"></a>Argomenti

`[ @rolename = ] 'role'` Nome del ruolo applicazione definito nel database corrente. *Role* è di **tipo sysname** e non prevede alcun valore predefinito. il *ruolo* deve esistere nel database corrente.  
  
`[ @password = ] { encrypt N'password' }` Password necessaria per attivare il ruolo applicazione. *password* è di **tipo sysname** e non prevede alcun valore predefinito. la *password* può essere offuscata utilizzando la funzione ODBC **Encrypt** . Quando si utilizza la funzione **Encrypt** , la password deve essere convertita in una stringa Unicode inserendo **N** prima della prima virgoletta.  
  
 L'opzione encrypt non è supportata per le connessioni che utilizzano **SqlClient**.  
  
> [!IMPORTANT]  
> La funzione ODBC **Encrypt** non fornisce la crittografia. È consigliabile evitare l'utilizzo di questa funzione per proteggere le password trasmesse in rete. Se queste informazioni verranno trasmesse in rete, utilizzare TLS o IPSec.
  
 **@encrypt =' none '**  
 Indica che non è richiesto l'utilizzo di tecniche di offuscamento. La password viene passata a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come testo normale. Questo è il valore predefinito.  
  
 **@encrypt=' ODBC '**  
 Specifica che la password viene offuscata da ODBC utilizzando la funzione ODBC **Encrypt** prima di inviare la password a [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . È possibile specificare questa opzione solo se si utilizza un client ODBC o il provider OLE DB per SQL Server.  
  
`[ @fCreateCookie = ] true | false` Specifica se è necessario creare un cookie. **true** viene convertito in modo implicito in 1. **false** viene convertito in modo implicito in 0.  
  
`[ @cookie = ] @cookie OUTPUT` Specifica un parametro di output che conterrà il cookie. Il cookie viene generato solo se il valore di **\@ fCreateCookie** è **true**. **varbinary(8000)**  
  
> [!NOTE]  
> Il parametro **OUTPUT** del cookie per **sp_setapprole** è attualmente documentato come **varbinary(8000)** che rappresenta la lunghezza massima corretta. Tuttavia, l'implementazione corrente restituisce **varbinary(50)** . Le applicazioni devono continuare a riservare **varbinary (8000),** in modo che l'applicazione continui a funzionare correttamente se le dimensioni restituite del cookie aumentano in una versione futura.
  
## <a name="return-code-values"></a>Valori del codice restituito

 0 (esito positivo) o 1 (esito negativo)  
  
## <a name="remarks"></a>Commenti

 Dopo l'attivazione di un ruolo applicazione tramite **sp_setapprole**, il ruolo rimane attivo fino a quando l'utente non si disconnette dal server o non esegue **sp_unsetapprole**. **sp_setapprole** possono essere eseguite solo da [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni dirette, a livello ad hoc e non all'interno di un'altra stored procedure, trigger o all'interno di una transazione definita dall'utente.  
  
 Per una panoramica dei ruoli applicazione, vedere [ruoli applicazione](../../relational-databases/security/authentication-access/application-roles.md).  
  
> [!IMPORTANT]  
> Per proteggere la password del ruolo applicazione durante la trasmissione in una rete, è consigliabile utilizzare sempre una connessione crittografata quando si Abilita un ruolo applicazione.
> L' [!INCLUDE[msCoName](../../includes/msconame-md.md)] opzione ODBC **Encrypt** non è supportata da **SqlClient**. Se è necessario archiviare credenziali, crittografarle con le funzioni CryptoAPI. La *password* del parametro viene archiviata come hash unidirezionale. Per mantenere la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , i criteri di complessità delle password non vengono applicati da **sp_addapprole**. Per applicare i criteri di complessità delle password, utilizzare [Crea ruolo applicazione](../../t-sql/statements/create-application-role-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni

Richiede l'appartenenza a **public** e la conoscenza della password per il ruolo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-activating-an-application-role-without-the-encrypt-option"></a>R. Attivazione di un ruolo applicazione senza l'opzione encrypt

 Nell'esempio seguente viene attivato il ruolo applicazione `SalesAppRole` con la password non crittografata `AsDeF00MbXX`, creato con autorizzazioni specifiche per l'applicazione utilizzata dall'utente corrente.

```sql
EXEC sys.sp_setapprole 'SalesApprole', 'AsDeF00MbXX';  
GO
```

### <a name="b-activating-an-application-role-with-a-cookie-and-then-reverting-to-the-original-context"></a>B. Attivazione di un ruolo applicazione con un cookie e ripristino del contesto originale

 Nell'esempio seguente viene attivato il ruolo applicazione `Sales11` con la password `fdsd896#gfdbfdkjgh700mM` e viene creato un cookie. Nell'esempio viene restituito il nome dell'utente corrente e quindi viene ripristinato il contesto originale tramite l'esecuzione di `sp_unsetapprole`.  

```sql
DECLARE @cookie varbinary(8000);  
EXEC sys.sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sys.sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.
GO
```

## <a name="see-also"></a>Vedere anche

 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [sp_unsetapprole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unsetapprole-transact-sql.md)

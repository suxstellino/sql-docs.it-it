---
description: IS_ROLEMEMBER (Transact-SQL)
title: IS_ROLEMEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_ROLEMEMBER
- IS_ROLEMEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_ROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 73efa688-ae91-4014-98bc-1cabe47321f7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a71bdc75ed965e377ec0fd8e5e6a8ddfd60b477
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746791"
---
# <a name="is_rolemember-transact-sql"></a>IS_ROLEMEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indica se un'entità di database specificata è un membro del ruolo del database specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
IS_ROLEMEMBER ( 'role' [ , 'database_principal' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *role* **'**  
 Nome del ruolo del database sottoposto al controllo. *role* è di tipo **sysname**.  
  
 **'** *database_principal* **'**  
 Nome dell'utente o ruolo del database o del ruolo applicazione da controllare. *database_principal* è di tipo **sysname** e il valore predefinito è NULL. Se non si specifica alcun valore, il risultato sarà basato sul contesto di esecuzione corrente. Se nel parametro è inclusa la parola NULL, verrà restituito NULL.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
|Valore restituito|Descrizione|  
|------------------|-----------------|  
|0|*database_principal* non è membro di *role*.|  
|1|*database_principal* è membro di *role*.|  
|NULL|*database_principal* o *role* non è valido oppure non si è autorizzati a visualizzare l'appartenenza al ruolo.|  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare IS_ROLEMEMBER per determinare se l'utente corrente è in grado di eseguire un'azione che richiede le autorizzazioni del ruolo del database.  
  
 Se *database_principal* si basa su un account di accesso di Windows, ad esempio Contoso\Mary5, IS_ROLEMEMBER restituisce NULL, a meno che a *database_principal* non sia stato concesso o negato l'accesso diretto a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se il parametro *database_principal* facoltativo non viene specificato e se *database_principal* si basa su un account di accesso a un domino Windows, può essere un membro di un ruolo del database tramite l'appartenenza a un gruppo di Windows. Per risolvere tali problemi di appartenenza indiretta, IS_ROLEMEMBER richiede le informazioni di appartenenza ai gruppi di Windows al controller di dominio. Se il controller di dominio non è accessibile o non risponde, IS_ROLEMEMBER restituisce le informazioni di appartenenza ai ruoli prendendo in considerazione solo l'utente e i gruppi locali. Se l'utente specificato non è l'utente corrente, il valore restituito da IS_ROLEMEMBER potrebbe essere diverso dall'ultimo aggiornamento dei dati dell'autenticatore, ad esempio Active Directory, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se il parametro *database_principal* facoltativo viene specificato, l'entità di database su cui viene eseguita la query deve essere presente in sys.database_principals. In caso contrario, IS_ROLEMEMBER restituisce NULL. Ciò indica che il parametro *database_principal* non è valido in questo database.  
  
 Quando il parametro *database_principal* si basa su un account di accesso a un dominio o su un gruppo di Windows e il controller di dominio non è accessibile, le chiamate a IS_ROLEMEMBER hanno esito negativo e possono restituire dati errati o incompleti.  
  
 Se il controller di dominio non è disponibile, la chiamata a IS_ROLEMEMBER restituisce informazioni accurate quando l'entità di Windows può essere autenticata localmente, ad esempio come account Windows locale o come account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **IS_ROLEMEMBER** restituisce sempre 0 quando viene usato un gruppo di Windows come argomento dell'entità di database e questo gruppo è membro di un altro gruppo di Windows che, a sua volta, è membro del ruolo del database specificato.  
  
 La funzionalità Controllo dell'account utente disponibile in [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] e Windows Server 2008 può inoltre restituire risultati diversi. Questo dipende dal fatto che l'utente acceda al server come membro di un gruppo di Windows o come utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specifico.  
  
 Questa funzione valuta l'appartenenza al ruolo, non l'autorizzazione sottostante. Il ruolo predefinito del database **db_owner** dispone ad esempio dell'autorizzazione **CONTROL DATABASE**. Se l'utente ha l'autorizzazione **CONTROL DATABASE** ma non è membro del ruolo, questa funzione indicherà correttamente che l'utente non è membro del ruolo **db_owner**, anche se ha le stesse autorizzazioni.  
  
## <a name="related-functions"></a>Funzioni correlate  
 Per determinare se l'utente corrente è membro del gruppo di Windows o del ruolo di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificato, usare [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md). Per determinare se un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è membro di un ruolo del server, usare [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DEFINITION per il ruolo del database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene indicato se l'utente corrente è un membro del ruolo predefinito del database `db_datareader`.  
  
```sql  
IF IS_ROLEMEMBER ('db_datareader') = 1  
   print 'Current user is a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') = 0  
   print 'Current user is NOT a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') IS NULL  
   print 'ERROR: The database role specified is not valid.';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
 [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)   
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  

---
description: IS_SRVROLEMEMBER (Transact-SQL)
title: IS_SRVROLEMEMBER (Transact-SQL)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_SRVROLEMEMBER_TSQL
- IS_SRVROLEMEMBER
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_SRVROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 3241a44a-6958-415b-b8b7-2a1207c36ab3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 10d6a11fccad8f4905015024f6d90a3aadb2dac4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165090"
---
# <a name="is_srvrolemember-transact-sql"></a>IS_SRVROLEMEMBER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Indica se un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è un membro del ruolo del server specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
IS_SRVROLEMEMBER ( 'role' [ , 'login' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *role* **'**  
 Nome del ruolo del server sottoposto al controllo. *role* è di tipo **sysname**.  
  
 I valori validi per *role* sono i ruoli del server definiti dall'utente e i ruoli predefiniti del server seguenti:  

- sysadmin
- serveradmin
- dbcreator
- setupadmin  
- bulkadmin
- securityadmin  
- diskadmin
- public  
- processadmin
  
 **'** *login* **'**  
 Nome dell'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da controllare. *login* è di tipo **sysname** e il valore predefinito è NULL. Se non si specifica alcun valore, il risultato sarà basato sul contesto di esecuzione corrente. Se nel parametro è inclusa la parola NULL, verrà restituito NULL.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
|Valore restituito|Descrizione|  
|------------------|-----------------|  
|0|*login* non è membro di *role*.<br /><br /> Nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] questa istruzione restituisce sempre 0.|  
|1|*login* è membro di *role*.|  
|NULL|*role* o *login* non è valido o non si è autorizzati a visualizzare l'appartenenza al ruolo.|  
  
## <a name="remarks"></a>Osservazioni  
 Usare IS_SRVROLEMEMBER per determinare se l'utente corrente è in grado di eseguire un'azione che richiede le autorizzazioni del ruolo del server.  
  
 Se per *login* viene specificato un account di accesso di Windows, ad esempio Contoso\Mary5, **IS_SRVROLEMEMBER** restituisce **NULL**, a meno che all'account di accesso non sia stato concesso o negato l'accesso diretto a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se il parametro *login* facoltativo non viene fornito e se *login* è un account di accesso a un domino Windows, può essere un membro di un ruolo predefinito del server tramite l'appartenenza a un gruppo di Windows. Per risolvere tali problemi di appartenenza indiretta, IS_SRVROLEMEMBER richiede le informazioni di appartenenza ai gruppi di Windows al controller di dominio. Se il controller di dominio non è accessibile o non risponde, **IS_SRVROLEMEMBER** restituisce le informazioni di appartenenza ai ruoli prendendo in considerazione solo l'utente e i relativi gruppi locali. Se l'utente specificato non è l'utente corrente, il valore restituito da IS_SRVROLEMEMBER potrebbe essere diverso dall'ultimo aggiornamento dei dati dell'autenticatore, ad esempio Active Directory, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se il parametro per l'account di accesso facoltativo viene fornito, l'account di accesso di Windows su cui vengono eseguite query deve essere presente in sys.server_principals. In caso contrario, IS_SRVROLEMEMBER restituisce NULL. Questo valore indica che l'account di accesso non è valido.  
  
 Quando il parametro di accesso è un account di accesso di dominio o è basato su un gruppo di Windows e il controller di dominio non è accessibile, le chiamate a IS_SRVROLEMEMBER hanno esito negativo e possono restituire dati errati o incompleti.  
  
 Se il controller di dominio non è disponibile, la chiamata a IS_SRVROLEMEMBER restituisce informazioni accurate quando l'entità di Windows può essere autenticata localmente, ad esempio come account Windows locale o come account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **IS_SRVROLEMEMBER** restituisce sempre 0 quando viene usato un gruppo di Windows come argomento di accesso e questo gruppo è membro di un altro gruppo di Windows che, a sua volta, è membro del ruolo del server specificato.  
  
 L'impostazione Controllo dell'account utente può inoltre determinare la restituzione di risultati diversi. Questo dipende dal fatto che l'utente acceda al server come membro di un gruppo di Windows o come utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specifico.  
  
 Questa funzione valuta l'appartenenza al ruolo, non l'autorizzazione sottostante. Il ruolo predefinito del server **sysadmin**, ad esempio, ha l'autorizzazione **CONTROL SERVER**. Se l'utente ha l'autorizzazione **CONTROL SERVER** ma non è membro del ruolo, questa funzione indicherà correttamente che l'utente non è membro del ruolo **sysadmin**, anche se ha le stesse autorizzazioni.  
  
## <a name="related-functions"></a>Funzioni correlate  
 Per determinare se l'utente corrente è membro del gruppo di Windows o del ruolo di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificato, usare [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md). Per determinare se un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è membro di un ruolo del database, usare [IS_ROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-rolemember-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DEFINITION per il ruolo del server.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene indicato se l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'utente corrente è un membro del ruolo predefinito del server `sysadmin`.  
  
```sql  
IF IS_SRVROLEMEMBER ('sysadmin') = 1  
   print 'Current user''s login is a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') = 0  
   print 'Current user''s login is NOT a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') IS NULL  
   print 'ERROR: The server role specified is not valid.';  
```  
  
 Nell'esempio seguente viene indicato se l'account di accesso del dominio Pat è un membro del ruolo predefinito del server **diskadmin**.  
  
```sql  
SELECT IS_SRVROLEMEMBER('diskadmin', 'Contoso\Pat');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  

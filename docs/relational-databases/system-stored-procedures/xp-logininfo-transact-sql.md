---
description: xp_logininfo (Transact-SQL)
title: xp_logininfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_logininfo_TSQL
- xp_logininfo
dev_langs:
- TSQL
helpviewer_keywords:
- xp_logininfo
ms.assetid: ee7162b5-e11f-4a0e-a09c-1878814dbbbd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6df69edd25c5c2f451e8a4aa657caf99a4c8103d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124104"
---
# <a name="xp_logininfo-transact-sql"></a>xp_logininfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni su utenti e gruppi di Windows.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
xp_logininfo [ [ @acctname = ] 'account_name' ]   
     [ , [ @option = ] 'all' | 'members' ]   
     [ , [ @privilege = ] variable_name OUTPUT]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @acctname = ] 'account_name'` Nome di un utente o gruppo di Windows a cui è stato concesso l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *account_name* è di **tipo sysname** e il valore predefinito è null. Se *account_name* non è specificato, vengono restituiti tutti i gruppi di Windows e gli utenti di Windows a cui sono state concesse in modo esplicito le autorizzazioni di accesso. *account_name* deve essere completo. ad esempio ADVWKS4\macraes o BUILTIN\Administrators.  
  
 **' all'**  |  **' members '**  
 Specifica se devono essere restituite informazioni su tutti i percorsi di autorizzazione per l'account oppure sui membri del gruppo di Windows. **\@ Option** è di tipo **varchar (10)** e il valore predefinito è null. A meno che non sia specificato **All** , viene visualizzato solo il primo percorso di autorizzazione.  
  
`[ @privilege = ] variable_name` Parametro di output che restituisce il livello di privilegio dell'account di Windows specificato. *variable_name* è di tipo **varchar (10)** e il valore predefinito è "not wanted". Il livello di privilegio restituito è **User**, **admin** o **null**.  
  
 OUTPUT  
 Quando specificato, inserisce *variable_name* nel parametro di output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome account**|**sysname**|Nome completo dell'account di Windows.|  
|**type**|**carattere (8)**|Tipo di account di Windows. I valori validi sono **utente** o **gruppo**.|  
|**privilegio**|**char(9)**|Privilegio di accesso per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I valori validi sono **admin**, **User** o **null**.|  
|**mapped login name**|**sysname**|Per gli account utente con privilegi utente, il nome dell'account di **accesso mappato** Mostra il nome dell'account di accesso con mapping che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di usare quando si accede con questo account usando le regole mappate con il nome di dominio aggiunto prima.|  
|**permission path**|**sysname**|Appartenenza al gruppo che ha permesso all'account di ottenere l'accesso.|  
  
## <a name="remarks"></a>Commenti  
 Se viene specificato *account_name* , **xp_logininfo** segnala il livello di privilegio più elevato del gruppo o dell'utente di Windows specificato. Se un utente di Windows può accedere sia come amministratore di sistema che come utente di dominio, verrà restituito come amministratore di sistema. Se l'utente è membro di più gruppi di Windows con livello di privilegio uguale, viene restituito soltanto il gruppo a cui è stato concesso per primo l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se *account_name* è un utente o un gruppo di Windows valido che non è associato a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso, viene restituito un set di risultati vuoto. Se non è possibile identificare *account_name* come un utente o un gruppo di Windows valido, viene restituito un messaggio di errore.  
  
 Se vengono specificati *account_name* e **tutti** , vengono restituiti tutti i percorsi di autorizzazione per l'utente o il gruppo di Windows. Se *account_name* è un membro di più gruppi a cui è stato concesso l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vengono restituite più righe. Le righe dei privilegi di **amministratore** vengono restituite prima delle righe dei privilegi **utente** e all'interno di un livello di privilegio le righe vengono restituite nell'ordine in cui sono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stati creati gli account di accesso corrispondenti.  
  
 Se vengono specificati *account_name* e **i membri** , viene restituito un elenco di membri di livello successivo del gruppo. Se *account_name* è un gruppo locale, l'elenco può includere utenti locali, utenti di dominio e gruppi. Se *account_name* è un account di dominio, l'elenco è costituito da utenti di dominio. Per recuperare informazioni sull'appartenenza ai gruppi, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere connesso al controller di dominio. Se il server non può contenere il controller di dominio, non verranno restituite informazioni.  
  
 **xp_logininfo** restituisce solo informazioni provenienti da Active Directory gruppi globali, non da gruppi universali.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo predefinito del database **public** nel database **Master** con l'autorizzazione Execute concessa.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono visualizzate informazioni sul gruppo di Windows `BUILTIN\Administrators`.  
  
```  
EXEC xp_logininfo 'BUILTIN\Administrators';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_denylogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure estese generali &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)  
  
  

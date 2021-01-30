---
description: sysmail_help_principalprofile_sp (Transact-SQL)
title: sysmail_help_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_principalprofile_sp_TSQL
- sysmail_help_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_principalprofile_sp
ms.assetid: 0cfd6464-09c7-4f03-9d25-58001c096a9e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb4b5cd655bf4a401d530e4c9bd52cbc9660a65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181962"
---
# <a name="sysmail_help_principalprofile_sp-transact-sql"></a>sysmail_help_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco di informazioni sulle associazioni tra i profili di Posta elettronica database e le entità di database.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_help_principalprofile_sp [ {   [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ]  
    [ [ , ] {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @principal_id = ] principal_id` ID dell'utente del database o del ruolo nel database **msdb** per l'associazione all'elenco. *principal_id* è di **tipo int** e il valore predefinito è null. È possibile specificare *principal_id* o *principal_name* .  
  
`[ @principal_name = ] 'principal_name'` Nome dell'utente del database o del ruolo nel database **msdb** per l'associazione all'elenco. *principal_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare *principal_id* o *principal_name* .  
  
`[ @profile_id = ] profile_id` ID del profilo per l'associazione da elencare. *profile_id* è di **tipo int** e il valore predefinito è null. È possibile specificare *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo per l'associazione da elencare. *profile_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare *profile_id* o *profile_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 Restituisce un set di risultati contenente le colonne elencate nella tabella seguente.  
  
| Nome colonna | Tipo di dati | Descrizione |
| ----------- | --------- | ----------- |
|**principal_id**|**int**|ID dell'utente del database.|  
|**principal_name**|**sysname**|Nome dell'utente del database.|  
|**profile_id**|**int**|Numero ID del profilo di Posta elettronica database.|  
|**profile_name**|**sysname**|Nome del profilo di Posta elettronica database.|  
|**is_default**|**bit**|Flag che indica se il profilo è il profilo predefinito per l'utente.|  
  
## <a name="remarks"></a>Commenti  
 Se **sysmail_help_principalprofile_sp** viene richiamato senza parametri, il set di risultati restituito elenca tutte le associazioni nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Negli altri casi il set di risultati conterrà le informazioni relative alle associazioni che corrispondono ai parametri specificati. Se ad esempio si specifica il nome di un profilo, la procedura elenca tutte le associazioni per tale profilo.  
  
 **sysmail_help_principalprofile_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-information-for-a-specific-association"></a>R. Visualizzazione delle informazioni relative a un'associazione specifica  
 Nell'esempio seguente viene illustrato come visualizzare le informazioni relative a tutte le associazioni tra il profilo `AdventureWorks Administrator` e l'entità `ApplicationLogin` nel database `msdb`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp  
    @principal_name = 'danw',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 Set di risultati di esempio, riformattato in base alla lunghezza di riga:  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
5            danw               9           AdventureWorks Administrator   1  
```  
  
### <a name="b-listing-information-for-all-associations"></a>B. Visualizzazione delle informazioni relative a tutte le associazioni  
 Nell'esempio seguente viene illustrato come visualizzare le informazioni relative a tutte le associazioni nell'istanza.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp ;  
```  
  
 Set di risultati di esempio, riformattato in base alla lunghezza di riga:  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
6            terrid             3           Product Update Profile         1  
5            danw               9           AdventureWorks Administrator   1  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  

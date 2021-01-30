---
description: sysmail_help_profile_sp (Transact-SQL)
title: sysmail_help_profile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_profile_sp_TSQL
- sysmail_help_profile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_profile_sp
ms.assetid: d7169a8e-92b1-49eb-9124-3b2f69755ddb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 865e871463d8ce15ef08db2d6431634e6e6bc08c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181945"
---
# <a name="sysmail_help_profile_sp-transact-sql"></a>sysmail_help_profile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza le informazioni relative a uno o più profili di posta.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_help_profile_sp  [   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo per cui restituire informazioni. *profile_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo per cui restituire informazioni. *profile_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Viene restituito un set di risultati con le colonne seguenti.  
  
| Nome colonna | Tipo di dati | Descrizione |
| ----------- | --------- | ----------- |
|**profile_id**|**int**|ID del profilo.|  
|**nome**|**sysname**|Nome del profilo.|  
|**description**|**nvarchar(256)**|Descrizione del profilo.|  
  
## <a name="remarks"></a>Commenti  
 Quando viene specificato un nome di profilo o un ID profilo, **sysmail_help_profile_sp** restituisce informazioni su tale profilo. In caso contrario, **sysmail_help_profile_sp** restituisce informazioni su ogni profilo nell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza.  
  
 Il stored procedure **sysmail_help_profile_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 **A. Visualizzazione di tutti i profili**  
  
 Nell'esempio seguente vengono visualizzati tutti i profili disponibili nell'istanza.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profile_sp;  
```  
  
 Quello che segue è un set di risultati di esempio, riformattato per adattarlo alla lunghezza di riga:  
  
```  
profile_id  name                          description  
----------- ----------------------------- ------------------------------  
56          AdventureWorks Administrator  Administrative mail profile.    
57          AdventureWorks Operator       Operator mail profile.          
```  
  
 **B. Visualizzazione di un profilo specifico**  
  
 Nell'esempio seguente vengono visualizzate le informazioni relative al profilo `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profile_sp  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 Quello che segue è un set di risultati di esempio, riformattato per adattarlo alla lunghezza di riga:  
  
```  
profile_id  name                          description  
----------- ----------------------------- ------------------------------  
56          AdventureWorks Administrator  Administrative mail profile.    
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  

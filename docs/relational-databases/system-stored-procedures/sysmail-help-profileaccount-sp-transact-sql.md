---
description: sysmail_help_profileaccount_sp (Transact-SQL)
title: sysmail_help_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_profileaccount_sp_TSQL
- sysmail_help_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_profileaccount_sp
ms.assetid: 3ea68271-0a6b-4d77-991c-4757f48f747a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45b542047875527a26d9c4854b223e626efda5ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181938"
---
# <a name="sysmail_help_profileaccount_sp-transact-sql"></a>sysmail_help_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elenca gli account associati a uno o più profili di Posta elettronica database.  
    
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_help_profileaccount_sp  
   {   [ @profile_id = ] profile_id   
      | [ @profile_name = ] 'profile_name' }  
   [ , {   [ @account_id = ] account_id  
         | [ @account_name = ] 'account_name' } ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id` ID del profilo da elencare. *profile_id* è di **tipo int** e il valore predefinito è null. È necessario specificare *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nome del profilo da elencare. *profile_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *profile_id* o *profile_name* .  
  
`[ @account_id = ] account_id` ID dell'account da elencare. *account_id* è di **tipo int** e il valore predefinito è null. Quando *account_id* e *ACCOUNT_NAME* sono entrambi null, elenca tutti gli account nel profilo.  
  
`[ @account_name = ] 'account_name'` Nome dell'account da elencare. *account_name* è di **tipo sysname** e il valore predefinito è null. Quando *account_id* e *ACCOUNT_NAME* sono entrambi null, elenca tutti gli account nel profilo.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 Viene restituito un set di risultati con le colonne seguenti.  
  
| Nome colonna | Tipo di dati | Descrizione |
| ----------- | --------- | ----------- |
|**profile_id**|**int**|ID del profilo.|  
|**profile_name**|**sysname**|Nome del profilo.|  
|**account_id**|**int**|ID dell'account.|  
|**account_name**|**sysname**|Nome dell'account.|  
|**sequence_number**|**int**|Numero di sequenza dell'account all'interno del profilo.|  
  
## <a name="remarks"></a>Commenti  
 Quando non viene specificato alcun *profile_id* o *profile_name* , questo stored procedure restituisce informazioni per ogni profilo nell'istanza.  
  
 Il stored procedure **sysmail_help_profileaccount_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 **A. Visualizzazione di un elenco degli account per un profilo specifico in base al nome**  
  
 Nell'esempio seguente viene visualizzato un elenco di informazioni per il profilo `AdventureWorks Administrator` specificando il nome del profilo.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
   @profile_name = 'AdventureWorks Administrator';  
```  
  
 Quello che segue è un set di risultati di esempio, modificato per adattarlo alla lunghezza di riga.  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **A. Visualizzazione di un elenco degli account per un profilo specifico in base all'ID del profilo**  
  
 Nell'esempio seguente viene visualizzato un elenco di informazioni per il profilo `AdventureWorks Administrator` specificando l'ID del profilo.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
    @profile_id = 131 ;  
```  
  
 Quello che segue è un set di risultati di esempio, modificato per adattarlo alla lunghezza di riga.  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **C. Visualizzazione di un elenco degli account di tutti i profili**  
  
 Nell'esempio seguente viene visualizzato un elenco di account per tutti i profili nell'istanza.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp;  
```  
  
 Quello che segue è un set di risultati di esempio, modificato per adattarlo alla lunghezza di riga.  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
106         AdventureWorks Operator      210         Operator-MainServer  1  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Creazione di un account Posta elettronica database](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  

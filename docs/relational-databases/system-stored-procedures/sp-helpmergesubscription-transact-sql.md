---
description: sp_helpmergesubscription (Transact-SQL)
title: sp_helpmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergesubscription
- sp_helpmergesubscription_TSQL
helpviewer_keywords:
- sp_helpmergesubscription
ms.assetid: da564112-f769-4e67-9251-5699823e8c86
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274048dcfcd76e815d3f4ec159dded0571f66e43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210886"
---
# <a name="sp_helpmergesubscription-transact-sql"></a>sp_helpmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni su una sottoscrizione, sia push che pull, di una pubblicazione di tipo merge. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione o nel database di sottoscrizione di un Sottoscrittore di ripubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergesubscription [ [ @publication=] 'publication']  
    [ , [ @subscriber=] 'subscriber']  
    [ , [ @subscriber_db=] 'subscriber_db']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
    [ , [ @found=] 'found' OUTPUT]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** . È necessario che la pubblicazione esista già e che sia conforme alle regole per gli identificatori. Se è NULL o **%** , vengono restituite informazioni su tutte le pubblicazioni e le sottoscrizioni di tipo merge nel database corrente.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e il valore predefinito è **%** . Se è NULL o %, vengono restituite informazioni su tutte le sottoscrizioni della pubblicazione specificata.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti i database di sottoscrizione.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. Il server di pubblicazione deve essere un server valido. *Publisher* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti i server di pubblicazione.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti i database del server di pubblicazione.  
  
`[ @subscription_type = ] 'subscription_type'` Tipo di sottoscrizione. *subscription_type* è di **tipo nvarchar (15)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**push** (impostazione predefinita)|Sottoscrizione push|  
|**tirare**|Sottoscrizione pull|  
|**sia**|Sottoscrizione sia push che pull|  
  
`[ @found = ] 'found'OUTPUT` Flag che indica la restituzione di righe. *trovato* è di **tipo int** e un parametro di output e il valore predefinito è null. **1** indica che la pubblicazione è stata trovata. **0** indica che la pubblicazione non è stata trovata.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**sysname**|Nome della sottoscrizione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**Sottoscrittore**|**sysname**|Nome del Sottoscrittore.|  
|**subscriber_db**|**sysname**|Nome del database di sottoscrizione.|  
|**Stato**|**int**|Stato della sottoscrizione:<br /><br /> **0** = tutti i processi sono in attesa di essere avviati<br /><br /> **1** = uno o più processi vengono avviati<br /><br /> **2** = tutti i processi sono stati eseguiti correttamente<br /><br /> **3** = almeno un processo è in esecuzione<br /><br /> **4** = tutti i processi sono pianificati e inattivi<br /><br /> **5** = è in corso un tentativo di esecuzione di almeno un processo dopo un errore precedente<br /><br /> **6** = almeno un processo non è stato eseguito correttamente|  
|**subscriber_type**|**int**|Tipo di Sottoscrittore.|  
|**subscription_type**|**int**|Tipo di sottoscrizione:<br /><br /> **0** = push<br /><br /> **1** = pull<br /><br /> **2** = entrambi|  
|**priority**|**float (8)**|Numero che indica il livello di priorità della sottoscrizione.|  
|**sync_type**|**tinyint**|Tipo di sincronizzazione della sottoscrizione.|  
|**description**|**nvarchar(255)**|Breve descrizione della sottoscrizione di tipo merge.|  
|**merge_jobid**|**binary(16)**|ID di processo dell'agente di merge.|  
|**full_publication**|**tinyint**|Specifica se la sottoscrizione si riferisce a una pubblicazione completa o filtrata.|  
|**offload_enabled**|**bit**|Specifica se per un agente di replica è impostata l'esecuzione con ripartizione del carico di lavoro nel Sottoscrittore. Se è NULL, l'agente viene eseguito nel server di pubblicazione.|  
|**offload_server**|**sysname**|Nome del server in cui è in esecuzione l'agente.|  
|**use_interactive_resolver**|**int**|Specifica se durante la fase di riconciliazione viene utilizzato il sistema di risoluzione dei conflitti interattivo. Se è **0**, il sistema di risoluzione interattivo non viene utilizzato.|  
|**hostname**|**sysname**|Valore fornito quando una sottoscrizione viene filtrata in base al valore della funzione [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) .|  
|**subscriber_security_mode**|**smallint**|Modalità di sicurezza nel Sottoscrittore, dove **1** indica l'autenticazione di Windows e **0** indica [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di.|  
|**subscriber_login**|**sysname**|Nome dell'account di accesso nel Sottoscrittore.|  
|**subscriber_password**|**sysname**|La password effettiva per il Sottoscrittore non viene mai restituita. Il risultato è mascherato da una **\*\*\*\*\*\*** stringa "".|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergesubscription** viene utilizzata nella replica di tipo merge per restituire le informazioni sulla sottoscrizione archiviate nel server di pubblicazione o nel Sottoscrittore.  
  
 Per le sottoscrizioni anonime, il valore *subscription_type* è sempre **1** (pull). Per informazioni sulle sottoscrizioni anonime, è tuttavia necessario eseguire [sp_helpmergepullsubscription](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md) nel Sottoscrittore.  
  
## <a name="permissions"></a>Autorizzazioni  
 È possibile eseguire **sp_helpmergesubscription** solo i membri del ruolo predefinito del server **sysadmin** , il **db_owner** ruolo predefinito del database o l'elenco di accesso alla pubblicazione per la pubblicazione a cui appartiene la sottoscrizione.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addmergesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

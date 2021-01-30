---
description: sp_helpdynamicsnapshot_job (Transact-SQL)
title: sp_helpdynamicsnapshot_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdynamicsnapshot_TSQL
- sp_helpdynamicsnapshot_job_TSQL
- job_TSQL
- helpdynamicsnapshot
- job
- sp_helpdynamicsnapshot
- sp_helpdynamicsnapshot_job
- helpdynamicsnapshot_TSQL
helpviewer_keywords:
- sp_helpdynamicsnapshot_job
ms.assetid: d6dfdf26-f874-495f-a8a6-8780699646d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: faf8fcfe5636dc588b111701c5b2cf0668feed78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204433"
---
# <a name="sp_helpdynamicsnapshot_job-transact-sql"></a>sp_helpdynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sui processi di agente che generano snapshot dei dati filtrati. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpdynamicsnapshot_job [ [ @publication = ] 'publication' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' ]  
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti i processi di snapshot dei dati filtrati che corrispondono al *dynamic_snapshot_jobid* e *dynamic_snapshot_jobname* specificati per tutte le pubblicazioni.  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` Nome di un processo di snapshot dei dati filtrati. *dynamic_snapshot_jobname* è di **tipo sysname** e il valore predefinito è **%** ', che restituisce tutti i processi dinamici per una pubblicazione con la *dynamic_snapshot_jobid* specificata. Se quando è stato creato il processo non è stato specificato in modo esplicito un nome di processo, il nome del processo verrà indicato nel formato seguente:  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` Identificatore di un processo di snapshot dei dati filtrati. *dynamic_snapshot_jobid* è di tipo **uniqueidentifier** e il valore predefinito è null, che restituisce tutti i processi di snapshot corrispondenti al *dynamic_snapshot_jobname* specificato.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifica il processo di snapshot dei dati filtrati.|  
|**job_name**|**sysname**|Nome del processo di snapshot dei dati filtrati.|  
|**job_id**|**uniqueidentifier**|Identifica il [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processo di Agent nel server di distribuzione.|  
|**dynamic_filter_login**|**sysname**|Valore utilizzato per la valutazione della funzione [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) in un filtro di riga con parametri definito per la pubblicazione.|  
|**dynamic_filter_hostname**|**sysname**|Valore utilizzato per la valutazione della funzione [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) in un filtro di riga con parametri definito per la pubblicazione.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Percorso della cartella da cui i file di snapshot vengono letti se viene utilizzato un filtro di riga con parametri.|  
|**frequency_type**|**int**|Frequenza pianificata per l'esecuzione dell'agente. I possibile valori sono i seguenti.<br /><br /> **1** = una volta<br /><br /> **2** = su richiesta<br /><br /> **4** = giornaliero<br /><br /> **8** = settimanale<br /><br /> **16** = mensile<br /><br /> **32** = mensile relativo<br /><br /> **64** = avvio automatico<br /><br /> **128** = ricorrente|  
|**frequency_interval**|**int**|Giorni in cui l'agente viene eseguito. I possibili valori sono i seguenti.<br /><br /> **1** = domenica<br /><br /> **2** = lunedì<br /><br /> **3** = martedì<br /><br /> **4** = mercoledì<br /><br /> **5** = giovedì<br /><br /> **6** = venerdì<br /><br /> **7** = sabato<br /><br /> **8** = giorno<br /><br /> **9** = giorni feriali<br /><br /> **10** = giorni festivi|  
|**frequency_subday_type**|**int**|Tipo che definisce la frequenza con cui l'agente viene eseguito quando *frequency_type* è **4** (giornaliera). i possibili valori sono i seguenti.<br /><br /> **1** = all'ora specificata<br /><br /> **2** = secondi<br /><br /> **4** = minuti<br /><br /> **8** = ore|  
|**frequency_subday_interval**|**int**|Numero di intervalli di *frequency_subday_type* che si verificano tra l'esecuzione pianificata dell'agente.|  
|**frequency_relative_interval**|**int**|Settimana in cui l'agente viene eseguito in un determinato mese quando *frequency_type* è **32** (mensile relativo). i possibili valori sono i seguenti.<br /><br /> **1** = prima<br /><br /> **2** = secondo<br /><br /> **4** = terzo<br /><br /> **8** = quarto<br /><br /> **16** = Ultima|  
|**frequency_recurrence_factor**|**int**|Numero di settimane o mesi tra le esecuzioni pianificate dell'agente.|  
|**active_start_date**|**int**|Data della prima esecuzione pianificata dell'agente nel formato YYYYMMDD.|  
|**active_end_date**|**int**|Data dell'ultima esecuzione pianificata dell'agente nel formato YYYYMMDD.|  
|**active_start_time**|**int**|Ora della prima esecuzione pianificata dell'agente nel formato HHMMSS.|  
|**active_end_time**|**int**|Ora dell'ultima esecuzione pianificata dell'agente nel formato HHMMSS.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpdynamicsnapshot_job** viene utilizzata nella replica di tipo merge.  
  
 Se vengono utilizzati tutti i valori predefiniti dei parametri, verranno restituite informazioni su tutti i processi di snapshot dei dati partizionati per l'intero database di pubblicazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , del ruolo predefinito del database **db_owner** e dell'elenco di accesso alla pubblicazione possono eseguire **sp_helpdynamicsnapshot_job**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

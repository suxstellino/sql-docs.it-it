---
description: sp_helppublication_snapshot (Transact-SQL)
title: sp_helppublication_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppublication_snapshot
- sp_helppublication_snapshot_TSQL
helpviewer_keywords:
- sp_helppublication_snapshot
ms.assetid: 97b4a7ae-40a5-4328-88f1-ff5d105bbb34
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ae7357cd8bc8f03805c6de948095ff2dbc4d541
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210867"
---
# <a name="sp_helppublication_snapshot-transact-sql"></a>sp_helppublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sull'agente snapshot per una pubblicazione specifica. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helppublication_snapshot [ @publication = ] 'publication'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  non utilizzare *Publisher* quando si aggiunge un articolo a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID dell'agente snapshot.|  
|**nome**|**nvarchar (100)**|Nome dell'agente snapshot.|  
|**publisher_security_mode**|**smallint**|Modalità di sicurezza utilizzata dall'agente durante la connessione al server di pubblicazione. Le possibili modalità sono le seguenti:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows.|  
|**publisher_login**|**sysname**|Account di accesso utilizzato per la connessione al server di pubblicazione.|  
|**publisher_password**|**nvarchar (524)**|Per motivi di sicurezza, **\*\*\*\*\*\*\*\*\*\*** viene sempre restituito un valore.|  
|**job_id**|**uniqueidentifier**|ID univoco del processo dell'agente.|  
|**job_login**|**nvarchar(512)**|Account di Windows utilizzato per l'esecuzione dell'agente snapshot, restituito nel formato *dominio* \\ *nomeutente*.|  
|**job_password**|**sysname**|Per motivi di sicurezza, **\*\*\*\*\*\*\*\*\*\*** viene sempre restituito un valore.|  
|**schedule_name**|**sysname**|Nome della pianificazione utilizzata per il processo dell'agente corrente.|  
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
 **sp_help_publication_snapshot** viene utilizzato in tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di pubblicazione o i membri del ruolo predefinito del database **db_owner** nel database di pubblicazione possono eseguire **sp_help_publication_snapshot**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà della pubblicazione](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addpublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_dropmergepublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_droppublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)  
  
  

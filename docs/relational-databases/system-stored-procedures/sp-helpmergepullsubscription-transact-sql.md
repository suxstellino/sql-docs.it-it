---
description: sp_helpmergepullsubscription (Transact-SQL)
title: sp_helpmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepullsubscription
- sp_helpmergepullsubscription_TSQL
helpviewer_keywords:
- sp_helpmergepullsubscription
ms.assetid: 6f3125f3-0dfa-40bd-b725-8aa1591234f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 91266e8d825fd669979e925753804230fd62027c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160594"
---
# <a name="sp_helpmergepullsubscription-transact-sql"></a>sp_helpmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulle sottoscrizioni pull esistenti in un Sottoscrittore. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergepullsubscription [ [ @publication=] 'publication']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
```  
  
## <a name="argument"></a>Argomento  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** . Se la *pubblicazione* è **%** , vengono restituite informazioni su tutte le pubblicazioni e le sottoscrizioni di tipo merge nel database corrente.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è **%** .  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è **%** .  
  
`[ @subscription_type = ] 'subscription_type'` Indica se visualizzare le sottoscrizioni pull. *subscription_type* è di **tipo nvarchar (10)** e il valore predefinito è **' pull '**. I valori validi sono **' push '**, **' pull '** o **' both '**.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**nvarchar (1000)**|Nome della sottoscrizione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**Sottoscrittore**|**sysname**|Nome del Sottoscrittore.|  
|**subscription_db**|**sysname**|Nome del database di sottoscrizione.|  
|**Stato**|**int**|Stato della sottoscrizione:<br /><br /> **0** = sottoscrizione inattiva<br /><br /> **1** = sottoscrizione attiva<br /><br /> **2** = sottoscrizione eliminata<br /><br /> **3** = sottoscrizione scollegata<br /><br /> **4** = sottoscrizione collegata<br /><br /> **5** = la sottoscrizione è stata contrassegnata per la reinizializzazione con il caricamento<br /><br /> **6** = connessione della sottoscrizione non riuscita<br /><br /> **7** = sottoscrizione ripristinata dal backup|  
|**subscriber_type**|**int**|Tipo di Sottoscrittore:<br /><br /> **1** = globale<br /><br /> **2** = locale<br /><br /> **3** = Anonimo|  
|**subscription_type**|**int**|Tipo di sottoscrizione:<br /><br /> **0** = push<br /><br /> **1** = pull<br /><br /> **2** = Anonimo|  
|**priority**|**float (8)**|Priorità della sottoscrizione. Il valore deve essere minore di **100,00**.|  
|**sync_type**|**tinyint**|Tipo di sincronizzazione per la sottoscrizione:<br /><br /> **1** = automatico<br /><br /> **2** = lo snapshot non viene utilizzato.|  
|**description**|**nvarchar(255)**|Breve descrizione della sottoscrizione pull.|  
|**merge_jobid**|**binary(16)**|ID di processo dell'agente di merge.|  
|**enabled_for_syncmgr**|**int**|Indica se è possibile sincronizzare la sottoscrizione tramite Gestione sincronizzazione [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**last_updated**|**nvarchar (26)**|Ora in cui l'agente di merge ha eseguito l'ultima sincronizzazione della sottoscrizione.|  
|**publisher_login**|**sysname**|Nome dell'account di accesso del server di pubblicazione.|  
|**publisher_password**|**sysname**|Password del server di pubblicazione.|  
|**publisher_security_mode**|**int**|Modalità di sicurezza del server di pubblicazione:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows|  
|**distribuzione**|**sysname**|Nome del server di distribuzione.|  
|**distributor_login**|**sysname**|Nome dell'account di accesso del server di distribuzione.|  
|**distributor_password**|**sysname**|Password per il server di distribuzione.|  
|**distributor_security_mode**|**int**|Modalità di sicurezza del server di distribuzione:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows|  
|**ftp_address**|**sysname**|Disponibile per compatibilità con le versioni precedenti. Indirizzo di rete del servizio FTP per il server di distribuzione.|  
|**ftp_port**|**int**|Disponibile per compatibilità con le versioni precedenti. Numero di porta del servizio FTP per il database di distribuzione.|  
|**ftp_login**|**sysname**|Disponibile per compatibilità con le versioni precedenti. Nome utente utilizzato per la connessione al servizio FTP.|  
|**ftp_password**|**sysname**|Disponibile per compatibilità con le versioni precedenti. Password utente utilizzata per la connessione al servizio FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Percorso di archiviazione della cartella snapshot, se diverso o aggiuntivo rispetto a quello predefinito.|  
|**working_directory**|**nvarchar(255)**|Percorso completo della directory in cui vengono trasferiti i file di snapshot tramite il servizio FTP, se l'opzione corrispondente è stata specificata.|  
|**use_ftp**|**bit**|Indica che la sottoscrizione viene inserita nella pubblicazione tramite Internet e che le proprietà di indirizzamento FTP sono configurate. Se è **0**, la sottoscrizione non utilizza FTP. Se è **1**, la sottoscrizione utilizza FTP.|  
|**offload_agent**|**bit**|Specifica se l'agente può essere attivato ed eseguito in remoto. Se è **0**, l'agente non può essere attivato in remoto.|  
|**offload_server**|**sysname**|Nome del server utilizzato per l'attivazione remota.|  
|**use_interactive_resolver**|**int**|Specifica se durante la fase di riconciliazione viene utilizzato il sistema di risoluzione dei conflitti interattivo. Se è **0**, il sistema di risoluzione interattivo non viene utilizzato.|  
|**subid**|**uniqueidentifier**|ID del Sottoscrittore.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Percorso della cartella in cui vengono salvati i file di snapshot.|  
|**last_sync_status**|**int**|Stato della sincronizzazione:<br /><br /> **1** = avvio in corso<br /><br /> **2** = operazione completata<br /><br /> **3** = in corso<br /><br /> **4** = inattivo<br /><br /> **5** = nuovo tentativo dopo un errore precedente<br /><br /> **6** = operazione non riuscita<br /><br /> **7** = convalida non riuscita<br /><br /> **8** = convalida superata<br /><br /> **9** = è stato richiesto un arresto|  
|**last_sync_summary**|**sysname**|Descrizione dei risultati dell'ultima sincronizzazione.|  
|**use_web_sync**|**bit**|Specifica se la sottoscrizione può essere sincronizzata tramite HTTPS, dove il valore **1** indica che questa funzionalità è abilitata.|  
|**internet_url**|**nvarchar(260)**|URL che rappresenta la posizione del listener per la replica per la sincronizzazione Web.|  
|**internet_login**|**nvarchar(128)**|Account di accesso utilizzato dall'agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite l'autenticazione di base.|  
|**internet_password**|**nvarchar (524)**|Password di accesso utilizzata dall'agente di merge per la connessione al server Web in cui viene eseguita la sincronizzazione Web tramite l'autenticazione di base.|  
|**internet_security_mode**|**int**|Modalità di autenticazione utilizzata per la connessione al server Web in cui viene eseguita la sincronizzazione Web. Il valore **1** indica l'autenticazione di Windows, mentre il valore **0** indica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di.|  
|**internet_timeout**|**int**|Periodo di tempo, espresso in secondi, al termine del quale una richiesta di sincronizzazione Web scade.|  
|**hostname**|**nvarchar(128)**|Specifica un valore di overload per [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) quando questa funzione viene utilizzata nella clausola WHERE di un filtro di riga con parametri.|  
|**job_login**|**nvarchar(512)**|Account di Windows utilizzato per l'esecuzione dell'agente di merge, restituito nel formato *dominio* \\ *nomeutente*.|  
|**job_password**|**sysname**|Per motivi di sicurezza, **\*\*\*\*\*\*\*\*\*\*** viene sempre restituito il valore "".|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergepullsubscription** viene utilizzata nella replica di tipo merge. Nel set di risultati, la data restituita in **last_updated** è formattata come *AAAAMMGG HH: mm: SS. fff*.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** e del ruolo predefinito del database **db_owner** possono eseguire **sp_helpmergepullsubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

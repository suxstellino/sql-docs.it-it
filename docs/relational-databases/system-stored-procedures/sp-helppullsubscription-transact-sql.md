---
description: sp_helppullsubscription (Transact-SQL)
title: sp_helppullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppullsubscription_TSQL
- sp_helppullsubscription
helpviewer_keywords:
- sp_helppullsubscription
ms.assetid: a0d9c3f1-1fe9-497c-8e2f-5b74f47a7346
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3e7711e7034190f5862d512c3da02df685c3aaf5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210845"
---
# <a name="sp_helppullsubscription-transact-sql"></a>sp_helppullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Visualizza informazioni su una o più sottoscrizioni nel Sottoscrittore. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helppullsubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @publication = ] 'publication' ]  
    [ , [ @show_push = ] 'show_push' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server remoto. *Publisher* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni per tutti i server di pubblicazione.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è **%** , che restituisce tutti i database del server di pubblicazione.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** , che restituisce tutte le pubblicazioni. Se questo parametro è uguale a ALL, vengono restituite solo le sottoscrizioni pull con independent_agent = **0** .  
  
`[ @show_push = ] 'show_push'` Indica se devono essere restituite tutte le sottoscrizioni push. *show_push* è di **tipo nvarchar (5)** e il valore predefinito è false, che non restituisce le sottoscrizioni push.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**database del server di pubblicazione**|**sysname**|Nome del database del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**independent_agent**|**bit**|Indica se per questa pubblicazione è disponibile un agente di distribuzione autonomo.|  
|**tipo di sottoscrizione**|**int**|Tipo di sottoscrizione della pubblicazione.|  
|**agente di distribuzione**|**nvarchar (100)**|Agente di distribuzione che gestisce la sottoscrizione.|  
|**publication description**|**nvarchar(255)**|Descrizione della pubblicazione.|  
|**last updating time**|**date**|Data e ora dell'aggiornamento delle informazioni della sottoscrizione. Si tratta di una stringa UNICODE con data ISO (114) + ora ODBC (121). Il formato è yyyymmdd hh:mi:sss.mmm dove 'yyyy' rappresenta l'anno, 'mm' il mese, 'dd' il giorno, 'hh' l'ora, 'mi' i minuti, 'sss' i secondi e 'mmm' i millisecondi.|  
|**nome sottoscrizione**|**varchar (386)**|Nome della sottoscrizione.|  
|**timestamp ultima transazione**|**varbinary(16)**|Timestamp dell'ultima transazione replicata.|  
|**modalità di aggiornamento**|**tinyint**|Tipo di aggiornamenti consentiti.|  
|**distribution agent job_id**|**int**|ID di processo dell'agente di distribuzione.|  
|**enabled_for_synmgr**|**int**|Indica se è possibile sincronizzare la sottoscrizione tramite Gestione sincronizzazione [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**GUID sottoscrizione**|**binary(16)**|Identificatore globale della versione della sottoscrizione nella pubblicazione.|  
|**subid**|**binary(16)**|Identificatore globale di una sottoscrizione anonima.|  
|**immediate_sync**|**bit**|Indica se i file di sincronizzazione vengono creati o ricreati a ogni esecuzione dell'agente snapshot.|  
|**account di accesso del server di pubblicazione**|**sysname**|ID dell'account di accesso utilizzato nel server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**password del server di pubblicazione**|**nvarchar (524)**|Password (crittografata) utilizzata dal server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher security_mode**|**int**|Modalità di sicurezza implementata nel server di pubblicazione:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows<br /><br /> **2** = i trigger di sincronizzazione utilizzano una voce **sysservers** statica per eseguire una chiamata di procedura remota (RPC) e il server di *pubblicazione* deve essere definito nella tabella **sysservers** come server remoto o collegato.|  
|**distribuzione**|**sysname**|Nome del server di distribuzione.|  
|**distributor_login**|**sysname**|ID dell'account di accesso utilizzato nel server di distribuzione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_password**|**nvarchar (524)**|Password (crittografata) utilizzata nel server di distribuzione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_security_mode**|**int**|Modalità di sicurezza implementata nel server di distribuzione:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows|  
|**ftp_address**|**sysname**|Disponibile solo per compatibilità con le versioni precedenti.|  
|**ftp_port**|**int**|Disponibile solo per compatibilità con le versioni precedenti.|  
|**ftp_login**|**sysname**|Disponibile solo per compatibilità con le versioni precedenti.|  
|**ftp_password**|**nvarchar (524)**|Disponibile solo per compatibilità con le versioni precedenti.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Percorso di archiviazione della cartella snapshot, se diverso o aggiuntivo rispetto a quello predefinito.|  
|**working_directory**|**nvarchar(255)**|Percorso completo della directory in cui vengono trasferiti i file di snapshot tramite il servizio FTP, se l'opzione corrispondente è stata specificata.|  
|**use_ftp**|**bit**|Indica che la sottoscrizione viene inserita nella pubblicazione tramite Internet e che le proprietà di indirizzamento FTP sono configurate. Se è **0**, la sottoscrizione non utilizza FTP. Se è **1**, la sottoscrizione utilizza FTP.|  
|**publication_type**|**int**|Specifica il tipo di replica della pubblicazione:<br /><br /> **0** = replica transazionale<br /><br /> **1** = replica snapshot<br /><br /> **2** = replica di tipo merge|  
|**dts_package_name**|**sysname**|Specifica il nome del pacchetto Data Transformation Services (DTS).|  
|**dts_package_location**|**int**|Posizione in cui è archiviato il pacchetto DTS:<br /><br /> **0** = server di distribuzione<br /><br /> **1** = Sottoscrittore|  
|**offload_agent**|**bit**|Specifica se l'agente può essere attivato in remoto. Se è **0**, l'agente non può essere attivato in remoto.|  
|**offload_server**|**sysname**|Nome di rete del server utilizzato per l'attivazione remota.|  
|**last_sync_status**|**int**|Stato della sottoscrizione:<br /><br /> **0** = tutti i processi sono in attesa di essere avviati<br /><br /> **1** = uno o più processi vengono avviati<br /><br /> **2** = tutti i processi sono stati eseguiti correttamente<br /><br /> **3** = almeno un processo è in esecuzione<br /><br /> **4** = tutti i processi sono pianificati e inattivi<br /><br /> **5** = è in corso un tentativo di esecuzione di almeno un processo dopo un errore precedente<br /><br /> **6** = almeno un processo non è stato eseguito correttamente|  
|**last_sync_summary**|**sysname**|Descrizione dei risultati dell'ultima sincronizzazione.|  
|**last_sync_time**|**datetime**|Data e ora dell'aggiornamento delle informazioni della sottoscrizione. Si tratta di una stringa UNICODE con data ISO (114) + ora ODBC (121). Il formato è yyyymmdd hh:mi:sss.mmm dove 'yyyy' rappresenta l'anno, 'mm' il mese, 'dd' il giorno, 'hh' l'ora, 'mi' i minuti, 'sss' i secondi e 'mmm' i millisecondi.|  
|**job_login**|**nvarchar(512)**|Account di Windows utilizzato per l'esecuzione dell'agente di distribuzione, restituito nel formato *dominio* \\ *nomeutente*.|  
|**job_password**|**sysname**|Per motivi di sicurezza, **\*\*\*\*\*\*\*\*\*\*** viene sempre restituito il valore "".|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helppullsubscription** viene utilizzata per la replica snapshot e transazionale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_helppullsubscription** .  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addpullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_droppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

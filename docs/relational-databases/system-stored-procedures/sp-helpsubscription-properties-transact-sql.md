---
description: sp_helpsubscription_properties (Transact-SQL)
title: sp_helpsubscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_properties
- sp_helpsubscription_properties_TSQL
helpviewer_keywords:
- sp_helpsubscription_properties
ms.assetid: 7a76a645-97eb-47ac-b3ea-e2d75012cbed
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 681a72a604af765311bb10833cc17842819e97f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103206"
---
# <a name="sp_helpsubscription_properties-transact-sql"></a>sp_helpsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Recupera le informazioni sulla sicurezza dalla tabella [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) . Questa stored procedure viene eseguita nel Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpsubscription_properties [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db =] 'publisher_db' ]   
    [ , [ @publication =] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti i server di pubblicazione.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutti i database del server di pubblicazione.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** , che restituisce informazioni su tutte le pubblicazioni.  
  
`[ @publication_type = ] publication_type` Tipo di pubblicazione. *publication_type* è di **tipo int** e il valore predefinito è null. Se specificato, *publication_type* deve essere uno dei valori seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Pubblicazione transazionale|  
|**1**|Pubblicazione snapshot|  
|**2**|Pubblicazione di tipo merge|  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**publication_type**|**int**|Tipo di pubblicazione:<br /><br /> **0** = transazionale<br /><br /> **1** = snapshot<br /><br /> **2** = Unione|  
|**publisher_login**|**sysname**|ID dell'account di accesso utilizzato nel server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_password**|**nvarchar (524)**|Password (crittografata) utilizzata nel server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_security_mode**|**int**|Modalità di sicurezza utilizzata nel server di pubblicazione:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows|  
|**distribuzione**|**sysname**|Nome del server di distribuzione.|  
|**distributor_login**|**sysname**|Account di accesso per il server di distribuzione.|  
|**distributor_password**|**nvarchar (524)**|Password (crittografata) per il server di distribuzione.|  
|**distributor_security_mode**|**int**|Modalità di sicurezza utilizzata nel server di distribuzione:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows|  
|**ftp_address**|**sysname**|Disponibile solo per compatibilità con le versioni precedenti. Indirizzo di rete del servizio FTP (File Transfer Protocol) per il server di distribuzione.|  
|**ftp_port**|**int**|Disponibile solo per compatibilità con le versioni precedenti. Numero di porta del servizio FTP per il server di distribuzione.|  
|**ftp_login**|**sysname**|Disponibile solo per compatibilità con le versioni precedenti. Nome utente utilizzato per la connessione al servizio FTP.|  
|**ftp_password**|**nvarchar (524)**|Disponibile solo per compatibilità con le versioni precedenti. Password utente utilizzata per la connessione al servizio FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Specifica la posizione della cartella alternativa per lo snapshot.|  
|**working_directory**|**nvarchar(255)**|Nome della directory di lavoro utilizzata per archiviare i file dei dati e di schema.|  
|**use_ftp**|**bit**|Specifica l'utilizzo di FTP anziché del protocollo normale per il recupero di snapshot. Se è **1**, viene utilizzato il protocollo FTP.|  
|**dts_package_name**|**sysame**|Specifica il nome del pacchetto Data Transformation Services (DTS).|  
|**dts_package_password**|**nvarchar (524)**|Password del pacchetto, se è disponibile.|  
|**dts_package_location**|**int**|Posizione di archiviazione del pacchetto DTS.<br /><br /> **0** = il percorso del pacchetto si trova nel database di distribuzione.<br /><br /> **1** = il percorso del pacchetto si trova nel Sottoscrittore.|  
|**offload_agent**|**bit**|Specifica se l'agente può essere attivato in remoto. Se è **0**, l'agente non può essere attivato in remoto.|  
|**offload_server**|**sysname**|Nome di rete del server utilizzato per l'attivazione remota.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Specifica il percorso della cartella in cui vengono salvati i file di snapshot.|  
|**use_web_sync**|**bit**|Specifica se la sottoscrizione può essere sincronizzata tramite HTTPS, dove il valore **1** indica che questa funzionalità è abilitata.|  
|**internet_url**|**nvarchar(260)**|URL che rappresenta la posizione del listener per la replica per la sincronizzazione Web.|  
|**internet_login**|**nvarchar(128)**|Account di accesso utilizzato dall'agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite l'autenticazione di base.|  
|**internet_password**|**nvarchar (524)**|Password di accesso utilizzata dall'agente di merge per la connessione al server Web in cui viene eseguita la sincronizzazione Web tramite l'autenticazione di base.|  
|**internet_security_mode**|**int**|Modalità di autenticazione utilizzata per la connessione al server Web che ospita la sincronizzazione Web, dove il valore **1** indica l'autenticazione di Windows e il valore **0** indica l'autenticazione di base.|  
|**internet_timeout**|**int**|Periodo di tempo, espresso in secondi, al termine del quale una richiesta di sincronizzazione Web scade.|  
|**hostname**|**nvarchar(128)**|Specifica il valore di HOST_NAME() se questa funzione viene utilizzata nella clausola WHERE di un filtro di riga con parametri.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpsubscription_properties** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_helpsubscription_properties**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

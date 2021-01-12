---
description: MSsubscription_properties (Transact-SQL)
title: MSsubscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_properties
- MSsubscription_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_properties system table
ms.assetid: f96fc1ae-b798-4b05-82a7-564ae6ef23b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9927dd1e15f04a073d0fb3cf7f95e13b1f425e0c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100497"
---
# <a name="mssubscription_properties-transact-sql"></a>MSsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSsubscription_properties** contiene righe per le informazioni sui parametri necessarie per eseguire gli agenti di replica nel Sottoscrittore. La tabella viene archiviata nel database di sottoscrizione nel Sottoscrittore per una sottoscrizione pull oppure nel database di distribuzione nel server di distribuzione per una sottoscrizione push.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**publication_type**|**int**|Tipo di pubblicazione:<br /><br /> **0** = transazionale.<br /><br /> **2** = Unione.|  
|**publisher_login**|**sysname**|ID dell'account di accesso utilizzato nel server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_password**|**nvarchar (524)**|Password (crittografata) utilizzata nel server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_security_mode**|**int**|Modalità di sicurezza implementata nel server di pubblicazione:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server l'autenticazione.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticazione di Windows.<br /><br /> **2** = i trigger di sincronizzazione utilizzano una voce **sysservers** statica per eseguire una chiamata di procedura remota (RPC) e il server di *pubblicazione* deve essere definito nella tabella **sysservers** come server remoto o server collegato.|  
|**distribuzione**|**sysname**|Nome del server di distribuzione.|  
|**distributor_login**|**sysname**|ID dell'account di accesso utilizzato nel server di distribuzione per l'autenticazione di SQL Server.|  
|**distributor_password**|**nvarchar (524)**|Password (crittografata) utilizzata nel server di distribuzione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_security_mode**|**int**|Modalità di sicurezza implementata nel server di distribuzione.<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0.<br /><br /> **1** = autenticazione di Windows.|  
|**ftp_address**|**sysname**|Indirizzo di rete del servizio File Transfer Protocol (FTP) per il server di distribuzione.|  
|**ftp_port**|**int**|Numero di porta del servizio FTP per il server di distribuzione.|  
|**ftp_login**|**sysname**|Nome utente utilizzato per la connessione al servizio FTP.|  
|**ftp_password**|**nvarchar (524)**|Password per l'utente utilizzata per la connessione al servizio FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Specifica la posizione della cartella alternativa per lo snapshot.|  
|**working_directory**|**nvarchar(255)**|Nome della directory di lavoro utilizzata per archiviare i file di dati e di schema.|  
|**use_ftp**|**bit**|Specifica l'utilizzo di FTP anziché del protocollo normale per il recupero di snapshot. Se è **1**, viene utilizzato il protocollo FTP.|  
|**dts_package_name**|**sysname**|Specifica il nome del pacchetto Data Transformation Services (DTS).|  
|**dts_package_password**|**nvarchar (524)**|Specifica la password per il pacchetto.|  
|**dts_package_location**|**int**|Percorso di archiviazione del pacchetto DTS.|  
|**enabled_for_syncmgr**|**bit**|Specifica se è possibile sincronizzare la sottoscrizione tramite Gestione sincronizzazione [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> **0** = la sottoscrizione non è registrata con Gestione sincronizzazione.<br /><br /> **1** = la sottoscrizione viene registrata con Gestione sincronizzazione e può essere sincronizzata senza avviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**offload_agent**|**bit**|Specifica se è possibile attivare l'agente in remoto. Se è **0**, l'agente non può essere attivato in remoto.|  
|**offload_server**|**sysname**|Nome di rete del server utilizzato per l'attivazione remota.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Specifica il percorso della cartella in cui vengono salvati i file di snapshot.|  
|**use_web_sync**|**bit**|Specifica se è possibile sincronizzare la sottoscrizione mediante HTTP. Il valore **1** indica che questa funzionalità è abilitata.|  
|**internet_url**|**nvarchar(260)**|URL che rappresenta il percorso del listener per la replica per la sincronizzazione tramite il Web.|  
|**internet_login**|**sysname**|Account di accesso utilizzato dal agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di.|  
|**internet_password**|**nvarchar (524)**|Password per l'account di accesso utilizzato dal agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di.|  
|**internet_security_mode**|**int**|Modalità di autenticazione utilizzata per la connessione al server Web che ospita la sincronizzazione Web, dove il valore **1** indica l'autenticazione di Windows e il valore **0** indica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di.|  
|**internet_timeout**|**int**|Intervallo di tempo, in secondi, prima della scadenza di una richiesta di sincronizzazione tramite il Web.|  
|**hostname**|**sysname**|Specifica il valore per **HOST_NAME** quando questa funzione viene utilizzata nella clausola **where** di un filtro join o di una relazione tra record logici.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  

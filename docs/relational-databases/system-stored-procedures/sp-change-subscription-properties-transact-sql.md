---
description: sp_change_subscription_properties (Transact-SQL)
title: sp_change_subscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_change_subscription_properties_TSQL
- sp_change_subscription_properties
helpviewer_keywords:
- sp_change_subscription_properties
ms.assetid: cf8137f9-f346-4aa1-ae35-91a2d3c16f17
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed6615b10487744ae5ac5c0775627bea97e29c66
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208302"
---
# <a name="sp_change_subscription_properties-transact-sql"></a>sp_change_subscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiorna le informazioni per le sottoscrizioni pull. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_change_subscription_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @property = ] 'property'  
        , [ @value = ] 'value'  
    [ , [ @publication_type = ] publication_type ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @property = ] 'property'` Proprietà da modificare. *Property* è di **tipo sysname**.  
  
`[ @value = ] 'value'` Nuovo valore della proprietà. *value* è di **tipo nvarchar (1000)** e non prevede alcun valore predefinito.  
  
`[ @publication_type = ] publication_type` Specifica il tipo di replica della pubblicazione. *publication_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Tipo di pubblicazione|  
|-----------|----------------------|  
|**0**|Transazionale|  
|**1**|Snapshot|  
|**2**|Unione|  
|NULL (predefinito)|Il tipo di pubblicazione è determinato dalla replica. Poiché la stored procedure deve analizzare più tabelle, questa opzione comporta un rallentamento delle prestazioni rispetto a quando viene specificato il tipo di pubblicazione esatto.|  
  
 Nella tabella seguente vengono descritte le proprietà degli articoli e i valori corrispondenti.  
  
|Proprietà|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**alt_snapshot_folder**||Specifica la posizione della cartella alternativa per lo snapshot. Se il valore è NULL, i file di snapshot vengono prelevati dalla posizione predefinita specificata dal server di pubblicazione.|  
|**distrib_job_login**||Account di accesso per l'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows utilizzato per l'esecuzione dell'agente.|  
|**distrib_job_password**||Password dell'account di Windows utilizzato per l'esecuzione dell'agente.|  
|**distributor_login**||Account di accesso per il server di distribuzione.|  
|**distributor_password**||Password per il server di distribuzione.|  
|**distributor_security_mode**|**1**|Consente di utilizzare l'autenticazione di Windows per la connessione al server di distribuzione.|  
||**0**|Consente di utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la connessione al server di distribuzione.|  
|**dts_package_name**||Specifica il nome del pacchetto di SQL Server 2000 Data Transformation Services (DTS). Questo valore può essere specificato solo se la pubblicazione è di tipo transazionale o snapshot.|  
|**dts_package_password**||Specifica la password per il pacchetto. *dts_package_password* è di **tipo sysname** e il valore predefinito è null, che indica che la proprietà della password deve essere lasciata invariata.<br /><br /> Nota: un pacchetto DTS deve avere una password.<br /><br /> Questo valore può essere specificato solo se la pubblicazione è di tipo transazionale o snapshot.|  
|**dts_package_location**||Posizione di archiviazione del pacchetto DTS. Questo valore può essere specificato solo se la pubblicazione è di tipo transazionale o snapshot.|  
|**dynamic_snapshot_location**||Specifica il percorso della cartella in cui vengono salvati i file di snapshot. Questo valore può essere specificato solo se la pubblicazione è di tipo merge.|  
|**ftp_address**||Disponibile solo per compatibilità con le versioni precedenti.|  
|**ftp_login**||Disponibile solo per compatibilità con le versioni precedenti.|  
|**ftp_password**||Disponibile solo per compatibilità con le versioni precedenti.|  
|**ftp_port**||Disponibile solo per compatibilità con le versioni precedenti.|  
|**hostname**||Nome host utilizzato per la connessione al server di pubblicazione.|  
|**internet_login**||Account di accesso utilizzato dall'agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite l'autenticazione di base.|  
|**internet_password**||Password utilizzata dall'agente di merge per la connessione al server Web in cui ha luogo la sincronizzazione Web mediante l'autenticazione di base.|  
|**internet_security_mode**|**1**|Consente di utilizzare l'autenticazione integrata di Windows per la sincronizzazione Web. È consigliabile utilizzare l'autenticazione di base per la sincronizzazione Web. Per altre informazioni, vedere [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md).|  
||**0**|Consente di utilizzare l'autenticazione di base per la sincronizzazione Web.<br /><br /> Nota: per la sincronizzazione tramite il Web è necessaria una connessione TLS al server Web.|  
|**internet_timeout**||Periodo di tempo, espresso in secondi, al termine del quale una richiesta di sincronizzazione Web scade.|  
|**internet_url**||URL che rappresenta la posizione del listener per la replica per la sincronizzazione Web.|  
|**merge_job_login**||Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente.|  
|**merge_job_password**||Password dell'account di Windows utilizzato per l'esecuzione dell'agente.|  
|**publisher_login**||Account di accesso per il server di pubblicazione. La modifica di *publisher_login* è supportata solo per le sottoscrizioni di pubblicazioni di tipo merge.|  
|**publisher_password**||Password del server di pubblicazione. La modifica di *publisher_password* è supportata solo per le sottoscrizioni di pubblicazioni di tipo merge.|  
|**publisher_security_mode**|**1**|Esegue la connessione al server di pubblicazione utilizzando l'autenticazione di Windows. La modifica di *publisher_security_mode* è supportata solo per le sottoscrizioni di pubblicazioni di tipo merge.|  
||**0**|Esegue la connessione al server di pubblicazione utilizzando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**use_ftp**|**true**|Consente di utilizzare il protocollo FTP anziché il protocollo regolare per il recupero degli snapshot.|  
||**false**|Consente di utilizzare il protocollo regolare per il recupero degli snapshot.|  
|**use_web_sync**|**true**|Abilita la sincronizzazione Web.|  
||**false**|Disabilita la sincronizzazione Web.|  
|**working_directory**||Nome della directory di lavoro utilizzata per l'archiviazione temporanea dei file di dati e dello schema della pubblicazione quando per il trasferimento dei file di snapshot viene utilizzato il protocollo FTP (File Transfer Protocol).|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_change_subscription_properties** viene utilizzato in tutti i tipi di replica.  
  
 **sp_change_subscription_properties** viene utilizzata per le sottoscrizioni pull.  
  
 Per i server di pubblicazione Oracle, il valore di *publisher_db* viene ignorato perché Oracle consente solo un database per ogni istanza del server.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_change_subscription_properties**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà delle sottoscrizioni pull](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [sp_addmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_addmergepullsubscription_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_addpullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_addpullsubscription_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

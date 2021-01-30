---
description: sp_changemergepullsubscription (Transact-SQL)
title: sp_changemergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergepullsubscription
- sp_changemergepullsubscription_TSQL
helpviewer_keywords:
- sp_changemergepullsubscription
ms.assetid: 5e0d04f2-6175-44a2-ad96-a8e2986ce4c9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f8590cd397f41ca4c4db8776f614f47b2b05c921
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159841"
---
# <a name="sp_changemergepullsubscription-transact-sql"></a>sp_changemergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le proprietà della sottoscrizione pull di tipo merge. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changemergepullsubscription [ [ @publication= ] 'publication' ]  
    [ , [ @publisher= ] 'publisher' ]  
    [ , [ @publisher_db= ] 'publisher_db' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è%.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è%.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è%.  
  
`[ @property = ] 'property'` Nome della proprietà da modificare. *Property* è di **tipo sysname**. i possibili valori sono i valori della tabella.  
  
`[ @value = ] 'value'` Nuovo valore per la proprietà specificata. *value* è di **tipo nvarchar (255)**. i possibili valori sono i valori della tabella.  
  
|Proprietà|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**alt_snapshot_folder**||Percorso di archiviazione della cartella snapshot, se diverso da quello predefinito o se si tratta di una cartella aggiuntiva.|  
|**description**||Descrizione della sottoscrizione pull di tipo merge.|  
|**distribuzione**||Nome del server di distribuzione.|  
|**distributor_login**||ID di accesso utilizzato nel server di distribuzione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_password**||Password (crittografata) utilizzata nel server di distribuzione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_security_mode**|**1**|Consente di utilizzare l'autenticazione di Windows per la connessione al server di distribuzione.|  
||**0**|Consente di utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la connessione al server di distribuzione.|  
|**dynamic_snapshot_location**||Percorso della cartella in cui vengono salvati i file di snapshot.|  
|**ftp_address**||Disponibile per compatibilità con le versioni precedenti. Indirizzo di rete del servizio FTP per il server di distribuzione.|  
|**ftp_login**||Disponibile per compatibilità con le versioni precedenti. Nome utente utilizzato per la connessione al servizio FTP.|  
|**ftp_password**||Disponibile per compatibilità con le versioni precedenti. Password utente utilizzata per la connessione al servizio FTP.|  
|**ftp_port**||Disponibile per compatibilità con le versioni precedenti. Numero di porta del servizio FTP per il database di distribuzione.|  
|**hostname**||Specifica un valore per HOST_NAME() se questa funzione viene utilizzata nella clausola WHERE di un filtro join o di una relazione tra record logici.|  
|**internet_login**||Account di accesso utilizzato dall'agente di merge per la connessione al server Web che ospita la sincronizzazione Web tramite l'autenticazione di base.|  
|**internet_password**||Password di accesso utilizzata dall'agente di merge per la connessione al server Web in cui viene eseguita la sincronizzazione Web tramite l'autenticazione di base.|  
|**internet_security_mode**|**1**|Utilizza l'autenticazione di Windows per la connessione al server Web in cui viene eseguita la sincronizzazione Web.|  
||**0**|Utilizza l'autenticazione di base per la connessione al server Web in cui viene eseguita la sincronizzazione Web.|  
|**internet_timeout**||Periodo di tempo, espresso in secondi, al termine del quale una richiesta di sincronizzazione Web scade.|  
|**internet_url**||URL che rappresenta la posizione del listener per la replica per la sincronizzazione Web.|  
|**merge_job_login**||Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente.|  
|**merge_job_password**||Password dell'account di Windows utilizzato per l'esecuzione dell'agente.|  
|**priority**||Disponibile solo per compatibilità con le versioni precedenti. in alternativa, eseguire [sp_changemergesubscription](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md) nel server di pubblicazione per modificare la priorità di una sottoscrizione.|  
|**publisher_login**||ID dell'account di accesso utilizzato nel server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_password**||Password (crittografata) utilizzata dal server di pubblicazione per l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_security_mode**|**0**|Esegue la connessione al server di pubblicazione utilizzando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**1**|Esegue la connessione al server di pubblicazione utilizzando l'autenticazione di Windows.|  
||**2**|I trigger di sincronizzazione utilizzano una voce **sysservers** statica per eseguire una chiamata di procedura remota (RPC) e il server di pubblicazione deve essere definito nella tabella **sysservers** come server remoto o server collegato.|  
|**sync_type**|**Automatico**|Lo schema e i dati iniziali per le tabelle pubblicate vengono trasferiti per primi nel Sottoscrittore.|  
||**nessuna**|Il Sottoscrittore dispone già dello schema e dei dati iniziali per le tabelle pubblicate. Le tabelle di sistema e i dati vengono sempre trasferiti.|  
|**use_ftp**|**true**|Utilizza il protocollo FTP anziché il protocollo normale per il recupero degli snapshot.|  
||**false**|Utilizza il protocollo normale per il recupero degli snapshot.|  
|**use_web_sync**|**true**|Le sottoscrizioni possono essere sincronizzate tramite HTTP.|  
||**false**|Le sottoscrizioni non possono essere sincronizzate tramite HTTP.|  
|**use_interactive_resolver**|**true**|Durante la riconciliazione viene utilizzato il sistema di risoluzione interattivo.|  
||**false**|Il sistema di risoluzione interattivo non viene utilizzato.|  
|**working_directory**||Percorso completo della directory in cui vengono trasferiti i file di snapshot tramite il servizio FTP, se l'opzione corrispondente è stata specificata.|  
|NULL (predefinito)||Restituisce l'elenco dei valori supportati per la *Proprietà*.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changemergepullsubscription** viene utilizzata nella replica di tipo merge.  
  
 Vengono considerati come Sottoscrittore e database del Sottoscrittore il server e il database correnti.  
  
 Dopo la modifica dell'account di accesso o della password di un agente, è necessario arrestare e riavviare l'agente per rendere effettiva la modifica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_changemergepullsubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà delle sottoscrizioni pull](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [sp_addmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

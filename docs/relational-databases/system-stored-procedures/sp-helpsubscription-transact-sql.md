---
description: sp_helpsubscription (Transact-SQL)
title: sp_helpsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_TSQL
- sp_helpsubscription
helpviewer_keywords:
- sp_helpsubscription
ms.assetid: ff96bcbf-e2b9-4da8-8515-d80d4ce86c16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73fc43bf20a9306d0224392d984b3f2bff74ec6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192901"
---
# <a name="sp_helpsubscription-transact-sql"></a>sp_helpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce l'elenco delle informazioni sulla sottoscrizione associate a una pubblicazione, un articolo, un Sottoscrittore o un set di sottoscrizioni. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpsubscription [ [ @publication = ] 'publication' ]   
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]   
    [ , [ @found=] found OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione associata. *Publication* è di **tipo sysname** e il valore predefinito è **%** , che restituisce tutte le informazioni sulla sottoscrizione per questo server.  
  
`[ @article = ] 'article'` Nome dell'articolo. *article* è di **tipo sysname** e il valore predefinito è **%** , che restituisce tutte le informazioni sulla sottoscrizione per le pubblicazioni e i Sottoscrittori selezionati. Se è **All**, viene restituita una sola voce per la sottoscrizione completa di una pubblicazione.  
  
`[ @subscriber = ] 'subscriber'` Nome del sottoscrittore su cui si desidera ottenere informazioni sulla sottoscrizione. *Subscriber* è di **tipo sysname** e il valore predefinito è **%** , che restituisce tutte le informazioni sulla sottoscrizione per le pubblicazioni e gli articoli selezionati.  
  
`[ @destination_db = ] 'destination_db'` Nome del database di destinazione. *destination_db* è di **tipo sysname** e il valore predefinito è **%** .  
  
`[ @found = ] 'found'OUTPUT` Flag che indica la restituzione di righe. *trovato* è di **tipo int** e un parametro di output e il valore predefinito è 23456.  
  
 **1** indica che la pubblicazione è stata trovata.  
  
 **0** indica che la pubblicazione non è stata trovata.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è il nome del server corrente.  
  
> [!NOTE]  
>  il *server di pubblicazione* non deve essere specificato, tranne quando si tratta di un server di pubblicazione Oracle.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Sottoscrittore**|**sysname**|Nome del Sottoscrittore.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**articolo**|**sysname**|Nome dell'articolo.|  
|**database di destinazione**|**sysname**|Nome del database di destinazione per i dati replicati.|  
|**Stato della sottoscrizione**|**tinyint**|Stato della sottoscrizione:<br /><br /> **0** = inattivo<br /><br /> **1** = sottoscritto<br /><br /> **2** = attivo|  
|**tipo di sincronizzazione**|**tinyint**|Tipo di sincronizzazione per la sottoscrizione:<br /><br /> **1** = automatico<br /><br /> **2** = nessuna|  
|**tipo di sottoscrizione**|**int**|Tipo di sottoscrizione:<br /><br /> **0** = push<br /><br /> **1** = pull<br /><br /> **2** = Anonimo|  
|**full subscription**|**bit**|Indica se la sottoscrizione è associata a tutti gli articoli della pubblicazione:<br /><br /> **0** = No<br /><br /> **1** = Sì|  
|**nome sottoscrizione**|**nvarchar(255)**|Nome della sottoscrizione.|  
|**modalità di aggiornamento**|**int**|**0** = sola lettura<br /><br /> **1** = sottoscrizione ad aggiornamento immediato|  
|**distribution job id**|**binary(16)**|ID di processo dell'agente di distribuzione.|  
|**loopback_detection**|**bit**|Il rilevamento di loopback determina se l'agente di distribuzione deve inviare nuovamente al Sottoscrittore le transazioni provenienti dal Sottoscrittore:<br /><br /> **0** = restituisce.<br /><br /> **1** = non viene restituito.<br /><br /> Utilizzato con la replica transazionale bidirezionale. Per altre informazioni, vedere [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|**offload_enabled**|**bit**|Specifica se per un agente di replica è impostata l'esecuzione con ripartizione del carico di lavoro nel Sottoscrittore.<br /><br /> Se è **0**, Agent viene eseguito nel server di pubblicazione.<br /><br /> Se è **1**, Agent viene eseguito nel Sottoscrittore.|  
|**offload_server**|**sysname**|Nome del server abilitato per l'attivazione remota degli agenti. Se NULL, viene utilizzata la offload_server corrente elencata in [MSdistribution_agents](../../relational-databases/system-tables/msdistribution-agents-transact-sql.md) tabella.|  
|**dts_package_name**|**sysname**|Specifica il nome del pacchetto Data Transformation Services (DTS).|  
|**dts_package_location**|**int**|Posizione del pacchetto DTS, se assegnato alla sottoscrizione. Se è presente un pacchetto, un valore pari a **0** indica il percorso del pacchetto nel **server di distribuzione**. Il valore **1** specifica il **Sottoscrittore**.|  
|**subscriber_security_mode**|**smallint**|Modalità di sicurezza nel Sottoscrittore, dove **1** indica l'autenticazione di Windows e **0** indica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di.|  
|**subscriber_login**|**sysname**|Nome dell'account di accesso nel Sottoscrittore.|  
|**subscriber_password**||La password effettiva per il Sottoscrittore non viene mai restituita. Il risultato è mascherato da una stringa "**&#42;&#42;&#42;&#42;&#42;&#42;**".|  
|**job_login**|**sysname**|Nome dell'account di Windows utilizzato per l'esecuzione dell'agente di distribuzione.|  
|**job_password**||La password effettiva per il processo non viene mai restituita. Il risultato è mascherato da una stringa "**&#42;&#42;&#42;&#42;&#42;&#42;**".|  
|**distrib_agent_name**|**nvarchar (100)**|Nome del processo dell'agente che sincronizza la sottoscrizione.|  
|**subscriber_type**|**tinyint**|Tipo di Sottoscrittore. I possibili tipi sono i seguenti:<br /><br /> **0** = sottoscrittore SQL Server<br /><br /> **1** = server origine dati ODBC<br /><br /> **2** = database Microsoft Jet (deprecato)<br /><br /> **3** = provider OLE DB|  
|**subscriber_provider**|**sysname**|ProgID univoco con il quale viene registrato il provider OLE DB per l'origine dei dati non SQL Server.|  
|**subscriber_datasource**|**nvarchar(4000)**|Nome dell'origine dei dati riconosciuto dal provider OLE DB.|  
|**subscriber_providerstring**|**nvarchar(4000)**|Stringa di connessione specifica del provider OLE DB che identifica l'origine dei dati.|  
|**subscriber_location**|**nvarchar(4000)**|Percorso del database riconosciuto dal provider OLE DB.|  
|**subscriber_catalog**|**sysname**|Catalogo da utilizzare per stabilire una connessione al provider OLE DB|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpsubscription** viene utilizzata per la replica snapshot e transazionale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione vengono assegnate per impostazione predefinita al ruolo **public** . All'utente vengono restituite solo le informazioni relative alle sottoscrizioni create dall'utente stesso. Le informazioni su tutte le sottoscrizioni vengono restituite ai membri del ruolo predefinito del server **sysadmin** nel server di pubblicazione o ai membri del **db_owner** ruolo predefinito del database nel database di pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

---
description: sp_attachsubscription (Transact-SQL)
title: sp_attachsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_attachsubscription
- sp_attachsubscription_TSQL
helpviewer_keywords:
- sp_attachsubscription
ms.assetid: b9bbda36-a46a-4327-a01e-9cd632e4791b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a7357d6a37472d9d530dfcf3ecfe3c9bddfffd58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206634"
---
# <a name="sp_attachsubscription-transact-sql"></a>sp_attachsubscription (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  Collega un database di sottoscrizione esistente a qualsiasi Sottoscrittore. Questa stored procedure viene eseguita nel database master del nuovo Sottoscrittore.  
  
> [!IMPORTANT]  
>  Questa funzionalità è deprecata e verrà rimossa a partire da una delle prossime versioni. Evitare di utilizzare questa funzionalità in un nuovo progetto di sviluppo. Nel caso di pubblicazioni di tipo merge partizionate mediante filtri con parametri, è consigliabile utilizzare la nuove funzionalità degli snapshot partizionati, che semplificano l'inizializzazione di un ampio numero di sottoscrizioni. Per altre informazioni, vedere [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). Per le pubblicazioni non partizionate, è possibile inizializzare una sottoscrizione con un backup. Per altre informazioni, vedere [Inizializzazione di una sottoscrizione transazionale senza uno snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_attachsubscription [ @dbname = ] 'dbname'  
        , [ @filename = ] 'filename'  
    [ , [ @subscriber_security_mode = ] 'subscriber_security_mode' ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @publisher_security_mode = ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @db_master_key_password = ] 'db_master_key_password' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'dbname'` Stringa che specifica il database di sottoscrizione di destinazione in base al nome. *dbname* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @filename = ] 'filename'` Nome e posizione fisica del file MDF primario (file di dati **Master** ). *filename* è di **tipo nvarchar (260)** e non prevede alcun valore predefinito.  
  
`[ @subscriber_security_mode = ] 'subscriber_security_mode'` Modalità di sicurezza del Sottoscrittore da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_security_mode* è di **tipo int** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario utilizzare l'autenticazione di Windows. Se *subscriber_security_mode* non è **1** (autenticazione di Windows), viene restituito un errore.  
  
`[ @subscriber_login = ] 'subscriber_login'` Nome dell'account di accesso del Sottoscrittore da utilizzare per la connessione a un Sottoscrittore durante la sincronizzazione. *subscriber_login* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. Se *subscriber_security_mode* non è **1** e viene specificato *subscriber_login* , viene restituito un errore.  
  
`[ @subscriber_password = ] 'subscriber_password'` Password del Sottoscrittore. *subscriber_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. Se *subscriber_security_mode* non è **1** e viene specificato *subscriber_password* , viene restituito un errore.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Modalità di sicurezza da utilizzare per la connessione a un database di distribuzione durante la sincronizzazione. *distributor_security_mode* è di **tipo int** e il valore predefinito è **0**. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. **1** specifica l'autenticazione di Windows. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Account di accesso del server di distribuzione da utilizzare per la connessione a un database di distribuzione durante la sincronizzazione. *distributor_login* è obbligatorio se *distributor_security_mode* è impostato su **0**. *distributor_login* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @distributor_password = ] 'distributor_password'` Password del server di distribuzione. *distributor_password* è obbligatorio se *distributor_security_mode* è impostato su **0**. *distributor_password* è di **tipo sysname** e il valore predefinito è null. Il valore di *distributor_password* deve essere inferiore a 120 caratteri Unicode.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa. Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Modalità di sicurezza da utilizzare per la connessione a un server di pubblicazione durante la sincronizzazione. *publisher_security_mode* è di **tipo int** e il valore predefinito è **1**. Se è **0**, viene specificata [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. Se è **1**, viene specificata l'autenticazione di Windows. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Account di accesso da utilizzare per la connessione a un server di pubblicazione durante la sincronizzazione. *publisher_login* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publisher_password = ] 'publisher_password'` Password utilizzata per la connessione al server di pubblicazione. *publisher_password* è di **tipo sysname** e il valore predefinito è null. Il valore di *publisher_password* deve essere inferiore a 120 caratteri Unicode.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa. Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e non prevede alcun valore predefinito. Questo account di Windows viene sempre utilizzato per le connessioni dell'agente al server di distribuzione.  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e non prevede alcun valore predefinito. Il valore di *job_password* deve essere inferiore a 120 caratteri Unicode.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @db_master_key_password = ] 'db_master_key_password'` Password di una chiave master del database definita dall'utente. *db_master_key_password* è di **tipo nvarchar (524)** e il valore predefinito è null. Se *db_master_key_password* non viene specificato, una chiave master del database esistente verrà eliminata e ricreata.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_attachsubscription** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
 Non è possibile collegare una sottoscrizione alla pubblicazione se il periodo di memorizzazione della pubblicazione è scaduto. Se si specifica una sottoscrizione quando il periodo di memorizzazione è scaduto, viene generato un errore durante il collegamento o la prima sincronizzazione della sottoscrizione. Le pubblicazioni con un periodo di memorizzazione della pubblicazione pari a **0** (nessuna scadenza) vengono ignorate.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_attachsubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

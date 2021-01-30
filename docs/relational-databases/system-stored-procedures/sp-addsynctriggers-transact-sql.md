---
description: sp_addsynctriggers (Transact-SQL)
title: sp_addsynctriggers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsynctriggers_TSQL
- sp_addsynctriggers
helpviewer_keywords:
- sp_addsynctriggers
ms.assetid: e37d0c3b-19bf-4719-9535-96ba361372b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 00782a900c02976c06f2cfcdf6c9184d90e8059d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211945"
---
# <a name="sp_addsynctriggers-transact-sql"></a>sp_addsynctriggers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea nel Sottoscrittore trigger utilizzati con tutti i tipi di sottoscrizioni aggiornabili, ovvero ad aggiornamento immediato, ad aggiornamento in coda e ad aggiornamento immediato sostituito dall'aggiornamento in coda in caso di errore. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
> [!IMPORTANT]  
>  È consigliabile utilizzare la procedura [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) anziché **sp_addsynctrigger**. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) genera uno script che contiene le chiamate di **sp_addsynctrigger** .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addsynctriggers [ @sub_table = ] 'sub_table'  
        , [ @sub_table_owner = ] 'sub_table_owner'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @ins_proc = ] 'ins_proc'   
        , [ @upd_proc = ] 'upd_proc'   
        , [ @del_proc = ] 'del_proc'   
        , [ @cftproc = ] 'cftproc'  
        , [ @proc_owner = ] 'proc_owner'  
    [ , [ @identity_col = ] 'identity_col' ]  
    [ , [ @ts_col = ] 'timestamp_col' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]   
        , [ @primary_key_bitmap = ] 'primary_key_bitmap'  
    [ , [ @identity_support = ] identity_support ]  
    [ , [ @independent_agent = ] independent_agent ]  
        , [ @distributor = ] 'distributor'   
    [ , [ @pubversion = ] pubversion  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @sub_table = ] 'sub_table'` Nome della tabella del Sottoscrittore. *sub_table* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @sub_table_owner = ] 'sub_table_owner'` Nome del proprietario della tabella del Sottoscrittore. *sub_table_owner* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito. Se è NULL, viene utilizzato il database corrente.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @ins_proc = ] 'ins_proc'` Nome della stored procedure che supporta gli inserimenti di transazioni sincroni nel server di pubblicazione. *ins_proc* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @upd_proc = ] 'upd_proc'` Nome della stored procedure che supporta gli aggiornamenti sincroni delle transazioni nel server di pubblicazione. *ins_proc* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @del_proc = ] 'del_proc'` Nome della stored procedure che supporta le eliminazioni sincrone delle transazioni nel server di pubblicazione. *ins_proc* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @cftproc = ] 'cftproc'` Nome della procedura generata automaticamente utilizzata dalle pubblicazioni che consentono l'aggiornamento in coda. *cftproc* è di **tipo sysname** e non prevede alcun valore predefinito. Nel caso di pubblicazioni che consentono l'aggiornamento immediato, questo valore è NULL. Questo parametro viene applicato alle pubblicazioni che consentono l'aggiornamento in coda (aggiornamento in coda e aggiornamento immediato sostituito dall'aggiornamento in coda in caso di errore).  
  
`[ @proc_owner = ] 'proc_owner'` Specifica l'account utente nel server di pubblicazione in cui sono state create tutte le stored procedure generate automaticamente per l'aggiornamento della pubblicazione (in coda e/o immediato). *proc_owner* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @identity_col = ] 'identity_col'` Nome della colonna Identity nel server di pubblicazione. *identity_col* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @ts_col = ] 'timestamp_col'` Nome della colonna di tipo **timestamp** nel server di pubblicazione. *timestamp_col* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @filter_clause = ] 'filter_clause'` Clausola di restrizione (WHERE) che definisce un filtro orizzontale. Quando si specifica la clausola di restrizione, omettere la parola chiave WHERE. *filter_clause* è di **tipo nvarchar (4000)** e il valore predefinito è null.  
  
`[ @primary_key_bitmap = ] 'primary_key_bitmap'` Mappa di bit delle colonne chiave primaria nella tabella. *primary_key_bitmap* è di tipo **varbinary (4000)** e non prevede alcun valore predefinito.  
  
`[ @identity_support = ] identity_support` Abilita e Disabilita la gestione automatica degli intervalli di valori Identity quando viene utilizzato l'aggiornamento in coda. *identity_support* è di **bit** e il valore predefinito è **0**. **0** indica che non è disponibile alcun supporto per l'intervallo di valori Identity, **1** consente la gestione automatica degli intervalli di valori Identity.  
  
`[ @independent_agent = ] independent_agent` Indica se è presente una singola agente di distribuzione (agente indipendente) per questa pubblicazione oppure una agente di distribuzione per ogni coppia di database di pubblicazione e database di sottoscrizione, ovvero un agente condiviso. Questo valore è determinato dal valore della proprietà independent_agent della pubblicazione definito nel server di pubblicazione. *independent_agent* è un bit e il valore predefinito è **0**. Se è **0**, l'agente è un agente condiviso. Se è **1**, l'agente è un agente indipendente.  
  
`[ @distributor = ] 'distributor'` Nome del server di distribuzione. *Distributor* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @pubversion = ] pubversion` Indica la versione del server di pubblicazione. *pubversion* è di **tipo int** e il valore predefinito è 1. **1** indica che la versione del server di pubblicazione è [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 2 o precedente; **2** indica che il server di pubblicazione è [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 3 (SP3) o versione successiva. *pubversion* deve essere impostato in modo esplicito su **2** quando la versione del server di pubblicazione è [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] SP3 o successiva.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addsynctriggers** viene utilizzato dal agente di distribuzione come parte dell'inizializzazione della sottoscrizione. In genere, non viene eseguita dagli utenti, ma può risultare utile se l'utente deve configurare una sottoscrizione nosync in modo manuale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addsynctriggers**.  
  
## <a name="see-also"></a>Vedere anche  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

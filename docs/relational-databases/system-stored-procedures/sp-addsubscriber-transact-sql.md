---
description: sp_addsubscriber (Transact-SQL)
title: sp_addsubscriber (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsubscriber
- sp_addsubscriber_TSQL
helpviewer_keywords:
- sp_addsubscriber
ms.assetid: b8a584ea-2a26-4936-965b-b84f026e39c0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9839d1daa3316fa8cb47af0ed2e362eba194f414
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212004"
---
# <a name="sp_addsubscriber-transact-sql"></a>sp_addsubscriber (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  Aggiunge in un server di pubblicazione un nuovo Sottoscrittore per abilitarlo alla ricezione di pubblicazioni. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione per pubblicazioni snapshot e transazionali. Per pubblicazioni di tipo merge che utilizzano un server di distribuzione remoto, viene eseguita nel server di distribuzione.  
  
> [!IMPORTANT]  
>  Questa stored procedure è deprecata. Non è più necessario registrare in modo esplicito un Sottoscrittore nel server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addsubscriber [ @subscriber = ] 'subscriber'  
    [ , [ @type = ] type ]   
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @commit_batch_size = ] commit_batch_size ]  
    [ , [ @status_batch_size = ] status_batch_size ]  
    [ , [ @flush_frequency = ] flush_frequency ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @description = ] 'description' ]  
    [ , [ @security_mode = ] security_mode ]  
    [ , [ @encrypted_password = ] encrypted_password ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @subscriber = ] 'subscriber'` Nome del server da aggiungere come Sottoscrittore valido per le pubblicazioni in questo server. *Subscriber* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @type = ] type` Tipo di Sottoscrittore. il *tipo* è **tinyint**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Sottoscrittore|  
|**1**|Server dell'origine dei dati ODBC.|  
|**2**|Database [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet|  
|**3**|Provider OLE DB|  
  
`[ @login = ] 'login'` ID di accesso per l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione. *login* è di tipo **sysname** e il valore predefinito è NULL.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @password = ] 'password'` Password per l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione. *password* è di **tipo nvarchar (524)** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @commit_batch_size = ] commit_batch_size` Questo parametro è stato deprecato e viene mantenuto per compatibilità con le versioni precedenti degli script.  
  
> [!NOTE]  
>  Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @status_batch_size = ] status_batch_size` Questo parametro è stato deprecato e viene mantenuto per compatibilità con le versioni precedenti degli script.  
  
> [!NOTE]  
>  Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @flush_frequency = ] flush_frequency` Questo parametro è stato deprecato e viene mantenuto per compatibilità con le versioni precedenti degli script.  
  
> [!NOTE]  
>  Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @frequency_type = ] frequency_type` Frequenza di pianificazione dell'agente di replica. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Singola occorrenza|  
|**2**|On demand|  
|**4**|Ogni giorno|  
|**8**|Settimanale|  
|**16**|Ogni mese|  
|**32**|Mensile relativa|  
|**64** (impostazione predefinita)|Avvio automatico|  
|**128**|Periodica|  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @frequency_interval = ] frequency_interval` Valore applicato alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int** e il valore predefinito è 1.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data dell'agente di replica. Questo parametro viene usato quando *frequency_type* è impostato su **32** (mensile relativo). *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1** (impostazione predefinita)|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è **0**.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @frequency_subday = ] frequency_subday` Frequenza di ripianificazione durante il periodo definito. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4** (impostazione predefinita)|Minuto|  
|**8**|Ora|  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è **5**.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui l'agente di replica viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è **0**.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di replica smette di essere pianificato, formattato come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è 235959, che indica le 11:59:59. nel formato 24 ore.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione dell'agente di replica, nel formato AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è 0.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della pianificazione dell'agente di replica, nel formato AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è 99991231, ovvero il 31 dicembre 9999.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @description = ] 'description'` È una descrizione testuale del Sottoscrittore. *Description* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @security_mode = ] security_mode` Modalità di sicurezza implementata. *security_mode* è di **tipo int** e il valore predefinito è 1. **0** specifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione di. **1** specifica l'autenticazione di Windows.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti. La proprietà viene ora specificata per ogni sottoscrizione durante l'esecuzione di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Se si specifica un valore, questo verrà utilizzato come valore predefinito per la creazione di sottoscrizioni nel Sottoscrittore e verrà restituito un messaggio di avviso.  
  
`[ @encrypted_password = ] encrypted_password` Questo parametro è stato deprecato e viene fornito per la compatibilità con le versioni precedenti *encrypted_password* a qualsiasi valore, ma **0** genera un errore.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *Server* di pubblicazione non deve essere utilizzato per la pubblicazione da un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addsubscriber** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
 **sp_addsubscriber** non è obbligatorio se il Sottoscrittore avrà solo sottoscrizioni anonime per le pubblicazioni di tipo merge.  
  
 **sp_addsubscriber** scrive nella tabella [MSsubscriber_info](../../relational-databases/system-tables/mssubscriber-info-transact-sql.md) del database di **distribuzione** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_addsubscriber**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [sp_changesubscriber &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)  
  
  

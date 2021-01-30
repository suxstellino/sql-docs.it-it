---
description: sp_addsubscriber_schedule (Transact-SQL)
title: sp_addsubscriber_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsubscriber_schedule_TSQL
- sp_addsubscriber_schedule
helpviewer_keywords:
- sp_addsubscriber_schedule
ms.assetid: a6225033-5c3b-452f-ae52-79890a3590ed
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e95e55157e4ab8f96fd1b715b08c2c2e377cf8e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207066"
---
# <a name="sp_addsubscriber_schedule-transact-sql"></a>sp_addsubscriber_schedule (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge una pianificazione per l'agente di distribuzione e l'agente di merge. Questa stored procedure viene eseguita in qualsiasi database del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addsubscriber_schedule [ @subscriber = ] 'subscriber'  
    [ , [ @agent_type = ] agent_type ]  
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
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname**. Il nome del Sottoscrittore deve essere univoco all'interno del database, non deve essere già esistente e non può essere NULL.  
  
`[ @agent_type = ] agent_type` Tipo di agente. *agent_type* è di **smallint**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0** (predefinito)|Agente di distribuzione|  
|**1**|Agente di merge|  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare la agente di distribuzione. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
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
  
`[ @frequency_interval = ] frequency_interval` Valore da applicare alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int** e il valore predefinito è **1**.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data del agente di distribuzione. Questo parametro viene usato quando *frequency_type* è impostato su **32** (mensile relativo). *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1** (impostazione predefinita)|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @frequency_subday = ] frequency_subday` Frequenza di ripianificazione durante il periodo definito. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4** (impostazione predefinita)|Minuto|  
|**8**|Ora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è **5**.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il agente di distribuzione viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'agente di distribuzione viene arrestata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è 235959, che indica le 11:59:59. nel formato 24 ore.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del agente di distribuzione, formattata come AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della agente di distribuzione pianificata, formattata come AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è 99991231, ovvero il 31 dicembre 9999.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *Server* di pubblicazione non deve essere specificato per un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addsubscriber_schedule** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_addsubscriber_schedule**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_changesubscriber_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscriber-schedule-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

---
description: sp_changesubscriber_schedule (Transact-SQL)
title: sp_changesubscriber_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriber_schedule
- sp_changesubscriber_schedule_TSQL
helpviewer_keywords:
- sp_changesubscriber_schedule
ms.assetid: ff84e8e2-d496-482c-b23e-38a6626596e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e93079a747c3766a47ababf3fdd866053b40968
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171767"
---
# <a name="sp_changesubscriber_schedule-transact-sql"></a>sp_changesubscriber_schedule (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica la pianificazione dell'agente di distribuzione o di merge per un Sottoscrittore. Questa stored procedure viene eseguita in qualsiasi database del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changesubscriber_schedule [ @subscriber = ] 'subscriber', [ @agent_type = ] type  
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
  
`[ @agent_type = ] type` Tipo di agente. il *tipo* è **smallint** e il valore predefinito è **0**. **0** indica un agente di distribuzione. **1** indica un agente di merge.  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare l'attività di distribuzione. *frequency_type* è di **tipo int** e il valore predefinito è **64**. Sono disponibili 10 colonne di pianificazione.  
  
`[ @frequency_interval = ] frequency_interval` Valore applicato alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int** e il valore predefinito è **1**.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data dell'attività di distribuzione. *frequency_relative_interval* è di **tipo int** e il valore predefinito è **1**.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @frequency_subday = ] frequency_subday` Frequenza, in minuti, di ripianificazione durante il periodo definito. *frequency_subday* è di **tipo int** e il valore predefinito è **4**.  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è **5**.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui l'attività di distribuzione viene pianificata per la prima volta. *active_start_time_of_day* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'attività di distribuzione smette di essere pianificata. *active_end_time_of_day* è di **tipo int** e il valore predefinito è **235959**, che indica le 11:59:59. nel formato 24 ore.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione dell'attività di distribuzione, nel formato AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è **0**.  
  
`[ @active_end_date = ] active_end_date` Data in cui viene arrestata la pianificazione dell'attività di distribuzione, nel formato AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è **99991231**, ovvero il 31 dicembre 9999.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Impossibile utilizzare *Publisher* quando si modificano le proprietà degli articoli in un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changesubscriber_schedule** viene utilizzato in tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_changesubscriber_schedule**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addsubscriber_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscriber-schedule-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

---
description: sp_add_operator (Transact-SQL)
title: sp_add_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_operator
- sp_add_operator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_operator
ms.assetid: 817cd98a-4dff-4ed8-a546-f336c144d1e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6cb3aed762c87430e75c14c954c80e58ab70d81e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195892"
---
# <a name="sp_add_operator-transact-sql"></a>sp_add_operator (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un operatore (destinatario delle notifiche) da utilizzare con avvisi e processi.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_operator [ @name = ] 'name'   
     [ , [ @enabled = ] enabled ]   
     [ , [ @email_address = ] 'email_address' ]   
     [ , [ @pager_address = ] 'pager_address' ]   
     [ , [ @weekday_pager_start_time = ] weekday_pager_start_time ]   
     [ , [ @weekday_pager_end_time = ] weekday_pager_end_time ]   
     [ , [ @saturday_pager_start_time = ] saturday_pager_start_time ]   
     [ , [ @saturday_pager_end_time = ] saturday_pager_end_time ]   
     [ , [ @sunday_pager_start_time = ] sunday_pager_start_time ]   
     [ , [ @sunday_pager_end_time = ] sunday_pager_end_time ]   
     [ , [ @pager_days = ] pager_days ]   
     [ , [ @netsend_address = ] 'netsend_address' ]   
     [ , [ @category_name = ] 'category' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome di un operatore (destinatario notifiche). Questo nome deve essere univoco e non può contenere il carattere di percentuale ( **%** ). *Name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @enabled = ] enabled` Indica lo stato corrente dell'operatore. *Enabled* è di **tinyint** e il valore predefinito è **1** (abilitato). Se è **0**, l'operatore non è abilitato e non riceve le notifiche.  
  
`[ @email_address = ] 'email_address'` Indirizzo di posta elettronica dell'operatore. Questa stringa viene passata direttamente al sistema di posta elettronica. *email_address* è di **tipo nvarchar (100)** e il valore predefinito è null.  
  
 È possibile specificare un indirizzo di posta elettronica fisico o un alias per *email_address*. Ad esempio:  
  
 '**jdoe**' o '**jdoe \@ XYZ.com**'  
  
> [!NOTE]  
>  È necessario utilizzare l'indirizzo di posta elettronica per Posta elettronica database.  
  
`[ @pager_address = ] 'pager_address'` Indirizzo del cercapersone dell'operatore. Questa stringa viene passata direttamente al sistema di posta elettronica. *pager_address* è di **tipo nvarchar (100)** e il valore predefinito è null.  
  
`[ @weekday_pager_start_time = ] weekday_pager_start_time` Tempo trascorso il quale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent invia una notifica tramite cercapersone all'operatore specificato nei giorni feriali, da lunedì a venerdì. *weekday_pager_start_time* è di **tipo int** e il valore predefinito è **090000**, che indica le ore 9:00 nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @weekday_pager_end_time = ] weekday_pager_end_time` Tempo trascorso il quale il servizio **SQLServerAgent** non invia più notifiche tramite cercapersone all'operatore specificato nei giorni feriali, da lunedì a venerdì. *weekday_pager_end_time* è di **tipo int** e il valore predefinito è 180000, che indica le 6:00. nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @saturday_pager_start_time = ] saturday_pager_start_time` Ora di sabato dopo la quale il servizio **SQLServerAgent** invia una notifica tramite cercapersone all'operatore specificato. *saturday_pager_start_time* è di **tipo int** e il valore predefinito è 090000, che indica le ore 9:00 nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @saturday_pager_end_time = ] saturday_pager_end_time` Tempo trascorso il quale il servizio **SQLServerAgent** non invia più notifiche tramite cercapersone all'operatore specificato il sabato. *saturday_pager_end_time* è di **tipo int** e il valore predefinito è **180000**, che indica le 6:00. nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @sunday_pager_start_time = ] sunday_pager_start_time` Tempo trascorso il quale il servizio **SQLServerAgent** invia una notifica tramite cercapersone all'operatore specificato la domenica. *sunday_pager_start_time* è di **tipo int** e il valore predefinito è **090000**, che indica le ore 9:00 nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @sunday_pager_end_time = ] sunday_pager_end_time` Tempo trascorso il quale il servizio **SQLServerAgent** non invia più notifiche tramite cercapersone all'operatore specificato la domenica. *sunday_pager_end_time* è di **tipo int** e il valore predefinito è **180000**, che indica le 6:00. nel formato a 24 ore e deve essere immesso nel formato HHMMSS.  
  
`[ @pager_days = ] pager_days` Numero che indica i giorni in cui l'operatore è disponibile per le pagine (in base alle ore di inizio e di fine specificate). *pager_days* è di **tinyint** e il valore predefinito è **0** che indica che l'operatore non è mai disponibile per la ricezione di una pagina. I valori validi sono compresi tra **0** e **127**. *pager_days* viene calcolato aggiungendo i singoli valori per i giorni richiesti. Ad esempio, da lunedì a venerdì sono **2** + **4** + **8** + **16** + **32**  =  **62**. Nella tabella seguente vengono elencati i valori disponibili per ogni giorno della settimana.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Sunday|  
|**2**|Monday|  
|**4**|Tuesday|  
|**8**|Wednesday|  
|**16**|Thursday|  
|**32**|Friday|  
|**64**|Sabato|  
  
`[ @netsend_address = ] 'netsend_address'` Indirizzo di rete dell'operatore a cui viene inviato il messaggio di rete. *netsend_address* è di **tipo nvarchar (100)** e il valore predefinito è null.  
  
`[ @category_name = ] 'category'` Nome della categoria per questo operatore. *Category* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_add_operator** deve essere eseguito dal database **msdb** .  
  
 L'invio di messaggi sul cercapersone è supportato dal sistema di posta elettronica, in cui deve essere disponibile la funzionalità per il trasferimento di messaggi su cercapersone.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è incluso un semplice strumento grafico per la gestione dei processi, che è lo strumento consigliato per la creazione e la gestione dell'infrastruttura dei processi.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_operator**.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono impostate e attivate le informazioni per l'operatore `danwi`. L'operatore è abilitato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent invia notifiche tramite cercapersone da lunedì a venerdì, dalle 8.00 alle 17.00.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_operator  
    @name = N'Dan Wilson',  
    @enabled = 1,  
    @email_address = N'danwi',  
    @pager_address = N'5551290AW@pager.Adventure-Works.com',  
    @weekday_pager_start_time = 080000,  
    @weekday_pager_end_time = 170000,  
    @pager_days = 62 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_delete_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_help_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_update_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

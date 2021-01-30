---
description: sysmail_help_queue_sp (Transact-SQL)
title: sysmail_help_queue_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_queue_sp
- sysmail_help_queue_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_queue_sp
ms.assetid: 94840482-112c-4654-b480-9b456c4c2bca
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6aece593fd32d5ee563c7b1fe37dc039aa3eec5c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181926"
---
# <a name="sysmail_help_queue_sp-transact-sql"></a>sysmail_help_queue_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In Posta elettronica database esistono due code, la coda della posta e la coda dello stato. Nella coda della posta vengono archiviati gli elementi di posta in attesa di essere inviati. Nella coda dello stato viene archiviato lo stato degli elementi già inviati. Questa stored procedure consente di visualizzare lo stato della coda della posta o dello stato. Se il parametro **\@ queue_type** non è specificato, il stored procedure restituisce una riga per ogni coda.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_help_queue_sp  [ @queue_type = ] 'queue_type'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @queue_type = ] 'queue_type'` Argomento facoltativo consente di eliminare i messaggi di posta elettronica del tipo specificato come *queue_type*. *queue_type* è di **tipo nvarchar (6)** e non prevede alcun valore predefinito. Le voci valide sono **mail** e **status**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**queue_type**|**nvarchar (6)**|Tipo di coda. I valori possibili sono **mail** e **status**.|  
|**length**|**int**|Numero di elementi di posta nella coda specificata.|  
|**state**|**nvarchar (64)**|Stato del server di monitoraggio. I valori possibili sono **inattivi** (la coda è inattiva), **notificata** (la ricezione della coda è stata notificata) e **RECEIVES_OCCURRING** (la coda sta ricevendo).|  
|**last_empty_rowset_time**|**DATETIME**|Data e ora dell'ultimo svuotamento della coda, sia nel formato 24 ore sia nel fuso orario GMT.|  
|**last_activated_time**|**DATETIME**|Data e ora dell'ultima attivazione della coda, sia nel formato 24 ore sia nel fuso orario GMT.|  
  
## <a name="remarks"></a>Commenti  
 Per la risoluzione dei problemi Posta elettronica database, utilizzare **sysmail_help_queue_sp** per visualizzare il numero di elementi presenti nella coda, lo stato della coda e l'ultimo attivazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, solo i membri del ruolo predefinito del server **sysadmin** possono accedere a questa procedura.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite entrambe le code della posta e dello stato.  
  
```  
EXECUTE msdb.dbo.sysmail_help_queue_sp ;  
GO  
```  
  
 Set di risultati di esempio, modificato per motivi di lunghezza.  
  
```  
queue_type length      state              last_empty_rowset_time  last_activated_time  
---------- -------- ------------------ ----------------------- -----------------------  
mail       0        RECEIVES_OCCURRING 2005-10-07 21:14:47.010 2005-10-10 20:52:51.517  
status     0        INACTIVE           2005-10-07 21:04:47.003 2005-10-10 21:04:47.003  
  
(2 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)  
  
  

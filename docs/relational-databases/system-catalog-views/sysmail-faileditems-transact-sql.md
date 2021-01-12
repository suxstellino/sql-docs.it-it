---
description: sysmail_faileditems (Transact-SQL)
title: sysmail_faileditems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_faileditems
- sysmail_faileditems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_faileditems database mail view
ms.assetid: a31562c5-358e-4cfc-a72d-b3faccc53851
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0fc32200b893022e16bcd4ee7049da82c4572660
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100133"
---
# <a name="sysmail_faileditems-transact-sql"></a>sysmail_faileditems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni messaggio di Posta elettronica database con lo stato **non riuscito** . Utilizzare questa vista per controllare quali messaggi non sono stati inviati correttamente.  
  
 Per visualizzare tutti i messaggi elaborati da Posta elettronica database, utilizzare [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Per visualizzare solo i messaggi non inviati, utilizzare [sysmail_unsentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Per visualizzare solo i messaggi inviati, utilizzare [sysmail_sentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md). Per visualizzare gli allegati di posta elettronica, utilizzare [sysmail_mailattachments &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificatore dell'elemento di posta nella coda della posta.|  
|**profile_id**|**int**|Identificatore del profilo utilizzato per l'invio del messaggio.|  
|**destinatari**|**ntext**|Indirizzi di posta elettronica dei destinatari del messaggio.|  
|**copy_recipients**|**ntext**|Indirizzi di posta elettronica degli utenti che ricevono una copia del messaggio.|  
|**blind_copy_recipients**|**ntext**|Indirizzi di posta elettronica degli utenti che ricevono una copia del messaggio, ma i cui nomi non sono indicati nell'intestazione del messaggio.|  
|**subject**|**nvarchar (510)**|Oggetto del messaggio.|  
|**body**|**ntext**|Corpo del messaggio.|  
|**body_format**|**varchar (20)**|Formato del corpo del messaggio. I possibili valori sono TEXT e HTML.|  
|**importance**|**varchar (6)**|Parametro di **importanza** del messaggio.|  
|**sensibilità**|**varchar (12)**|Parametro di **riservatezza** del messaggio.|  
|**file_attachments**|**ntext**|Elenco delimitato da punti e virgola dei nomi dei file allegati al messaggio di posta elettronica.|  
|**Attachment_encoding**|**varchar (20)**|Tipo di allegato del messaggio di posta elettronica.|  
|**Query**|**ntext**|Query eseguita dal programma di posta elettronica.|  
|**execute_query_database**|**sysname**|Contesto di database all'interno del quale il programma di posta elettronica ha eseguito la query.|  
|**attach_query_result_as_file**|**bit**|Quando il valore è 0, i risultati della query sono inclusi nel corpo del messaggio di posta elettronica, dopo il contenuto del corpo. Quando il valore è 1, i risultati sono restituiti come file allegato.|  
|**query_result_header**|**bit**|Quando il valore è 1, i risultati della query includono le intestazioni di colonna. Quando il valore è 0, i risultati della query non includono le intestazioni di colonna.|  
|**query_result_width**|**int**|Parametro **query_result_width** del messaggio.|  
|**query_result_separator**|**char(1)**|Carattere utilizzato per separare le colonne nell'output della query.|  
|**exclude_query_output**|**bit**|Parametro **exclude_query_output** del messaggio. Per ulteriori informazioni, vedere [sp_send_dbmail &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|Parametro **append_query_error** del messaggio. 0 indica che Posta elettronica database non deve inviare il messaggio di posta elettronica se la query contiene un errore.|  
|**send_request_date**|**datetime**|Data e ora di inserimento del messaggio nella coda della posta.|  
|**send_request_user**|**sysname**|Utente che ha inviato il messaggio. Corrisponde al contesto utente della procedura di Posta elettronica database e non al campo Da del messaggio.|  
|**sent_account_id**|**int**|Identificatore dell'account di Posta elettronica database utilizzato per l'invio del messaggio. Per questa vista è sempre NULL.|  
|**sent_status**|**varchar (8)**|Stato del messaggio. **Non è sempre riuscito** per questa visualizzazione.|  
|**sent_date**|**datetime**|Data e ora di rimozione del messaggio dalla coda della posta.|  
|**last_mod_date**|**datetime**|Data e ora dell'ultima modifica della riga.|  
|**last_mod_user**|**sysname**|Autore dell'ultima modifica della riga.|  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare la vista **sysmail_faileditems** per vedere quali messaggi non sono stati inviati da posta elettronica database. Quando si risolvono i problemi relativi a Posta elettronica database, questa vista può consentire di identificare la natura del problema in quanto indica gli attributi dei messaggi che non sono stati inviati. Per visualizzare la causa dell'errore, vedere la voce relativa al messaggio non riuscito nel [sysmail_event_log &#40;vista&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md) .  
  
## <a name="permissions"></a>Autorizzazioni  
 Concesso al ruolo predefinito del server **sysadmin** e al ruolo del database **DatabaseMailUserRole** . Se eseguita da un membro del ruolo predefinito del server **sysadmin** , questa vista Mostra tutti i messaggi non riusciti. Tutti gli altri utenti vedono semplicemente i messaggi non recapitati che hanno cercato di inviare personalmente.  
  
  

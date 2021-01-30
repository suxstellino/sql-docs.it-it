---
description: sysmail_sentitems (Transact-SQL)
title: sysmail_sentitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_sentitems_TSQL
- sysmail_sentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_sentitems database mail view
ms.assetid: 16eb2a44-cebb-4cec-93ac-e2498c39989f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ef28d1d962fbdcfffd13dee4599b30617bbc36ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199703"
---
# <a name="sysmail_sentitems-transact-sql"></a>sysmail_sentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contiene una riga per ogni messaggio inviato da Posta elettronica database. Utilizzare **sysmail_sentitems** quando si desidera visualizzare i messaggi inviati correttamente.  
  
 Per visualizzare tutti i messaggi elaborati da Posta elettronica database, utilizzare [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Per visualizzare solo i messaggi con stato non riuscito, utilizzare [sysmail_faileditems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Per visualizzare solo i messaggi non inviati o ritentati, utilizzare [sysmail_unsentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Per visualizzare gli allegati di posta elettronica, utilizzare [sysmail_mailattachments &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificatore dell'elemento di posta nella coda della posta.|  
|**profile_id**|**int**|Identificatore del profilo utilizzato per l'invio del messaggio.|  
|**destinatari**|**ntext**|Indirizzi di posta elettronica dei destinatari del messaggio.|  
|**copy_recipients**|**ntext**|Indirizzi di posta elettronica degli utenti che ricevono una copia del messaggio.|  
|**blind_copy_recipients**|**ntext**|Indirizzi di posta elettronica degli utenti che ricevono una copia del messaggio, ma i cui nomi non sono indicati nell'intestazione del messaggio.|  
|**subject**|**nvarchar (510)**|Oggetto del messaggio.|  
|**body**|**ntext**|Corpo del messaggio.|  
|**body_format**|**varchar (20)**|Formato del corpo del messaggio. I valori possibili sono **Text** e **HTML**.|  
|**importance**|**varchar (6)**|Parametro di **importanza** del messaggio.|  
|**sensibilità**|**varchar (12)**|Parametro di **riservatezza** del messaggio.|  
|**file_attachments**|**ntext**|Elenco delimitato da punti e virgola dei nomi dei file allegati al messaggio di posta elettronica.|  
|**attachment_encoding**|**varchar (20)**|Tipo di allegato del messaggio di posta elettronica.|  
|**query**|**ntext**|Query eseguita dal programma di posta elettronica.|  
|**execute_query_database**|**sysname**|Contesto di database all'interno del quale il programma di posta elettronica ha eseguito la query.|  
|**attach_query_result_as_file**|**bit**|Quando il valore è 0, i risultati della query sono inclusi nel corpo del messaggio di posta elettronica, dopo il contenuto del corpo. Quando il valore è 1, i risultati sono restituiti come file allegato.|  
|**query_result_header**|**bit**|Quando il valore è 1, i risultati della query includono le intestazioni di colonna. Quando il valore è 0, i risultati della query non includono le intestazioni di colonna.|  
|**query_result_width**|**int**|Parametro **query_result_width** del messaggio.|  
|**query_result_separator**|**char(1)**|Carattere utilizzato per separare le colonne nell'output della query.|  
|**exclude_query_output**|**bit**|Parametro **exclude_query_output** del messaggio. Per ulteriori informazioni, vedere [sp_send_dbmail &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|Parametro **append_query_error** del messaggio. 0 indica che Posta elettronica database non deve inviare il messaggio di posta elettronica se la query contiene un errore.|  
|**send_request_date**|**datetime**|Data e ora di inserimento del messaggio nella coda della posta.|  
|**send_request_user**|**sysname**|Utente che ha inviato il messaggio. Corrisponde al contesto utente della procedura di Posta elettronica database e non al campo Da del messaggio.|  
|**sent_account_id**|**int**|Identificatore dell'account di Posta elettronica database utilizzato per l'invio del messaggio.|  
|**sent_status**|**varchar (8)**|Stato del messaggio. **Inviati** sempre per questa visualizzazione.|  
|**sent_date**|**datetime**|Data e ora di invio del messaggio.|  
|**last_mod_date**|**datetime**|Data e ora dell'ultima modifica della riga.|  
|**last_mod_user**|**sysname**|Autore dell'ultima modifica della riga.|  
  
## <a name="remarks"></a>Commenti  
 Quando si risolvono i problemi relativi a Posta elettronica database, questa vista può consentire di identificare la natura del problema in quanto indica gli attributi dei messaggi che sono stati inviati. Posta elettronica database contrassegna i messaggi come inviati quando sono stati inoltrati correttamente a un server di posta elettronica SMTP. In genere, il recapito di un messaggio di posta elettronica richiede alcuni minuti, ma può essere ritardato a causa di problemi del server SMTP. Posta elettronica database contrassegna il messaggio come inviato quando viene accettato dal server di posta elettronica SMTP. Gli errori dei messaggi di posta elettronica che si verificano nel server di posta elettronica SMTP, ad esempio un indirizzo di posta elettronica errato per un destinatario, non vengono restituiti a Posta elettronica database. Questi messaggi di posta elettronica risultano pertanto inviati anche se non sono stati recapitati. Questo tipo di errore deve essere risolto nel server SMTP. Inoltre, il server di posta elettronica SMTP può inviare una notifica per segnalare il mancato recapito di un messaggio all'indirizzo di posta elettronica per le risposte di un account di Posta elettronica database.  
  
## <a name="permissions"></a>Autorizzazioni  
 Concesso al ruolo predefinito del server **sysadmin** e al ruolo del database **DatabaseMailUserRole** . Se eseguita da un membro del ruolo predefinito del server **sysadmin** , questa vista Mostra tutti i messaggi inviati. Tutti gli altri utenti vedono semplicemente i messaggi che hanno inviato personalmente.  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetti di messaggistica di Posta elettronica database](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
  

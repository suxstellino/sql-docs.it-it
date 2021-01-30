---
description: sysmail_allitems (Transact-SQL)
title: sysmail_allitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_allitems_TSQL
- sysmail_allitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_allitems database mail view
ms.assetid: 21fb8432-7677-4435-902f-64a58bba4cbb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 25b60bb95655cea1d4e460b46a0e21faf28a3103
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199725"
---
# <a name="sysmail_allitems-transact-sql"></a>sysmail_allitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contiene una riga per ogni messaggio elaborato da Posta elettronica database. Utilizzare questa vista quando si desidera controllare lo stato di tutti i messaggi.  
  
 Per visualizzare solo i messaggi con stato non riuscito, utilizzare [sysmail_faileditems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Per visualizzare solo i messaggi non inviati, utilizzare [sysmail_unsentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Per visualizzare solo i messaggi inviati, utilizzare [sysmail_sentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
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
|**sent_status**|**varchar (8)**|Stato del messaggio. I valori possibili sono:<br /><br /> **inviato** : il messaggio di posta elettronica è stato inviato.<br /><br /> non **inviato** -posta elettronica database sta ancora tentando di inviare il messaggio.<br /><br /> nuovo **tentativo** -posta elettronica database non è riuscito a inviare il messaggio ma sta provando a inviarlo di nuovo.<br /><br /> **errore** : posta elettronica database non è stato in grado di inviare il messaggio.|  
|**sent_date**|**datetime**|Data e ora di invio del messaggio.|  
|**last_mod_date**|**datetime**|Data e ora dell'ultima modifica della riga.|  
|**last_mod_user**|**sysname**|Autore dell'ultima modifica della riga.|  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la vista **sysmail_allitems** per visualizzare lo stato di tutti i messaggi elaborati da posta elettronica database. Quando si risolvono i problemi relativi a Posta elettronica database, questa vista può consentire di identificare la natura del problema in quanto indica gli attributi dei messaggi che sono stati inviati e gli attributi dei messaggi che non sono stati inviati.  
  
 Le tabelle di sistema esposte da questa vista contengono tutti i messaggi e possono causare l'aumento delle dimensioni del database **msdb** . Eliminare periodicamente i messaggi meno recenti da questa vista al fine di limitare le dimensioni delle tabelle. Per ulteriori informazioni, vedere [la pagina relativa alla creazione di un processo di SQL Server Agent per l'archiviazione di messaggi posta elettronica database e log eventi](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Concesso al ruolo predefinito del server **sysadmin** e al ruolo del database **DatabaseMailUserRole** . Quando eseguita da un membro del ruolo predefinito del server **sysadmin** , questa vista Mostra tutti i messaggi. Tutti gli altri utenti vedono semplicemente i messaggi che hanno cercato di inviare personalmente.  
  
  

---
description: sysmail_unsentitems (Transact-SQL)
title: sysmail_unsentitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_unsentitems_TSQL
- sysmail_unsentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_unsentitems database mail view
ms.assetid: 993c12da-41e5-4e53-a188-0323feb70c67
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c532febf9074456ec1806fdb9d7ad0eed9b9b26a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199683"
---
# <a name="sysmail_unsentitems-transact-sql"></a>sysmail_unsentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contiene una riga per ogni messaggio di Posta elettronica database con stato non **inviato** o nuovo **tentativo** . I messaggi con uno di questi due stati si trovano ancora nella coda della posta e potrebbero essere inviati in qualsiasi momento. Per i messaggi è possibile che lo stato non sia **inviato** per i motivi seguenti:  
  
-   Il messaggio è nuovo e, anche se è stato inserito nella coda della posta, non è ancora stato elaborato da Posta elettronica database che sta inviando altri messaggi.  
  
-   Il programma esterno Posta elettronica database non è in esecuzione e attualmente la posta non viene inviata.  
  
 I messaggi possono avere lo stato nuovo **tentativo** per i motivi seguenti:  
  
-   Posta elettronica database ha tentato di inviare il messaggio, ma non è stato possibile contattare il server di posta elettronica SMTP. Posta elettronica database continuerà a tentare di inviare il messaggio utilizzando altri account di Posta elettronica database assegnati al profilo che ha inviato il messaggio. Se nessun account è in grado di inviare il messaggio, Posta elettronica database attenderà il periodo di tempo configurato per il parametro **ritardo tentativi account** , quindi tenterà di inviare nuovamente il messaggio. Posta elettronica database usa il parametro **tentativi account** per determinare il numero di tentativi di invio del messaggio. I messaggi **mantengono lo stato nuovo tentativo finché** posta elettronica database sta tentando di inviare il messaggio.  
  
 Utilizzare questa vista quando si desidera controllare il numero di messaggi in attesa di essere inviati e da quanto tempo sono presenti nella coda della posta. Normalmente il numero di messaggi non **inviati** sarà basso. Eseguire un test di benchmark durante il normale funzionamento per determinare il numero ragionevole di messaggi che può essere presente nella coda in condizioni di lavoro regolari.  
  
 Per visualizzare tutti i messaggi elaborati da Posta elettronica database, utilizzare [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Per visualizzare solo i messaggi con stato non riuscito, utilizzare [sysmail_faileditems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Per visualizzare solo i messaggi inviati, utilizzare [sysmail_sentitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
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
|**send_request_user**|**sysname**|Utente che ha inviato il messaggio. Questo è il contesto utente della procedura di posta elettronica database e non il campo **da** del messaggio.|  
|**sent_account_id**|**int**|Identificatore dell'account di Posta elettronica database utilizzato per l'invio del messaggio. Per questa vista è sempre NULL.|  
|**sent_status**|**varchar (8)**|**Verrà annienteto se posta elettronica database** non ha tentato di inviare il messaggio di posta elettronica. Verrà eseguito un nuovo **tentativo** se posta elettronica database non è riuscito a inviare il messaggio, ma sta provando nuovamente.|  
|**sent_date**|**datetime**|Data e ora dell'ultimo tentativo di invio del messaggio. Se Posta elettronica database non ha tentato di inviare il messaggio, il valore di questo parametro è NULL.|  
|**last_mod_date**|**datetime**|Data e ora dell'ultima modifica della riga.|  
|**last_mod_user**|**sysname**|Autore dell'ultima modifica della riga.|  
  
## <a name="remarks"></a>Commenti  
 Quando si risolvono i problemi relativi a Posta elettronica database, questa vista può consentire di identificare la natura del problema in quanto indica il numero di messaggi in attesa di essere inviati e il tempo di attesa nella coda. Se nessun messaggio viene inviato, è possibile che il programma esterno Posta elettronica database non sia in esecuzione oppure che esista un problema di rete che impedisce a Posta elettronica database di contattare i server SMTP. Se molti dei messaggi non inviati hanno lo stesso **profile_id**, potrebbe essersi verificato un problema con il server SMTP. Considerare l'opportunità di aggiungere altri account al profilo. Se i messaggi vengono inviati ma il tempo di attesa nella coda è eccessivo, è possibile che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necessiti di un maggior numero di risorse per l'elaborazione del volume di messaggi scambiati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Concesso al ruolo predefinito del server **sysadmin** e al ruolo del database **DatabaseMailUserRole** . Quando viene eseguita da un membro del ruolo predefinito del server **sysadmin** , questa vista Mostra tutti i messaggi non **inviati** o **ritentati** . Tutti gli altri utenti visualizzano **solo i messaggi inviati o non** **inviati** .  
  
  

---
description: sysmail_event_log (Transact-SQL)
title: sysmail_event_log (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_event_log
- sysmail_event_log_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_event_log database mail view
ms.assetid: 440bc409-1188-4175-afc4-c68e31e44fed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 58c8e734ed9acdef41b22772c8d3955b825a46de
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096638"
---
# <a name="sysmail_event_log-transact-sql"></a>sysmail_event_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni messaggio di Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito dal sistema di Posta elettronica database. Il messaggio in questo contesto si riferisce a un messaggio, ad esempio un messaggio di errore e non un messaggio di posta elettronica. Configurare il parametro del **livello di registrazione** utilizzando la finestra di dialogo **Configura parametri di sistema** della configurazione guidata di Posta elettronica database o il stored procedure [sysmail_configure_sp](../../relational-databases/system-stored-procedures/sysmail-configure-sp-transact-sql.md) per determinare quali messaggi vengono restituiti.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Log_id**|**int**|Identificatore degli elementi nel log.|  
|**event_type**|**varchar (11)**|Tipo di avviso inserito nel log. I possibili valori sono errori, avvisi, messaggi informativi, messaggi di operazione riuscita e altri messaggi interni.|  
|**log_date**|**datetime**|Data e ora di creazione della voce del log.|  
|**description**|**nvarchar(max)**|Testo del messaggio registrato.|  
|**process_id**|**int**|ID di processo del programma esterno Posta elettronica database. In genere, viene modificato a ogni avvio del programma esterno Posta elettronica database.|  
|**mailitem_id**|**int**|Identificatore dell'elemento di posta nella coda della posta. Se il messaggio non è correlato a un elemento di posta elettronica specifico, viene restituito NULL.|  
|**account_id**|**int**|**Account_id** dell'account correlato all'evento. Se il messaggio non è correlato a un account specifico, viene restituito NULL.|  
|**last_mod_date**|**datetime**|Data e ora dell'ultima modifica della riga.|  
|**last_mod_user**|**sysname**|Autore dell'ultima modifica della riga. Per i messaggi di posta elettronica corrisponde all'utente che ha inviato il messaggio. Per i messaggi generati dal programma esterno Posta elettronica database corrisponde al contesto utente del programma.|  
  
## <a name="remarks"></a>Osservazioni  
 Per la risoluzione dei problemi Posta elettronica database, cercare gli eventi relativi agli errori di posta elettronica nella visualizzazione **sysmail_event_log** . Alcuni messaggi, ad esempio quelli relativi agli errori del programma esterno Posta elettronica database, non sono associati a messaggi di posta elettronica specifici. Per cercare gli errori relativi a messaggi di posta elettronica specifici, cercare l' **mailitem_id** del messaggio di posta elettronica non riuscito nella visualizzazione **sysmail_faileditems** e quindi cercare nei **sysmail_event_log** i messaggi correlati a tale **mailitem_id**. Quando viene restituito un errore da **sp_send_dbmail**, il messaggio di posta elettronica non viene inviato al sistema posta elettronica database e l'errore non viene visualizzato in questa vista.  
  
 Quando i tentativi di recapito con singoli account hanno esito negativo, i messaggi di errore rimangono visualizzati durante i successivi tentativi di invio fino a quando il recapito dell'elemento di posta non ha definitivamente esito positivo o negativo. In caso di esito positivo, tutti gli errori accumulati vengono registrati come avvisi distinti, inclusa la **account_id**. Pertanto, è possibile che vengano visualizzati messaggi di avviso anche se il messaggio di posta elettronica è stato inviato. In caso di errore di recapito finale, tutti gli avvisi precedenti vengono registrati come un messaggio di errore senza una **account_id**, perché tutti gli account hanno avuto esito negativo.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per accedere a questa vista, è necessario essere un membro del ruolo predefinito del server **sysadmin** o del ruolo del database **DatabaseMailUserRole** . I membri di **DatabaseMailUserRole** che non sono membri del ruolo **sysadmin** possono visualizzare solo gli eventi relativi ai messaggi di posta elettronica che inviano.  
  
## <a name="see-also"></a>Vedere anche  
 [sysmail_faileditems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [Programma esterno di Posta elettronica database](../../relational-databases/database-mail/database-mail-external-program.md)  
  
  

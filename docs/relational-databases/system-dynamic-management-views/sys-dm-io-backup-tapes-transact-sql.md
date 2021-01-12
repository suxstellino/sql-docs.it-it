---
description: sys.dm_io_backup_tapes (Transact-SQL)
title: sys.dm_io_backup_tapes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_io_backup_tapes
- dm_io_backup_tapes_TSQL
- sys.dm_io_backup_tapes_TSQL
- dm_io_backup_tapes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_backup_tapes dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bc4eb0de019ee40145c15c344850744d9fef3b15
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101581"
---
# <a name="sysdm_io_backup_tapes-transact-sql"></a>sys.dm_io_backup_tapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce l'elenco dei dispositivi nastro e lo stato delle richieste di montaggio per i backup.   
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**physical_device_name**|**nvarchar (520)**|Nome del dispositivo fisico in cui è possibile creare un backup. Non ammette i valori Null.|  
|**logical_device_name**|**nvarchar(256)**|Nome specificato dall'utente per l'unità (da **sys.backup_devices**). NULL se non è disponibile alcun nome specificato dall'utente. Ammette i valori Null.|  
|**Stato**|**int**|Stato del nastro:<br /><br /> 1 = Aperto, disponibile per l'utilizzo<br /><br /> 2 = Montaggio in sospeso<br /><br /> 3 = In uso<br /><br /> 4 = Caricamento in corso<br /><br /> **Nota:** Mentre è in corso il caricamento di un nastro (**stato = 4**), l'etichetta del supporto non è ancora letta. Colonne in cui vengono copiati i valori delle etichette dei supporti, ad esempio **media_sequence_number**, visualizzano i valori previsti, che possono essere diversi dai valori effettivi sul nastro. Dopo la lettura dell'etichetta, **lo stato** diventa **3** (in uso) e le colonne con etichetta media riflettono il nastro effettivo caricato.<br /><br /> Non ammette i valori Null.|  
|**status_desc**|**nvarchar (520)**|Descrizione dello stato del nastro:<br /><br /> AVAILABLE<br /><br /> MOUNT PENDING<br /><br /> IN USE<br /><br /> LOADING MEDIA<br /><br /> Non ammette i valori Null.|  
|**mount_request_time**|**datetime**|Data e ora in cui il montaggio è stato richiesto. NULL se non è presente alcun montaggio in sospeso (**status! = 2**). Ammette i valori Null.|  
|**mount_expiration_time**|**datetime**|Data e ora di scadenza della richiesta di montaggio (timeout). NULL se non è presente alcun montaggio in sospeso (**status! = 2**). Ammette i valori Null.|  
|**database_name**|**nvarchar(256)**|Database di cui eseguire il backup nel dispositivo. Ammette i valori Null.|  
|**spid**|**int**|ID di sessione. Identifica l'utente del nastro. Ammette i valori Null.|  
|**command**|**int**|Comando che esegue il backup. Ammette i valori Null.|  
|**command_desc**|**nvarchar(120)**|Descrizione del comando. Ammette i valori Null.|  
|**media_family_id**|**int**|Indice del gruppo di supporti (1..*. n*), *n* è il numero di gruppi di supporti nel set di supporti. Ammette i valori Null.|  
|**media_set_name**|**nvarchar(256)**|Nome del set di supporti (se esistente) specificato nell'opzione MEDIANAME al momento della creazione del set di supporti. Ammette i valori Null.|  
|**media_set_guid**|**uniqueidentifier**|Identificatore univoco del set di supporti. Ammette i valori Null.|  
|**media_sequence_number**|**int**|Indice del volume in un gruppo di supporti (1..*. n*). Ammette i valori Null.|  
|**tape_operation**|**int**|Operazione su nastro eseguita:<br /><br /> 1 = Lettura<br /><br /> 2 = Formattazione<br /><br /> 3 = Inizializzazione<br /><br /> 4 = Accodamento<br /><br /> Ammette i valori Null.|  
|**tape_operation_desc**|**nvarchar(120)**|Operazione su nastro in esecuzione:<br /><br /> READ<br /><br /> FORMAT<br /><br /> INIT<br /><br /> APPEND<br /><br /> Ammette i valori Null.|  
|**mount_request_type**|**int**|Tipo di richiesta di montaggio:<br /><br /> 1 = Nastro specifico. Il nastro identificato dai campi **media_ \** _ è obbligatorio.<br /><br /> 2 = Successivo gruppo di supporti. È richiesto il successivo gruppo di supporti non ancora ripristinato. Viene utilizzato quando si effettua il ripristino da un numero di dispositivi inferiore al numero di gruppi di supporti.<br /><br /> 3 = Nastro di continuità. Il gruppo di supporti viene esteso ed è necessario un nastro di continuità.<br /><br /> Ammette i valori Null.|  
|_ *mount_request_type_desc**|**nvarchar(120)**|Tipo di richiesta di montaggio:<br /><br /> SPECIFIC TAPE<br /><br /> NEXT MEDIA FAMILY<br /><br /> CONTINUATION VOLUME<br /><br /> Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre dell'autorizzazione VIEW SERVER STATE nel server.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative a I/O &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


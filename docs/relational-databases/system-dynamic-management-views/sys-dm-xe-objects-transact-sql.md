---
description: sys.dm_xe_objects (Transact-SQL)
title: sys.dm_xe_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_xe_objects
- sys.dm_xe_objects
- sys.dm_xe_objects_TSQL
- dm_xe_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_objects dynamic management view
- extended events [SQL Server], views
ms.assetid: 5d944b99-b097-491b-8cbd-b0e42b459ec0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6e54e02ec730fae0f57a8be8976d7fa694824bd0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098875"
---
# <a name="sysdm_xe_objects-transact-sql"></a>sys.dm_xe_objects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni oggetto esposto da un pacchetto dell'evento. Gli oggetti possibili sono i seguenti:  
  
-   Eventi. Gli eventi indicano punti di interesse in un percorso di esecuzione. Tutti gli eventi contengono informazioni su un punto di interesse.  
  
-   Azioni. Le azioni vengono eseguite in modo sincrono quando vengono generati gli eventi. Un'azione può aggiungere dati di runtime a un evento.  
  
-   Destinazioni. Le destinazioni elaborano gli eventi, in modo sincrono sul thread che attiva l'evento o in modo asincrono su un thread fornito dal sistema.  
  
-   Predicati. Le origini dei predicati recuperano i valori dalle origini dell'evento per confrontare le operazioni. Nel confronto tra predicati vengono comparati tipi di dati specifici e viene restituito un valore booleano.  
  
-   Tipi. Nei tipi vengono incapsulati la lunghezza e le caratteristiche della raccolta di byte, necessarie per interpretare i dati.  

 |Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(60)**|Nome dell'oggetto. il nome è univoco all'interno di un pacchetto per un tipo di oggetto specifico. Non ammette i valori Null.|  
|object_type|**nvarchar(60)**|Tipo dell'oggetto. object_type è uno dei seguenti:<br /><br /> evento<br /><br /> azione<br /><br /> target<br /><br /> pred_source<br /><br /> pred_compare<br /><br /> type<br /><br /> Non ammette i valori Null.|  
|package_guid|**uniqueidentifier**|GUID del pacchetto che espone questa azione. C'è una relazione molti-a-uno con sys.dm_xe_packages.package_id. Non ammette i valori Null.|  
|description|**nvarchar(256)**|Descrizione dell'azione. la descrizione viene impostata dall'autore del pacchetto. Non ammette i valori Null.|  
|capabilities|**int**|Bitmap che descrive le funzionalità dell'oggetto. Ammette i valori Null.|  
|capabilities_desc|**nvarchar(256)**|Elenca tutte le funzionalità dell'oggetto. Ammette i valori Null.<br /><br /> **Funzionalità che si applicano a tutti i tipi di oggetto**<br /><br /> -<br />                                **Privato**. Unico oggetto disponibile per uso interno e a cui non è possibile accedere tramite CREATE/ALTER EVENT SESSION DDL. In questa categoria rientrano le destinazioni e gli eventi di controllo oltre a un esiguo numero di oggetti utilizzati internamente.<br /><br /> ===============<br /><br /> **Funzionalità degli eventi**<br /><br /> -<br />                                **No_block**. L'evento si trova in un percorso di codice critico che non può essere bloccato per alcun motivo. Gli eventi con questa funzionalità non possono essere aggiunti ad alcuna sessione eventi che specifica NO_EVENT_LOSS.<br /><br /> ===============<br /><br /> **Funzionalità che si applicano a tutti i tipi di oggetto**<br /><br /> -<br />                                **Process_whole_buffers**. La destinazione utilizza un buffer di eventi alla volta, anziché evento per evento.<br /><br /> -<br />                        **Singleton**. In un processo può essere presente una sola istanza della destinazione. Sebbene più sessioni eventi possano fare riferimento alla stessa destinazione singleton, in realtà è presente una sola istanza e tale istanza visualizzerà ogni evento univoco solo una volta. Questo è importante se la destinazione viene aggiunta a più sessioni che raccolgono tutte lo stesso evento.<br /><br /> -<br />                                **Sincrono**. La destinazione viene eseguita sul thread che ha generato l'evento, prima che il controllo venga restituito alla riga di codice chiamante.|  
|type_name|**nvarchar(60)**|Nome per oggetti pred_source e pred_compare. Ammette i valori Null.|  
|type_package_guid|**uniqueidentifier**|GUID per il pacchetto che espone il tipo sul quale questo oggetto opera. Ammette i valori Null.|  
|type_size|**int**|Dimensione del tipo di dati espressa in byte. Solo per tipi di oggetti validi. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
### <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_objects.package_guid|sys.dm_xe_packages.guid|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  


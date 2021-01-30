---
description: sys.resource_usage (Database di SQL Azure)
title: sys.resource_usage (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.resource_usage_TSQL
- resource_usage
- sys.resource_usage
- resource_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- resource_usage
- sys.resource_usage
ms.assetid: b90147a3-fd8e-408e-961d-5c7000e068ad
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 1c9ef43ee6834a890359ea46bcab7ff388ec7d4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193004"
---
# <a name="sysresource_usage-azure-sql-database"></a>sys.resource_usage (Database di SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

    
> [!IMPORTANT]
>  Questa funzionalità si trova nello stato anteprima. Non specificare una dipendenza dall'implementazione specifica di questa funzionalità perché potrebbe essere modificata o rimossa in una versione successiva.  
> 
>  Nello stato anteprima, il team operativo del [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] potrebbe abilitare e disabilitare la raccolta dati per questa DMV:  
> 
>  -   Quando è abilitata, la DMV restituisce i dati correnti man mano che vengono aggregati.  
> -   Quando è disabilitata, la DMV restituisce i dati cronologici, che potrebbero non essere aggiornati.  
  
 Fornisce il riepilogo orario dei dati di utilizzo delle risorse per i database utente nel server corrente. I dati cronologici vengono mantenuti per 90 giorni.  
  
 Per ogni database utente è presente una riga per ogni ora in modo continuo. Anche se il database è inattivo durante quest'ora, è presente una riga e il valore di usage_in_seconds per il database sarà 0. Il rollup delle informazioni SKU e dell'utilizzo dell'archiviazione viene eseguito per l'ora in modo appropriato.  
  
|Colonne|Tipo di dati|Descrizione|  
|-------------|---------------|-----------------|  
|time|**datetime**|Ora (UTC) in incrementi di un'ora.|  
|database_name|**nvarchar**|Nome del database utente.|  
|sku|**nvarchar**|Nome della SKU. Di seguito sono indicati i valori possibili:<br /><br /> Web<br /><br /> Business<br /><br /> Basic<br /><br /> Standard<br /><br /> Premium|  
|usage_in_seconds|**int**|Somma del tempo della CPU utilizzato nell'ora.<br /><br /> Nota: questa colonna è deprecata per V11 e non si applica a V12. **Il valore è sempre impostato su 0.**|  
|storage_in_megabytes|**decimal**|Capacità di archiviazione massima per l'ora, inclusi dati del database, indici, stored procedure e metadati.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa vista è disponibile per tutti i ruoli utente con autorizzazioni per la connessione al database **Master** virtuale.  
  
  

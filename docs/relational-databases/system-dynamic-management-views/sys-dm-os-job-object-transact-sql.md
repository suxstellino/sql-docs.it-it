---
description: sys.dm_os_job_object (database SQL di Azure)
title: sys.dm_os_job_object (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: c9dbbe97b010e5fb47dd58f60f5863a63a5a36fd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342142"
---
# <a name="sysdm_os_job_object-azure-sql-database"></a>sys.dm_os_job_object (database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Restituisce una singola riga che descrive la configurazione dell'oggetto processo che gestisce il processo di SQL Server, nonché alcune statistiche sull'utilizzo delle risorse a livello di oggetto processo. Restituisce un set vuoto se SQL Server non è in esecuzione in un oggetto processo.

Un oggetto processo è un costrutto di Windows che implementa la governance delle risorse di CPU, memoria e i/o a livello del sistema operativo. Per ulteriori informazioni sugli oggetti processo, vedere [oggetti processo](/windows/desktop/ProcThread/job-objects).
  
|Colonne|Tipo di dati|Descrizione|  
|-------------|---------------|-----------------|  
|cpu_rate|**int**|Specifica la parte di cicli del processore che SQL Server thread possono utilizzare durante ogni intervallo di pianificazione. Il valore viene segnalato come percentuale dei cicli disponibili all'interno di un intervallo di pianificazione di 10000 cicli, moltiplicato per il numero di CPU logiche. Ad esempio, il valore 800 in un'istanza SQL Server con 8 CPU logiche significa che i thread possono utilizzare le CPU sono la capacità completa.|
|cpu_affinity_mask|**bigint**|Maschera di bit che descrive i processori logici che il processo di SQL Server può utilizzare all'interno del gruppo di processori. Ad esempio, cpu_affinity_mask 255 (1111 1111 in formato binario) indica che è possibile utilizzare i primi otto processori logici. <br /><br />Questa colonna è disponibile per la compatibilità con le versioni precedenti. Non segnala il gruppo di processori e il valore restituito potrebbe non essere corretto quando un gruppo di processori contiene più di 64 processori logici. Utilizzare la `process_physical_affinity` colonna per determinare invece l'affinità del processore.|
|cpu_affinity_group|**int**|Numero del gruppo di processori utilizzato dal SQL Server.|
|memory_limit_mb|**bigint**|Quantità massima di memoria vincolata, in MB, che tutti i processi nell'oggetto processo, incluso SQL Server, possono utilizzare cumulativamente.| 
|process_memory_limit_mb |**bigint**|Quantità massima di memoria vincolata, in MB, che un singolo processo nell'oggetto processo, ad esempio SQL Server, può utilizzare.|
|workingset_limit_mb |**bigint**|Quantità massima di memoria, in MB, che può essere utilizzata dal SQL Server working set.|
|non_sos_mem_gap_mb|**bigint**|Quantità di memoria, in MB, riservata per gli stack di thread, le dll e altre allocazioni di memoria non SOS. La memoria di destinazione SOS è la differenza tra `process_memory_limit_mb` e `non_sos_mem_gap_mb` .| 
|low_mem_signal_threshold_mb|**bigint**|Una soglia di memoria, in MB. Quando la quantità di memoria disponibile per l'oggetto processo è inferiore a questa soglia, al processo di SQL Server viene inviato un segnale di notifica di memoria insufficiente. |
|total_user_time|**bigint**|Il numero totale di cicli di 100 NS che i thread all'interno dell'oggetto processo hanno impiegato in modalità utente, dal momento della creazione dell'oggetto processo. |
|total_kernel_time |**bigint**|Il numero totale di cicli di 100 NS che i thread all'interno dell'oggetto processo hanno impiegato in modalità kernel, dall'inizio della creazione dell'oggetto processo. |
|write_operation_count |**bigint**|Numero totale di operazioni di i/o di scrittura sui dischi locali emessi da SQL Server dall'oggetto del processo creato. |
|read_operation_count |**bigint**|Numero totale di operazioni di i/o di lettura sui dischi locali emessi da SQL Server dall'oggetto del processo creato. |
|peak_process_memory_used_mb|**bigint**|Quantità massima di memoria, in MB, utilizzata da un singolo processo nell'oggetto processo, ad esempio SQL Server, dopo la creazione dell'oggetto processo.| 
|peak_job_memory_used_mb|**bigint**|Quantità massima di memoria, in MB, che tutti i processi nell'oggetto processo sono stati utilizzati cumulativamente dall'oggetto processo creato.|
|process_physical_affinity|**nvarchar (3072)**|Maschere di bit che descrivono i processori logici che il processo di SQL Server può utilizzare in ogni gruppo di processori. Il valore di questa colonna è costituito da una o più coppie di valori, ciascuna racchiusa tra parentesi graffe. In ogni coppia il primo valore è il numero del gruppo di processori e il secondo valore è la maschera di bit di affinità per il gruppo di processori. Ad esempio, il valore `{{0,a}{1,2}}` indica che la maschera di affinità per il gruppo di processori `0` è `a` ( `1010` in formato binario, che indica che vengono utilizzati i processori 2 e 4) e che la maschera di affinità per il gruppo di processori `1` è `2` ( `10` in formato binario, che indica che viene utilizzato il processore 2).|
  
## <a name="permissions"></a>Autorizzazioni  
In SQL Istanza gestita richiede l' `VIEW SERVER STATE` autorizzazione. Nel database SQL è richiesta l'autorizzazione `VIEW DATABASE STATE` nel database.  
 
## <a name="see-also"></a>Vedere anche  

Per informazioni sulle istanze gestite, vedere [SQL istanza gestita](/azure/sql-database/sql-database-managed-instance).

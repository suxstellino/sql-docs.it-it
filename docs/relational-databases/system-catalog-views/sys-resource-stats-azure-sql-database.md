---
description: sys.resource_stats (Database SQL di Azure)
title: sys.resource_stats (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 09/13/2018
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- resource_stats
- sys.resource_stats
- sys.resource_stats_TSQL
- resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_stats
- resource_stats
ms.assetid: 02379a1b-3622-4578-8c59-a1b8f1a17914
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 54732718d3c31c8dc646c9f61b3afcfa23cb07d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343142"
---
# <a name="sysresource_stats-azure-sql-database"></a>sys.resource_stats (Database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce i dati di archiviazione e di utilizzo della CPU per un database SQL di Azure. I dati vengono raccolti e aggregati in intervalli di cinque minuti. Per ogni database utente è presente una riga per ogni finestra di report di cinque minuti in cui viene apportata una modifica al consumo delle risorse. I dati restituiti includono l'utilizzo della CPU, la modifica delle dimensioni di archiviazione e la modifica dello SKU del database. I database inattivi senza modifiche potrebbero non avere righe per ogni intervallo di cinque minuti. I dati cronologici vengono mantenuti per circa 14 giorni.  
  
 La visualizzazione **sys.resource_stats** ha definizioni diverse a seconda della versione del server di database SQL di Azure a cui è associato il database. Prendere in considerazione queste differenze e le eventuali modifiche richieste dall'applicazione durante l'aggiornamento a una nuova versione del server.  
  
 La tabella seguente descrive le colonne disponibili in un server v12:  
  
|Colonne|Tipo di dati|Descrizione|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime**|Ora UTC che indica l'inizio dell'intervallo di Reporting di cinque minuti.|  
|end_time|**datetime**|Ora UTC che indica la fine dell'intervallo di Reporting di cinque minuti.|  
|database_name|**nvarchar(128)**|Nome del database utente.|  
|sku|**nvarchar(128)**|Livello di servizio del database. Di seguito sono indicati i valori possibili:<br /><br /> Basic<br /><br /> Standard<br /><br /> Premium<br /><br />Utilizzo generico<br /><br />Business Critical|  
|storage_in_megabytes|**float**|Dimensioni massime di archiviazione in megabyte per il periodo di tempo, inclusi dati del database, indici, stored procedure e metadati.|  
|avg_cpu_percent|**Decimal (5, 2)**|Percentuale dell'utilizzo medio del calcolo del limite del livello del servizio.|  
|avg_data_io_percent|**Decimal (5, 2)**|Percentuale dell'utilizzo medio di I/O in base al limite del livello del servizio. Per i database con iperscalabilità, vedere i/o [dati nelle statistiche sull'utilizzo delle risorse](/azure/sql-database/sql-database-hyperscale-performance-diagnostics#data-io-in-resource-utilization-statistics).|  
|avg_log_write_percent|**Decimal (5, 2)**|Percentuale dell'utilizzo medio delle risorse di scrittura del limite del livello del servizio.|  
|max_worker_percent|**Decimal (5, 2)**|Numero massimo di ruoli di lavoro simultanei (richieste) in percentuale in base al limite del livello di servizio del database.<br /><br /> Il valore massimo è attualmente calcolato per l'intervallo di cinque minuti in base ai campioni di 15 secondi dei conteggi di lavoro simultanei.|  
|max_session_percent|**Decimal (5, 2)**|Numero massimo di sessioni simultanee in percentuale in base al limite del livello di servizio del database.<br /><br /> Il valore massimo è attualmente calcolato per l'intervallo di cinque minuti in base agli esempi di 15 secondi relativi ai conteggi delle sessioni simultanee.|  
|dtu_limit|**int**|Impostazione DTU database Max corrente per questo database durante questo intervallo. |
|xtp_storage_percent|**Decimal (5, 2)**|Utilizzo dello spazio di archiviazione per In-Memory OLTP in percentuale del limite del livello di servizio (alla fine dell'intervallo di Reporting). Questo include la memoria usata per l'archiviazione dei seguenti oggetti di In-Memory OLTP: tabelle ottimizzate per la memoria, indici e variabili di tabella. Include inoltre la memoria utilizzata per l'elaborazione delle operazioni ALTER TABLE.<br /><br /> Restituisce 0 se In-Memory OLTP non viene utilizzato nel database.|
|avg_login_rate_percent|**Decimal (5, 2)**|Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|
|avg_instance_cpu_percent|**Decimal (5, 2)**|Utilizzo medio della CPU del database come percentuale del processo del database SQL.|
|avg_instance_memory_percent|**Decimal (5, 2)**|Utilizzo medio della memoria del database come percentuale del processo del database SQL.|
|cpu_limit|**Decimal (5, 2)**|Numero di Vcore per il database durante questo intervallo. Per i database che usano il modello basato su DTU, questa colonna è NULL.|
|allocated_storage_in_megabytes|**float**|Quantità di spazio file formattato in MB reso disponibile per l'archiviazione dei dati del database. Lo spazio file formattato viene anche definito spazio dati allocato.  Per ulteriori informazioni, vedere la pagina relativa alla [gestione dello spazio file nel database SQL](/azure/sql-database/sql-database-file-space-management)|
  
> [!TIP]  
>  Per ulteriori informazioni sui limiti e sui livelli di servizio, vedere gli argomenti [livelli di servizio](/azure/azure-sql/database/purchasing-models).  
    
## <a name="permissions"></a>Autorizzazioni  
 Questa vista è disponibile per tutti i ruoli utente con autorizzazioni per la connessione al database **Master** virtuale.  
  
## <a name="remarks"></a>Commenti  
 I dati restituiti da **sys.resource_stats** vengono espressi come percentuale dei limiti massimi consentiti per il livello di servizio o il livello di prestazioni che si sta eseguendo.  
  
 Quando un database è membro di un pool elastico, le statistiche sulle risorse presentate come valori percentuali vengono espresse come percentuale del limite massimo per i database impostati nella configurazione del pool elastico.  
  
 Per una visualizzazione più granulare di questi dati, utilizzare **sys.dm_db_resource_stats** DMV in un database utente. Questa vista acquisisce i dati ogni 15 secondi e conserva i dati cronologici per 1 ora.  Per altre informazioni, vedere [sys.dm_db_resource_stats &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md).  

## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituiti tutti i database che hanno una media di almeno l'80% di utilizzo del calcolo nell'ultima settimana.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT database_name, AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.resource_stats   
WHERE start_time BETWEEN @s AND @e  
GROUP BY database_name  
HAVING AVG(avg_cpu_percent) >= 80  
```  
    
## <a name="see-also"></a>Vedere anche  
 [Livelli di servizio](/azure/azure-sql/database/purchasing-models)   
 [Limiti e funzionalità dei livelli di servizio](/azure/azure-sql/database/performance-guidance)  
  

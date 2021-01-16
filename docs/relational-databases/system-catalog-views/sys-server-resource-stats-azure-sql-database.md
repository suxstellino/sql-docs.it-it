---
description: sys.server_resource_stats (database SQL di Azure)
title: sys.server_resource_stats (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2018
ms.service: sql-database
ms.topic: language-reference
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
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current
ms.openlocfilehash: 8a913c3bf4f01828fcf75df1e3c69dca9149e2de
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241822"
---
# <a name="sysserver_resource_stats-azure-sql-database"></a>sys.server_resource_stats (database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Restituisce l'utilizzo della CPU, l'i/o e i dati di archiviazione per Azure SQL Istanza gestita. I dati vengono raccolti e aggregati in intervalli di cinque minuti. Una riga viene segnalata ogni 15 secondi. I dati restituiti includono l'utilizzo della CPU, le dimensioni di archiviazione, l'utilizzo di IO e lo SKU. I dati cronologici vengono mantenuti per circa 14 giorni.

La visualizzazione **sys.server_resource_stats** ha definizioni diverse a seconda della versione di Azure SQL istanza gestita a cui è associato il database. Prendere in considerazione queste differenze e le eventuali modifiche richieste dall'applicazione durante l'aggiornamento a una nuova versione del server.
 
  
 La tabella seguente descrive le colonne disponibili in un server v12:  
  
|Colonne|Tipo di dati|Descrizione|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime2**|Ora UTC che indica l'inizio dell'intervallo di Reporting di quindici secondi|  
|end_time|**datetime**|Ora UTC che indica la fine dell'intervallo di Reporting di quindici secondi|
|resource_type|Nvarchar(128)|Tipo di risorsa per cui vengono fornite le metriche|
|resource_name|nvarchar(128)|Nome della risorsa.|
|sku|nvarchar(128)|Istanza gestita livello di servizio dell'istanza. Di seguito sono indicati i valori possibili: <br><ul><li>Utilizzo generico</li></ul><ul><li>Business Critical</li></ul>|
|hardware_generation|nvarchar(128)|Identificatore di generazione hardware: come gen 4 o gen 5|
|virtual_core_count|int|Rappresenta il numero di core virtuali per istanza (8, 16 o 24 in anteprima pubblica)|
|avg_cpu_percent|Decimal (5, 2)|Utilizzo medio del calcolo in percentuale del limite del livello di servizio Istanza gestita utilizzato dall'istanza. Viene calcolato come somma del tempo di CPU di tutti i pool di risorse per tutti i database nell'istanza e diviso per il tempo di CPU disponibile per tale livello nell'intervallo specificato.|
|reserved_storage_mb|bigint|Archiviazione riservata per istanza (quantità di spazio di archiviazione acquistata dal cliente per l'istanza gestita)|
|storage_space_used_mb|Decimal (18, 2)|Archiviazione usata da tutti i file di database in un'istanza gestita (inclusi i database utente e di sistema)|
|io_request|bigint|Numero totale di operazioni fisiche di i/o nell'intervallo|
|io_bytes_read|bigint|Numero di byte fisici letti nell'intervallo|
|io_bytes_written|bigint|Numero di byte fisici scritti nell'intervallo|

 
> [!TIP]  
>  Per ulteriori informazioni sui limiti e sui livelli di servizio, vedere gli argomenti [istanza gestita livelli di servizio](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers).  
    
## <a name="permissions"></a>Autorizzazioni  
 Questa vista è disponibile per tutti i ruoli utente con autorizzazioni per la connessione al database **Master** .  
  
## <a name="remarks"></a>Commenti  
 I dati restituiti da **sys.server_resource_stats** vengono espressi come il totale utilizzato in byte o megabyte (indicati in nomi di colonna) diversi da avg_cpu, espresso come percentuale dei limiti massimi consentiti per il livello di servizio o il livello di prestazioni in esecuzione.  
 
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene restituito l'utilizzo medio della CPU negli ultimi sette giorni.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.server_resource_stats   
WHERE start_time BETWEEN @s AND @e  
GO;
```  
    
## <a name="see-also"></a>Vedere anche  
 [Livelli di servizio di Istanza gestita](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers)

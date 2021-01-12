---
description: sys.dm_os_server_diagnostics_log_configurations
title: sys.dm_os_server_diagnostics_log_configurations | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_server_diagnostics_log_configurations
- sys.dm_os_server_diagnostics_log_configurations_TSQL
- dm_os_server_diagnostics_log_configurations
- dm_os_server_diagnostics_log_configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_server_diagnostics_log_configurations
- sys.dm_os_server_diagnostics_log_configurations
ms.assetid: c09ea433-d283-4f83-af69-d458aad59217
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d447261f24c503287dfa5a6503718cf804161194
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099817"
---
# <a name="sysdm_os_server_diagnostics_log_configurations"></a>sys.dm_os_server_diagnostics_log_configurations
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga con la configurazione corrente per il log di diagnostica del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Queste impostazioni delle proprietà determinano se la registrazione di diagnostica è abilitata o disabilitata e il percorso, il numero e la dimensione dei file di log.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|is_enabled|**bit**|Indica se la registrazione è abilitata o disabilitata.<br /><br /> 1 = la registrazione dei dati di diagnostica è abilitata<br /><br /> 0 = la registrazione dei dati di diagnostica è disabilitata|  
|max_size|**int**|Dimensione massima in megabyte che può raggiungere ogni log di diagnostica. Il valore predefinito è 100 MB.|  
|max_files|**int**|Numero massimo di file di log di diagnostica che è possibile archiviare nel computer prima che vengano riciclati per nuovi log di diagnostica.|  
|path|**nvarchar(260)**|Percorso che indica la posizione dei log di diagnostica. Il percorso predefinito è \<\MSSQL\Log> all'interno della cartella di installazione dell'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede le autorizzazioni VIEW SERVER STATE sull'istanza del cluster di failover di SQL Server.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato sys.dm_os_server_diagnostics_log_configurations per restituire le impostazioni delle proprietà per i log di diagnostica del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT <list of columns>  
FROM sys.dm_os_server_diagnostics_log_configurations;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|IS_ENABLED|PATH|MAX_SIZE|MAX_FILES|  
|-----------------|----------|---------------|----------------|  
|1|\<C:\Program Files\Microsoft SQL Server\MSSQL13\MSSQL\Log>|10|10|  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e leggere il log di diagnostica dell'istanza del cluster di failover](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md)  
  
  

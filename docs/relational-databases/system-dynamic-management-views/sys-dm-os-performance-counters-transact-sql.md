---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
ms.assetid: a1c3e892-cd48-40d4-b6be-2a9246e8fbff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e0e080554f8ee1a9a0ba68fe4c4292d2abdb3ee4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839152"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per contatore delle prestazioni gestito dal server. Per informazioni su ogni contatore delle prestazioni, vedere [usare oggetti SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_performance_counters**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar (128)**|Categoria di appartenenza del contatore.|  
|**counter_name**|**nchar (128)**|Nome del contatore. Per ottenere ulteriori informazioni su un contatore, questo è il nome dell'argomento da selezionare dall'elenco di contatori in [utilizzare SQL Server oggetti](../../relational-databases/performance-monitor/use-sql-server-objects.md). |  
|**instance_name**|**nchar (128)**|Nome dell'istanza specifica del contatore. Spesso include il nome del database.|  
|**cntr_value**|**bigint**|Valore corrente del contatore.<br /><br /> **Nota:** Per i contatori per secondo, questo valore è cumulativo. Il valore relativo alla frequenza deve essere calcolato tramite il campionamento del valore a intervalli di tempo discreti. La differenza tra due valori di campionamento successivi è uguale alla frequenza dell'intervallo di tempo utilizzato.|  
|**cntr_type**|**int**|Tipo di contatore definito dall'architettura di controllo delle prestazioni di Windows. Per ulteriori informazioni sui tipi di contatori delle prestazioni, vedere [tipi di contatori delle prestazioni WMI](/windows/desktop/WmiSdk/wmi-performance-counter-types) in docs o nella documentazione di Windows Server.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="remarks"></a>Commenti  
 Se l'istanza dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di visualizzare i contatori delle prestazioni del sistema operativo Windows, utilizzare la query [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente per verificare se i contatori delle prestazioni sono stati disabilitati.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Se il valore restituito è di 0 righe, i contatori delle prestazioni sono stati disabilitati. Esaminare quindi il registro di installazione e cercare l'errore 3409, `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` che indica che i contatori delle prestazioni non sono stati abilitati. Gli errori immediatamente precedenti all'errore 3409 dovrebbero indicare la causa principale per l'errore relativo all'abilitazione dei contatori delle prestazioni. Per ulteriori informazioni sui file di log del programma di installazione, vedere [visualizzare e leggere SQL Server file di log del programma di installazione](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

Contatori delle prestazioni in cui il `cntr_type` valore della colonna è 65792, 272696320 e 537003264 visualizzano un valore del contatore istantaneo istantaneo.

Contatori delle prestazioni in cui il `cntr_type` valore della colonna è 272696576, 1073874176 e 1073939712 visualizzano i valori dei contatori cumulativi anziché uno snapshot istantaneo. Di conseguenza, per ottenere una lettura simile a uno snapshot, è necessario confrontare il delta tra due punti di raccolta.

## <a name="permission"></a>Autorizzazione

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
 
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituiti tutti i contatori delle prestazioni che visualizzano i valori dei contatori dello snapshot.  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>Vedere anche  
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  

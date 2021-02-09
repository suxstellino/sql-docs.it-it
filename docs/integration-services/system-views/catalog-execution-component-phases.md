---
description: catalog.execution_component_phases
title: catalog.execution_component_phases | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 07a9a163-4787-40f7-b371-ac5c6cb4b095
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d449dae68ce74abcf8509eb9f6b2e22ef46c51bb
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835307"
---
# <a name="catalogexecution_component_phases"></a>catalog.execution_component_phases 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Visualizza il tempo trascorso da un componente del flusso di dati in ogni fase di esecuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|phase_stats_id|**bigint**|Identificatore univoco (ID) della fase.|  
|execution_id|**bigint**|ID univoco per l'istanza di esecuzione.|  
|package_name|**nvarchar(260)**|Nome del primo pacchetto avviato durante l'esecuzione.|  
|task_name|**nvarchar(4000)**|Nome dell'attività del flusso di dati.|  
|subcomponent_name|**nvarchar(4000)**|Nome del componente del flusso di dati.|  
|fase|**nvarchar(128)**|Nome della fase di esecuzione.|  
|start_time|**datetimeoffset(7)**|Ora di inizio della fase.|  
|end_time|**datetimeoffset(7)**|Ora di fine della fase.|  
|execution_path|**nvarchar(max)**|Percorso di esecuzione dell'attività del flusso di dati.|  
  
## <a name="remarks"></a>Commenti  
 In questa vista viene visualizzata una riga per ogni fase di esecuzione di un componente del flusso di dati, ad esempio Convalida, Pre-esecuzione, Post-esecuzione, PrimeOutput e ProcessInput. In ogni riga viene visualizzata l'ora di inizio e di fine per una fase di esecuzione specifica.  
  
## <a name="example"></a>Esempio  
 L'esempio seguente usa la vista catalog.execution_component_phases per calcolare il tempo totale necessario per l'esecuzione di un pacchetto specifico in tutte le fasi (**active_time**) e il tempo trascorso totale per il pacchetto (**total_time**).  
  
> [!WARNING]  
>  Nella vista catalog.execution_component_phases vengono fornite queste informazioni se il livello di registrazione dell'esecuzione del pacchetto è impostato su Prestazioni o Dettagliato. Per altre informazioni, vedere [Abilitare la registrazione per l'esecuzione di pacchetti nel server SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)  
  
```sql
use SSISDB  
select package_name, task_name, subcomponent_name, execution_path,  
    SUM(DATEDIFF(ms,start_time,end_time)) as active_time,  
    DATEDIFF(ms,min(start_time), max(end_time)) as total_time  
from catalog.execution_component_phases  
where execution_id = 1841  
group by package_name, task_name, subcomponent_name, execution_path  
order by package_name, task_name, subcomponent_name, execution_path  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'istanza di esecuzione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
> [!NOTE]  
>  Quando si dispone delle autorizzazioni per eseguire un'operazione nel server, si dispone anche delle autorizzazioni per visualizzare le informazioni sull'operazione. È applicata la sicurezza a livello di riga, pertanto vengono visualizzate solo le righe per le quali si dispone delle autorizzazioni per la visualizzazione.  
  
  

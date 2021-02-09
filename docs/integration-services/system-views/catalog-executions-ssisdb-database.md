---
description: catalog.executions (database SSISDB)
title: catalog.executions (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- executions view [Integration Services]
- catalog.executions view [Integration Services]
ms.assetid: 879f13b0-331d-4dee-a079-edfaca11ae5b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a95232db301aa538a166e930c4dde830691adaac
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835117"
---
# <a name="catalogexecutions-ssisdb-database"></a>catalog.executions (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Vengono visualizzate le istanze di esecuzione del pacchetto nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. I pacchetti eseguiti con l'attività Esegui pacchetto vengono eseguiti nella stessa istanza di esecuzione del pacchetto padre.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|execution_id|**bigint**|Identificatore (ID) univoco per l'istanza di esecuzione.|  
|folder_name|**sysname(nvarchar(128))**|Nome della cartella in cui è contenuto il progetto.|  
|project_name|**sysname(nvarchar(128))**|Nome del progetto.|  
|package_name|**nvarchar(260)**|Nome del primo pacchetto avviato durante l'esecuzione.|  
|reference_id|**bigint**|Ambiente a cui fa riferimento l'istanza di esecuzione.|  
|reference_type|**char(1)**|Viene indicato se l'ambiente può essere individuato nella stessa cartella del progetto (riferimento relativo) o in una cartella diversa (riferimento assoluto). Quando il valore è `R`, l'ambiente viene individuato tramite un riferimento relativo. Quando il valore è `A`, l'ambiente viene individuato tramite un riferimento assoluto.|  
|environment_folder_name|**nvarchar(128)**|Nome della cartella in cui è contenuto l'ambiente.|  
|environment_name|**nvarchar(128)**|Nome dell'ambiente a cui è stato fatto riferimento durante l'esecuzione.|  
|project_lsn|**bigint**|Versione del progetto che corrisponde all'istanza di esecuzione. La sequenzialità di questo numero non è garantita.|  
|executed_as_sid|**varbinary(85)**|SID dell'utente che ha avviato l'istanza di esecuzione.|  
|executed_as_name|**nvarchar(128)**|Nome dell'entità di database utilizzata per avviare l'istanza di esecuzione.|  
|use32bitruntime|**bit**|Viene indicato se il runtime a 32 bit viene utilizzato per eseguire il pacchetto in un sistema operativo a 64 bit. Quando il valore è `1`, l'esecuzione avviene con il runtime a 32 bit. Quando il valore è `0`, l'esecuzione viene eseguita con il runtime a 64 bit.|  
|object_type|**smallint**|Tipo di oggetto. Può trattarsi di un progetto (`20`) o di un pacchetto (`30`).|  
|object_id|**bigint**|ID dell'oggetto interessato dall'operazione.|  
|status|**int**|Stato dell'operazione. I valori possibili sono Creata (`1`), In esecuzione (`2`), Operazione annullata (`3`) Operazione non riuscita (`4`), In sospeso (`5`), Terminata in modo inatteso (`6`), Operazione riuscita (`7`), Arresto in corso (`8`) Operazione completata (`9`).|  
|start_time|**datetimeoffset**|Ora di avvio dell'istanza di esecuzione.|  
|end_time|**datetimeoffsset**|Ora di fine dell'istanza di esecuzione.|  
|caller_sid|**varbinary(85)**|ID di sicurezza (SID) dell'utente se per l'accesso è stata utilizzata l'autenticazione di Windows.|  
|caller_name|**nvarchar(128)**|Nome dell'account tramite cui è stata eseguita l'operazione.|  
|process_id|**int**|ID processo del programma esterno, se applicabile.|  
|stopped_by_sid|**varbinary(85)**|ID di sicurezza (SID) dell'utente che ha arrestato l'istanza di esecuzione.|  
|stopped_by_name|**nvarchar(128)**|Nome dell'utente che ha arrestato l'istanza di esecuzione.|  
|total_physical_memory_kb|**bigint**|Memoria fisica totale disponibile (in megabyte) nel server all'avvio dell'esecuzione.|  
|available_physical_memory_kb|**bigint**|Memoria fisica disponibile (in megabyte) nel server all'avvio dell'esecuzione.|  
|total_page_file_kb|**bigint**|Memoria in pagine totale (in megabyte) nel server all'avvio dell'esecuzione.|  
|available_page_file_kb|**bigint**|Memoria in pagine disponibile (in megabyte) nel server all'avvio dell'esecuzione.|  
|cpu_count|**int**|Numero di CPU logiche nel server all'avvio dell'esecuzione.|  
|server_name|**nvarchar(128)**|Informazioni relative al server Windows e all'istanza per un'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|machine_name|**nvarchar(128)**|Nome del computer in cui è in esecuzione l'istanza del server.|  
|dump_id|**uniqueidentifier**|ID di un dump di esecuzione.|  
  
## <a name="remarks"></a>Commenti  
 In questa vista viene visualizzata una riga per ogni istanza di esecuzione nel catalogo.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'istanza di esecuzione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
> [!NOTE]  
>  È applicata la sicurezza a livello di riga, pertanto vengono visualizzate solo le righe per le quali si dispone delle autorizzazioni per la visualizzazione.  
  
  

---
description: catalog.execution_data_statistics
title: catalog.execution_data_statistics | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 6f51407e-0e4e-4b44-af33-db14c9d40ded
author: chugugrace
ms.author: chugu
ms.openlocfilehash: daf22fecd119c23a60cdaf33f80d30331f9f7a5b
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835308"
---
# <a name="catalogexecution_data_statistics"></a>catalog.execution_data_statistics 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Nella vista viene visualizzata una riga ogni volta che un componente flusso di dati invia dati a un componente a valle, per l'esecuzione di un determinato pacchetto. Le informazioni contenute in questa vista possono essere utilizzate per calcolare la velocità effettiva dei dati per un componente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|data_stats_id|**bigint**|Identificatore univoco (ID) dei dati.|  
|execution_id|**bigint**|ID univoco per l'istanza di esecuzione.|  
|package_name|**nvarchar(260)**|Nome del primo pacchetto avviato durante l'esecuzione.|  
|task_name|**nvarchar(4000)**|Nome dell'attività del flusso di dati.|  
|dataflow_path_id_string|**nvarchar(4000)**|Stringa di identificazione del percorso del flusso di dati.|  
|dataflow_path_name|**nvarchar(4000)**|Nome del percorso del flusso di dati.|  
|source_component_name|**nvarchar(4000)**|Nome del componente flusso di dati che invia i dati.|  
|destination_component_name|**nvarchar(4000)**|Nome del componente flusso di dati che riceve i dati.|  
|rows_sent|**bigint**|Numero di righe inviate dal componente di origine.|  
|created_time|**datatimeoffset(7)**|Ora in cui vengono ottenuti i valori.|  
|execution_path|**nvarchar(max)**|Percorso di esecuzione del componente.|  
  
## <a name="remarks"></a>Osservazioni  
  
-   In presenza di più output dal componente, viene aggiunta una riga per ogni output.  
  
-   Per impostazione predefinita, quando viene avviata un'esecuzione, le informazioni sul numero di righe inviate non vengono registrate.  
  
-   Per visualizzare i dati per l'esecuzione di un determinato pacchetto, impostare il livello di registrazione su **Verbose**. Per altre informazioni, vedere [Abilitare la registrazione per l'esecuzione di pacchetti nel server SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'istanza di esecuzione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
> [!NOTE]  
>  Quando si dispone delle autorizzazioni per eseguire un'operazione nel server, si dispone anche delle autorizzazioni per visualizzare le informazioni sull'operazione. È applicata la sicurezza a livello di riga, pertanto vengono visualizzate solo le righe per le quali si dispone delle autorizzazioni per la visualizzazione.  
  
  

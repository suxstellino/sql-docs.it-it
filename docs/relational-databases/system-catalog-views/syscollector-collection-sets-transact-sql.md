---
description: syscollector_collection_sets (Transact-SQL)
title: syscollector_collection_sets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_collection_sets_TSQL
- syscollector_collection_sets
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_collection_sets view
ms.assetid: db0def92-f25b-45da-9709-eab972b33800
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ffa91193b3c09f64d608c255bb8960b7dfb85823
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199773"
---
# <a name="syscollector_collection_sets-transact-sql"></a>syscollector_collection_sets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fornisce informazioni su un set di raccolta, inclusa pianificazione, modalità di raccolta e stato.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|collection_set_id|**int**|Identificatore locale del set di raccolta. Non ammette i valori Null.|  
|collection_set_uid|**uniqueidentifier**|Identificatore univoco globale del set di raccolta. Non ammette i valori Null.|  
|name|**nvarchar(4000)**|Nome del set di raccolta. Ammette i valori Null.|  
|target|**nvarchar(max)**|Identifica la destinazione per il set di raccolta. Ammette i valori Null.|  
|is_system|**bit**|Attivato (1) o disattivato (0) per indicare se il set di raccolta è stato fornito con l'agente di raccolta dati o se è stato aggiunto in seguito da dc_admin. Potrebbe trattarsi di un set di raccolta personalizzato sviluppato internamente o da terze parti. Non ammette i valori Null.|  
|is_running|**bit**|Indica se il set di raccolta è in esecuzione o no. Non ammette i valori Null.|  
|collection_mode|**smallint**|Specifica la modalità di raccolta per il set di raccolta. Non ammette i valori Null.<br /><br /> La modalità di raccolta è una delle seguenti:<br /><br /> 0 - Modalità cache. La raccolta e il caricamento dei dati seguono una pianificazione differente.<br /><br /> 1 - Modalità non in cache. La raccolta e il caricamento dei dati seguono la stessa pianificazione.|  
|proxy_id|**int**|Identifica il proxy utilizzato per eseguire il passaggio processo del set di raccolta. Ammette i valori Null.|  
|schedule_uid|**uniqueidentifier**|Fornisce un puntatore alla pianificazione del set di raccolta. Ammette i valori Null.|  
|collection_job_id|**uniqueidentifier**|Identifica il processo di raccolta. Ammette i valori Null.|  
|upload_job_id|**uniqueidentifier**|Identifica il processo di caricamento raccolta. Ammette i valori Null.|  
|logging_level|**smallint**|Specifica il livello di registrazione (0, 1 o 2). Non ammette i valori Null.|  
|days_until_expiration|**smallint**|Numero di giorni durante i quali i dati raccolti vengono salvati nel data warehouse di gestione. Non ammette i valori Null.|  
|description|**nvarchar(4000)**|Descrive il set di raccolta. Ammette i valori Null.|  
|dump_on_any_error|**bit**|Attivato (1) o disattivato (0) per indicare se creare un file di [!INCLUDE[ssIS](../../includes/ssis-md.md)] dump in caso di errore. Non ammette i valori Null.|  
|dump_on_codes|**nvarchar(max)**|Contiene l'elenco dei [!INCLUDE[ssIS](../../includes/ssis-md.md)] codici di errore utilizzati per attivare il file dump. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede SELECT per dc_operator, dc_proxy.  
  
## <a name="remarks"></a>Commenti  
 L'API dell'agente di raccolta dati consente di modificare o eliminare solo i set di raccolta da essi creati. I set di raccolta forniti con il sistema non possono essere modificati o eliminati. Tuttavia, è possibile abilitare o disabilitare un set di raccolta del sistema e modificare la configurazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Viste dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)  
  
  

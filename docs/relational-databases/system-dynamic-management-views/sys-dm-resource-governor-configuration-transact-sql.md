---
description: sys.dm_resource_governor_configuration (Transact-SQL)
title: sys.dm_resource_governor_configuration (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_resource_governor_configuration_TSQL
- dm_resource_governor_configuration
- sys.dm_resource_governor_configuration
- sys.dm_resource_governor_configuration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_configuration dynamic management view
ms.assetid: c89aab6a-0434-4ce6-af8c-f8a1a3284e38
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 876433464108e2a2503a46da8281a66ac6abacab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99137532"
---
# <a name="sysdm_resource_governor_configuration-transact-sql"></a>sys.dm_resource_governor_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga che contiene lo stato corrente di configurazione in memoria di Resource Governor.  
  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|classifier_function_id|**int**|ID della funzione di classificazione attualmente utilizzata da Resource Governor. Restituisce un valore di 0 se non è utilizzata alcuna funzione. Non ammette i valori Null.<br /><br /> **Nota:** Questa funzione viene utilizzata per classificare le nuove richieste e utilizza le regole per indirizzare queste richieste al gruppo di carico di lavoro appropriato. Per altre informazioni, vedere [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).|  
|is_reconfiguration_pending|**bit**|Indica se le modifiche a un gruppo o a un pool sono state realizzate o meno con l'istruzione ALTER RESOURCE GOVERNOR RECONFIGURE, senza che siano state applicate alla configurazione in memoria. Il valore restituito è uno dei seguenti:<br /><br /> 0 - Non è richiesta un'istruzione di riconfigurazione.<br /><br /> 1 - È richiesta un'istruzione di riconfigurazione o un riavvio del server per applicare le modifiche di configurazione in sospeso.<br /><br /> **Nota:** Il valore restituito è sempre 0 quando Resource Governor è disabilitato.<br /><br /> Non ammette i valori Null.|  
|max_outstanding_io_per_volume|**int**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Numero massimo di operazioni di I/O in sospeso per volume.|  
  
## <a name="remarks"></a>Commenti  
 Questa vista a gestione dinamica mostra la configurazione in memoria. Per vedere i metadati di configurazione memorizzati, utilizzare la vista del catalogo corrispondente.  
  
 Nell'esempio seguente viene illustrato come ottenere e confrontare i valori dei metadati archiviati e i valori in memoria della configurazione di Resource Governor.  
  
```  
USE master;  
go  
-- Get the stored metadata.  
SELECT   
object_schema_name(classifier_function_id) AS 'Classifier UDF schema in metadata',   
object_name(classifier_function_id) AS 'Classifier UDF name in metadata'  
FROM   
sys.resource_governor_configuration;  
go  
-- Get the in-memory configuration.  
SELECT   
object_schema_name(classifier_function_id) AS 'Active classifier UDF schema',   
object_name(classifier_function_id) AS 'Active classifier UDF name'  
FROM   
sys.dm_resource_governor_configuration;  
go  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.resource_governor_configuration &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-resource-governor-configuration-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)  
  
  


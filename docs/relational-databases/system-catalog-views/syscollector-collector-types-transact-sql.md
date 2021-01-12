---
description: syscollector_collector_types (Transact-SQL)
title: syscollector_collector_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syscollector_collector_types
- syscollector_collector_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_collector_types view
ms.assetid: d5cd30bb-89fd-4814-a7e8-9074f043f90f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 04a9bc5f6336119d08285ae73dfa7a09ae5d3648
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094259"
---
# <a name="syscollector_collector_types-transact-sql"></a>syscollector_collector_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fornisce informazioni su un tipo di agente di raccolta per un elemento della raccolta.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**collector_type_uid**|**uniqueidentifer**|GUID per un tipo di raccolta. Non ammette i valori Null.|  
|**nome**|**sysname**|Nome del tipo di raccolta. Non ammette i valori Null.|  
|**parameter_schema**|**xml**|XML Schema che descrive la configurazione del tipo di agente di raccolta specificato. Questo XML Schema è utilizzato per convalidare la configurazione XML effettiva associata a una particolare istanza dell'elemento della raccolta. Ammette i valori Null.|  
|**parameter_formatter**|**xml**|Determina il modello da utilizzare per trasformare l'XML per l'utilizzo nella pagina delle proprietà del set di raccolta. Ammette i valori Null.|  
|**collection_package_id**|**uniqueidentifer**|GUID per un pacchetto di raccolta. Non ammette i valori Null.|  
|**collection_package_path**|**nvarchar(4000)**|Fornisce il percorso al pacchetto di raccolta. Ammette i valori Null.|  
|**collection_package_name**|**sysname**|Nome del pacchetto di raccolta. Non ammette i valori Null.|  
|**upload_package_id**|**uniqueidentifer**|GUID per il pacchetto di caricamento. Non ammette i valori Null.|  
|**upload_package_path**|**nvarchar(4000)**|Fornisce il percorso al pacchetto di caricamento. Ammette i valori Null.|  
|**upload_package_name**|**sysname**|Nome del pacchetto di caricamento. Non ammette i valori Null.|  
|**is_system**|**bit**|Attivato (1) o disattivato (0) per indicare se il tipo di agente di raccolta è stato fornito con l'agente di raccolta dati o se è stato aggiunto successivamente dal **dc_admin**. Potrebbe trattarsi di un tipo personalizzato sviluppato internamente o da terze parti. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede SELECT per **dc_operator**, **dc_proxy**.  
  
## <a name="change-history"></a>Cronologia modifiche  
  
|Contenuto aggiornato|  
|---------------------|  
|Aggiornamento del nome della colonna **collection_type_uid** **collector_type_uid**.|  
|Correzione della descrizione della colonna **parameter_schema** per indicare che ammette valori null.|  
|Aggiunta della colonna **parameter_formatter** .|  
|Correzione del tipo di dati per la colonna **collection_package_path** e aggiornamento della descrizione per indicare che ammette valori null.|  
|Correzione del tipo di dati per la colonna **upload_package_path** e aggiornamento della descrizione per indicare che ammette valori null.|  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Viste dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)  
  
  

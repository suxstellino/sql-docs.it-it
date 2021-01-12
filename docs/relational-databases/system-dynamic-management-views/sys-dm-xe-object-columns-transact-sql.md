---
description: sys.dm_xe_object_columns (Transact-SQL)
title: sys.dm_xe_object_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_xe_object_columns
- sys.dm_xe_object_columns_TSQL
- dm_xe_object_columns_TSQL
- dm_xe_object_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_object_columns dynamic management view
- extended events [SQL Server], views
ms.assetid: d96a14f3-4284-45ff-b1fe-4858e540a013
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5da079cfecbf1ddaa622f05ce71e5c04d63f4f0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095061"
---
# <a name="sysdm_xe_object_columns-transact-sql"></a>sys.dm_xe_object_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni sullo schema per tutti gli oggetti.  
  
> [!NOTE]  
>  Gli oggetti evento espongono schemi fissi per i dati di sola lettura e di lettura/scrittura.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Nome della colonna. il nome è univoco all'interno dell'oggetto. Non ammette i valori Null.|  
|column_id|**int**|Identificatore della colonna. column_id è univoco all'interno dell'oggetto se utilizzato con column_type. Non ammette i valori Null.|  
|object_name|**nvarchar(256)**|Nome dell'oggetto a cui appartiene la colonna. Esiste una relazione molti-a-uno con sys.dm_xe_objects. ID. Non ammette valori null.|  
|object_package_guid|**uniqueidentifier**|GUID del pacchetto che contiene l'oggetto. Non ammette i valori Null.|  
|type_name|**nvarchar(256)**|Nome del tipo per questa colonna. Non ammette i valori Null.|  
|type_package_guid|**uniqueidentifier**|GUID del pacchetto che contiene il tipo di dati della colonna. Non ammette i valori Null.|  
|column_type|**nvarchar(60)**|Indica il modo in cui viene utilizzata la colonna. Non ammette i valori Null. column_type può essere uno dei seguenti:<br /><br /> readonly. La colonna contiene un valore statico che non può essere modificato.<br /><br /> del modello. La colonna contiene dati runtime esposti dall'oggetto.<br /><br /> customizable. La colonna contiene un valore che può essere modificato.<br /><br /> Nota: la modifica di questo valore può modificare il comportamento dell'oggetto.|  
|column_value|**nvarchar(256)**|Visualizza valori statici associati alla colonna dell'oggetto. Ammette i valori Null.|  
|capabilities|**int**|Bitmap che descrive le funzionalità della colonna. Ammette i valori Null.|  
|capabilities_desc|**nvarchar(256)**|Descrizione delle funzionalità della colonna dell'oggetto. I valori validi sono i seguenti:<br /><br /> Mandatory. Il valore deve essere impostato in caso di associazione dell'oggetto padre a una sessione dell'evento.<br /><br /> Ammette i valori Null.|  
|description|**nvarchar (3072)**|Descrizione della colonna dell'oggetto. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
### <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_object_columns.object_name, sys.dm_xe_object_columns.object_package_guid|sys.dm_xe_objects.name,<br /><br /> sys.dm_xe_objects.package_guid|Molti-a-uno|  
|sys.dm_xe_object_columns.type_name<br /><br /> sys.dm_xe_object_columns.type_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_objects.package_guid|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  


---
description: sys.dm_xe_database_session_object_columns (database SQL di Azure)
title: sys.dm_xe_database_session_object_columns
titleSuffix: Azure SQL Database
ms.date: 06/10/2016
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 0e6adc54-4d97-4ef0-bf4f-b4538d69f136
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 0831d3c4f6634e1b7c5ae2568d853567eeaf32b0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093947"
---
# <a name="sysdm_xe_database_session_object_columns-azure-sql-database"></a>sys.dm_xe_database_session_object_columns (database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Mostra i valori di configurazione per gli oggetti associati a una sessione.  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 e a tutte le versioni successive.|  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Indirizzo di memoria della sessione dell'evento. Ha una relazione molti-a-uno con sys.dm_xe_database_sessions. Address. Non ammette i valori Null.|  
|column_name|**nvarchar(60)**|Nome del valore di configurazione. Non ammette i valori Null.|  
|column_id|**int**|ID della colonna. Valore univoco all'interno dell'oggetto. Non ammette i valori Null.|  
|column_value|**nvarchar(2048)**|Valore configurato della colonna. Ammette i valori Null.|  
|object_type|**nvarchar(60)**|Tipo dell'oggetto.  Non è nullable.object_type è uno dei seguenti:<br /><br /> evento<br /><br /> target|  
|object_name|**nvarchar(60)**|Nome dell'oggetto a cui appartiene la colonna. Non ammette i valori Null.|  
|object_package_guid|**uniqueidentifier**|GUID del pacchetto che contiene l'oggetto. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|dm_xe_database_session_object_columns dm_xe_database_session_object_columns.object_name<br /><br /> dm_xe_database_session_object_columns dm_xe_database_session_object_columns.object_package_guid|sys.dm_xe_objects.package_guid<br /><br /> sys.dm_xe_objects.name|Molti-a-uno|  
|dm_xe_database_session_object_columns dm_xe_database_session_object_columns.column_name<br /><br /> dm_xe_database_session_object_columns dm_xe_database_session_object_columns.column_id|sys.dm_xe_object_columns.name<br /><br /> sys.dm_xe_object_columns.column_id|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
  

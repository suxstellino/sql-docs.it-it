---
description: sys.dm_db_objects_impacted_on_version_change (Database di SQL Azure)
title: sys.dm_db_objects_impacted_on_version_change
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_objects_impacted_on_version_change_TSQL
- dm_db_objects_impacted_on_version_change
- dm_db_objects_impacted_on_version_change_TSQL
- sys.dm_db_objects_impacted_on_version_change
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_objects_impacted_on_version_change
- sys.dm_db_objects_impacted_on_version_change
ms.assetid: b94af834-c4f6-4a27-80a6-e8e71fa8793a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: f7830e874026b9d5af250fde294c8c72294d013d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097701"
---
# <a name="sysdm_db_objects_impacted_on_version_change-azure-sql-database"></a>sys.dm_db_objects_impacted_on_version_change (Database di SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Questa vista di sistema con ambito database è progettata per fornire un sistema di avviso anticipato per determinare gli oggetti che saranno interessati da un aggiornamento importante del [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. È possibile utilizzare la vista prima o dopo l'aggiornamento per ottenere un'enumerazione completa degli oggetti interessati. È necessario eseguire query su questa vista in ogni database per ottenere un conteggio completo per l'intero server.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|classe|**int** NOT NULL|Classe dell'oggetto che sarà interessato:<br /><br /> **1** = vincolo<br /><br /> **7** = indici e heap|  
|class_desc|**nvarchar (60)** NOT NULL|Descrizione della classe:<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **INDEX**|  
|major_id|**int** NOT NULL|ID oggetto del vincolo o ID oggetto della tabella che contiene l'indice o l'heap.|  
|minor_id|**int** NULL|**NULL** per vincoli<br /><br /> Index_id per indici e heap|  
|dependency|**nvarchar (60)** NOT NULL|Descrizione della dipendenza che causerà l'interessamento di un vincolo o di un indice. Lo stesso valore viene inoltre utilizzato per gli avvisi generati durante l'aggiornamento.<br /><br /> Esempi:<br /><br /> **space** (per tipo intrinseco)<br /><br /> **geometry** (per UDT di sistema)<br /><br /> **geography::Parse** (per metodo UDT di sistema)|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene mostrata una query sulla vista **sys.dm_db_objects_impacted_on_version_change** per trovare gli oggetti interessati da un aggiornamento alla prossima versione principale del server.  
  
```  
SELECT * FROM sys.dm_db_objects_disabled_on_version_change;  
GO  
```  
  
```  
class  class_desc        major_id    minor_id    dependency                       
------ ----------------- ----------- ----------- ----------   
1      OBJECT_OR_COLUMN  181575685   NULL        geometry                        
7      INDEX             37575172    1           geometry                        
7      INDEX             2121058592  1           geometry                        
1      OBJECT_OR_COLUMN  101575400   NULL        geometry     
```  
  
## <a name="remarks"></a>Osservazioni  
  
### <a name="how-to-update-impacted-objects"></a>Come aggiornare gli oggetti interessati  
 Nei passaggi ordinati seguenti viene descritta l'azione correttiva da intraprendere dopo l'aggiornamento dalla versione del servizio di giugno.  
  
|JSON|Oggetto interessato|Azione correttiva|  
|-----------|---------------------|-----------------------|  
|1|**Indici**|Ricompilare gli indici identificati da **sys.dm_db_objects_impacted_on_version_change** ad esempio:  `ALTER INDEX ALL ON <table> REBUILD`<br />oppure<br />`ALTER TABLE <table> REBUILD`|  
|2|**Object**|Tutti i vincoli identificati dalla vista **sys.dm_db_objects_impacted_on_version_change** devono essere riconvalidati dopo la rielaborazione dei dati geometry e geography nella tabella sottostante. Per i convalidi, riconvalidare utilizzando ALTER TABLE. <br />Ad esempio: <br />`ALTER TABLE <tab> WITH CHECK CHECK CONSTRAINT <constraint name>`<br />o<br />`ALTER TABLE <tab> WITH CHECK CONSTRAINT ALL`|  
  
  

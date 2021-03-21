---
title: IDBProperties (OLE DB Driver) | Microsoft Docs
description: Informazioni sull'interfaccia IDBProperties in OLE DB driver per SQL Server, incluso il metodo IDBProperties::GetPropertyInfo.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8cb88bb3064dde4f997095cb8d39e61d26e0ab67
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752601"
---
# <a name="idbproperties-ole-db"></a>IDBProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  La specifica dello standard OLE DB consente ai provider di specificare VT_EMPTY per **DBPROPINFO::vValues**. Tuttavia, il driver OLE DB per SQL Server restituisce sempre VT_EMPTY quando si chiama **IDBProperties::GetPropertyInfo** con **DBPROPSET_ROWSETALL** per recuperare le proprietà dei set di righe.  
  
## <a name="see-also"></a>Vedere anche  
 [Interfacce &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md) 
  
  

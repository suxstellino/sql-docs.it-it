---
title: ISSCommandWithParameters (OLE DB Driver)
description: Informazioni su come l'interfaccia ISSCommandWithParameters supporta i tipi definiti dall'utente e XML di SQL Server in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSCommandWithParameters interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a30947176efb820a7105f005e3d372f090d4190
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172169"
---
# <a name="isscommandwithparameters-ole-db"></a>ISSCommandWithParameters (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L'interfaccia **ISSCommandWithParameters** espone il supporto per i tipi definiti dall'utente e XML di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Questa interfaccia facoltativa eredita dall'interfaccia OLE DB principale **ICommandWithParameters**. Oltre ai tre metodi ereditati da **ICommandWithParameters**, **GetParameterInfo**, **MapParameterNames** e **SetParameterInfo**, **ISSCommandWithParameters** fornisce due nuovi metodi che vengono utilizzati per gestire i tipi di dati specifici del server.  
  
> [!NOTE]  
>  L'interfaccia **ISSCommandWithParameters** può essere impiegata quando si utilizzano i componenti di servizio che tuttavia non la utilizzeranno.  
  
|Metodo|Descrizione|  
|------------|-----------------|  
|[ISSCommandWithParameters::GetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md)|Restituisce una struttura del set di proprietà **SSPARAMPROPS** della matrice per ogni parametro XML o tipo definito dall'utente passato al comando. Per gli altri tipi di parametro non ne restituisce nessuna.|  
|[ISSCommandWithParameters::SetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md)|Imposta le proprietà per i singoli parametri in base al numero ordinale oppure imposta proprietà dei parametri bulk specificando una matrice di strutture **SSPARAMPROPS**.|  
  
## <a name="see-also"></a>Vedere anche  
 [Interfacce &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [Uso di tipi di dati XML](../../oledb/features/using-xml-data-types.md)   
 [Uso dei tipi definiti dall'utente](../../oledb/features/using-user-defined-types.md)  
  
  

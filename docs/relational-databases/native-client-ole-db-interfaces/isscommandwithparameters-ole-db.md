---
description: ISSCommandWithParameters (provider OLE DB Native Client)
title: ISSCommandWithParameters (provider OLE DB Native Client)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSCommandWithParameters (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSCommandWithParameters interface
ms.assetid: 3419b7f3-36a3-4863-816e-e641e4e90496
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7924ee21c5bbf6184654303328952afb93fc97cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469322"
---
# <a name="isscommandwithparameters-native-client-ole-db-provider"></a>ISSCommandWithParameters (provider OLE DB Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **ISSCommandWithParameters** espone il supporto per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] XML e i tipi definiti dall'utente (UDT). Si tratta di un'interfaccia facoltativa che eredita dall'interfaccia di base OLE DB **ICommandWithParameters**. Oltre ai tre metodi ereditati da **ICommandWithParameters**; **GetParameterInfo**, **MapParameterNames** e **separameterinfo**; In **ISSCommandWithParameters** sono disponibili due nuovi metodi utilizzati per gestire i tipi di dati specifici del server.  
  
> [!NOTE]  
>  L'interfaccia **ISSCommandWithParameters** può essere utilizzata quando si utilizzano i componenti del servizio, ma i componenti del servizio non utilizzeranno questa interfaccia.  
  
|Metodo|Descrizione|  
|------------|-----------------|  
|[ISSCommandWithParameters::GetParameterProperties &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md)|Restituisce una struttura del set di proprietà **SSPARAMPROPS** della matrice per ogni parametro XML o tipo definito dall'utente passato al comando. Per gli altri tipi di parametro non ne restituisce nessuna.|  
|[ISSCommandWithParameters::SetParameterProperties &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md)|Imposta le proprietà per i singoli parametri in base al numero ordinale oppure imposta proprietà dei parametri bulk specificando una matrice di strutture **SSPARAMPROPS**.|  
  
## <a name="see-also"></a>Vedere anche  
 [Interfacce &#40;OLE DB&#41;](./sql-server-native-client-ole-db-interfaces.md)   
 [Uso di tipi di dati XML](../../relational-databases/native-client/features/using-xml-data-types.md)   
 [Uso dei tipi definiti dall'utente](../../relational-databases/native-client/features/using-user-defined-types.md)  
  

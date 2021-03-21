---
title: Individuazione dei metadati | Microsoft Docs
description: Informazioni sul modo in cui i miglioramenti apportati all'individuazione dei metadati consentono alle applicazioni OLE DB Driver per SQL Server di garantire la compatibilità dei metadati.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d0b4a7d53f857f34f2d7fea85e0213145efd1699
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742461"
---
# <a name="metadata-discovery"></a>Individuazione dei metadati
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  I miglioramenti apportati all'individuazione dei metadati in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] consentono alle applicazioni del driver OLE DB per SQL Server di garantire che i metadati delle colonne o dei parametri restituiti dall'esecuzione di una query siano identici a o compatibili con il formato dei metadati specificato prima di eseguire la query. Se i metadati restituiti dopo l'esecuzione di una query non sono compatibili con il formato dei metadati specificato prima dell'esecuzione della query, viene generato un errore.  
  
 In bcp nonché nelle interfacce IBCPSession e IBCPSession2, è ora possibile specificare una lettura ritardata (individuazione dei metadati ritardata) per evitare l'individuazione dei metadati per le operazioni di esportazione di query. In questo modo, è possibile migliorare le prestazioni ed eliminare gli errori di individuazione dei metadati.  
  
 Se si sviluppa un'applicazione utilizzando il driver OLE DB per SQL Server ma si esegue la connessione a una versione del server precedente a [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], la funzionalità di individuazione dei metadati corrisponderà alla versione del server.  
  
## <a name="remarks"></a>Osservazioni   
 Le funzioni membro OLE DB seguenti sono state migliorate in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] per garantire una migliore individuazione dei metadati:  
  
-   IColumnsInfo::GetColumnInfo  
  
-   IColumnsRowset::GetColumnsRowset  
  
-   ICommandWithParameters:: GetParameterInfo (per altre informazioni, vedere [ICommandWithParameters](../../oledb/ole-db-interfaces/icommandwithparameters.md))  
  
 È inoltre possibile notare un miglioramento nelle prestazioni quando si specifica il formato dei metadati utilizzando IBCPSession::BCPSetBulkMode  
  
 Il miglioramento dell'individuazione dei metadati in OLE DB Driver per SQL Server è stato reso possibile dall'aggiunta di due stored procedure in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]:  
  
-   sp_describe_first_result_set  
  
-   sp_describe_undeclared_parameters  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  

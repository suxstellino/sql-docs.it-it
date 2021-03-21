---
title: Informazioni nelle interfacce di errore
description: OLE DB Driver per SQL Server segnala informazioni sullo stato e sugli errori nelle interfacce di errore definite da OLE DB IErrorInfo, IErrorRecords e ISQLErrorInfo.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- IErrorRecords interface
- IErrorInfo interface
- OLE DB error handling, error interfaces
- ISQLErrorInfo interface
- errors [OLE DB], error interfaces
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4f5789606d3e4ad25f8ba714f95f0427171f1597
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742121"
---
# <a name="information-in-error-interfaces"></a>Informazioni nelle interfacce di errore
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il driver OLE DB per SQL Server segnala informazioni sullo stato e sugli errori nelle interfacce di errore definite da OLE DB **IErrorInfo**, **IErrorRecords** e **ISQLErrorInfo**.  
  
 OLE DB Driver per SQL Server supporta le funzioni membro di **IErrorInfo** come illustrato di seguito.  
  
|Funzione membro|Descrizione|  
|---------------------|-----------------|  
|**GetDescription**|Stringa descrittiva del messaggio di errore.|  
|**GetGUID**|GUID dell'interfaccia che ha definito l'errore.|  
|**GetHelpContext**|Non supportato. Restituisce sempre zero.|  
|**GetHelpFile**|Non supportato. Viene restituito sempre NULL.|  
|**GetSource**|Stringa "Microsoft OLE DB Driver per SQL Server".|  
  
 OLE DB Driver per SQL Server supporta le funzioni membro di **IErrorRecords** disponibili per il consumer come illustrato di seguito.  
  
|Funzione membro|Descrizione|  
|---------------------|-----------------|  
|**GetBasicErrorInfo**|Inserisce in una struttura ERRORINFO le informazioni di base su un errore. Una struttura ERRORINFO contiene membri che identificano il valore restituito HRESULT per l'errore nonché il provider e l'interfaccia alle quali si applica l'errore.|  
|**GetCustomErrorObject**|Restituisce un riferimento nelle interfacce **ISQLErrorInfo** e [ISQLServerErrorInfo](../ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md).|  
|**GetErrorInfo**|Restituisce un riferimento in un'interfaccia **IErrorInfo**.|  
|**GetErrorParameters**|OLE DB Driver per SQL Server non restituisce parametri al consumer tramite **GetErrorParameters**.|  
|**GetRecordCount**|Conteggio dei record di errore disponibili.|  
  
 OLE DB Driver per SQL Server supporta i parametri **ISQLErrorInfo::GetSQLInfo** come illustrato di seguito.  
  
|Parametro|Descrizione|  
|---------------|-----------------|  
|*pbstrSQLState*|Restituisce un valore SQLSTATE per l'errore. I valori SQLSTATE vengono definiti nelle specifiche API, SQL-92, ODBC e ISO SQL. Né [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] né OLE DB Driver per SQL Server hanno definito valori SQLSTATE specifici dell'implementazione.|  
|*plNativeError*|Restituisce il numero di errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] da **master.dbo.sysmessages**, quando disponibile. Gli errori nativi sono disponibili dopo un tentativo riuscito di inizializzare un'origine dati di OLE DB Driver per SQL Server. Prima del tentativo, OLE DB Driver per SQL Server restituisce sempre zero.|  
  
## <a name="see-also"></a>Vedere anche  
 [Errori](../../oledb/ole-db-errors/errors.md)  
  

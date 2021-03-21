---
title: OLE DB Errors
description: Informazioni sul modo in cui vengono restituiti gli errori in OLE DB Driver per SQL Server e su come è possibile ottenere informazioni su di essi.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- OLE/COM errors
- errors [OLE DB]
- OLE DB error handling, about error handling
- OLE DB error handling
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: eb30ffa542d584626f762c0d0eeb990426a5f152
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741951"
---
# <a name="errors"></a>Errors
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Gli oggetti OLE/COM segnalano gli errori tramite il codice restituito HRESULT delle funzioni membro oggetto. Un HRESULT OLE/COM è una struttura costituita da pacchetti di byte. OLE fornisce macro che risolvono i riferimenti dei membri di struttura.  
  
 OLE/COM specifica l'interfaccia **IErrorInfo**. L'interfaccia espone metodi come **GetDescription**. In questo modo, i client possono estrarre i dettagli relativi agli errori dai server OLE/COM. OLE DB estende **IErrorInfo** per supportare la restituzione di più pacchetti di informazioni sugli errori nell'esecuzione di una funzione con singolo membro.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] può restituire più errori. Un'applicazione può recuperare gli errori del server uno alla volta chiamando [IMultipleResults::GetResult](/previous-versions/windows/desktop/ms721289(v=vs.85)) insieme a ISQLErrorInfo e IErrorRecords.  
  
 Il driver OLE DB per SQL Server espone le interfacce per oggetti errore seguenti: l'interfaccia OLE DB **IErrorInfo** ottimizzata per i record, l'interfaccia **IErrorRecords** personalizzata e l'interfaccia [ISQLErrorInfo](../ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md) specifica del provider.  
  
 Per informazioni sulla traccia degli errori, vedere [Data Access Tracing](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)) (Traccia di accesso ai dati). Per informazioni sui miglioramenti apportati alla traccia degli errori aggiunta in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], vedere [Accesso alle informazioni di diagnostica nel log degli eventi estesi](../../oledb/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Codici restituiti](../../oledb/ole-db-errors/return-codes.md)  
  
-   [Informazioni nelle interfacce di errore](../../oledb/ole-db-errors/information-in-error-interfaces.md)  
  
-   [Dettagli relativi agli errori di SQL Server](../../oledb/ole-db-errors/sql-server-error-detail.md)  
  
-   [Recupero delle informazioni sugli errori](../../oledb/ole-db-errors/retrieving-error-information.md)  
  
-   [Risultati dei messaggi di SQL Server](../../oledb/ole-db-errors/sql-server-message-results.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  

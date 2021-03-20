---
title: Dettagli sugli errori di SQL Server (OLE DB Driver)
description: Informazioni sull'interfaccia di errore specifica del provider ISQLServerErrorInfo in OLE DB Driver per SQL Server, che restituisce informazioni dettagliate su un errore di SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- errors [OLE DB], error details
- IErrorRecords interface
- IErrorInfo interface
- OLE DB error handling, error details
- ISQLServerErrorInfo interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8ebb23d36e4f305e7e3a1ed4cab338d4ddb68760
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741801"
---
# <a name="sql-server-error-detail"></a>Dettagli relativi agli errori SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver per SQL Server definisce l'interfaccia degli errori specifica del provider [ISQLServerErrorInfo](../ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md). L'interfaccia restituisce maggiori dettagli relativi agli errori [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e risulta molto utile quando operazioni di esecuzione di comandi o del set di righe non riescono.  
  
 È possibile accedere all'interfaccia **ISQLServerErrorInfo** in due modi.  
  
 Il consumer può chiamare **IErrorRecords::GetCustomerErrorObject** per ottenere un puntatore **ISQLServerErrorInfo**, come indicato nell'esempio di codice seguente. Non è necessario ottenere **ISQLErrorInfo.** Sia **ISQLErrorInfo** sia **ISQLServerErrorInfo** sono oggetti errore OLE DB personalizzati, in cui **ISQLServerErrorInfo** rappresenta l'interfaccia da usare per ottenere informazioni sugli errori del server, inclusi dettagli quali i numeri di riga e il nome di procedura.  
  
```  
// Get the SQL Server custom error object.  
if(FAILED(hr=pIErrorRecords->GetCustomErrorObject(  
   nRec, IID_ISQLServerErrorInfo,  
   (IUnknown**)&pISQLServerErrorErrorInfo)))  
```  
  
 Un altro modo per ottenere un puntatore **ISQLServerErrorInfo** consiste nel chiamare il metodo **QueryInterface** su un puntatore **ISQLErrorInfo** già ottenuto. Dal momento che **ISQLServerErrorInfo** contiene un superset delle informazioni disponibili in **ISQLErrorInfo**, è consigliabile accedere direttamente a **ISQLServerErrorInfo** mediante **GetCustomerErrorObject**.  
  
 L'interfaccia **ISQLServerErrorInfo** espone una funzione membro, [ISQLServerErrorInfo::GetErrorInfo](../../oledb/ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md). La funzione restituisce un puntatore a una struttura SSERRORINFO e un puntatore a un buffer di stringhe. Entrambi i puntatori fanno riferimento a una memoria che il consumer deve deallocare usando il metodo **IMalloc::Free**.  
  
 I membri di struttura SSERRORINFO vengono interpretati dal consumer come segue.  
  
|Membro|Descrizione|  
|------------|-----------------|  
|*pwszMessage*|Messaggio di errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Identico alla stringa restituita in **IErrorInfo::GetDescription**.|  
|*pwszServer*|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per la sessione.|  
|*pwszProcedure*|Se appropriato, restituisce il nome della stored procedure in cui ha avuto origine l'errore. In caso contrario, una stringa vuota.|  
|*lNative*|Numero di errore nativo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Identico al valore restituito nel parametro *plNativeError* di **ISQLErrorInfo::GetSQLInfo**.|  
|*bState*|Stato di un messaggio di errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*bClass*|Gravità di un messaggio di errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*wLineNumber*|Quando è applicabile, restituisce il numero di riga di una stored procedure in cui si è verificato l'errore.|  
  
## <a name="see-also"></a>Vedere anche  
 [Errori](../../oledb/ole-db-errors/errors.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../../t-sql/language-elements/raiserror-transact-sql.md)  
  

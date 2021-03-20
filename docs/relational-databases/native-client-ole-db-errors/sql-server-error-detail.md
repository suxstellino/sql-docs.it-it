---
description: Dettagli errore SQL Server Native Client
title: Dettagli errore SQL Server (provider OLE DB Native Client)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, errors
- errors [OLE DB], error details
- IErrorRecords interface
- IErrorInfo interface
- OLE DB error handling, error details
- ISQLServerErrorInfo interface
ms.assetid: 51500ee3-3d78-47ec-b90f-ebfc55642e06
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eac31d0cec00655071aa84561c2f0f77f8e681ce
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738821"
---
# <a name="sql-server-native-client-error-detail"></a>Dettagli errore SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client definisce l'interfaccia di errore specifica del provider [ISQLServerErrorInfo](../native-client-ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md). L'interfaccia restituisce maggiori dettagli relativi agli errori [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e risulta molto utile quando operazioni di esecuzione di comandi o del set di righe non riescono.  
  
 È possibile accedere all'interfaccia **ISQLServerErrorInfo** in due modi.  
  
 Il consumer può chiamare **IErrorRecords::GetCustomerErrorObject** per ottenere un puntatore **ISQLServerErrorInfo**, come indicato nell'esempio di codice seguente. Non è necessario ottenere **ISQLErrorInfo.** Sia **ISQLErrorInfo** sia **ISQLServerErrorInfo** sono oggetti errore OLE DB personalizzati, in cui **ISQLServerErrorInfo** rappresenta l'interfaccia da usare per ottenere informazioni sugli errori del server, inclusi dettagli quali i numeri di riga e il nome di procedura.  
  
```  
// Get the SQL Server custom error object.  
if(FAILED(hr=pIErrorRecords->GetCustomErrorObject(  
   nRec, IID_ISQLServerErrorInfo,  
   (IUnknown**)&pISQLServerErrorErrorInfo)))  
```  
  
 Un altro modo per ottenere un puntatore **ISQLServerErrorInfo** consiste nel chiamare il metodo **QueryInterface** su un puntatore **ISQLErrorInfo** già ottenuto. Dal momento che **ISQLServerErrorInfo** contiene un superset delle informazioni disponibili in **ISQLErrorInfo**, è consigliabile accedere direttamente a **ISQLServerErrorInfo** mediante **GetCustomerErrorObject**.  
  
 L'interfaccia **ISQLServerErrorInfo** espone una funzione membro, [ISQLServerErrorInfo::GetErrorInfo](../../relational-databases/native-client-ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md). La funzione restituisce un puntatore a una struttura SSERRORINFO e un puntatore a un buffer di stringhe. Entrambi i puntatori fanno riferimento a una memoria che il consumer deve deallocare usando il metodo **IMalloc::Free**.  
  
 I membri di struttura SSERRORINFO vengono interpretati dal consumer come segue.  
  
|Membro|Descrizione|  
|------------|-----------------|  
|*pwszMessage*|Messaggio di errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Identico alla stringa restituita in **IErrorInfo::GetDescription**.|  
|*pwszServer*|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la sessione.|  
|*pwszProcedure*|Se appropriato, restituisce il nome della stored procedure in cui ha avuto origine l'errore. In caso contrario, una stringa vuota.|  
|*lNative*|Numero di errore nativo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Identico al valore restituito nel parametro *plNativeError* di **ISQLErrorInfo::GetSQLInfo**.|  
|*bState*|Stato di un messaggio di errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*bClass*|Gravità di un messaggio di errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*wLineNumber*|Quando è applicabile, restituisce il numero di riga di una stored procedure in cui si è verificato l'errore.|  
  
## <a name="see-also"></a>Vedere anche  
 [Errori](../../relational-databases/native-client-ole-db-errors/errors.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)  
  
